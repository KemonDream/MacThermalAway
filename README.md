# MacThermalAway
This repo will help you with exploding your macbook and trashing any thermal protection. Whether to stop your macbook from throttling with a bad battery,  vrm heating or bd prochot. Os version supports BigSur to Sequoia, from 11 to 15. IF you know the principles you could apply to any version of Oses and hardwares.
- This repo tested on Macbook pro 16,1, CPU Intell-i5-1038ng7 ,no dedicate Gpu, MacOS Sequoia Beta2, File-Volt on. Battery in Service State, bd_prochot  and CPU throttle a lot to 0.2Ghz that drove me crazy.(Modern Computer running at 200Mhz can be something dramatic. The touching temperature won't bounce you knee cause the cpu is at around 50 celsius--you can cold the cpu to ill under this temp.)
# Pre
- you need to dignose u computer first for any potaintial issue causing heat and throttle, incase of damaging your hardware before appling this repo.
- eg. Take off the back pack and add some thermal pattles on vrm chips and other heating compounents. Reset SMC and common methods may solve your problem.
- Xcode
- A basic English reading level for foreign materials reading.(Well at least a patient to figure them out. Like this repo written in English, while the writer is a native Chinese.)
- Stable Global Netwrok Connection
# Concepts
 ## How CPU's Performance, Power and Heat balance and what can we do? ##
- For CPU, this is a huge and complex monitor and control system, but we can simplify them into three parts: Thermal, MSRs and OSs.
  - Thermal prevent CPU or iGPU from burning themselese from the inside, while MSR codes like BD_Prochot and others keep other components from firing the cores. The OS also do a lot things.
  - Since the MSR or Micro Specific Registers may be read and write by any software and may cause safety issues. OSes like MacOS may take over the function by using custom designed CPU and MSRs from intel (like my Macbook CPU i5-1038ng7) or just make one by themselves(((.
  - A Complex system may or may not be better for your personal usage, it may be too strict or too loose that can cause performance lag or heat.
- In this case, to know the heat management superior is a key to the answer.
  - If using MSR tools like Voltageshift and get no effects. Consider OS.
  - As I know, The MacOS using kernal kexts like IOPlatformPluginFamily and X86PlatformPlugin to control Intel CPU functions . Turbo, Power, Sleep and so on.
  - The sensors on the motherboard will not communicate directly to the CPU. Probably. So the OS takes the main part in this whole thing.
# Install Guide 
- Step 1 : Check prerequisites again. (Well, though my macbook is a tough guy, i don't know about you. Since this repo is now in a long test progress, i strongly not recommend you applying it on your working computer.)
- Step 1.1 : You may check what causing CPU throttle by using ThrottleStop by TechPowerUp on Windows Bootcamp. In my case, the BD_PROCHOT title in "Limits" will turn red when doing p95 test for a while on Windows.
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
- Step N-1: Use repo "TurboMac" Install-Script. And we only need a few line of it.
  ## mount System Current Booting Snapshot to a folder, and after that we create a new one. ##
  - ROOT_LABEL=`df | grep /$ | awk '{print $1;}' | grep -o ".*disk\d*s\d*"`
echo "Root label: $ROOT_LABEL" ##Means determinating root disk label
  - sudo mkdir $HOME/nonroot
  - sudo mount -t apfs -o nobrowse $ROOT_LABEL $HOME/nonroot
  - sudo mkdir $HOME/BackupKexts ##for backup# although there are snapshot# just incase
  - sudo mv -f $HOME/nonroot/System/Library/Extensions/IOPlatformPluginFamily.kext/Contents/PlugIns $HOME/BackupKexts/
  - sudo kmutil install -u -v --update-preboot -R $HOME/nonroot ##will update preboot and kext cache
  - sudo bless --folder $HOME/nonroot/System/Library/CoreServices --bootefi --create-snapshot ##snapshot the root system and boot from it
- Step N : The kext that I'am currently using: GoodbyeBigSlow NoBdProcHot TurboMac
  - we can run "kextstat | grep -v com.apple" to determine whitch kext is really loaded.
  - In my case, I have installed those three kext but NoBdProcHot was not loaded. And I have to run a automatic code on Automator to enable all the performances.
    -The code for Automator will be provided.
# Known Issues
- Login progress bar may be a much slower loading than usual.
- Login items may be slow due to throttle (bd_prochot, kext conflicts).
- Bluetooth would get stuck at boot and a while before it can be used for good.(Thanks to a bilibili fan mentioned this issue)
- Crashing and overheat may happend more often.(May because the beta OS I'm using.)
- Wake from Sleep or plug in buggy usb devices may cause serious throlling(not BD_Prochot but some regular unknown laggy). The reason is vague. May because of root and kext modifying.
# Resources and Description 
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
