# NETSCANNER

This project is the product of my final year undergraduate development project for my Bachelor of Science degree in Computer Networking and Cyber Security at London Metropolitan University.

This program will conduct a comprehensive scan of the local network and surrounding wireless networks using basic OS utilities, Nmap and airodump-ng.

## Disclaimer
This program is designed to conduct a rigorous and comprehensive scan of your local network by actively engaging network hosts using common host discovery and port scanning techniques to produce an extensive overview of the local network. This program is also designed to produce an overview of surrounding wireless networks by capturing 802.11 frames in the vicinity of the host interface. 

This program is a network reconnaissance tool and when executing the module you are wholly responsible and liable for any physical, mental or financial damage, complications and consequences of all operations that you execute using this utility. 

It is strongly recommended that you execute this program on networks you are authorised to audit. Use at your own discretion and risk.

## Demo
![gif](https://user-images.githubusercontent.com/111141495/222961530-c4722153-e6e1-4b4b-a0af-5a69abc2875d.gif)


## Screenshots
*MAC Addresses have been obscured.

### Running in the terminal
![Screenshot 1](https://i.ibb.co/PDy14fL/Screenshot-from-2023-02-21-18-07-54-1.png)

![Screenshot 2](https://i.ibb.co/DfKzcfH/Screenshot-from-2023-02-21-18-08-02-1.png)

![Screenshot 3](https://i.ibb.co/Sm2X70F/Screenshot-from-2023-02-21-18-08-09-1.png)

### Viewing the output file
![Screenshot 4](https://i.ibb.co/Mp7HWJQ/Screenshot-from-2023-02-22-22-40-39-1.png)

![Screenshot 5](https://i.ibb.co/Fg90thh/Screenshot-from-2023-02-22-22-40-56-1.png)

![Screenshot 6](https://i.ibb.co/KDbbcSD/Screenshot-from-2023-02-22-22-41-15-1.png)

![Screenshot 7](https://i.ibb.co/3y2SDwb/Screenshot-from-2023-02-22-22-41-29-1.png)

## Usage
```bash
  python3 -m netscanner < -h | -nP | -w | -l | -hD >
  < --wP <seconds> >
  < --pP <seconds> >
  < --pR <first port-last port> >
```

## Requirements
* `python3`: NETSCANNER was designed to work with Python 3.10.
* [`ifconfig`](https://linux.die.net/man/8/ifconfig): For gathering statistical data on local interfaces.
* [`ethtool`](https://linux.die.net/man/8/ethtool): For gathering statistical data on local interfaces
* [`iwconfig`](https://linux.die.net/man/8/iwconfig): For gathering statistical data on local wireless-capable interfaces.
* [`airmon-ng`](https://www.aircrack-ng.org/doku.php?id=airmon-ng): For enabling monitor mode on capable interfaces.
* [`airodump-ng`](https://www.aircrack-ng.org/doku.php?id=airodump-ng): For capturing 802.11 beacon frames.
* [`Nmap`](https://nmap.org/): For conducting local network reconnaissance.

A monitor-mode capable wireless interface is also required if you wish to use the wireless network discovery feature. See [here](http://www.aircrack-ng.org/doku.php?id=compatible_cards) for more information on this.

## Execution
### Using PyPI
The preferred method of running the program is installing the Python package from PyPI directly.

```bash
  pip install netscanner
```

Then running the program:

```bash
  python3 -m netscanner <mode specification> <options>
```

### Manually
You can also run the module itself by downloading the `__main__.py` module from this repository and running it.

```bash
  python3 __main__.py <mode specification> <options>
```

### Avoiding sudo
The program runs shell commands with sudo privileges in the background, which may require you to provide your sudo password frequently.

To avoid this, append this line to your `/etc/sudoers` file using `sudo visudo`

```bash
# NETSCANNER PACKAGE
<USERNAME> ALL=(ALL) NOPASSWD: ALL
```

This will allow you to run the module without being asked for your sudo password. This is wholly optional and it is recommended that you comment this line when done using the program. **Attempt to run the program initially before you do this.**

## Modes and Options
### Modes 
* Mode 1 
    * This mode will execute all functions of the program. If no flags are specified this will be the mode of operation.
* Mode 2 `(-nP)`
    * This flag will execute Mode 2, NO PORT SCAN, which will execute the Host Discovery and 802.11 WLAN Discovery processes.
* Mode 3 `(-w)`
    * This flag will execute Mode 3, WIRELESS ONLY, which will execute the 802.11 WLAN Discovery process exclusively.
* Mode 4 `(-l)`
    * This flag will execute Mode 4, LOCAL SCAN ONLY, which will execute the Host Discovery and Port Scan processes.
* Mode 5 `(-hD)`
    * This flag will execute Mode 5, HOST DISCOVERY ONLY, which will execute the Host Discovery Process exclusively.

### Options
* Wireless Scan Period `(--wP <seconds>)`
    * This option allows you to specify a scan period for the 802.11 WLAN Discovery process. The default is 60. This value is ignored if the mode of operation is not Mode 1, 2 or 3. Large values will result in longer scan times but greater verbosity.
* Port Scan Period `(--pP <seconds>)`
    * This option allows you to specify a scan period for the Port Scan process. The default is 60. This value is ignored if the mode of operation is not Mode 1 or 4. Large values will result in longer scan times but greater verbosity. 
* Port Range `(--pR <first port - last port>)`
    * This option allows you to specify a port range for the Port Scan process. The default is the 100 most common ports determined by Nmap (`-F`). Large values will result in longer scan times but greater verbosity. It is useful to combine this option with the `--pP` option to avoid scan timeouts when scanning large ranges. 

## Processes
This section provides a brief synopsis of each process used in the program. There are three processes that are used.

### Host Discovery
This process gathers characteristics about the local network and hosts on the local network using ifconfig, iwconfig, ethtool. And the [ARP Request Ping](https://nmap.org/book/host-discovery-techniques.html#arp-scan) and [rDNS Query Flood](https://nmap.org/book/host-discovery-dns.html) in Nmap.

### Port Scan
By default, this process uses Nmap to determine the state of the 100 most used TCP and UDP ports [`(-F)`](https://nmap.org/book/man-port-specification.html#idm45323729683296) on all active hosts on the local network using the [TCP Half-Open Scan](https://nmap.org/book/synscan.html) and the [UDP Scan](https://nmap.org/book/scan-methods-udp-scan.html), port scanning techniques. The ports that are scanned can be changed using the `--pR` flag in the command line, to indicate a port range. 

This process also has a default timeout period of 60 seconds which can be changed using the `--pP` flag.

### Remote WLAN Discovery
This process determines the characteristics of remote wireless networks in the vicinity of the host machine if a wireless interface is present, available and capable of 802.11 monitor mode, using the [802.11 packet capture](https://www.aircrack-ng.org/doku.php?id=wpa_capture) technique in airodump-ng.

The program will attempt to use [airmon-ng](https://www.aircrack-ng.org/doku.php?id=airmon-ng) to enable monitor mode on the interface. If this is unsuccessful you will not see an error, the table for Remote WLAN data will simply be empty.

This process has a default timeout period of 60 seconds which can be changed using the `--wP` flag.

## Supported Operating Systems
The program was designed to work with any UNIX-like operating system. It will not work on Windows.

I have confirmed the following operating systems to support the program.

* Ubuntu 22.10
* Kali Linux 2022.4
* BlackArch Slim 2021.09.01
* ParrotOS Security 5.2 Electro Ara

## Understanding the wireless data output
### Access Points

| Header | Description |
| --- | --- |
| `BSSID` | The MAC address of a detected access point. |
| `SIGNAL` | The RSSI between the host machine and a detected access point. |
| `CH` | The operating channel of a detected access point. |
| `RATE `| The maximum speed in Mbps supported by a detected access point. |
| `ENC` | The encryption algorithm in use by a detected access point. |
| `CIPHER` | The cipher in use by a detected access point. |
| `AUTH` | The authentication protocol in use by a detected access point. |
| `VENDOR` | The vendor of the NIC of a detected access point. |

### Stations

| Header | Description |
| --- | --- |
| `STATION` | The MAC address of a detected station. |
| `ASSOCIATED-BSSID` | The MAC address of the access point the station is connected to. |
| `SIGNAL` | The RSSI between the host machine and a detected station. |
| `VENDOR` | The vendor of the NIC of a detected station. |

Please view the airodump-ng documentation [here](https://www.aircrack-ng.org/doku.php?id=airodump-ng#usage_tips), for more detailed information about each header. Note: `PWR` and `MB` in airodump-ng correspond to `SIGNAL` and `RATE`.

## FAQs
#### The terminal output is unreadable, now what?
Sometimes the output may be format incorrectly due to the size of the terminal window or for unknown reasons. You can access all output files in the `home/<username>/Documents/NetScanner` directory which is created when you run the program initially.

All output files are in `.txt` format and are timestamped with the local date and time of the scan, and a tag indicating the mode of operation that was specified.

#### Why is there no wireless data in the output?
Be sure to check your host machine has a 802.11 NIC. Then [verify](http://www.aircrack-ng.org/doku.php?id=compatible_cards) that your interface supports monitor mode.

The program will attempt to enable monitor mode on the most active interface of your machine. This is determined by comparing the amount of data exchanged by all available interfaces excluding the loopback interface.

If the most active interface does not support 802.11, or monitor mode - you will not see any wireless output data. If you are running the program in a virtual machine, you are also unlikely to get any wireless output data.

#### How long should the scan take?
Mode 1 will take at least two minutes to complete. Other modes will take less time. If you specify longer wireless scan, or port scan periods, and/or a large port range - the time period the scan will take will increase proportionately.

#### What are hidden access points?
These are access points that belong to a wireless network that has been configured to use SSID cloaking. This disables the broadcast of the SSID.

#### What are stations?
Stations are wireless endpoints.

#### What are unassociated stations?
These are stations that are either not connected to, or are attempting to connect to, a detected wireless network.
