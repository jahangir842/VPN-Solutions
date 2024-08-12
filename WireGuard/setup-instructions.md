Installing WireGuard on Ubuntu is straightforward. WireGuard is a simple and efficient VPN that runs inside the Linux kernel, making it both secure and fast.

**Official Installation :** https://www.wireguard.com/quickstart/

Here’s a step-by-step guide to installing WireGuard on Ubuntu:

### Step 1: Update Your System
First, update your system packages to ensure everything is up-to-date.

```bash
sudo apt update
sudo apt upgrade -y
```

### Step 2: Install WireGuard
WireGuard is included in the default Ubuntu repositories, so you can install it using `apt`.

```bash
sudo apt install wireguard -y
```

### Step 3: Generate Key Pairs
WireGuard requires a private and public key for both the server and client.

1. **Create a directory to store the keys**:

   ```bash
   sudo mkdir -p /etc/wireguard
   sudo chmod 700 /etc/wireguard
   ```

2. **Generate the private key**:

   ```bash
   sudo wg genkey | sudo tee /etc/wireguard/privatekey | sudo chmod 600 /etc/wireguard/privatekey
   ```

3. **Generate the public key** from the private key:

   ```bash
   sudo cat /etc/wireguard/privatekey | sudo wg pubkey | sudo tee /etc/wireguard/publickey
   ```

### Step 4: Configure WireGuard
Create a configuration file for WireGuard.

1. **Create a configuration file** for the interface, typically `wg0.conf`:

   ```bash
   sudo nano /etc/wireguard/wg0.conf
   ```

2. **Add the following configuration** to the file:

   ```ini
   [Interface]
   PrivateKey = YOUR_PRIVATE_KEY
   Address = 10.0.0.1/24
   ListenPort = 51820
   SaveConfig = true

   [Peer]
   PublicKey = CLIENT_PUBLIC_KEY
   AllowedIPs = 10.0.0.2/32
   ```

   - Replace `YOUR_PRIVATE_KEY` with the contents of `/etc/wireguard/privatekey`.
   - `Address` is the IP address of the WireGuard interface.
   - `ListenPort` is the port that WireGuard will listen on.
   - `CLIENT_PUBLIC_KEY` is the public key of the client that will connect to the server.
   - `AllowedIPs` specifies which IPs are allowed to route through the VPN.

### Step 5: Enable IP Forwarding
To allow traffic to route through the VPN, enable IP forwarding.

1. **Edit the sysctl configuration file**:

   ```bash
   sudo nano /etc/sysctl.conf
   ```

2. **Uncomment the following line**:

   ```ini
   net.ipv4.ip_forward=1
   ```

3. **Apply the changes**:

   ```bash
   sudo sysctl -p
   ```

### Step 6: Start and Enable WireGuard
Start the WireGuard service and enable it to start on boot.

```bash
sudo systemctl start wg-quick@wg0
sudo systemctl enable wg-quick@wg0
```

### Step 7: Set Up Firewall (Optional)
If you are using `ufw` (Uncomplicated Firewall), you might need to allow the WireGuard port.

```bash
sudo ufw allow 51820/udp
```

### Step 8: Connect a Client
To connect a client, you will need to configure the client with the server’s public key, the server’s IP address, and the client’s private key. The client configuration might look like this:

```ini
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.0.0.2/24

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = SERVER_IP:51820
AllowedIPs = 0.0.0.0/0
```

### Step 9: Test the Connection
After setting up both the server and the client, test the connection to ensure everything is working correctly.

```bash
sudo wg show
```

This command will show the status of the WireGuard interface and the connected peers.

That's it! WireGuard should now be installed and running on your Ubuntu system.
