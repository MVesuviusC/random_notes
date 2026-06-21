## Voron firmware install
1. Install raspberry pi imager (https://docs.vorondesign.com/build/software/installing_mainsail.html)
    1.1 select pi version under device
    1.2 select OS (specific purpose -> 3 printing -> mainsail OS)
    1.3 select start and pick your SD card
2. Put SD card into pi and turn on printer
3. Connect to pi over wifi via putty/mobaxterm
4. Install KIAUH
    4.1 
    ```
    cd ~/
    git clone https://github.com/KlipperScreen/KlipperScreen.git
    ./KlipperScreen/scripts/KlipperScreen-install.sh 
    or
    BACKEND="X" SERVICE="Y" NETWORK="N" START=1 ./KlipperScreen/scripts/KlipperScreen-install.sh
    ```
    4.2 When installing
        4.2.1 say yes to install regular desktop app
        4.2.2 install xserver
        4.2.3 *** Do not install the network manager ***
            This will delete your wifi info and you'll have to start all over
5. Add config files
    5.1 Open http://mainsailos.local/config
    5.2 Go to machine
    5.3 Create a file called printer.cfg if it doesn't exist
    5.4 Make other files from github
    5.5 Restart
6. Update software on http://mainsailos.local/config machine page
7. Set up webcam
    7.1 less ./printer_data/logs/crowsnest.log | grep libcamera
    7.2 Edit crowsnest.conf
    ```
        mode: camera-streamer
        port: 8080
        device: /base/soc/i2c0mux/i2c@1/ov5647@36
        resolution: 1920x1080
        max_fps: 30
    ```
    7.3 Go to ttp://mainsailos.local/config and click on gears
    7.4 Add webcam - should just need to name it

