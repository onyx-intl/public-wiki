radio
========


SI4735 (FM/AM/LW/SW): 
PS: Below character in red is the parameter can be changed.

* Set Audio Path, At java layer, to change the audio path, you can set use the API:
* AudioManager.setParameters(“fm_route=1”), but remember to set “fm_route=0” when closing # the FM application.
alsa_amixer cset numid=42 1
alsa_amixer cset numid=45 1 
alsa_amixer cset numid=9 110


* Enable FM
```
echo 1 > /sys/bus/i2c/drivers/si4735/1-0063/pwr_ctl
```

* Switch to AM, 0 is FM, 1 is AM, 2 is LW, 3 is SW
```
echo 1 > /sys/bus/i2c/drivers/si4735/1-0063/receiver
```


* Set AM frequency to 567Hz, range between 520kHz and 1.71MHz
```
echo 567 > /sys/bus/i2c/drivers/si4735/1-0063/tune_freq
```

* u for seek up, d for seek down
```
echo u >  /sys/bus/i2c/drivers/si4735/1-0063/seek
```

* Power Off
```
echo 0 > /sys/bus/i2c/drivers/si4735/1-0063/pwr_ctl
```


Si4707 (WeatherBand)
=============================================================================

* Set Audio Path, At java layer, to change the audio path, you can set use the API:
* AudioManager.setParameters(“fm_route=1”), but remember to set “fm_route=0” when closing #the FM application.

alsa_amixer cset numid=42 1
alsa_amixer cset numid=45 1 
alsa_amixer cset numid=9 110

* Enable power
```
echo 1 > /sys/bus/i2c/drivers/si4707/2-0011/pwr_ctl
```

* tune Frequency, range between 162.4MHz and 162.55 MHz in 2.5 kHz step
```
echo 162525 > /sys/bus/i2c/drivers/si4707/2-0011/tune_freq
```

* Power Off
```
echo 0 > /sys/bus/i2c/drivers/si4707/2-0011/pwr_ctl
```

 Walkie talkie
=================================================================
The test command is located at external/uv2w

Use uv2w –s “AT+DMOVER” can get the module version. Other command can refer to the documents “HKT-UV2W 串口AT通信协议VER01_test.pdf”