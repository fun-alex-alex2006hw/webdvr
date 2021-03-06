webdvr
======

Web based DVR scheduling for HDHomerun using Raspberry Pi, NodeJS and Firebase.

Pi Configurations
=================
There are some configuration items that may be handy, I'm documenting them here so I can access them in the future if necessary (or automatically setup when I get to it).

Using Wifi to connect to internet and ethernet to connect to HDHomerun Dual.

Backup your existing network interfaces in case you need to revert.
    
    sudo cp nano /etc/network/interfaces /etc/network/interfaces.bak

Edit the interfaces file.

    sudo nano /etc/network/interfaces

    auto lo
    
    iface lo inet loopback
    iface eth0 inet static
    address 169.254.0.1
    netmask 255.255.0.0
    
    allow-hotplug wlan0
    iface wlan0 inet manual
    wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
    iface default inet dhcp

The main difference here is that I set the eth0 to static and gave it a link local address, perhaps another address might work, but this worked and I'm sticking with it.
   
Give it a reboot to be sure it works right.
If you have the command line tools installed below

    hdhomerun_config discover
    
Should result in a IP address in the 169 range for the first octet.

Finally

    ping www.google.com
    
Should work!


Automount your hard drive. setup.


How it works
=============
Start with:

    git clone https://github.com/onaclov2000/webdvr.git

Next:

You'll have to choose a firebase reference, I picked a project and used /dvr as the endpoint.

Next:
Make sure to update controller.js to contain a path to your firebase reference.

Place controller.js and index.html into your web hosting location.

Next:

Update config.js to point to the same firebase reference as your angularjs app.

Next:

Ensure you install the HDHomerun command line tools.

Next:
Run the command otherwise it won't work

    chmod +x record.sh

Update record.sh to point to your particular Tuner (just update the name), as well as where you want recordings to land.

Run the following:
    npm install

Finally start the server on your host PC

    node app.js

At that point things are ready.

TODO LIST (Make suggestions or pull requests please!)
==========
1. Update documentation Make things clearer, better directions.
2. Manage Schedule Conflicts
3. Optionally use second tuner

         1. Look through all scheduled tasks and look for a date that is during the date time + duration (overlapping).
         1a. If none exists, then return 0
         1b. If only one exists, check the tuner identifier, if it's 0 return 1
         1b. If more than one exists, set fb/conflict list

4. Upgrade to using HDHomerun nodejs package (currently using shell script with hdhomerun_config installed).
5. Implement Record Now (and start recording if no conflicts exist, rather than in the past).
6. Enable New episodes recording I'm thinking program ID lookup based, should we use tvguide, or another service?
7. Replace boring text with pictures (a la netflix style).
8. Combine config.js and angularjs's firebase reference into one file, so you copy/paste the same file in two places.
9. Figure out "newness" of episode, I *think* AiringAttrib 44 means new, but there are others (I'm certain of that), not sure how to tell.
10. For the Web Site, display a footer that you can click on that will slide up and show all recordings, while closed it'll show the closest upcoming recording though. <-- Kinda made a dropdown, it's not great but it's there
11. If size remaining and scheduled overlap then send email possibly (using sendgrid?)
12. Figure out commercial skipping (perhaps this would be useful)
13. Update jobs_queue with firebase existing jobs when starting up (otherwise they get out of sync).

https://github.com/MythTV/mythtv/tree/master/mythtv/programs/mythcommflag

http://www.mythtv.org/wiki/How_to_write_a_new_method_of_commercial_detection

https://code.mythtv.org/doxygen/classClassicCommDetector.html

Possible Python option

https://www.mythtv.org/wiki/Mythcommflag-wrapper


