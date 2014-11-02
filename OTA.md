# update fw 
adb push update.zip /mnt/sdcard/update.zip 
adb shell am start -n com.onyx.android.onyxotaservice/.OtaInfoActivity 

# ota fingerprint 
cat /system/build.prop |busybox grep ro.build.fingerprint 
ro.build.fingerprint=ONYX/M96/M96:4.0.4/2014-10-31_22-53_dev_bac779c/238:eng/release-keys 


# update wavefrom 
echo 156 > /sys/devices/platform/tps6518x_sensor.0/vcom_value 
adb push xx.fw /vendor/firmware/imx/epdc.fw  