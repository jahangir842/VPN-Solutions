Perfect! Let's walk through **Option 2: Setting up your own VPN server using WireGuard** on **Ubuntu (Linux)**. WireGuard is modern, fast, secure, and relatively easy to set up.

---

## ✅ Step-by-Step: Set Up Your Own WireGuard VPN on Ubuntu

> 🛡️ This guide assumes:
> - You have **root/sudo** access to an Ubuntu machine (e.g., cloud VM or home server).
> - You want to configure both **server and a client** (like your laptop or phone).

---

### 🔧 STEP 1: Install WireGuard
```bash
sudo apt update
sudo apt install wireguard
```

---

### 🔐 STEP 2: Generate Keys

```bash
umask 077
wg genkey | tee server_private.key | wg pubkey > server_public.key
```

Save these two:
- `server_private.key` → used in server config
- `server_public.key` → shared with clients

---

### 🧾 STEP 3: Create Server Configuration

Create a new file:
```bash
sudo nano /etc/wireguard/wg0.conf
```

Paste this config (replace `<private-key>` later):

```ini
[Interface]
Address = 10.0.0.1/24
ListenPort = 51820
PrivateKey = <server_private_key>
SaveConfig = true

# Add your clients here as [Peer]
```

Now insert the private key:
```bash
SERVER_PRIVATE=$(cat server_private.key)
sudo sed -i "s|<server_private_key>|$SERVER_PRIVATE|" /etc/wireguard/wg0.conf
```

---

### 🌐 STEP 4: Enable IP Forwarding

```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

---

### 🔁 STEP 5: Set Up NAT (Outgoing traffic)

Assuming your main interface is `eth0` (check with `ip a`):
```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

Make it persistent (optional):
```bash
sudo apt install iptables-persistent
sudo netfilter-persistent save
```

---

### ▶️ STEP 6: Start WireGuard

```bash
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
```

Check status:
```bash
sudo wg show
```

---

### 📱 STEP 7: Add a Client (Laptop, Mobile, etc.)

On server:
```bash
wg genkey | tee client_private.key | wg pubkey > client_public.key
```

Then append this to `/etc/wireguard/wg0.conf`:
```ini
[Peer]
PublicKey = <client_public_key>
AllowedIPs = 10.0.0.2/32
```

On client, config would be:
```ini
[Interface]
PrivateKey = <client_private_key>
Address = 10.0.0.2/24
DNS = 1.1.1.1

[Peer]
PublicKey = <server_public_key>
Endpoint = <your_server_public_ip>:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

---

### 🧪 STEP 8: Connect and Test

On client (Linux):
```bash
sudo wg-quick up myvpn
```

Or use **WireGuard app** on Android/iOS — scan a QR code if needed.

---

## 🔐 Security Tips
- Restrict port `51820/udp` on your firewall to known IPs, or at least ensure it’s open:
  ```bash
  sudo ufw allow 51820/udp
  ```

- You can monitor connections with:
  ```bash
  sudo wg
  ```

---
