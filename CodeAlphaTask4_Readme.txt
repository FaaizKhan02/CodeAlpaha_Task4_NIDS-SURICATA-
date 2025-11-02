✅ Full Setup Guide: Suricata Network Monitoring on Linux (Kali/Ubuntu)
🔧 1. Install Suricata

First, install Suricata from the official repo or package manager:
sudo apt update
sudo apt install suricata -y
-------------------------------------
🔍 2. Configure Suricata
Copy Default Configuration
sudo cp /etc/suricata/suricata.yaml /etc/suricata/suricata.yaml.backup
sudo nano /etc/suricata/suricata.yaml
Edit the AF_PACKET Section

Find and update this part to monitor your network interface (replace eth0 with your actual interface, e.g. wlan0):
af-packet:
  - interface: eth0
------------------------------------
🛠️ 3. Add Custom Rules

Create a new rule file:
sudo nano /etc/suricata/rules/custom.rules
Add a simple ICMP (ping) monitoring rule:
alert icmp any any -> any any (msg:"ICMP Ping detected"; sid:1000001; rev:1;)

Add to suricata.yaml under rule-files:
  - custom.rules
--------------------------------
🔄 4. Test Configuration

Test Suricata config for errors:
sudo suricata -T -c /etc/suricata/suricata.yaml -v
▶️ 5. Start Suricata
sudo systemctl start suricata
sudo systemctl status suricata --no-pager
If you get errors, you can reset the state:
sudo systemctl stop suricata
sudo killall suricata
sudo rm /var/run/suricata/suricata.pid
sudo systemctl start suricata
-----------------------------------------
📁 6. Check Logs and Output

Suricata logs are stored at:
/var/log/suricata/fast.log        # Alerts
/var/log/suricata/suricata.log    # Base log
Example log entry from fast.log:
11/02/2025-10:05:24.567890 [**] [1:1000001:1] ICMP Ping detected [**] [Classification: Misc activity] [Priority: 3] {ICMP} 192.168.1.10 -> 192.168.1.1
---------------------------------
