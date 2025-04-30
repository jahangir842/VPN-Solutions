Great — let’s now cover the **client-side setup** for your WireGuard VPN. This can be done on:

- 🐧 **Linux client (like Ubuntu)**
- 💻 **Windows/macOS**
- 📱 **Android/iOS**

I'll walk you through the **Linux client** first (like Ubuntu), and briefly touch on mobile/desktop later.

---

## ✅ Client-Side Setup (Linux/Ubuntu)

Assume:
- Server IP = `YOUR_SERVER_IP`
- Server port = `51820`
- VPN subnet = `10.0.0.0/24`
- Client IP = `10.0.0.2`
- You have:
  - `client_private.key`
  - `server_public.key`
  - `YOUR_SERVER_IP`

---

### 🔧 STEP 1: Install WireGuard

```bash
sudo apt update
sudo apt install wireguard
```

---

### 🔐 STEP 2: Create Client Key Pair (if not already done on server)

```bash
umask 077
wg genkey | tee client_private.key | wg pubkey > client_public.key
```

Then share the `client_public.key` with the server to add to its `/etc/wireguard/wg0.conf`.

---

### 🧾 STEP 3: Create Client Config

Create a config file:

```bash
sudo nano /etc/wireguard/wg0.conf
```

Paste:

```ini
[Interface]
PrivateKey = <client_private_key>
Address = 10.0.0.2/24
DNS = 1.1.1.1

[Peer]
PublicKey = <server_public_key>
Endpoint = YOUR_SERVER_IP:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

Replace:
- `<client_private_key>` = from `client_private.key`
- `<server_public_key>` = from `server_public.key`
- `YOUR_SERVER_IP` = your server's public IP or domain

Save and exit.

---

### 🔓 STEP 4: Start VPN

```bash
sudo wg-quick up wg0
```

Check status:
```bash
sudo wg
```

---

### 🧪 STEP 5: Test

- Run:
  ```bash
  curl ifconfig.me
  ```
  It should return your **server's IP** — meaning traffic is routed through the VPN.

---

## 🧹 To Stop the VPN

```bash
sudo wg-quick down wg0
```

---

## 📱 Mobile or 💻 Desktop Clients

### 📱 **Android/iOS**:
1. Install **WireGuard app**
2. Create config:
   - Use same `[Interface]` and `[Peer]` as above.
3. Add manually or **scan a QR code** (`qrencode` can generate it on server).

### 💻 **Windows/macOS**:
1. Download official [WireGuard client](https://www.wireguard.com/install/)
2. Import the same config
3. Start the tunnel with 1 click.

---

Would you like me to generate the QR code or a pre-filled config file for your mobile/desktop client?
