# Mac-Pro-5.1-Fan-Script
A startup script to increase the Mac Pro 5,1's fans from idle to a 'working' rpm.

My cMP has six fans but for older models, just comment out or exclude as required.

## Background
I wanted to repurpose my Mac Pro, I had been running Open Core with macOS Big Sur and Windows as a dual boot but after installing Arch on my aging iMac, I thought Linux on the Mac Pro was the best way to go.

After installing, I went looking for a way to control the fans and the temps were rising, especially the Northbrige. I found a few apps, namely MBPFAN and MACFAND but these were either limited on the number of fans and/or I found the service reset.

###### My Script
My solution was something a little simpler than an daemon, a simple script to load at startup and re-write the apple smc min fan speed files, increasing the speed to a set level. 

The fan speeds within the uploaded scripts are based on my every day useage and the position of my system as the fans can be noisy.

## Testing
I am using pure Arch on my system but have tested it in Ubuntu, Mint, Manjaro. 

## Instillation
Either clone the script and service or create your own as below.

###### Create the service

      sudo nano /etc/systemd/service/startfan.service
    
    [Unit]
    Description=Script

    [Service]
    ExecStart=/usr/bin/fan.sh

    [Install]
    WantedBy=multi-user.target

###### Create the script

      sudo nano /usr/bin/fan.sh

    #!/bin/sh
    echo 900 > /sys/devices/platform/applesmc.768/fan1_min
    echo 1400 > /sys/devices/platform/applesmc.768/fan2_min
    echo 1400 > /sys/devices/platform/applesmc.768/fan3_min
    echo 900 > /sys/devices/platform/applesmc.768/fan4_min
    echo 3000 > /sys/devices/platform/applesmc.768/fan5_min
    echo 3000 > /sys/devies/platform/applesmc.768/fan6_min

###### Make the script executable

    sudo chmod 755 /usr/bin/fan.sh

###### Enable the service

    sudo systemctl enable startfan.service

###### Start the service

    sudo systemctl start startfan.service

You should hear the fans spin up. 