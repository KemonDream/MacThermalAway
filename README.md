# MacThermalAway
This repo will share a prospective and comprehensive view on how to stop macbook from throttling with a bad battery/a vrm heating/bd prochot. Os version supports BigSur to Sequoia, from 11 to 15. IF you know the principles you could apply to any version of Oses and hardwares.
- This repo tested on Macbook pro 16,1, CPU Intell-i5-1038ng7 ,no dedicate Gpu, MacOS Sequoia Beta2, File-Volt on. Battery in Service State, bd_prochot  and CPU throttle a lot to 0.2Ghz that drove me crazy.(Modern Computer running at 200Mhz can be something dramatic. The touching temperature won't bounce you knee cause the cpu is at around 50 celsius--you can cold the cpu to ill under this temp.)
# Pre
- you need to dignose u computer first for any potaintial issue causing heat and throttle, incase of damaging your hardware before appling this repo.
- eg. Take off the back pack and add some thermal pattles on vrm chips and other heating compounents. Reset SMC and common methods may solve your problem.
- Xcode
- A basic English reading level for foreign materials reading.(Well at least a patient to figure them out. Like this repo written in English, while the writer is a native Chinese.)
- Stable Global Netwrok Connection
# Concepts
 ## SO, How do we control the temperature of the PC ? ##
- For CPU, this is a huge and complex monitor and control system, but we can simplify them into three parts: Thermal, MSRs and OSs.
  - Thermal prevent CPU or iGPU from burning themselese from the inside, while MSR codes like BD_Prochot and others keep other components from firing the cores. The OS also do a lot things.
  - Since the MSR or Micro Specific Registers may be read and write by any software and may cause safety issues. OSes like MacOS may take over the function by using custom designed CPU and MSRs from intel (like my Macbook CPU i5-1038ng7) or just make one by themselves(((.
  - A Complex system may or may not be better for your personal usage, it may be too strict or too loose that can cause performance lag or heat.
- In this case, to know the heat management superior is a key to the answer.
  - If using MSR tools like Voltageshift and get no effects. Consider OS.
  - As I know, The MacOS using kernal kexts like IOPlatformPluginFamily and X86PlatformPlugin to control Intel CPU functions . Turbo, Power, Sleep and so on.
  - The sensors on the motherboard will not communicate directly to the cpu but the MacOS. Probably.
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
# Known Issues
- Login progress bar may be a much slower loading than usual
- Login items may be slow due to throttle (bd_prochot).
- Bluetooth would get stuck at boot and a while before it can be used for good.(Thanks to a bilibili fan mentioned this issue)
- Crashing and overheat may happend more often.
# Resources
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
