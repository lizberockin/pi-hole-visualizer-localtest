# Pi-hole Visualizer  
Pi-hole Visualizer displays Pi-hole statistics on the Raspberry Pi Sense-HAT with multiple animations.

![sense-hat display](https://github.com/simianAstronaut/pi-hole-visualizer/blob/master/images/sense_hat.gif)  

### Details  
- Pi-hole Visualizer cycles between five different customizable animations at regular intervals.  

- The first animation displays the global connection status. A green pulsating icon represents a functioning internet connection.

- The second animation(vertical bar chart) depicts the overall volume of DNS queries generated on the network. Each column represents a specific and adjustable time interval relative to the previous 24-hour timeframe. The time interval ranges from 10 minutes to 3 hours. The chart can be color coded to represent the level of DNS traffic or percentage of ads blocked.  

- In the third animation(spiral graph), the daily ad block percentage is represented by the number of red pixels displayed.  

- The fourth animation(horizontal bar chart) displays the relative level of DNS queries generated by top clients on the network in descending order.  

- The last animation(pie chart) displays the proportion of IPV4(orange pixels) to IPV6(green pixels) queries.  

- Options include manual chart selection, randomization of pixel generation, specifying the orientation of the display, and low-light mode.  

- Joystick controls allow for adjustment of program options interactively.  

- Pi-hole Visualizer is either run from the command line or enabled as a systemd service to run automatically at boot.  
---  
  
### Requirements
* To install Pi-hole, run `curl -sSL https://install.pi-hole.net | bash`.
* The Sense-HAT package can be installed with `sudo apt-get install sense-hat`.  
 
---  

### Authorization  
To view statistics regarding top clients and query types, authorization from the Pi-hole web server is required. If you are running Pi-hole Visualizer on the same machine that is running Pi-hole, it is assumed a configuration file containing the password hash is located at (`/etc/pihole/setupVars.conf`) and no action is required.  
If you are on a remote machine, you can enable authorization by creating an environment variable containing the password hash. Append the following line to (`~/.bash_profile`) or (`~/.profile`): `export WEBPASSWORD="hash_value"`.  

---  
  
### Usage
**`dns_stats.py [OPTION]`**  

#### Options  
`-h, --help`  
Show this help message and exit.  

`-i {10, 30, 60, 120, 180}, --interval {10, 30, 60, 120, 180}`  
Specify interval time in minutes. Defaults to one hour.

`-c {basic, traffic, ads}, --color {basic, traffic, ads}`  
Enter 'basic' to generate bar charts in the default red color, 'traffic' to color code based on level of DNS queries, or 'ads' to color code by ad block percentage.

`-a ADDRESS, --address ADDRESS`  
Specify address of DNS server, defaults to localhost.

`-o {0, 90, 180, 270}, --orientation {0, 90, 180, 270}`  
Specify orientation of display so that RPi may be installed in non-default orientation.

`-ll, --lowlight`  
Lower LED matrix brightness for use in low light environments.  

`-r, --randomize`  
Randomize order of pixels displayed.  

`-s {1, 2, 3, 4, 5}, --select {1, 2, 3, 4, 5}`  
Specify which animation(s) to display, with multiple items separated by a space.  

#### Joystick Controls  
- _UP - PUSH_  
Cycle color mode.  

- _RIGHT - PUSH_  
Cycle interval selection.  

- _DOWN - PUSH_  
Toggle low-light mode.  

- _LEFT - PUSH_  
Cycle display orientation.  

- _MIDDLE - PUSH_  
Toggle randomization.  

- _MIDDLE - HOLD_  
Exit program.  
 
---  
  
 ### To Install As a System Service  
 1. Make the script and unit file executable:  
 `sudo chmod +x dns_stats.py`  
 `sudo chmod +x dns_stats.service`  
 
 2. Check that the path in the unit file after `ExecStart` matches the path of your script.  
 
 3. Copy the unit file to the system directory:  
 `sudo cp dns_stats.service /lib/systemd/system`  
 
 4. Enable the service to run at startup:  
 `sudo systemctl enable dns_stats`  
 
 5. Reboot:  
 `sudo reboot`  
 
 6. To check the status of the service:  
 `sudo systemctl status dns_stats`
