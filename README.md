# MI50-gfx906-Windows
Guide on running Radeon Instinct MI50 32GB as a normal GPU in Windows

## Things to know before you install
# This only works on the 32gb version of the Radeon Instinct MI50, and doing this on the 16gb version will result in bricking your GPU. There are already a lot of guides on flashing the 16gb version so you should use that instead
# This only works on the 32gb version of the Radeon Instinct MI50, and doing this on the 16gb version will result in bricking your GPU. There are already a lot of guides on flashing the 16gb version so you should use that instead
# This only works on the 32gb version of the Radeon Instinct MI50, and doing this on the 16gb version will result in bricking your GPU. There are already a lot of guides on flashing the 16gb version so you should use that instead
- As far as I'm concerned, there are two 'official' and one unofficial version of the 32gb version of the MI50. The official version has P/N that either ends with '1413'/'1711' and have multiple stickers on the corner of the backplate, while the unofficial ones have one sticker on the backplate and have less cores. I don't know if it works on the unofficial version since both of my cards were the 1413 variant.
- 1413 varient has a yellow dot on the small chip on the back of the GPU near the barcode and a red dot on the small chip near the PCIE slot, while both of the dots on the 1711 varient are green. The 16gb version(P/N: 102D1631410) has a yellow dot near the barcode, and a blue dot near the PCIE slot.
- There are multiple rom options that can be flashed, but searching on Chinese forums using a translator, they say that the v420 rom should be the best since it has support for UEFI, ReBAR, display output in Windows, and has support for PCIE 4. While the other options such as the Apple rom lacks some features and is less stable, so there is no reason to run the other version of the bios unless it's specifically needed.
- If you don't need video output and don't know how to flash the bios, you can use rdn-id driver(https://rdn-id.com/) on Windows. You can run it without flashing the bios, but display output won't be available so if you don't have another GPU, you won't be able to boot unless you have a server motherboard.
- If your card is beeping, it either means that the power plug is not connected or the gpu temp is too high(there could be more reasons than these two). On Windows before flashing the Bios, my card would overheat while doing nothing and would beep like crazy after around 30 minutes until i shut the whole pc down. I don't know if this issue happenes only on my card or on all cards.


## Host Environment
- Linux install
- Windows install
- ASPM options related to PCI should be off in Bios. Some options may work, so you'll need to do some trial and error
- Above 4g Decoding should be on
- CSM related options should be off
- It's recommended to have ReBAR on, but on my Z690 Hero having 2x MI50s plus 1 Arc B580 gave me d4 pci resource allocation error(2x MI50 / 2x MI50 + 1 GTX1070 posted without error), so I had to turn if off
- Bios file for the AMD Radeon Pro V420
- AMDVBFlash software
- Radeon Driver for the AMD Radeon VII


## Download links for the necessary files
V420 Bios: https://drive.google.com/file/d/1AQlbrnsafzrFXaliV5narZpsJqvfZkft/view?usp=sharing
 - The size of the Bios should be 1.0MB, unlike the Bios from Techpowerup, which is less than half in size.

AMDVBFlash: https://www.techpowerup.com/download/ati-atiflash/
 - The version I used is v4.71

Radeon Driver: https://www.amd.com/en/support/downloads/previous-drivers.html/graphics/radeon-rx/radeon-rx-vega-series/amd-radeon-vii.html
 - Latest version that works as far as I know is v24.9.1, released in 2024-10-01. Newest version doesn't include the driver for the Radeon Pro VII, which is the one we need.


## Getting ready
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


## Flashing the Bios
- Download the V420 Bios, copy the file to the AMDVBFlash folder, and change the name to something simple, such as v420.bin
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
# Flashing the Bios can possibly brick your GPU, so do it at your own risk!! Check the command before executing it
```
sudo ./amdvbflash -f -p #GPU_ID #FILE_NAME
```
- Example
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

- lspci
```
lspci -vvv
```

- Output Example
```
VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] Vega 20 [Radeon Pro/Radeon Instinct] (prog-if 00 [VGA controller])
	Subsystem: Advanced Micro Devices, Inc. [AMD/ATI] Vega 20 [Radeon Pro/Radeon Instinct]
	Control: I/O+ Mem+ BusMaster+ SpecCycle- MemWINV- VGASnoop- ParErr- Stepping- SERR- FastB2B- DisINTx+
	Status: Cap+ 66MHz- UDF- FastB2B- ParErr- DEVSEL=fast >TAbort- <TAbort- <MAbort- >SERR- <PERR- INTx-
	Latency: 0, Cache Line Size: 64 bytes
	Interrupt: pin A routed to IRQ 222
	IOMMU group: 24
	Region 0: Memory at 4800000000 (64-bit, prefetchable) [size=32G]
	Region 2: Memory at 4400000000 (64-bit, prefetchable) [size=2M]
	Region 4: I/O ports at 5000 [size=256]
	Region 5: Memory at 75300000 (32-bit, non-prefetchable) [size=512K]
	Expansion ROM at 75380000 [disabled] [size=128K]
	Capabilities: [48] Vendor Specific Information: Len=08 <?>
	Capabilities: [50] Power Management version 3
		Flags: PMEClk- DSI- D1- D2- AuxCurrent=0mA PME(D0-,D1+,D2+,D3hot+,D3cold+)
		Status: D0 NoSoftRst+ PME-Enable- DSel=0 DScale=0 PME-
	Capabilities: [64] Express (v2) Legacy Endpoint, MSI 00
		DevCap:	MaxPayload 256 bytes, PhantFunc 0, Latency L0s <4us, L1 unlimited
			ExtTag+ AttnBtn- AttnInd- PwrInd- RBE+ FLReset-
		DevCtl:	CorrErr- NonFatalErr- FatalErr- UnsupReq-
			RlxdOrd+ ExtTag+ PhantFunc- AuxPwr- NoSnoop+
			MaxPayload 256 bytes, MaxReadReq 512 bytes
		DevSta:	CorrErr+ NonFatalErr- FatalErr- UnsupReq+ AuxPwr- TransPend-
		LnkCap:	Port #0, Speed 16GT/s, Width x16, ASPM L0s L1, Exit Latency L0s <64ns, L1 <1us
			ClockPM- Surprise- LLActRep- BwNot- ASPMOptComp+
		LnkCtl:	ASPM L0s L1 Enabled; RCB 64 bytes, Disabled- CommClk+
			ExtSynch- ClockPM- AutWidDis- BWInt- AutBWInt-
		LnkSta:	Speed 16GT/s, Width x16
			TrErr- Train- SlotClk+ DLActive- BWMgmt- ABWMgmt-
		DevCap2: Completion Timeout: Range ABCD, TimeoutDis+ NROPrPrP- LTR+
			 10BitTagComp+ 10BitTagReq+ OBFF Not Supported, ExtFmt+ EETLPPrefix+, MaxEETLPPrefixes 1
			 EmergencyPowerReduction Not Supported, EmergencyPowerReductionInit-
			 FRS-
			 AtomicOpsCap: 32bit+ 64bit+ 128bitCAS-
		DevCtl2: Completion Timeout: 50us to 50ms, TimeoutDis- LTR+ 10BitTagReq- OBFF Disabled,
			 AtomicOpsCtl: ReqEn+
		LnkCap2: Supported Link Speeds: 2.5-16GT/s, Crosslink- Retimer+ 2Retimers+ DRS-
		LnkCtl2: Target Link Speed: 16GT/s, EnterCompliance- SpeedDis-
			 Transmit Margin: Normal Operating Range, EnterModifiedCompliance- ComplianceSOS-
			 Compliance Preset/De-emphasis: -6dB de-emphasis, 0dB preshoot
		LnkSta2: Current De-emphasis Level: -3.5dB, EqualizationComplete+ EqualizationPhase1+
			 EqualizationPhase2+ EqualizationPhase3+ LinkEqualizationRequest-
			 Retimer- 2Retimers- CrosslinkRes: unsupported
	Capabilities: [a0] MSI: Enable+ Count=1/1 Maskable- 64bit+
		Address: 00000000fee00f58  Data: 0000
	Capabilities: [100 v1] Vendor Specific Information: ID=0001 Rev=1 Len=010 <?>
	Capabilities: [150 v2] Advanced Error Reporting
		UESta:	DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-
		UEMsk:	DLP- SDES- TLP- FCP- CmpltTO- CmpltAbrt- UnxCmplt- RxOF- MalfTLP- ECRC- UnsupReq- ACSViol-
		UESvrt:	DLP+ SDES+ TLP- FCP+ CmpltTO- CmpltAbrt- UnxCmplt- RxOF+ MalfTLP+ ECRC- UnsupReq- ACSViol-
		CESta:	RxErr- BadTLP- BadDLLP- Rollover- Timeout- AdvNonFatalErr+
		CEMsk:	RxErr- BadTLP- BadDLLP- Rollover- Timeout- AdvNonFatalErr+
		AERCap:	First Error Pointer: 00, ECRCGenCap+ ECRCGenEn- ECRCChkCap+ ECRCChkEn-
			MultHdrRecCap- MultHdrRecEn- TLPPfxPres- HdrLogCap-
		HeaderLog: 00000000 00000000 00000000 00000000
	Capabilities: [200 v1] Physical Resizable BAR
		BAR 0: current size: 32GB, supported: 256MB 512MB 1GB 2GB 4GB 8GB 16GB 32GB
		BAR 2: current size: 2MB, supported: 2MB 4MB 8MB 16MB 32MB 64MB 128MB 256MB
	Capabilities: [270 v1] Secondary PCI Express
		LnkCtl3: LnkEquIntrruptEn- PerformEqu-
		LaneErrStat: 0
	Capabilities: [2a0 v1] Access Control Services
		ACSCap:	SrcValid- TransBlk- ReqRedir- CmpltRedir- UpstreamFwd- EgressCtrl- DirectTrans-
		ACSCtl:	SrcValid- TransBlk- ReqRedir- CmpltRedir- UpstreamFwd- EgressCtrl- DirectTrans-
	Capabilities: [2b0 v1] Address Translation Service (ATS)
		ATSCap:	Invalidate Queue Depth: 00
		ATSCtl:	Enable-, Smallest Translation Unit: 00
	Capabilities: [2c0 v1] Page Request Interface (PRI)
		PRICtl: Enable- Reset-
		PRISta: RF- UPRGI- Stopped+
		Page Request Capacity: 00000100, Page Request Allocation: 00000000
	Capabilities: [2d0 v1] Process Address Space ID (PASID)
		PASIDCap: Exec+ Priv+, Max PASID Width: 10
		PASIDCtl: Enable- Exec- Priv-
	Capabilities: [320 v1] Latency Tolerance Reporting
		Max snoop latency: 15728640ns
		Max no snoop latency: 15728640ns
	Capabilities: [400 v1] Data Link Feature <?>
	Capabilities: [410 v1] Physical Layer 16.0 GT/s <?>
	Capabilities: [440 v1] Lane Margining at the Receiver <?>
	Kernel driver in use: amdgpu
	Kernel modules: amdgpu
```


## ASPM option
- After turning on ASPM back on in the Bios, Linux might not be able to initialize the GPU. You should add ASPM related options in /etc/default/grub to initialize the GPU.
```
nano /etc/default/grub
```
- Add the line below to the 'GRUB_CMDLINE_LINUX_DEFAULT'
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash pci realloc pci=noaer pcie_aspm=off iommu=pt"
```


## Installing Radeon driver in Windows
- Reboot into Windows
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


## Additional Infos
- On Ubuntu, 1 flashed MI50 uses around 17~21W of power when idle(reported from rocm-smi). Meanwhile on Windows GPU-Z reports that 1 card uses 12~18W while idle for some reason. I don't know if the power consumption on idle is actually different or the calculation method is the one that's different.
- If you got a fan module where you have to replace the original shroud, it's not a bad idea to cover the beeper with 1 layer of electrical tape so that even when it beeps it's not too loud.
- To cool the GPU, you can install Fan Control on Windows and CoolerControl on Ubuntu to control the fan installed to the GPU with the temperature of the GPU core. One interesting thing is you can see a header for the 'AMD Radeon Pro VII', so you might be able to solder the pins to the GPU PCB and use the fan from there if you really want.

- You can set the power limit to your liking on Ubuntu by using this command.
```
rocm-smi --setpoweroverdrive NUMBER_IN_WATTS(ex. 125)
```
- After executing the command you can use 'rocm-smi' to find out if it's working.

- The power limit resets every time you reboot your computer, so you need to make a script to execute this command every time you log on.
```
sudo nano /etc/systemd/system/rocm-powerlimit.service
```
- Paste the command below
```
[Unit]
Description=Set ROCm GPU Power Limit
After=multi-user.target

[Service]
Type=oneshot
ExecStartPre=/bin/sleep 5
ExecStart=/opt/rocm/bin/rocm-smi --setpower 100
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```
- Save and exit
```
sudo systemctl daemon-reload
sudo systemctl enable rocm-power.service
```
- Reboot, execute 'rocm-smi' to see if the script works


## Issues
- I don't know if this happenes on every card, but having the pc on sleep without the fan on overheats my gpu after a couple of hours for some reason. So test if the gpu is overheating while the pc is on sleep before you leave it without turning the pc off.
