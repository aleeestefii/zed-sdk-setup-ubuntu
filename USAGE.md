ZED 2 Python Usage Tutorial - Estefania Rico
This section provides a practical, end-to-end tutorial for using the StereoLabs ZED platform and a ZED 2 camerafrom Python, while also documenting a real, reproducible workstation environment (Conda + venv) and a known GPU-architecture limitation observed during validation.
Authoritative references:
ZED Python API installation guide (official): https://www.stereolabs.com/docs/development/python/install  
ZED Python API repository: https://github.com/stereolabs/zed-python-api  
Additional Python/OpenCV tutorial (official): https://www.stereolabs.com/docs/opencv/python  

1) System Prerequisites
1.1 Hardware
Camera: ZED 2
Connection: USB 3.x direct to the laptop (avoid hubs/adapters during setup)
1.2 Software Requirements (Python API)
StereoLabs’ Python API is a wrapper around the ZED SDK (C++), exposed via Cython.  
Minimum dependencies (as documented by StereoLabs):  
ZED SDK installed
Python 3.6+ (x64)
Cython
NumPy
(Optional) OpenCV Python
(Optional) PyOpenGL
Practical note: in recent ZED SDK generations, Python version and NumPy constraints can be sensitive; isolating the environment is strongly recommended.  

2) Validated Environment (This Repository)
All steps below were validated on the following platform:
Device
Laptop workstation (ASUS)
Ubuntu Linux (x86_64)
ZED 2 camera (USB 3.x direct)
GPU Stack (as reported by NVIDIA tooling)
CUDA Toolkit: 12.9
NVIDIA Driver: 575.64.03
NVIDIA-SMI: 575.64.03
This environment record is included intentionally because the ZED Python API depends on GPU compatibility and correct CUDA/driver alignment.  

3) Recommended Installation Strategy (Conda + Virtual Environment)
A two-layer isolation strategy was used:
Conda to manage the base Python distribution cleanly
A Python virtual environment (venv) to isolate project dependencies
This approach was selected because initial attempts resulted in Python package version conflicts typical of CUDA-linked stacks (NumPy/Cython compatibility). Community reports confirm that Python/NumPy/Cython compatibility can be a common pain point depending on SDK version and platform.  

4) Install the ZED Python API (Linux)
4.1 Install Python dependencies (inside your environment)
StereoLabs recommends installing the Python-side dependencies via pip.  
python -m pip install --upgrade pip
python -m pip install cython numpy opencv-python pyopengl
(OpenCV and PyOpenGL are optional but commonly used in tutorials.)  

4.2 Use the official installer script: 
get_python_api.py
StereoLabs provides a script inside the ZED SDK installation that detects your platform/CUDA/Python and downloads the correct precompiled pyzed wheel.  
Linux default location:
cd /usr/local/zed/
IMPORTANT (virtual environment users): activate your environment before running the script.  
Run:
python3 get_python_api.py
If you need to install the wheel manually into the currently active environment (typical when using venv/conda):
 python -m pip install --ignore-installed /usr/local/zed/pyzed-*.whl
This matches the install guidance emitted by the script itself in the official documentation example.  

5) Quick Verification
5.1 Verify import
python -c "import pyzed.sl as sl; print('pyzed import OK')"
If you see an import error, the most common root cause is running Python from a different environment than the one where get_python_api.py installed the wheel.  
5.2 Verify camera availability (common pitfall: “camera busy”)
The ZED camera should be controlled by one process at a time. If ZED Explorer was opened earlier, fully close it before running Python.

6) Minimal “Hello ZED” Script
Create hello_zed.py:
import pyzed.sl as sl

def main():
    zed = sl.Camera()

    init_params = sl.InitParameters()
    init_params.sdk_verbose = 1  # enables detailed logs during startup

    err = zed.open(init_params)
    print("zed.open() ->", err)

    if err != sl.ERROR_CODE.SUCCESS:
        return

    serial = zed.get_camera_information().serial_number
    print(f"Hello! This is my serial number: {serial}")

    zed.close()

if __name__ == "__main__":
    main()
Run:
python hello_zed.py

7) Known Issue Observed During Validation (GPU Architecture Requirement)
Symptom
The first Python script (hello_zed.py) failed during initialization with a GPU-related message, even though the camera worked in ZED Explorer.
Cause (Documented Limitation)
The ZED SDK Python workflow requires an NVIDIA GPU meeting a minimum supported architecture (commonly referenced as SM70 or newer in practice). On the validation laptop, the installed GPU did not meet the requirement.

This section is included to prevent a common false assumption: “ZED Explorer works, therefore Python must work.” The Python bindings can fail earlier if GPU requirements are not satisfied.

8) Next Steps (Tutorials and Samples)
Once hello_zed.py works, proceed with the official Python tutorials:
OpenCV integration tutorial (capture/display):  
Additional official Python tutorials and samples:  

9) Troubleshooting Checklist (Most Common)
Environment mismatch: ensure you run Python from the same venv/conda env where pyzed was installed.  
Python version issues: some SDK releases may not support the newest Python versions.  
Dependency conflicts: NumPy and Cython constraints can vary by SDK version/platform.  
Camera busy: close ZED Explorer and any process using the camera before running Python.
