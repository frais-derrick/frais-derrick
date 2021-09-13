--------------------------------------
	GPIOS
--------------------------------------

wiringpi: non à jour pour les pi4...

	cd /tmp
	wget https://project-downloads.drogon.net/wiringpi-latest.deb
	sudo dpkg -i wiringpi-latest.deb

pour lire l'état des GPIO

	gpio readall


--------------------------------------
	SCRIPTS : restart
--------------------------------------

#!/bin/sh

COMMAND='endless.py -args'
LOGFILE=restart.txt

writelog() {
  now=`date`
  echo "$now $*" >> $LOGFILE
}

writelog "Starting"
while true ; do
  $COMMAND
  writelog "Exited with status $?"
  writelog "Restarting"
done


------------------------------------
	UNDERCLOCKING
------------------------------------

# underclock
#arm_freq=1200
#arm_freq_min=200

core_freq_min=100
gpu_freq_min=100
h264_freq_min=100
isp_freq_min=100
v3d_freq_min=100

over_voltage=-3
over_voltage_min=-6

over_voltage_sdram=-2

------------------------------------
	DESACTIVATION WIFI
------------------------------------
crontab -e
@reboot sudo ifdown wlan0


------------------------------------
	AUDIO SOUND CARD IQAUDIO-CODEC
------------------------------------

/boot/config.txt

#dtparam=audio=on
dtoverlay=iqaudio-codec

and enable i2s, i2c, spi


after boot
download https://github.com/iqaudio/Pi-Codec

run in console :

alsactl restore -f IQaudIO_Codec_OnBoardMIC_record_and_HP_playback.state 

TEST:

player:	aplay -l
speaker: speaker-test -c2
loopback: arecord -f cd | aplay -v

examples:  https://github.com/respeaker/seeed-voicecard/wiki


------------------------------------
	REMOVE SUDO PASSWORD REQUEST
------------------------------------

sudo nano /etc/sudoers

add below :

#includedir /etc/sudoers.d
pi ALL=(ALL:ALL) NOPASSWD:ALL


------------------------------------
	ENABLE SERIAL LINE
------------------------------------
sudo nano /boot/config.txt

Add:  enable_uart=1

------------------------------------
	REMOVE CONSOLE AT STARTUP
------------------------------------

sudo nano /boot/cmdline.txt

remove (or add to re-enable)  : console=serial0,115200 console=tty1



------------------------------------
	ENABLE TV_OUT
------------------------------------
edit /boot/config.txt

- comment all HDMI topics
- add "enable_tvout=1"
- add "sdtv_mode=2"

-----------------------------------
STREAMING
-----------------------------------
mpv --no-cache --untimed --no-demuxer-thread --video-sync=audio --vd-lavc-threads=1 udp://$HOST_IP:$PORT 

-----------------------------------
ENABLE I2C_0
-----------------------------------
edit /boot/config.txt
dtparam=i2c_vc=on

dtoverlay=i2c-gpio,bus=0,i2c_gpio_delay_us=1,i2c_gpio_sda=44,i2c_gpio_scl=45
-----------------------------------
ENABLE USB
-----------------------------------
edit /boot/config.txt
dtoverlay=dwc2,dr_mode=host

-----------------------------------
FIST BOOT SSH
-----------------------------------
in boot directory, create "ssh" file name (empty, no extension)


-----------------------------------
GPIO STATE AT STARTUP
-----------------------------------

to read GPIO, avoid gpio readall (not supported in CM4)
use : raspi-gpio get / set
ex : raspi-gpio set 11 pu

or in /boot/config.txt: 

gpio=11=pu
Warning : for example, if gpio11 is used for SPI, SPI will override pu ( so uncheck SPI in raspi-config )

