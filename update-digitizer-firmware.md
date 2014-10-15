# update firmware
adb shell
hw0808_updater path_of_fw

# display digitizer fw version
killall onyx_tpd
hw0808_updater -v 
