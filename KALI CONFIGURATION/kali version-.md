## 💿 Choosing Your Kali Linux Version
***
Kali Linux offers a wide variety of installation images, each tailored for a specific use case. Choosing the right one depends on your hardware, your goal, and your level of expertise.

---
### 📀 Installer Images
***
This is the traditional method for a full bare-metal installation of Kali Linux onto a computer's hard drive. This gives you a permanent, persistent operating system.

*   **Installer:** This is the standard, recommended download for most users. It contains a complete image that can be installed without needing an internet connection during the process. It includes the default set of Kali tools.
*   **Weekly:** This is a "bleeding-edge" version of the installer image, updated weekly with the latest packages and fixes. It's great for users who need the newest software but may be slightly less stable than the main release.
*   **NetInstaller:** A very small image that contains only the components needed to start the installation. It downloads all the required packages from the internet during the setup process. This ensures you get the most up-to-date version of everything but requires a stable internet connection.
*   **Everything:** A massive, comprehensive image that includes every single tool available in the Kali repositories. This is designed for completely offline installations where you need access to the full suite of tools without an internet connection.

---
### 🖥️ Virtual Machines
***
These are pre-built, pre-configured images designed to run inside virtualization software. This is often the easiest and safest way to get started with Kali, as it runs in an isolated environment on top of your existing operating system.

*   **VMware:** Pre-configured virtual machine files (.vmx) optimized for VMware Workstation, VMware Player, and VMware Fusion. These images come with VMware Tools pre-installed for better performance and seamless integration.
*   **VirtualBox:** Ready-to-use OVA (Open Virtual Appliance) files specifically configured for Oracle VirtualBox. These include Guest Additions for enhanced graphics, shared folders, and clipboard functionality.
*   **Hyper-V:** Native images for Microsoft's Hyper-V virtualization platform, available on Windows Pro and Enterprise editions. These are optimized for Windows integration and enhanced session mode.
*   **QEMU:** Raw disk images suitable for QEMU/KVM virtualization on Linux hosts. These provide excellent performance for Linux-based virtualization environments.

### 🍓 ARM
***
These images are specifically compiled for ARM-based single-board computers and devices. Each image is tailored for specific hardware to ensure optimal performance and compatibility.

*   **Raspberry Pi:** Multiple versions supporting Pi 1, Pi 2/3/4, Pi Zero/Zero W, and Pi 400. These include both 32-bit and 64-bit versions where applicable, with full hardware acceleration support.
*   **Pinebook/Pinebook Pro:** Optimized images for Pine64's laptop offerings, including proper power management and hardware driver support.
*   **BeagleBone Black:** Customized for this popular development board with full GPIO and hardware interface support.
*   **ODROID:** Various images supporting ODROID-C2, XU4, and other models in the ODROID family.
*   **Generic ARM:** A more universal image that can work on various ARM devices but may require manual configuration.

### 📱 Mobile
***
This refers to **Kali NetHunter**, a version of Kali designed for mobile penetration testing on Android devices. Different versions cater to various installation methods and device capabilities.

*   **NetHunter Rootless:** Can be installed on any Android device without root access. Provides limited functionality but works on unmodified phones.
*   **NetHunter Lite:** For rooted devices that don't support custom kernels. Offers more features than Rootless but less than the full version.
*   **NetHunter Full:** The complete NetHunter experience requiring a custom kernel. Includes wireless injection, HID attacks, and BadUSB capabilities.
*   **NetHunter Pro:** Premium version for specific devices like OnePlus, Nexus, and Pixel phones with dedicated kernel patches.

### ☁️ Cloud
***
These are pre-built images optimized for major cloud service providers. They're configured to work seamlessly with cloud-specific features and billing models.

*   **AWS AMI:** Amazon Machine Images available in the AWS Marketplace. Pre-configured with cloud-init for automatic setup and AWS CLI tools.
*   **Azure:** Available through Azure Marketplace with Azure Linux Agent pre-installed for proper integration with Azure services.
*   **Google Cloud:** GCP-optimized images with Google Cloud SDK and proper network configuration for Google's infrastructure.
*   **Digital Ocean:** Available as a one-click droplet with optimized kernels and automatic security updates enabled.
*   **Linode:** Stackscripts and images configured for Linode's infrastructure with proper networking and storage drivers.

### 📦 Containers
***
Lightweight Kali Linux images designed for containerization. These provide isolated environments for specific tools without the overhead of a full operating system.

*   **Docker Official:** The official Kali Docker image maintained by Offensive Security. Minimal base system with apt repositories configured.
*   **Docker Full:** A larger Docker image that includes the default metapackage of tools, ready for immediate use.
*   **Docker Core:** An ultra-minimal image containing only essential components for building custom containers.
*   **LXC/LXD:** Linux container images optimized for LXC/LXD with proper permissions and device access configurations.
*   **Podman:** OCI-compliant images that work with Podman for rootless container deployments.

### ⚡ Live Boot
***
These images allow you to run Kali Linux directly from removable media without installation. Perfect for forensics, temporary use, or testing on different hardware.

*   **Live Standard:** The default live image with the XFCE desktop environment and standard tool selection. Supports both BIOS and UEFI boot.
*   **Live Everything:** A comprehensive live image containing all available Kali tools. Requires larger USB drives but provides offline access to the entire toolkit.
*   **Live Encrypted Persistence:** Configured to support encrypted persistent storage on USB drives, allowing you to save files and settings between boots.
*   **Live Forensics Mode:** Special live image that doesn't auto-mount drives and includes forensic tools, designed to preserve evidence integrity.

### 🪟 WSL (Windows Subsystem for Linux)
***
Kali Linux integrated into Windows through Microsoft's Windows Subsystem for Linux. Different versions cater to various WSL implementations and use cases.

*   **WSL 2:** The recommended version using a real Linux kernel for better compatibility and performance. Available through the Microsoft Store.
*   **WSL 1:** Legacy version using translation layer. Still available for older Windows versions or specific compatibility needs.
*   **Win-KeX:** Kali's desktop experience for WSL, providing a full GUI environment within Windows using VNC or RDP.
*   **WSL Enterprise:** Custom WSL images for corporate environments with pre-configured security policies and tool restrictions.

### 📜 Build Scripts
***
For advanced users who want complete control over their Kali installation. These tools allow you to create custom images tailored to specific needs.

*   **live-build:** The official Debian live-build system customized for Kali. Create custom ISOs with your choice of desktop environments and tools.
*   **kali-arm-build-scripts:** Scripts for building custom ARM images for unsupported or new ARM devices.
*   **kali-docker:** Dockerfile and build scripts for creating custom Docker containers with specific tool combinations.
*   **kali-cloud-build:** Templates and scripts for building cloud-optimized images with custom configurations.
*   **kali-nethunter-build:** Build system for creating custom NetHunter images for Android devices.

---
---