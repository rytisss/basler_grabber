# basler_grabber

A simple live-view and capture tool for a stereo pair of Basler GigE cameras using `pypylon` and OpenCV. Shows the left and right camera feeds side by side and saves synchronized PNG snapshots on key press.

## Requirements

- Windows / Linux PC with a Gigabit Ethernet port
- Python 3.8+
- 2x Basler GigE cameras (default IPs: `192.168.1.101`, `192.168.1.102`)
- Basler Pylon SDK (Python bindings depend on the native runtime)

## Setup

### 1. Install the Pylon SDK

Download and install Pylon from Basler:

https://www.baslerweb.com/en/downloads/software/2012599532/?downloadCategory.values.label.data=pylon

During installation, choose **Developer** so the native runtime needed by `pypylon` is included.

### 2. Configure the network adapter

The cameras are configured on the `192.168.1.0/24` subnet. Set the PC's NIC that the cameras are plugged into to a static IP:

- **IP address:** `192.168.1.100`
- **Subnet mask:** `255.255.0.0`
- **Gateway:** leave empty

Camera IPs (default):
- Left camera: `192.168.1.101`
- Right camera: `192.168.1.102`

You can verify the cameras are reachable using the **Pylon IP Configurator** that ships with the SDK, or by pinging the camera IPs.

### 3. Install Python dependencies

It is recommended to use a virtual environment:

```bash
python -m venv .venv
.venv\Scripts\activate          # Windows (PowerShell: .venv\Scripts\Activate.ps1)
# source .venv/bin/activate      # Linux / macOS

pip install -r requirements.txt
```

## Running

```bash
python camera_grabber.py
```

Controls:
- **`c`** – capture a synchronized frame pair (saved under `data/left/` and `data/right/`)
- **`ESC`** – exit

Captured images are named `left_<timestamp>_<index>.png` / `right_<timestamp>_<index>.png`.

## Troubleshooting

- **`Found only 0 camera(s)`** – the cameras are not reachable. Check cabling, that the NIC is set to `192.168.1.100`, and that both cameras show up in the Pylon IP Configurator.
- **Timeout / dropped frames** – ensure the NIC supports jumbo frames (set MTU to `9000`) and is connected via a Gigabit-capable cable/switch.
- **`pypylon` import error** – the Pylon SDK runtime is missing or the wrong architecture. Reinstall the SDK matching your Python interpreter's bitness (typically 64-bit).
