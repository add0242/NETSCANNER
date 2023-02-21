
# NETSCANNER

This project was created as part of my final year undergraduate development project for my Bachelor of Science degree. 

This program will conduct a comprehensive scan of the local network and surrounding wireless networks using basic OS utilities, Nmap and airodump-ng.

## Demo




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

### Using PyPi
The preferred method of running the program is installing the Python package from PyPi directly.

```bash
  pip install netscanner
```

### Manually
You can also run the module itself by downloading a ZIP file of this repository and running

```bash
  python3 NetScanner
```
