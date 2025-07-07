Raspberry Pi Private Cloud + NAS
This project sets up a private cloud and LAN-accessible NAS using a Raspberry Pi, with static IP setup, NAT configuration, Nginx server, and local storage handling.

🔧 Features
Hosted a personal portfolio using Nginx

Configured static IP & NAT to manage LAN traffic

Mounted external storage for file hosting

Accessible from any device on the same LAN

Base setup to later upgrade to OpenMediaVault or public hosting

📸 Project Architecture
(Insert image in docs/setup_diagram.png or a photo of the Pi)

⚙️ Tech Stack
Raspberry Pi 4 (or 3)

Raspbian OS

Nginx

iptables / NAT for network routing

External Storage (USB or SD card)

🛠 Setup Instructions
# 1. Clone this repo
git clone https://github.com/yourusername/raspberrypi-private-cloud.git

# 2. Copy nginx config
sudo cp nginx/default.conf /etc/nginx/sites-available/default

# 3. Set static IP (manual or script)
sudo nano /etc/dhcpcd.conf

# 4. Set up NAT rules
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# 5. Start Nginx
sudo systemctl restart nginx


🔒 Future Plans
Deploy with OpenMediaVault

Enable remote access via DuckDNS + VPN

Add UI for managing files

🧠 Author
Puneeth Kumar M — Engineering student, tech enthusiast, and passionate builder.

📜 License
MIT License

✅ Step 3: Push to GitHub
If your folder is ready:

bash
Copy
Edit
cd raspberrypi-private-cloud
git init
git add .
git commit -m "Initial commit - Private cloud using Raspberry Pi"
git branch -M main
git remote add origin https://github.com/yourusername/raspberrypi-private-cloud.git
git push -u origin main

⚠️ Make sure to replace yourusername with your actual GitHub username.