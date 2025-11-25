# Honeypot Automation

Owner: Rushank

**Project Title:** Enhanced Honeypot Detection and Analysis using Cowrie

---

### 1. Introduction

This project focuses on deploying an intelligent honeypot system using Cowrie to simulate a vulnerable SSH server. The goal is to capture, analyze, and visualize attacker behavior for security research, all within a controlled virtual lab environment.

---

### 2. Project Objectives

- Deploy Cowrie honeypot on Kali Linux
- Simulate attacker SSH access and log activities
- Analyze captured data using Python and visualize patterns
- Build a dashboard using Streamlit for real-time monitoring
- Enhance honeypot capabilities for attacker classification and intelligence

---

### 3. Environment Setup

- **Host System:** macOS
- **Virtualization Software:**
    - Kali Linux (Virtual Box)
- **Networking Mode:** Connect to the VM using a Bridged Adapter
- **How to Set Bridged Mode in VirtualBox**
    1. Open VirtualBox.
    2. Select the Kali Linux VM.
    3. Go to **Settings → Network**.
    4. Set **Adapter 1 → Attached to: Bridged Adapter**.
    5. Choose the correct network interface (Wi-Fi/Ethernet) from the dropdown.
- **Assigning Static IP in Kali:**
Edit the file `/etc/network/interfaces`:

```
auto eth0
iface eth0 inet static
  address 192.168.1.231
  netmask 255.255.255.0
  gateway 192.168.1.1
```

Then run:

```
sudo systemctl restart networking
```

---

### 4. Cowrie Honeypot Installation and Configuration

**Cowrie Setup Steps:**

```jsx
sudo apt update && sudo apt install git python3 python3-venv python3-pip libssl-dev libffi-dev build-essential libpython3-dev libxslt1-dev libmariadb-dev libxml2-dev zlib1g-dev -y 
cd ~
git clone https://github.com/cowrie/cowrie.git
cd cowrie

```

```
python3 -m venv cowrie-env

source cowrie-env/bin/activate

pip install --upgrade pip

pip install -r requirements.txt

cp etc/cowrie.cfg.dist etc/cowrie.cfg
```

![](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/Screenshot_2025-11-17_at_11.35.45_AM.png)

check status:

```
./bin/cowrie status
```

![image alt](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/00eba0be-de37-4b88-ada0-906720ccf369.png)

**Configuration Changes in `cowrie.cfg` & Creating userdb.txt password file:**

![image alt](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/Screenshot_2025-11-17_at_11.18.32_AM.png)

- Open up port and Redirect all the request of port 2222 to the cowrie server.

```
[ssh]
enabled = true
listen_port = 2222
auth_class = cowrie.core.auth.UserDB
userdb_file = /home/krishna/cowrie/etc/userdb.txt
```

the above configurations file (cowrie.cfg) also says that, user will only enter into the cowrie server when his/her credentials matches with the credentials that are there in database(userdb.txt).

![image alt](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/Screenshot_2025-11-17_at_11.28.13_AM.png)

- userdb.txt is file that has various vulnerable passwords, which has potential to let any user enter into the server very easily.

**Created userdb.txt file:**

```
root:toor
admin:admin
user:1234
```

start the Cowrie to listen from the attackers:

```
cd ~/cowrie
source cowrie-env/bin/activate
./bin/cowrie start
```

Cowrie now listens on **port 2222** for SSH…

---

### 5. Simulated Attacks

- SSH login attempts were made from the host Mac using:
    
    ```
    ssh root@192.168.1.231 -p 2222
    ```
    
- Commands like `ls`, `whoami`, `cat /etc/passwd` were entered
- Cowrie captured:
    - Login credentials
    - Source IP
    - Command history

![image alt](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/Screenshot_2025-05-25_at_12.44.59_PM.png)

- Informatin Gathering
    
    ![image alt](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/image.png)
    

Exploitation : SSH login attempt

![image alt](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/Screenshot_2025-05-25_at_12.43.42_PM.png)

---

6. Log Collection and Parsing

- Log file location: `~/cowrie/var/log/cowrie/cowrie.json`
- Python script `cowrie_log_analysis.py` created to:
    - Parse JSON log data
    - Extract fields: timestamp, src_ip, username, password, input
    - Visualize:
        - Top source IPs
        - Common usernames
        - Frequent commands
    
    ![image alt](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/Screenshot_2025-11-17_at_11.48.01_AM.png)
    

---

### 7. Streamlit Dashboard

- Streamlit used to build a live dashboard
- Dashboard includes:
    - Bar charts for top IPs, usernames, commands
    - Auto-refresh toggle
    - CSV download option (to be added)

Command to launch dashboard:

```
streamlit run cowrie_dashboard.py --server.address=0.0.0.0
```

Accessible via:

```
http://192.168.1.231:8501
```

---

Built an **interactive dashboard** using Streamlit.

Visualizations included:

- Bar charts for IPs, usernames
- Commands Used

![image alt](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/image%201.png)

![image alt](https://github.com/Rushank4593/assets-.gitkeep/blob/084226b666e4ca0d6512394a2c6f2e1ae8a1ab1e/image%202.png)

---

### 9. Next Phase Enhancements (Planned)

- Add GeoIP lookup to determine attacker location
- Classify attacker type using AI- behavioural rules
- Integrate IP reputation checks (e.g., VirusTotal, AbuseIPDB)
- Display risk scores and threat levels in dashboard

---

### 10. Conclusion

This project successfully simulated real-world SSH attacks in a lab, captured detailed attacker data, and visualized patterns through a custom dashboard. It lays a strong foundation for extending toward automated threat intelligence and machine learning-based attacker classification.
