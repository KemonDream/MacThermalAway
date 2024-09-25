# MacThermalAway
This repo will share multiple ways to issue the Thermal Problem on newer MacOS.(help you explode your macbook and trashing thermal protection). Whether a bad battery, vrm heating or bd prochot,etc. Os version supports BigSur to Sequoia, from 11 to 15. IF you know the principles you could apply to any version of Os.
- This repo tested on Macbook pro 16,1 , Intel-i5-1038ng7 ,iGpu, MacOS Sequoia Beta2, File-Volt on. Battery in Service State, bd_prochot  and CPU throttle a lot to 0.2Ghz that drove me crazy.(Modern Computer running at 200Mhz can be something dramatic.)
# Pre
- you need to dignose u computer first for any potaintial issue causing heat and throttle, incase of damaging your hardware before appling this repo.
  eg. Take off the back pack and add some thermal pattles on vrm chips and other heating compounents.
- Xcode
- A basic patience.
- Stable Global Netwrok Connection
# Concepts
 ## Where we start? IF we can tell how the CPU can be controled we can also know what to do. ##
- For CPU, there is a huge and complex monitor and control system, but we can simplify them into three parts: Thermal, MSRs and OSs.
  - Thermal prevent CPU or iGPU from burning themselese from the inside, while MSR codes like BD_Prochot and others keep other components from firing the cores. The OS also do a lot things.
  - Since the MSR or Micro Specific Registers may be read and write by any software and may cause safety issues. OSes like MacOS may take over the function by using custom designed CPU and MSRs from intel (like my Macbook CPU i5-1038ng7) or just make one by themselves(((.
  - A Complex system may or may not be better for your personal usage, it may be too strict or too loose that can cause performance lag or heat.
- **In this case, we can focus on the keys that affect the CPU management, that are OS ,Bootloader and CPU itself.**
  - If using MSR tools like Voltageshift and get no effects, it's the OS taken controled. 
  - As I know, The MacOS using kernal kexts like IOPlatformPluginFamily and X86PlatformPlugin to control Intel CPU functions . Turbo, Power, Sleep and so on.
  - The sensors on the motherboard will not communicate directly to the CPU. Probably. So the OS takes the main part in this whole thing.
  - **UPDATE**: I ignored how strong the bootloader affect this whole thing. On MacOS, it's the BridgeOS.
  - 
# Install Plan A (Currently tesing, working fine, Recommand)
- **Use OCPL to create OpenCore bootloader and load your MacOS by pressing OPTION on startup.**
    - Step 1: Download the latest OCPL at https://github.com/dortania/Opencore-Legacy-Patcher/releases
    - Step 1.1: Revert root patch in "Post-install root patch" tab if you did some root modification or something.
    - Step 2: Click Settings button and enable 1>APP>"Allow native models" & 2>Advenced>"Disable Firmwre Throttling"
    - Step 2.5: Leave others default or choose at your own appetite and click return.
    - Step 3: You can now click "Build and Install OpenCroe" to follow  their instructions. This action should copy OpenCore to your Bootdisk EFI patition. No System files would be hramed.
    - Step 4: Reboot your System and SO fxxxking fast.
      Tip: don't forget to hold OPTION button when booting
      
# Install Plan B
- Step 1 : Check prerequisites again. (Well, though my macbook is a tough guy, i don't know about you. Since this repo is now in a long test progress, i strongly not recommend you applying it on your working computer.)
- Step 1.1 : You may check what causing CPU throttle by using ThrottleStop by TechPowerUp on Windows Bootcamp. In my case, the BD_PROCHOT title in "Limits" will turn red when doing p95 test for a while on Windows. By running p95 you could also know your mac's thermal limits. When it gets too hot, the cpu will shutdown to prevent damaged.
- Step 2 : Install Developer Tools
  - Xcode either in AppStore or at https://developer.apple.com for a suitable version.
  - Xcode CommandLineTools simply by runing "xcode-select --install".
  - Apple KDKs at https://github.com/dortania/KdkSupportPkg/releases You should install which matches it the best with your MacOS.
- Step 3 : Download this repo(Comming). Or find list below for needed repo.
  - Will include scripts marked its roles. Run carefully cause there're highly dangerous code that modified the root system. You should check each line for safe.
  - VoltageShift at https://github.com/sicreative/VoltageShift build or using provided unix files.
- Step 4 : Turn off SIP and authenticated_root completely.(We doing root modifications and third party kext injection.)
  - Boot into recovery...
- Step 5 :
- Step N-1: "TurboMac" Install-Script. And we only need a few line of it.
  ## mount System Current Booting Snapshot to a folder, and after that we create a new one. ##
  - ROOT_LABEL=`df | grep /$ | awk '{print $1;}' | grep -o ".*disk\d*s\d*"`
echo "Root label: $ROOT_LABEL" ##Means determinating root disk label
  - sudo mkdir $HOME/nonroot
  - sudo mount -t apfs -o nobrowse $ROOT_LABEL $HOME/nonroot
  - sudo mkdir $HOME/BackupKexts ##for backup# although there are snapshot# just incase
  - sudo mv -f $HOME/nonroot/System/Library/Extensions/IOPlatformPluginFamily.kext/Contents/PlugIns $HOME/BackupKexts/
  - sudo kmutil install -u -v --update-preboot -R $HOME/nonroot ##will update preboot and kext cache
  - sudo bless --folder $HOME/nonroot/System/Library/CoreServices --bootefi --create-snapshot ##snapshot the root system and boot from it
- Step N : Install kext
  The kext that I'am currently using: GoodbyeBigSlow NoBdProcHot TurboMac
  - we can run "kextstat | grep -v com.apple" to determine whitch kext is really loaded.
  - In my case, I have installed those three kext but NoBdProcHot was not loaded. And I have to run a automatic code on Automator to manually diable BD_PROCHOT.
    -The code for Automator will be provided.
 - Step X : Test your System stability by running Cinebench or p95 under MacOS. Test known issues for good.
# Known Issues
- Not stable for now
- Plan A is under testing.
- Only on Plan B
  - Login progress bar may be a much slower loading than usual.
  - Login items may be slow due to throttle (bd_prochot, kext conflicts).
  - Bluetooth would get stuck at boot and a while before it can be used for good.(Thanks to a bilibili fan mentioned this issue)
  - Crashing and overheat may happend more often.(May because the beta OS I'm using.)
  - Wake from Sleep or plug in buggy usb devices may cause serious throlling(not BD_Prochot but some regular unknown laggy). The reason is vague. May because of root and kext modifying.
  -  Uploading things from Safari may freeze OS
# Resources and Description 
- https://github.com/dortania/Opencore-Legacy-Patcher/releases
  - OpenCore Legacy Patcher
- https://github.com/cocafe/msr-utility/issues/8#issuecomment-1853242674
  - This resource discuss what a msr means to cpu
- https://github.com/sicreative/VoltageShift
  - This is the best tool to write into the msr of the cpu when macos already booted
- https://github.com/calasanmarko/TurboMac
  - This resource shows runable commands that can modify the root of the newer system and what files need to be dealt with
- https://github.com/balfabb87/GoodbyeBigSlow.kext
  - This resource is what we need to manage cpu thermal after deleting the files
- https://github.com/syscl/CPUTune/issues/21
  - This resource discuss a little **why** modifying msr that controls the **bd_prochot does not work** and what to delete in system files.
- https://github.com/benbaker76/Hackintool
  - Calculator that can convert binary to hex. For thoese have a bad math or just lazy like me.
- https://github.com/dortania/KdkSupportPkg/releases
  - When running TurboMac things you will need this.
- https://dortania.github.io/OpenCore-Legacy-Patcher/TROUBLESHOOTING.html#stuck-on-boot-after-root-patching
  - This will help when you mess up your system and get back up again.
- /335592-sdm-vol-4.pdf
  - This pdf is a official description of MSRs on each generation of intel chips. Not very complete but useful enough for turbo adjustments.
