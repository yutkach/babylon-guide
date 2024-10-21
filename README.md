# babylon-guide

### **Prerequisites**

1. **Operating System**: Ubuntu 20.04+ or any Linux distro.
2. **System Requirements**: 
    - **Memory**: At least 4GB RAM.
    - **Disk Space**: Minimum 250GB SSD storage.
    - **CPU**: 2 CPU cores.
3. **Dependencies**: Ensure the following are installed:
    - Git
    - Docker and Docker Compose
    - Make
    - curl
    - jq

You can install dependencies with:

```bash
sudo apt update
sudo apt install -y git curl make jq
```

Install Docker and Docker Compose if not already installed:

```bash
# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

---

### **Step 1: Clone the BTC Staking Testnet Repository**

First, clone the Babylon BTC staking testnet repository:

```bash
git clone https://github.com/babylon/babylon-node-btc-staking-testnet
cd babylon-node-btc-staking-testnet
```

---

### **Step 2: Configure the Environment Variables**

Next, create a `.env` file with your custom configurations. This file will hold important environment variables.

```bash
cp .env.example .env
```

Open the `.env` file and modify the settings as needed, for example:

```bash
# Example configuration in the .env file
BTC_NODE_HOST=127.0.0.1
BTC_NODE_PORT=8332
BTC_RPC_USER=user
BTC_RPC_PASSWORD=password
```

Ensure you set up correct credentials for your BTC node.

---

### **Step 3: Build and Start the Docker Containers**

Once the environment is set up, you can build and start the Docker containers:

```bash
make build
make up
```

This command will build the Docker images and bring up the necessary containers for the BTC staking node.

---

### **Step 4: Verify the Node is Running**

To check if your node is running correctly, use the following command:

```bash
docker-compose ps
```

You should see a list of services running with their status.

---

### **Step 5: Access Node Logs**

You can check the logs of your running node to ensure everything is working correctly:

```bash
docker-compose logs -f
```

Look for any error messages and ensure the node is syncing with the Bitcoin testnet blockchain.

---

### **Step 6: Staking Node Configuration**

Once your BTC node is running, you need to configure the staking parameters. Open the staking configuration file `staking-config.yaml` and modify it based on your needs:

```bash
nano staking-config.yaml
```

An example configuration:

```yaml
# staking-config.yaml example
network: testnet
staking:
  enabled: true
  min_stake: 0.01 BTC
  reward_address: tb1qyourrewardaddress
```

Save the file after making changes.

---

### **Step 7: Monitoring and Staking**

After configuration, restart the node to apply the new settings:

```bash
make restart
```

You can monitor the staking process using:

```bash
docker-compose logs -f staking
```

Ensure the staking node is actively participating in the testnet.

---

### **Step 8: Staking Rewards**

Rewards from staking will be sent to the address you configured in `staking-config.yaml`. To check the balance or transactions:

```bash
docker exec -it btc-node bitcoin-cli -testnet getbalance
```

This command will return the current balance of your staking wallet.

---

### **Step 9: Troubleshooting**

If you encounter issues, here are some common troubleshooting steps:

- **Node Not Syncing**: Ensure your firewall or security group allows traffic on the Bitcoin testnet ports (8332, 18333).
- **Docker Errors**: Use `docker-compose logs` to investigate issues with specific services.
- **Configuration Issues**: Double-check your `.env` and `staking-config.yaml` files for errors.

---

### **Step 10: Shutting Down the Node**

To gracefully stop the node, use the following command:

```bash
make down
```

