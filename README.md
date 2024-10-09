# RKNPU For RK3399Pro
This project mainly provides NPU drivers and related examples for Rockchip RK3399Pro.


## Supported Plarform
- RK3399Pro

* Note:
- For drivers and C API applicable to RK1808/RV1109/RV1126, refer to: https://github.com/rockchip-linux/rknpu  
- For drivers and C API applicable to RK3566/RK3568/RK3588/RV1103/RV1106, refer to: https://github.com/rockchip-linux/rknpu2  


## RKNN Toolkit

Before deploying using the RKNN API, the original model needs to be converted to an RKNN model using either the RKNN Toolkit or RKNN Toolkit2.
- For RK1808/RK1806/RV1109/RV1126/RK3399Pro use: https://github.com/rockchip-linux/rknn-toolkit  
- For RK3566/RK3568/RK3588/RV1103/RV1106 use：https://github.com/rockchip-linux/rknn-toolkit2  
    
Refer to the corresponding website for detailed instructions.


## NPU Driver Overview

### NPU Driver Directory Description

The NPU driver for RK3399Pro is encapsulated in the boot.img file of the NPU. To update the NPU driver on the RK3399Pro, simply replace the relevant boot.img and other files.

Different RK3399Pro development boards may use different methods (PCIE and USB 3.0) to communicate with the NPU, and the NPU firmware (such as boot.img) will vary accordingly.

The main contents of the driver directory are as follows:
```
drivers/
├── npu_firmware
│   ├── npu_fw
│   └── npu_pcie_fw
└── npu_transfer_proxy
    ├── android-arm64-v8a
    ├── android-armeabi-v7a
    ├── linux-aarch64
    └── linux-arm
```
- npu_firmware: Contains NPU firmware
  - npu_fw_pcie: NPU firmware for the PCIE interface, including boot.img, MiniLoaderAll.bin, trust.img, uboot.img, etc.
  - npu_fw: NPU firmware for the USB interface, including boot.img, MiniLoaderAll.bin, trust.img, uboot.img, etc.
- npu_transfer_proxy: Contains npu_transfer_proxy for different operating systems. The AI application relies on the npu_transfer_proxy service to communicate with the NPU.

*To confirm the NPU connection type (PCIE or USB), run the following command on the board:
```
# For USB connection
/ # npu_transfer_proxy devices
List of ntb devices attached
2010fcfcde48fafd    80f3eb90    USB_DEVICE

# For PCI connection
rk3399pro:/ $ npu_transfer_proxy devices
List of ntb devices attached
0123456789ABCDEF    cfbc0c55    PCIE
```

### Manual NPU Driver Update

Updating the NPU driver on RK3399Pro is done by updating the boot.img and other files related to the NPU. Here’s how to update:

- Updating PCIE interface NPU:
```
# If it's Android firmware, gain root access and mount the system as writable. Skip for Linux firmware.
adb shell root
adb shell remount

# Update boot.img and other files
adb push npu_firmware/npu_pcie_fw/* /vendor/etc/npu_fw/
adb shell reboot
```

- Updating USB interface NPU:
```
# If it's Android firmware, gain root access and mount the system as writable. Skip for Linux firmware.
adb shell root
adb shell remount

# Update boot.img and other files
adb push npu_firmware/npu_fw/* /vendor/etc/npu_fw/
adb shell reboot
```

***Note: The path to npu_fw may differ depending on the RK3399Pro firmware. Verify the folder location before updating boot.img and other files.***

### npu_transfer_proxy Usage
When the AI application interacts with the NPU for model initialization, inference, etc., it depends on the npu_transfer_proxy service for communication.

Before running the AI application, ensure the npu_transfer_proxy service is running. By default, RK3399Pro firmware will automatically start this service. You can verify it with:
```
/ # ps -ef |grep npu_transfer_proxy
  268 root      252m S    ./usr/bin/npu_transfer_proxy
```
If the process is running, the service is active. If it is not running, you can start it manually and run it in the background, or add it to the boot startup services.

To start npu_transfer_proxy manually and run it in the background:
```
./npu_transfer_proxy &
```

## RKNN C API
For detailed instructions on using the RKNN C API, refer to the user guide in the rknn-api/doc folder.

### Copied from original repo 
https://github.com/airockchip/RK3399Pro_npu/tree/main
url = https://github.com/airockchip/RK3399Pro_npu.git
