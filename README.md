# Discovery-of-Wi-Fi-networks-with-hidden-SSID-using-a-2.4-GHz-Parabolic-Antenna
This project demonstrates how a high-gain **directional antenna** can be used to discover Wi-Fi networks, including those with **hidden SSIDs** (networks that do not broadcast their name).  


It builds upon my earlier repositories:  
- 3D-Printed Modular Parabolic Dish Antenna  https://github.com/SudoTraceRoute/2.4GHz-Modular-Parabolic-Dish-Antenna
- 2D Wi-Fi Radar Mapping  https://github.com/SudoTraceRoute/wifi-radar-mapping  

The objective is to study **signal propagation, network visibility, and antenna directivity** while demonstrating that “hiding a network name” is not an effective security measure.  

⚠️ **Disclaimer**: This project is for **educational and research purposes only**. All measurements were made in a controlled environment using my own equipment. Unauthorized access to networks is illegal.

---

**🛠️ Hardware and Software**

- **Antenna**: 3D-printed parabolic dish antenna for 2.4 GHz band (link to repo above).  
- **Wi-Fi Adapter**: *WiFi Nation WN-H3* (supports monitor mode and packet injection).  
- **Operating System**: Parrot OS (Debian-based, penetration testing distribution).  
- **Software Tools**:  
  - [Aircrack-ng suite](https://www.aircrack-ng.org/) (scanning & monitoring)  
  - CSV log exports for analysis  
  - Standard Linux networking tools (`iwconfig`, `ifconfig`, `nmcli`)  

This setup was chosen because it reflects the type of hardware and tools that a network administrator or penetration tester would realistically use in troubleshooting or research.

---

**📡 Key Terminology**

- **SSID (Service Set Identifier):** The public name of a Wi-Fi network (e.g., `HomeWiFi`). A hidden SSID simply suppresses this broadcast but does not make the network secure.  
- **BSSID (Basic Service Set Identifier):** The MAC address of a Wi-Fi access point. This uniquely identifies the device, even if the SSID is hidden.  
- **RSSI (Received Signal Strength Indicator):** A measure of signal strength, reported in dBm (closer to 0 = stronger).  
- **Hidden SSID:** A Wi-Fi network that does not broadcast its SSID. While invisible to basic scans, it is still detectable by analyzing beacon and probe frames.  
- **Monitor Mode:** A special mode for Wi-Fi adapters that allows passive listening to all nearby traffic, not just connected networks.  

---

**🔧 Methodology**

1. Assembled the **directional antenna** and connected it to the Wi-Fi adapter.  
2. Performed a **0° → 180° directional sweep**, moving in 15° increments.  
3. At each position:  
   - Captured nearby networks with `airodump-ng`.  
   - Logged visible and hidden SSIDs, BSSIDs, channels, encryption type, and RSSI values.  
4. Exported results to `.csv` format for later analysis.  
5. Generated a **radar-style visualization** to compare visible and hidden networks across antenna angles.  

This experiment highlights how antenna orientation strongly influences which networks are discovered and how reliably.

---

**💻 Basic Aircrack-ng Workflow**

Step-by-step commands used in this project:

**1. Identify Wi-Fi interfaces**
```bash
iwconfig


2. Stop services that interfere with monitor mode:

- sudo systemctl stop NetworkManager

- sudo systemctl stop wpa_supplicant


3. Kill interfering processes and enable monitor mode:

- sudo airmon-ng check kill

- sudo airmon-ng start wlan0

This creates a monitor-mode interface (e.g., wlan0mon).


4. Scan for networks:

- sudo airodump-ng wlan0mon

Hidden networks will appear with ESSID set to <hidden> or <length:…>.


5. Save results to CSV
- sudo airodump-ng --output-format csv -w wifi_scan wlan0mon



