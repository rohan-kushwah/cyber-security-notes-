# **Complete HiDPI Scaling Guide for Kali Linux (125% Scale)**

## **Introduction: Understanding Display Scaling Methods**

When dealing with high-resolution displays on small screens (like a 14" laptop with 1920x1200 resolution), text and UI elements can appear too small. There are several ways to handle this scaling issue:

1. **xrandr scaling** (e.g., `xrandr --output eDP --scale 1.25x1.25`) - This method stretches the rendered image, resulting in blurry text and poor quality. NOT recommended.

2. **Lowering resolution** - You could change from 1920x1200 to 1600x1000, but this wastes your display's capabilities and reduces sharpness.

3. **Desktop Environment scaling** - Some DEs offer built-in scaling (especially on Wayland), but support varies and may not work for all applications.

4. **DPI-based scaling** - This method tells applications to render at higher resolution from the start, maintaining sharpness and quality. **This is what this guide focuses on.**

**This document provides a comprehensive guide for implementing proper DPI-based scaling** that will:
- Keep text and images sharp and clear
- Work across different application frameworks (GTK, Qt, X11)
- Persist across reboots
- Scale everything consistently to 125% (or your preferred size)

The DPI method is the most reliable and provides the best quality, which is why we're using it as our primary approach in this guide.

---

## **Table of Contents**
1. [System Requirements Check](#system-requirements-check)
2. [Pre-Configuration Checklist](#pre-configuration-checklist)
3. [Step-by-Step Scaling Setup](#step-by-step-scaling-setup)
4. [Post-Configuration Verification](#post-configuration-verification)
5. [Troubleshooting](#troubleshooting)
6. [Rollback Instructions](#rollback-instructions)

---

## **System Requirements Check**

### **1. Check Your Hardware**

**What this does:** Identifies your laptop model and display specifications  
```bash
sudo dmidecode -t system | grep -A5 "System Information"
```
**Why:** Different hardware may need different scaling approaches

**What this does:** Shows your display resolution and size  
```bash
xrandr --current | grep -A5 "connected"
```
**Why:** Helps calculate if you need scaling (14" with 1920x1200 = needs scaling)

### **2. Check Your Desktop Environment**

**What this does:** Identifies which desktop environment you're using  
```bash
echo $XDG_CURRENT_DESKTOP
```
**Why:** Different DEs have different scaling methods

**Common outputs:**
- `GNOME` → Follow GNOME instructions
- `XFCE` → Follow XFCE instructions  
- `KDE` → Follow KDE instructions
- `MATE` → Follow MATE instructions

### **3. Check Display Server**

**What this does:** Shows if you're using X11 or Wayland  
```bash
echo $XDG_SESSION_TYPE
```
**Why:** Wayland has better scaling support than X11

### **4. Check Current Scaling Status**

**What this does:** Shows current DPI settings  
```bash
xdpyinfo | grep -B2 resolution
```
**Why:** Default is usually 96 DPI; we'll change to 120 for 125% scaling

**What this does:** Shows GNOME scaling settings (if using GNOME)  
```bash
gsettings get org.gnome.desktop.interface text-scaling-factor
```
**Why:** Should be 1.0 by default; we'll change to 1.25

---

## **Pre-Configuration Checklist**

### **Backup Current Settings**

**What this does:** Creates backup of current configurations  
```bash
mkdir -p ~/scaling-backup
```
**Why:** Safety first - can restore if something goes wrong

**What this does:** Backs up existing X resources  
```bash
[[ -f ~/.Xresources ]] && cp ~/.Xresources ~/scaling-backup/
```
**Why:** Preserves any existing custom settings

**What this does:** Backs up shell configurations  
```bash
cp ~/.bashrc ~/scaling-backup/bashrc.backup
cp ~/.zshrc ~/scaling-backup/zshrc.backup 2>/dev/null || true
```
**Why:** We'll be modifying these files

---

## **Step-by-Step Scaling Setup**

### **For GNOME on X11 (Most Common Kali Setup)**

#### **Step 1: Set Base DPI**

**What this does:** Creates X resources file with 120 DPI (125% of 96)  
```bash
echo "Xft.dpi: 120" > ~/.Xresources
```
**Why:** Tells X11 applications to render at 125% scale

**What this does:** Applies the DPI setting immediately  
```bash
xrdb -merge ~/.Xresources
```
**Why:** Without this, changes only apply after restart

#### **Step 2: Set GNOME Text Scaling**

**What this does:** Sets GNOME text to 125% size  
```bash
gsettings set org.gnome.desktop.interface text-scaling-factor 1.25
```
**Why:** Ensures GTK applications scale consistently with X11 apps

**What this does:** Sets appropriate cursor size for scaled display  
```bash
gsettings set org.gnome.desktop.interface cursor-size 30
```
**Why:** Default 24px cursor looks tiny at 125% scale

#### **Step 3: Configure Environment Variables**

**For Bash users:**

**What this does:** Opens bash configuration file  
```bash
nano ~/.bashrc
```
**Why:** Need to add scaling variables for Qt/GTK applications

Add these lines at the end:
```bash
# HiDPI Support (1.25x scaling)
export GDK_SCALE=1
export GDK_DPI_SCALE=1.25
export QT_AUTO_SCREEN_SCALE_FACTOR=0
export QT_SCALE_FACTOR=1.25
export QT_FONT_DPI=120
```

**For Zsh users (if you have it):**

**What this does:** Opens zsh configuration file  
```bash
nano ~/.zshrc
```
**Why:** Zsh doesn't read .bashrc, needs its own configuration

Add the same lines as above.

#### **Step 4: Auto-load Settings on Login**

**What this does:** Creates X profile to load settings automatically  
```bash
nano ~/.xprofile
```
**Why:** Ensures DPI settings apply every time you log in

Add these lines:
```bash
# Load DPI settings
[[ -f ~/.Xresources ]] && xrdb -merge ~/.Xresources

# Load Xmodmap if it exists
[[ -f ~/.Xmodmap ]] && xmodmap ~/.Xmodmap
```

#### **Step 5: Optimize Font Rendering**

**What this does:** Enables subpixel font rendering  
```bash
gsettings set org.gnome.desktop.interface font-antialiasing 'rgba'
```
**Why:** Makes text sharper on LCD displays

**What this does:** Sets slight font hinting  
```bash
gsettings set org.gnome.desktop.interface font-hinting 'slight'
```
**Why:** Improves readability at fractional scaling

---

### **For XFCE**

If using XFCE instead of GNOME:

**What this does:** Opens XFCE appearance settings  
```bash
xfce4-appearance-settings
```
**Why:** XFCE has different scaling controls

1. Go to "Fonts" tab
2. Set "Custom DPI" to 120
3. Click Apply

**What this does:** Sets XFCE window scaling  
```bash
xfconf-query -c xsettings -p /Gdk/WindowScalingFactor -s 1
```
**Why:** Prevents double-scaling issues

---

### **For KDE Plasma**

If using KDE:

**What this does:** Opens KDE system settings  
```bash
systemsettings5
```
**Why:** KDE has built-in scaling options

1. Go to "Display and Monitor" → "Display Configuration"
2. Set "Global Scale" to 125%
3. Click Apply

---

### **For GNOME on Wayland**

If using Wayland (better scaling support):

**What this does:** Enables fractional scaling in GNOME  
```bash
gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"
```
**Why:** Allows 125%, 150%, 175% options in display settings

**What this does:** Opens display settings  
```bash
gnome-control-center display
```
**Why:** Can now select 125% from dropdown

---

## **Post-Configuration Verification**

### **Required: Log Out and Log In**

**IMPORTANT:** Changes only fully apply after logout/login

### **Verify Settings Applied**

**What this does:** Checks if DPI was applied  
```bash
xrdb -query | grep dpi
```
**Expected output:** `Xft.dpi: 120`

**What this does:** Verifies environment variables  
```bash
env | grep -E "(GDK|QT).*SCALE"
```
**Expected output:** Should show all scaling variables

**What this does:** Tests with a GTK application  
```bash
gtk3-demo
```
**Why:** Should appear 25% larger than before

---

## **Troubleshooting**

### **If Text is Too Large/Small**

For 150% scaling instead:
```bash
echo "Xft.dpi: 144" > ~/.Xresources
xrdb -merge ~/.Xresources
gsettings set org.gnome.desktop.interface text-scaling-factor 1.5
```

For 100% scaling (default):
```bash
echo "Xft.dpi: 96" > ~/.Xresources
xrdb -merge ~/.Xresources
gsettings set org.gnome.desktop.interface text-scaling-factor 1.0
```

### **If Some Apps Don't Scale**

**For Firefox:**
1. Type `about:config` in address bar
2. Search for `layout.css.devPixelsPerPx`
3. Set to `1.25`

**For Chrome/Chromium:**
```bash
google-chrome --force-device-scale-factor=1.25
```

### **If Blurry Text**

**What this does:** Forces full hinting  
```bash
gsettings set org.gnome.desktop.interface font-hinting 'full'
```
**Why:** Some displays need stronger hinting

---

## **Rollback Instructions**

If something goes wrong:

**What this does:** Restores original files  
```bash
cp ~/scaling-backup/bashrc.backup ~/.bashrc
cp ~/scaling-backup/zshrc.backup ~/.zshrc 2>/dev/null || true
rm ~/.Xresources ~/.xprofile
```

**What this does:** Resets GNOME settings  
```bash
gsettings reset org.gnome.desktop.interface text-scaling-factor
gsettings reset org.gnome.desktop.interface cursor-size
gsettings reset org.gnome.desktop.interface font-antialiasing
gsettings reset org.gnome.desktop.interface font-hinting
```

Log out and log in to complete rollback.

---

## **Quick Reference Card**

### **For Fresh Kali Install with GNOME:**

1. Check your setup: `echo $XDG_SESSION_TYPE` and `echo $XDG_CURRENT_DESKTOP`
2. If X11 + GNOME, run these commands:

```bash
# Set DPI
echo "Xft.dpi: 120" > ~/.Xresources
xrdb -merge ~/.Xresources

# Set GNOME scaling
gsettings set org.gnome.desktop.interface text-scaling-factor 1.25

# Add to .bashrc
cat >> ~/.bashrc << 'EOF'
# HiDPI Support
export GDK_SCALE=1
export GDK_DPI_SCALE=1.25
export QT_AUTO_SCREEN_SCALE_FACTOR=0
export QT_SCALE_FACTOR=1.25
export QT_FONT_DPI=120
EOF   
# Create .xprofile
echo '[[ -f ~/.Xresources ]] && xrdb -merge ~/.Xresources' > ~/.xprofile
```

3. Log out and log in

### **Scaling Formula Reference**

- **100% (default)**: 96 DPI, text-scaling-factor 1.0
- **125%**: 120 DPI, text-scaling-factor 1.25
- **150%**: 144 DPI, text-scaling-factor 1.5
- **175%**: 168 DPI, text-scaling-factor 1.75
- **200%**: 192 DPI, text-scaling-factor 2.0

### **Compatible Applications**

This DPI scaling method works with:
- ✅ GTK3/GTK4 applications (most GNOME apps)
- ✅ Qt5/Qt6 applications (with environment variables)
- ✅ X11 applications (via Xft.dpi)
- ✅ Electron apps (VS Code, Discord, etc.)
- ✅ Java applications (with proper flags)
- ⚠️ Some older GTK2 apps may need individual configuration
- ⚠️ Wine applications may need separate scaling

---

**Applies to:** Kali Linux 2024.x with GNOME desktop environment

---
---