# HOW TO STREAM RT VIDEO over UDP ?


WINDOWS      (IP = 192.168.2.11) : setup VLC to grab desktop and stream it to RASPI IP (eg 192.168.2.55 )

PI cmd line  (IP = 192.168.2.55) : mpv --profile=low-latency --no-cache --untimed --no-demuxer-thread --vd-lavc-threads=1 udp://192.168.2.11:1234
