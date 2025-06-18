Blood Bank File Integrity System - Local Setup Guide
This guide provides step-by-step commands to set up and run the Blood Bank File Integrity System locally on your Windows machine. Follow the instructions carefully and in order.

IMPORTANT PRE-CHECK:

Free Up C: Drive Space: Ensure you have at least 5-10 GB of free space on your C: drive. Node.js (npm) uses temporary cache directories on C: drive, which can fill up and cause installation failures even if your project is on D: drive.

Install Node.js & npm: Download and install the LTS version from nodejs.org. Ensure it's added to your system's PATH during installation. Verify by opening a new Command Prompt and typing node -v and npm -v.

Install Python 3.8+ & pip: Download from python.org. Crucially, check "Add Python to PATH" during installation. Verify with python --version and pip --version.

Install IPFS Desktop: Download and install from ipfs.io/downloads/. This is the easiest way to run the IPFS daemon. Ensure it's running before starting the Python backend.

1. Prepare Your Project Directory
Navigate to your project directory using Command Prompt (CMD) or PowerShell. All commands below should be run from this directory:

D:
cd \blood-bank-dapp

2. Hardhat (Ethereum Smart Contract) Setup
This sets up your local blockchain development environment.

a.  Initialize Node.js Project:
(This creates package.json)

```cmd
npm init -y
```

b.  Install Hardhat:

```cmd
npm install --save-dev hardhat
```

c.  Initialize Hardhat Project Configuration:
This command will prompt you.

```cmd
npx hardhat
```
**Manual Step:** When prompted by Hardhat, use your arrow keys to select `Create a JavaScript project` and press Enter.

d.  Create Smart Contract File:
Create a new file contracts\FileIntegrity.sol inside your D:\blood-bank-dapp directory.
Paste the Solidity code for FileIntegrity.sol (from the previous response's immersive code block) into this file.

e.  Compile the Smart Contract:
This generates the ABI needed by your Python backend.

```cmd
npx hardhat compile
```

f.  Create Deployment Script:
Create a new file scripts\deploy.js inside your D:\blood-bank-dapp directory.
Paste the JavaScript code for deploy.js (from the previous response's immersive code block in the "Hardhat (Ethereum Smart Contract) Setup" section) into this file.

3. Python Backend Setup
This section sets up the Flask server that will interact with the blockchain and IPFS.

a.  Create Python Virtual Environment:
(Highly recommended for dependency management)

```cmd
python -m venv venv
```

b.  Activate Virtual Environment:

```cmd
.\venv\Scripts\activate
```

c.  Install Python Dependencies:

```cmd
pip install Flask flask-cors web3 pycryptodome ipfshttpclient
```

d.  Create Backend File:
Create a new file app.py in your D:\blood-bank-dapp directory.
Paste the Python Flask backend code for app.py (from the previous response's immersive code block) into this file.

4. HTML Frontend Setup
a.  Create Frontend File:
Create a new file hema.html in your D:\blood-bank-dapp directory.
Paste the HTML frontend code for hema.html (from the previous response's immersive code block) into this file.

5. Running the Application (Crucial Order!)
You will need 4 separate Command Prompt/PowerShell windows open simultaneously.

a.  Window 1: Start Hardhat Local Blockchain Node
Open a new Command Prompt/PowerShell window.
Navigate to D:\blood-bank-dapp:
cmd D: cd \blood-bank-dapp 
Run the Hardhat node:
cmd npx hardhat node 
Keep this window open.

b.  Window 2: Deploy Smart Contract
Open another new Command Prompt/PowerShell window.
Navigate to D:\blood-bank-dapp:
cmd D: cd \blood-bank-dapp 
Deploy the contract:
cmd npx hardhat run scripts/deploy.js --network localhost 
Manual Step: Look for the output FileIntegrity deployed to address: <YOUR_CONTRACT_ADDRESS>. Copy this address.
* Open your app.py file.
* Find the line CONTRACT_ADDRESS = os.getenv('CONTRACT_ADDRESS', '0x5FbDB2315678afecb367f032d93F642f64180aa3').
* Replace the example address (0x5FbDB2315678...) with the address you just copied from the deployment output.
* Save app.py.

c.  Window 3: Start IPFS Daemon
Ensure IPFS Desktop is running, or open another new Command Prompt/PowerShell window and run the IPFS daemon:
cmd ipfs daemon 
Keep this window open.

d.  Window 4: Run Python Backend Server
Open a new Command Prompt/PowerShell window.
Navigate to D:\blood-bank-dapp:
cmd D: cd \blood-bank-dapp 
Activate your Python virtual environment:
cmd .\venv\Scripts\activate 
Run the Flask application:
cmd python app.py 
Keep this window open.

e.  Open the Frontend in Your Browser:
Finally, open the hema.html file in your web browser. You can usually do this by double-clicking the file in your D:\blood-bank-dapp folder, or by dragging it into your browser.

Your Blood Bank File Integrity System should now be fully operational! You can proceed to upload and verify files.
