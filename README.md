# MI50-gfx906-Windows
Guide on running Radeon Instinct MI50 32GB as a normal GPU in Windows

## Things to know before you install
# This only works on the 32gb version of the Radeon Instinct MI50, and doing this on the 16gb version will probably result in bricking your GPU. There are already a lot of guides on flashing the 16gb version so use that instead
- As far as I'm concerned, there are two 'official' and one unofficial version of the 32gb version of the MI50. The official version has P/N that either ends with '1413'/'1711' and have multiple stickers on the corner of the backplate, while the unofficial ones have one sticker on the backplate and have less cores. I don't know if it works on the unofficial version since both of my cards were the 1413 variant.
- There are multiple rom options that can be flashed, but searching on Chinese forums using a translator, they say that the v420 rom should be the best since it has support for UEFI, ReBAR, display output in Windows, and has support for PCIE 4. While the other options such as the Apple rom lacks some features and is less stable, so there is no reason to run the other version of the bios unless it's specifically needed.


## Host Environment
- Linux install
- Windows install
- Bios file for the AMD Radeon Pro V420(Needed in Linux)
- AMDVBFlash(Needed in Linux)
- Radeon Driver for the AMD Radeon VII(Needed in Linux)
- ASPM options related to PCI should be off in Bios. Some options may work, so you'll need some trial and error
- Above 4g Decoding should be on

## Download links for the necessary files
V420 Bios: https://drive.google.com/file/d/1AQlbrnsafzrFXaliV5narZpsJqvfZkft/view?usp=sharing
 - The size of the Bios should be 1.0MB, unlike the Bios from Techpowerup, which is a lot smaller in size.

AMDVBFlash: https://www.techpowerup.com/download/ati-atiflash/
 - The version I used is v4.71

Radeon Driver: https://www.amd.com/en/support/downloads/previous-drivers.html/graphics/radeon-rx/radeon-rx-vega-series/amd-radeon-vii.html
 - Latest version that works as far as I know is v24.9.1, released in 2024-10-01. Newest version doesn't include the driver for the Radeon Pro VII, which is the one we need.

## Commands
- After installing Linux(Ubuntu in this case), download AMDVBFlash.
- Extract the file and cd into the extracted folder.
```
cd /EXTRACTED-FOLDER
```

- Edit the permission so that it can be executed.
```
sudo chmod +x ./amdvbflash
```

- List current installed AMD GPUs.
```
sudo ./amdvbflash -ai
```
- Output example
```
Adapter  0    SEG=0000, BN=07, DN=00, PCIID=66A11002, SSID=********)
    Asic Family        :  Vega20         
    Flash Type         :  GD25Q80C    (1024 KB)
    Product Name       :  Vega20 A1 SERVER XL D16317 Hynix/Samsung 32GB 8HI
    Bios Config File   :  D1631700.111   
    Bios P/N           :  113-D1631700-111
    Bios Version       :  016.004.000.056.013522
    Bios Date          :  01/16/20 21:38
    ROM Image Type     :  Legacy Images
    ROM Image Details  :  
        Image[0]: Size(41984 Bytes), Type(Legacy Image)
```

- Backup the original Bios
```
sudo ./amdvbflash -s 0 originalvbios.rom
```
- If you have multiple AMD GPUs, the number '0' should be replaced with the Adapter number of the MI50.
- Output
```
0x100000 bytes saved, checksum-=0x382B
```
- After executing the command, backup the original Bios.

- Unlock the ROM
```
sudo ./amdvbflash -unlockrom 0
```
- Output
```
ROM Unlocked
```

- Flashing the Bios
- Download the V420 Bios, copy the file to the AMDVBFlash folder, and change the name to something simple, such as v420.bin
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
```
sudo ./amdvbflash -f(Force flash) -p(Select GPU) #GPU_ID #FILE_NAME
```
```
sudo ./amdvbflash -f -p 0 v420.bin
```
- Output example
```
Old SSID: 0834
New SSID: 081E
Old P/N: 113-D1631700-111
New P/N: 113-D1640200-043
The result of RSA signature verify is PASS.
Old DeviceID: 66A1
New DeviceID: 66A0
Old Product Name: Vega20 A1 SERVER XL D16317 Hynix/Samsung 32GB 8HI 
New Product Name: Vega20 A1 GLXT WS D16402 32GB 8HI 1000m 
Old BIOS Version: 016.004.000.056.013522
New BIOS Version: 016.004.000.043.012098
Flash type: GD25Q80C
Burst size is 256 
100000/100000h bytes programmed
100000/100000h bytes verified

Restart System To Complete VBIOS Update.
```

- Check if the Bios flash worked(restart was not required)
```
sudo ./amdvbflash -ai
```
- Output example
```
Adapter  0    SEG=0000, BN=07, DN=00, PCIID=66A11002, SSID=********)
    Asic Family        :  Vega20         
    Flash Type         :  GD25Q80C    (1024 KB)
    Product Name       :  Vega20 A1 GLXT WS D16402 32GB 8HI 1000m 
    Bios Config File   :  D1640200.043   
    Bios P/N           :  113-D1640200-043
    Bios Version       :  016.004.000.0543.012098
    Bios Date          :  05/02/19 11:36
    ROM Image Type     :  Legacy Images
    ROM Image Details  :  
        Image[0]: Size(59392 Bytes), Type(Legacy Image)
        Image[1]: Size(44032 Bytes), Type(Hybrid Image)
```

## Installing the drivers in Windows
- Reboot into Windows
- Download Radeon Driver for the Radeon VII
- Launch the Radeon Driver file. It should Initialize install, but fail.
- Go to C:. There should be a new folder called 'AMD'.
- Launch Device Manager, you should see 'Microsoft Basic Display Adapter' under 'Display adapters'. If you don't see it, select 'View' -> 'Show Hidden Devices' and see if it's there. If you get Error 45, turning off ASPM should solve the problem. 
- In 'Events', it should say 'Device install requested'. Information: '-DEV_66A0- requires further installation'.
- Go to 'Details' -> 'Update Driver' -> 'Browse my computer for drivers' -> 'Let me pick from a list of available drivers on my computer' -> 'Have Disk...' -> 'Browse'
- Select 'C://AMD/AMD-Software-Installer/Packages/Drivers/Display/WT6A_INF/u0407052.inf' and press 'OK'
- Scroll down, and select 'AMD Radeon Pro VII'. If it's not on the list, make sure you installed the v24.9.1 instead of the newest version.
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

- To cool the GPU, you can install Fan Control, and control the fan installed to the GPU with the temperature of the GPU core. One interesting thing is you can see a header for the 'AMD Radeon Pro VII', so you might be able to solder the pins to the GPU PCB and use the fan from there. You can just use the motherboard header and control using Fan Control on Windows and CoolerControl on Ubuntu, so there's really no reason to do it unless you really hate seeing cables in your pc.
