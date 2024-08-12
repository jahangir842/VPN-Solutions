Recommended:   

New Repository: https://github.com/wg-easy/wg-easy            (application is not password enabled)
Old Repository: https://hub.docker.com/r/weejewel/wg-easy     (application is password enabled)



Here’s a detailed procedure on how to install **WG-Easy** using Docker Compose. WG-Easy is a user-friendly web interface for managing WireGuard VPN servers, which simplifies the setup and management process.

### Step 1: Install Docker and Docker Compose

#### 1.1. Update Your System
First, ensure your system is up-to-date:
```bash
sudo apt-get update
sudo apt-get upgrade -y
```

#### 1.2. Install Docker
Install Docker by running the following commands:
```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
Check if Docker is installed correctly:
```bash
docker --version
```

#### 1.3. Install Docker Compose
Next, install Docker Compose:
```bash
sudo apt-get install docker-compose -y
```
Verify the installation:
```bash
docker-compose --version
```

### Step 2: Set Up WG-Easy with Docker Compose

#### 2.1. Create a Directory for WG-Easy
Create a directory where your Docker Compose configuration and WG-Easy files will be stored:
```bash
mkdir wg-easy
cd wg-easy
```

#### 2.2. Create a Docker Compose File
Create a `docker-compose.yml` file inside the `wg-easy` directory:
```bash
nano docker-compose.yml
```

Use the docker-compose.yml file provided in this repository


- **WG_HOST**: Replace with your public IP address or domain.
- **PASSWORD**: Set a secure password for accessing the WG-Easy web interface.

#### 2.3. Configure Firewall (Optional)
If you are using a firewall, ensure that ports `51820/udp` and `51821/tcp` are open:
```bash
sudo ufw allow 51820/udp
sudo ufw allow 51821/tcp
```

### Step 3: Launch WG-Easy

#### 3.1. Start WG-Easy
Run the following command to start WG-Easy with Docker Compose:
```bash
sudo docker-compose up -d
```
This will download the WG-Easy image, create the container, and start the service.

#### 3.2. Verify the Installation
Check if the WG-Easy container is running:
```bash
sudo docker ps
```
You should see `wg-easy` in the list of running containers.

### Step 4: Access the WG-Easy Web Interface

- Open a web browser and navigate to `http://your.domain.com:51821` (replace `your.domain.com` with your domain or IP address).
- Log in using the password you set in the `docker-compose.yml` file.

### Step 5: Manage Your WireGuard VPN

Now that WG-Easy is up and running, you can manage your WireGuard VPN through the web interface. You can create, delete, and manage WireGuard clients easily through this interface.

### Step 6: (Optional) Update WG-Easy

To update WG-Easy to the latest version, simply pull the latest Docker image and restart the container:
```bash
sudo docker-compose pull
sudo docker-compose up -d
```

### Step 7: (Optional) Enable Auto-Start on Boot

To ensure that WG-Easy starts automatically on system boot, enable Docker Compose services to start on boot:
```bash
sudo systemctl enable docker
```

### Troubleshooting Tips
- **Port Conflicts**: Ensure that the ports `51820/udp` and `51821/tcp` are not in use by other services.
- **Firewall**: Double-check firewall settings if the WG-Easy web interface or VPN connections are not accessible.
- **Logs**: Check Docker logs for any errors:
  ```bash
  sudo docker logs wg-easy
  ```

This guide should help you successfully install and manage WG-Easy using Docker Compose.
