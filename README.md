# MacThermalAway
This repo will share a prospective and comprehensive view on how to stop macbook from throttling with a bad battery/a vrm heating/bd prochot. Os version supports BigSur to Sequoia, from 11 to 15. IF you know the principles you could apply to any version of Oses and hardwares. 
# Pre
- you need to dignose u computer first for any potaintial issue causing heat and throttle, incase of damaging your hardware before appling this repo.
- eg. Take off the back pack and add some thermal pattles on vrm chips and other heating compounents.
- Xcode
# Resources
- https://github.com/cocafe/msr-utility/issues/8#issuecomment-1853242674 This resource discuss what a msr means to cpu
- https://github.com/sicreative/VoltageShift This is the best tool to write into the msr of the cpu when macos already booted
- https://github.com/calasanmarko/TurboMac This resource shows runable commands that can modify the root of the newer system and what files need to be dealt with
- https://github.com/balfabb87/GoodbyeBigSlow.kext This resource is what we need to manage cpu thermal after deleting the files
- https://github.com/syscl/CPUTune/issues/21 This resource discuss a little why modify msr that controls the bd_prochot does not work and what to delete in system files.
- https://github.com/benbaker76/Hackintool calculator that can convert binary to hex. For thoese have a bad math or just lazy like me.
