# UefiSeven
## Summary
UefiSeven is an efi module that enables Windows 7 to boot under UEFI Class 3 systems.

Windows 7 does not fully support UEFI and relies on legacy BIOS interrupt 10 (Int10h) during initial graphics initialization.
On computers without proper legacy support Windows 7 might freeze on 'Starting Windows' screen or fail with error code 0xc000000d.
Some motherboards have 'CSM' or 'legacy' boot options to fix this problem but it is getting increasingly rare as Intel decided to get rid of legacy BIOS support by 2020, and the option is virtually non-existent on consumer mobile devices.
UefiSeven installs a minimal Int10h handler in the memory before Windows boots up so that the boot process do not fail because of the missing interrupt.

UefiSeven also contains a hack to enable screen output on hardwares that do not natively support 1024x768 as is needed by Windows Installer.
Int10h handler comes with a macro, when enabled before compiling, that will enable serial console output which can be used to debug the handler itself.

## Usage instructions
1. Prepare Windows 7 installation USB Drive
2. Rename bootx64.efi at (UsbDrive)\EFI\Boot\ to bootx64.original.efi
3. Unpack bootx64.efi from UefiSeven archive and copy it to (UsbDrive)\EFI\Boot\
4. Finish initial installation and wait for reboot prompt
5. Power off computer
6. Rename bootmgfw.efi at (HDD)\EFI\Microsoft\Boot\ to bootmgfw.original.efi
7. Copy UefiSeven bootx64.efi to (HDD)\EFI\Microsoft\Boot\bootmgfw.efi using EFI shell

## Settings
Settings can be applied by placing UefiSeven.ini file in the directory containing the main efi file.
Refer to the sample configuration file for available options.

## Build instructions
    git clone https://git.mananet.net/manatails/uefiseven
    (Copy or symlink UefiSevenPkg and IntelFrameworkPkg to the edk2 directory)
    source ./edksetup.sh
    make -C BaseTools/
    ./MdeModulePkg/Application/UefiSeven/Int10hHandler.sh ; Regenerate Int10h assembly. Optional
    build -a X64 -t GCC49 -b RELEASE -p UefiSevenPkg/UefiSevenPkg.dsc --conf=UefiSevenPkg/Conf
    
## Credits
* Original VgaShim project
* OVMF project
* EDK II project
