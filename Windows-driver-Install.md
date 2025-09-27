# Windows driver Install
Guide on downloading drivers for the MI50 flashed with v420 rom in Windows


## Host environment
- Windows Install
- Flashed MI50 with v420 Bios(Didn't test with other Bios versions)
- Radeon Driver for the AMD Radeon VII


## Download links for the necessary files

Radeon Driver: https://www.amd.com/en/support/downloads/previous-drivers.html/graphics/radeon-rx/radeon-rx-vega-series/amd-radeon-vii.html
 - Latest version that works as far as I know is v24.9.1, released in 2024-10-01. Newest version doesn't include the driver for the Radeon Pro VII, which is the one we need.


## Installing Radeon driver in Windows
- After flashing the Bios, reboot into Windows
- Download Radeon Driver for the Radeon VII
- Launch the Radeon Driver file. It should Initialize install, but fail.
- Go to C:. There should be a new folder called 'AMD'.
- Launch Device Manager, you should see 'Microsoft Basic Display Adapter' under 'Display adapters'. If you don't see it, select 'View' -> 'Show Hidden Devices' and see if it's there. If you get Error 45, turning off ASPM should solve the problem. 
- In 'Events', it should say 'Device install requested'. Information: '-DEV_66A0- requires further installation'.
- Go to 'Details' -> 'Update Driver' -> 'Browse my computer for drivers' -> 'Let me pick from a list of available drivers on my computer' -> 'Have Disk...' -> 'Browse'
- Select 'C://AMD/AMD-Software-Installer/Packages/Drivers/Display/WT6A_INF/u0407052.inf' and press 'OK'
- Scroll down and select 'AMD Radeon Pro VII'. If it's not on the list, make sure you installed the v24.9.1 instead of the newest version.
- Press 'Next' -> 'Yes'. It should install the driver.
- Output
```
Windows has successfully updated your drivers
Windows has finished installing the drivers for this device:
AMD Radeon Pro VII
```
- Refresh Device Manager, 'Microsoft Basic Display Adapter' should be replaced with 'AMD Radeon Pro VII'.
- Launch Task Manager, you should be able to see 'AMD Radeon Pro VII' with 32GB Vram.
- If you download and launch GPU-Z, you can see other details about the GPU and view the sensors.

- Now, you'll be able to use the Mi50 just like a normal gpu and have display output.
