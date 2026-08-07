🛠️ VPS Bot Management System: Operations Guide
,
Phase 1: Prerequisites
Before running the script, ensure you have:
 * A fresh Ubuntu (20.04/22.04/24.04) or Debian (11/12) server.
 * Root access (Run sudo -i before starting).
 * A Telegram Bot Token (Get this from @BotFather).
 * Your Admin Telegram ID (Get this from @userinfobot).
 
Phase 2: Master Node Setup
The Master node will host the Telegram bot controller and SQLite database.
 * Execute the Script: Run your bash script (./bot_vps.sh).
 * Select Option 1 (Install Master):
   * Enter your Telegram Bot Token and Admin ID when prompted.
   * The script will generate a secure master_... API key. Save this key in a secure password manager.
   * Choose Option 1 to proceed with the generated key.
 * Completion: The script will download the Go binary, download all 18 control scripts, set up the Python environment, and start the vps-master systemd service.
 
Phase 3: Registering a Slave (IP VPS Manager)
Before setting up the Slave server, you need to register its IP in the Master's database to generate a connection key.
 * On the Master server, run the bash script and select Option 4 (IP VPS Manager).
 * Select 1 (Add/Register New VPS IP).
 * Enter the details for your new Slave:
   * Node Name: (e.g., VPS-SG-01)
   * VPS Public IP: The IP of the Slave server.
   * Port: Default is 8000.
 * The system will generate a node_... API Key. Copy this API key. You will need it right now for the Slave setup.
 
Phase 4: Slave Node Setup
Log into your Slave server via SSH.
 * Execute the Script: Run your bash script (./bot_vps.sh).
 * Select Option 2 (Install Slave):
 * API Key Configuration:
   * Select Option 2 (Enter existing API key from Master).
   * Paste the node_... API key you generated in Phase 3.
 * Network Configuration:
   * Port: 8000 (Must match what you entered in Phase 3).
   * Master VPS Public IP: Enter the IP address of your Master server. This is critical—the Slave's UFW firewall will lock down port 8000 so that ONLY the Master can communicate with it.
 * Completion: The script sets up the hardened API, downloads the helper scripts, and starts the vps-slave service.
 
🖥️ How the Bash Script Menu Works:

 * 1) Install Master: Deploys the main Telegram bot controller (bot-master) and database.
 
 * 2) Install Slave: Deploys the remote agent (bot-slave) and locks down the firewall to accept commands only from the Master.
 
 * 3) Status Service: A diagnostic tool. It checks if your vps-master, vps-slave, nginx, ufw, and fail2ban services are active and running.
 
 * 4) IP VPS Manager: The built-in SQLite database manager (Run this on the Master).
   * Add new Slaves and generate their authentication keys.
   * Delete old/offline Slaves from the database.
   
 * 5) API Key Generator: A standalone cryptographic utility to generate secure, 15-character random keys with custom prefixes (e.g., prod_, dev_). Useful if you ever need to manually rotate compromised keys.
 
 * 6) Uninstall: A clean wipe tool. It stops services, deletes the vpsbot user, removes firewall rules, and deletes all files in /opt/vps-bot and /etc/vps-bot.
