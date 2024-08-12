Here’s how you can install Docker manually on Amazon Linux if `amazon-linux-extras` is not available:

### Step 1: Update Your System

First, update your package list:

```bash
sudo yum update -y
```

### Step 2: Install Docker

Install Docker from the official repositories:

```bash
sudo yum install docker -y
```

### Step 3: Start and Enable Docker

Start the Docker service:

```bash
sudo service docker start
```

Enable Docker to start at boot:

```bash
sudo chkconfig docker on
```

### Step 4: Add Your User to the Docker Group (Optional)

If you want to run Docker commands without using `sudo`, add your user to the Docker group:

```bash
sudo usermod -aG docker $USER
```

To apply the changes, log out and log back in, or run:

```bash
newgrp docker
```

### Step 5: Verify the Installation

Check if Docker is installed correctly by running:

```bash
docker --version
```

You can also test Docker by running a test container:

```bash
docker run hello-world
```

This command will download a test image and run it in a container. If Docker is working correctly, you'll see a "Hello from Docker!" message.
