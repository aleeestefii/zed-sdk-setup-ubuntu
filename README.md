# zed-sdk-setup-ubuntu
ZED SDK Installation on Ubuntu (StereoLabs)


Overview

This repository documents the correct and reproducible installation process of the StereoLabs ZED SDK on Ubuntu Linux.

The ZED camera is a passive stereo vision system that estimates depth through binocular triangulation, enabling 3D perception, spatial mapping, and positional tracking.
All processing is performed on the host computer; the camera functions solely as a stereo sensor.

This guide is written to minimize installation errors commonly encountered in academic, laboratory, and student environments.


Hardware Reference

Ensure the following items are available:
	•	ZED Stereo Camera (ZED / ZED2 / ZED2i)
	•	USB 3.0 cable
	•	Mini tripod
	•	USB drive (drivers and documentation)

The SDK is installed on the computer, not on the camera.

System Requirements

Operating System
	•	Ubuntu 20.04 LTS or 22.04 LTS

GPU
	•	NVIDIA GPU required
	•	CUDA-capable GPU (compute is mandatory)

Verify GPU detection:

nvidia-smi

NVIDIA Drivers

Recommended driver version 535 or newer:

sudo ubuntu-drivers autoinstall
sudo reboot



Step 1 — System Preparation

Update system packages:

  sudo apt update && sudo apt upgrade -y

Install required utilities:

  sudo apt install -y build-essential cmake git wget



Step 2 — Download ZED SDK

Download the official ZED SDK for Ubuntu from StereoLabs:
	•	https://www.stereolabs.com/en-tw/developers
	•	https://support.stereolabs.com/hc/en-us/articles/207616785

Select the installer matching:
	•	Your Ubuntu version (20.04 / 22.04)
	•	Linux x86_64



Step 3 — Install the ZED SDK

Navigate to the download directory:

cd ~/Downloads

Make the installer executable:

  chmod +x ZED_SDK_Ubuntu*.run

Run the installer:

  sudo ./ZED_SDK_Ubuntu*.run

During installation:
	•	Accept the license agreement
	•	Allow CUDA installation if prompted
	•	Keep default installation paths unless required otherwise



Step 4 — USB Permissions

Grant camera access permissions:

  sudo usermod -aG video $USER
  sudo reboot



Step 5 — Verify Installation

Check SDK version:

zed_version

Launch the ZED Explorer:

ZED_Explorer

Expected behavior:
	•	Live stereo image
	•	Depth map visualization
	•	Camera recognized without errors



Common Issues and Fixes

Camera Not Detected
	•	Confirm USB 3.0 connection
	•	Avoid USB hubs
	•	Verify user is in the video group

CUDA Errors

Verify CUDA installation:

nvcc --version

If missing, reinstall the SDK and enable CUDA support.

Black Image or Application Crash
	•	Update NVIDIA drivers
	•	Reboot the system
	•	Close other GPU-intensive applications



Important Clarifications
	•	The ZED SDK does not run on the camera
	•	The camera does not store software
	•	All computation is performed on the host machine GPU
	•	USB connection is used exclusively for data transfer



Installation Directory Structure

After installation:

/usr/local/zed/
├── tools/
├── libraries/
├── samples/
├── settings/




Typical Use Cases
	•	Computer Vision
	•	Robotics and Autonomous Systems
	•	SLAM and Mapping
	•	Depth-Based Perception
	•	Academic and Research Projects



References
	•	StereoLabs Developer Documentation
	•	StereoLabs Support Center

⸻

License

This repository is provided for educational and research use.
All ZED SDK components are subject to StereoLabs licensing terms.
