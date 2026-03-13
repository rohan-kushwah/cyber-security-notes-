# NVIDIA-GPU-Drivers-and-CUDA-Toolkit-on-Kali-Linux
## Install NVIDIA GPU Drivers and CUDA Toolkit on Kali Linux
***
```bash
systemctl start ssh
```

```bash
systemctl enable ssh
```

Installing NVIDIA drivers and the CUDA Toolkit enables GPU-accelerated tasks such as cracking passwords with Hashcat or video processing with FFmpeg.

### Download the CUDA keyring package
***
Visit the official CUDA download site for additional details:

https://developer.nvidia.com/cuda-downloads

Download the CUDA keyring .deb package:
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/debian12/x86_64/cuda-keyring_1.1-1_all.deb
```

### Install the keyring package
***
This allows apt to trust the NVIDIA repository.
```bash
apt install ./cuda-keyring_1.1-1_all.deb
```

### Comment the kali linux repo `/etc/apt/sources.list`
***
Ensure the new NVIDIA repository was added correctly. If needed, manually verify and comment the kali linux repo:

```bash
vim /etc/apt/sources.list
```

```bash
# See https://www.kali.org/docs/general-use/kali-linux-sources-list-repositories/
#deb https://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware

# Additional line for source packages
# deb-src http://http.kali.org/kali kali-rolling main contrib non-free non-free-firmware
```

> Note: If you have dedicated ( additional ) GPU then disable the integrated GPU . See steps in 1.6 then continue below.
### Update package index
***
```bash
apt update
```
### Install the NVIDIA GPU driver
***
This installs the latest driver compatible with your GPU.

```bash
apt install nvidia-driver
```

- IF any dependency error comes, the un-comment the lines source list and install the dependency needed. Then  again comment the lines in the source list and install nvdia-driver again.
### Reboot to load the driver
***
```bash
reboot
```

### Install the CUDA Toolkit
***
This installs version 12.9 of the CUDA development environment.
```bash
apt install cuda-toolkit-12-9
```

### Install kernel modules and driver interface (optional but recommended)
***
**Note:**  If this command install another  cuda driver ( `nvdia-open-dksm` )  then do not install it .

```bash
apt install cuda-drivers nvidia-kernel-dkms
```

### Reboot again
***
```bash
reboot
```

### Verify driver installation
***
Check if NVIDIA driver is loaded and GPU is recognized:
```bash
nvidia-smi
```
- Check the G card name there, if found means success.

List loaded kernel modules related to NVIDIA:
```bash
lsmod | grep -i nvidia
```
- check if nvdia kernal modules are loaded or not. If not load try reboot then again run this command.

If everything appears correct, reboot one more time (optional, for good measure):
```bash
reboot
```

---
### Install FFmpeg with NVENC support
***
This enables hardware-accelerated video encoding using NVIDIA GPUs.

#### Install FFmpeg
```bash
apt install ffmpeg
```
- ffmpeg is the encoder for media files.
#### Check for NVENC encoders
```bash
ffmpeg -encoders 2>/dev/null | grep nenc
```
- check that is nvdia is showing in the encoder or not.
#### Benchmark GPU with Hashcat
Check performance and CUDA support:
```bash
hashcat -b
```
- checking that hashcat is using it or not, it show the G card there. 

If all things are fine means graphic card is successfully configured.

----
### Install NVIDIA Video Codec SDK on Kali Linux
***
The NVIDIA Video Codec SDK allows developers to access NVENC (hardware-accelerated encoding) and NVDEC (decoding) on supported NVIDIA GPUs. It's also required for advanced FFmpeg and media-based applications.

#### Step 1: Download the SDK
Visit the official download page:
https://developer.nvidia.com/nvidia-video-codec-sdk/download

> ⚠️ Note: You need an NVIDIA Developer account to download the SDK.

#### Step 2: Extract the SDK archive
After downloading the `Video_Codec_Interface_13.0.19.zip` file:
```bash
unzip Video_Codec_Interface_13.0.19.zip
```
Move it to a standard location for easier management:
```bash
mv -v Video_Codec_Interface_13.0.19 /opt/Video_Codec_Interface_13.0.19
```

#### Step 3: Check CUDA include path
Make sure CUDA headers are available in the standard include path:
```bash
ls /usr/local/cuda/include
```

#### Step 4: Copy SDK headers into CUDA path
Navigate to the interface directory:
```bash
cd /opt/Video_Codec_Interface_13.0.19/Interface
```
Copy the interface headers to CUDA's include path:
```bash
cp -v * /usr/local/cuda/include/
```
> ✅ This makes the headers available for compilation with applications that use NVENC/NVDEC.

---
---

### NOTE:
### AFTER NVDIA DRIVER INSTALLATION, DOES NOT UPGRADE (apt upgrade), ELSE NVDIA DRIVER WILL BE REPLACED AND GOT MISCONFIGURED.

---
---