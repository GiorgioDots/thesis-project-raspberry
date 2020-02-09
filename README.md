# Thesis Project Raspberry

## Description

This script is the part of the my project that create the events. To work it needs this configuration:

- Raspberry PI 3 B+;
- Camera for raspberry;
- Dedicated alimentation (if you use the pc alimentation for the raspberry it won't work, cause this project needs "a lot" of energy)
- PC;
- VNC Viewer or a program to connect via ssh to the raspberry.

### How it works

The script `object-detection.py` is the main, once it's executed it will start to record with the camera and when it detects an object "person" it will send the actual frame of the record to the backend (`{url}/events/:raspiId`).

To detect the person object it uses a python library called CV2.

# Installation

To install this script you need to follow these steps:

1. Clone or download this repo;
2. Open the file `install.sh` and replace the `pi` user home folder with your user name (if you use the default raspberry user leave it);
3. From shell execute `sudo install.sh`;
4. Wait for the instalation done;
5. Run `chmode 777 object-detection.py`
6. Run `chmode 777 logs.log` 
7. Change the url and the raspiId from `raspi_config.json`

Done, to test the function run `sudo python /home/pi/thesis-project-raspberry/object-detection.py --prototxt /home/pi/thesis-project-raspberry/MobileNetSSD_deploy.prototxt.txt --model /home/pi/thesis-project-raspberry/MobileNetSSD_deploy.caffemodel --confidence 0.5 & > /home/pi/thesis-project-raspberry/logs.log 2>&1`

Execute parameters: 

- `--prototxt`: absolute path to MobileNetSSD_deploy.prototxt.txt
- `--model`: absolute path to MobileNetSSD_deploy.caffemodel
- `--confidence`: the minimun confidence (%) to recognize objects (default 0.2) (e.g. 0.5 means that if the function rilevates an object, it will do nothing while the confidence its < 50%)

## Run at boot

to run this function at boot follow these steps:

1. `sudo nano /etc/rc.local`
2. insert at the bottom of this file but before the `exit 0` command:
`sudo python /home/pi/thesis-project-raspberry/object-detection.py --prototxt /home/pi/thesis-project-raspberry/MobileNetSSD_deploy.prototxt.txt --model /home/pi/thesis-project-raspberry/MobileNetSSD_deploy.caffemodel --confidence 0.5 & > /home/pi/thesis-project-raspberry/logs.log 2>&1`
3. ctr+x -> Y -> enter: to exit and save the file.
4. reboot the raspberry, wait 1/2 minutes and it will work.

