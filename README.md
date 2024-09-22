# Xian

ABCI application to be used with CometBFT 0.38.8

## Installation

### Ubuntu 22.04

Follow these steps to set up the environment on Ubuntu 22.04:

1. **Update and prepare the system:**

   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   ```

2. **Install necessary packages:**

   ```bash
   sudo apt-get install pkg-config libhdf5-dev build-essential
   ```

3. **Install Python 3.11.8**

   Use your preferred way of installing this.

4. **Clone Xian repository:**

   ```bash
   git clone https://github.com/Endogen/xian-core.git
   ```

5. **Set up Python virtual environment and dependencies:**

   ```bash
   python3.11 -m venv xian-venv
   source xian-venv/bin/activate
   pip install -e src/contracting/ -e .
   ```

6. **Download, unpack, and initialize CometBFT:**

   ```bash
   wget https://github.com/cometbft/cometbft/releases/download/v0.38.8/cometbft_0.38.8_linux_amd64.tar.gz
   tar -xf cometbft_0.38.8_linux_amd64.tar.gz
   rm cometbft_0.38.8_linux_amd64.tar.gz
   ./cometbft init
   ```

7. **Configuring your node:**

   - **For starting your own local network:**

     ```bash
     python src/xian/tools/configure.py --moniker "Node" --copy-genesis --service-node --genesis-file-name "genesis.json" --validator-privkey "cd6cc45ffe7cebf09c6c6025575d50bb42c6c70c07e1dbc5150aaadc98705c2b"
     ```

     The `--validator-privkey` flag should be set to your validator's private key. The example above uses a key from the genesis file for testing purposes so you can start developing directly. `--moniker` is the node name in the CometBFT network.

   - **For joining an existing network:**

     ```bash
     python src/xian/tools/configure.py --moniker "Node" --copy-genesis --genesis-file-name "genesis-testnet.json" --seed-node "91.108.112.184" --validator-privkey "ENTER YOUR WALLET PRIVATE KEY HERE"
     ```

     Use `--seed-node` to specify the seed node's IP address you want to connect to. `--validator-privkey` is your validator wallet's private key. Ensure ports 26657 (REST API), 26656 (Node Communication), and 26660 (Prometheus Metrics) are open on your firewall.

   - **Other arguments**
      - `--prometheus` Enables export of prometheus metrics
      - `--service-node` Enables service_node mode for features such as Stamp Calculation and more.
      - `--enable-pruning` Prune blocks. Related to "blocks-to-keep" value
      - `--blocks-to-keep` Number of blocks to keep. Related to "enable-pruning" value
      - `--allow-cors` Allow CORS
      - `--snapshot-url` URL of snapshot in tar.gz format
      - `--seed-node-address` Seed node address e.g. <node_id>@91.108.112.184 . For cold booting a test network.

8. **Run CometBFT and Xian:**

   Install dependencies

   ```bash
   sudo apt-get install npm
   npm install pm2 -g
   make up # To start
   make down # To stop
   ```
   
    Run node

    Inside `xian-core` directory run:    

   ```
   make up-bds
   ```
   
    Stop node

   ```
   make down
   ```

#### Accessing the Application

CometBFT RPC is exposed on `localhost:26657`.