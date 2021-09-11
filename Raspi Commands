# List of main raspi command for slowing down temperature

---------------------------------------
Stress you board :
---------------------------------------

  STRESS PACKAGE INSTALL:
    
      sudo apt-get install stress

  STRESS RUN (V1):

    stress -c 4 -t 900s &
    vcgencmd measure_clock arm
    vcgencmd measure_temp
    
  STRESS RUN (V2)
  
    while true; do vcgencmd measure_clock arm; vcgencmd measure_temp; sleep 10; done& stress -c 4 -t 900s


---------------------------------------
Monitor CPU_governor ( cpu0 to cpu3)
---------------------------------------

  GET CPU SETTINGS ( get cpu0 to cpu3)

    sudo cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_frequencies
    sudo cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_max_freq
    sudo cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq
    
  GET/SET CPU GOVERNOR (get/set cpu0 to cpu3)

    sudo cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor

    There are 5 different governor settings:
      1. ondemand : default, moves speed from min to max and back at a % of load
      2. conservative : gradually switch frequencies at % of load
      3. userspace : use user specified frequencies
      4. powersave : all cores set at minimum frequency
      5. performance : all cores set at maximum frequency
      
      echo "powersave"| sudo tee /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
      
      do it for all cpus :
        echo "powersave"| sudo tee /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
        echo "powersave"| sudo tee /sys/devices/system/cpu/cpu1/cpufreq/scaling_governor
        echo "powersave"| sudo tee /sys/devices/system/cpu/cpu2/cpufreq/scaling_governor
        echo "powersave"| sudo tee /sys/devices/system/cpu/cpu3/cpufreq/scaling_governor
      
      
