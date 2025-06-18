Blood Bank File Integrity System
Overview
The Blood Bank File Integrity System is a decentralized application (dApp) designed to ensure the immutability and verifiable integrity of sensitive blood bank records. By combining the power of Ethereum blockchain for immutable record-keeping and IPFS for decentralized file storage, this system provides a robust solution to prevent unauthorized tampering and verify the authenticity of critical healthcare data.

Key Features:
Decentralized Storage: Files are stored on the InterPlanetary File System (IPFS), offering content-addressable, distributed, and censorship-resistant storage.

Blockchain-Anchored Integrity: Cryptographic hashes (SHA256) and IPFS Content Identifiers (CIDs) of the files are immutably recorded on the Ethereum blockchain via a smart contract.

Tamper Detection: Any modification to an uploaded file can be immediately detected by re-hashing the file and comparing it against the original hash stored on the blockchain.

Metadata Association: Records can be uploaded with relevant metadata (e.g., Donor ID, Blood Type) for better organization and searchability.

User-Friendly Interface: A simple and intuitive web interface for uploading files, verifying integrity, and viewing stored records.

Dark Mode: A toggle for a comfortable dark mode viewing experience.

Technologies Used
Frontend: HTML, CSS, JavaScript

Backend: Python (Flask, Web3.py, ipfshttpclient, hashlib, uuid)

Blockchain: Ethereum (Solidity Smart Contracts)

Development Tools: Hardhat (for Ethereum development), IPFS Daemon

Setup Guide
Follow these steps to set up and run the Blood Bank File Integrity System locally.

1. Prerequisites
Before you begin, ensure you have the following installed:

Node.js and npm: Download from nodejs.org. Required for Hardhat.

Python 3.8+ and pip: Download from python.org.

IPFS Desktop or IPFS CLI: You need an IPFS daemon running.

IPFS Desktop (Recommended for ease of use): Download from ipfs.io/downloads/. It provides a user-friendly interface and automatically runs the daemon.

IPFS CLI: Install via instructions on docs.ipfs.tech/install/.

2. Project Initialization
Create a new directory for your project and navigate into it:

mkdir blood-bank-dapp
cd blood-bank-dapp

3. Hardhat (Ethereum Smart Contract) Setup
This section sets up your local Ethereum development environment and deploys the smart contract.

a.  Initialize Hardhat Project:

```bash
npm init -y
npm install --save-dev hardhat
npx hardhat
```

When prompted by `npx hardhat`, select `Create a basic sample project`. This will set up the necessary Hardhat project structure.

b.  Create Smart Contract File:

Create a new file `contracts/FileIntegrity.sol` inside your Hardhat project (`blood-bank-dapp/contracts/FileIntegrity.sol`). Paste the Solidity code for `FileIntegrity.sol` (provided in the detailed report above) into this file.

c.  Compile the Smart Contract:

This command compiles your Solidity code and generates the ABI (Application Binary Interface) and bytecode, which your Python backend needs.

```bash
npx hardhat compile
```

The ABI will be located at `artifacts/contracts/FileIntegrity.sol/FileIntegrity.json`.

d.  Create Deployment Script:

Create a new file `scripts/deploy.js` inside your Hardhat project (`blood-bank-dapp/scripts/deploy.js`). Paste the following JavaScript code into it:

```javascript
const { ethers } = require("hardhat");

async function main() {
  const [deployer] = await ethers.getSigners();
  console.log("Deploying contracts with the account:", deployer.address);

  const FileIntegrity = await ethers.getContractFactory("FileIntegrity");
  const fileIntegrity = await FileIntegrity.deploy();

  console.log("FileIntegrity deployed to address:", fileIntegrity.address);
  // *** IMPORTANT: COPY THIS ADDRESS ***
  // You will need to paste this address into the 'CONTRACT_ADDRESS' variable in your 'app.py' file.
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```

4. Python Backend Setup
This section sets up the Python Flask server that will interact with the blockchain and IPFS.

a.  Create Python Virtual Environment:

It's highly recommended to use a virtual environment to manage dependencies:

```bash
python -m venv venv
# On macOS/Linux:
source venv/bin/activate
# On Windows:
venv\Scripts\activate
```

b.  Install Python Dependencies:

Install the necessary Python libraries:

```bash
pip install Flask flask-cors web3 pycryptodome ipfshttpclient
```

c.  Create Backend File:

Create a new file `app.py` in your main `blood-bank-dapp` directory (`blood-bank-dapp/app.py`). Paste the Python Flask backend code for `app.py` (provided in the detailed report above) into this file.

d.  Update Contract Address in app.py:

After you've deployed your smart contract (in step 5b below), you *must* copy the deployed contract address and update the `CONTRACT_ADDRESS` variable in your `app.py` file with that address.

```python
# app.py
CONTRACT_ADDRESS = os.getenv('CONTRACT_ADDRESS', 'YOUR_DEPLOYED_CONTRACT_ADDRESS_HERE') # <-- Update this line
```

5. Frontend (HTML) Setup
This section sets up the user interface.

a.  Create Frontend File:

Create a new file `hema.html` in your main `blood-bank-dapp` directory (`blood-bank-dapp/hema.html`). Paste the HTML frontend code for `hema.html` (provided in the detailed report above) into this file.

Running the Application
Follow these steps precisely. The order of starting services is crucial.

Start Hardhat Local Blockchain Node: Open your first terminal. Navigate to your blood-bank-dapp directory and run:

npx hardhat node

Keep this terminal open. It will print blockchain transaction details.

Deploy Smart Contract:
Open your second terminal. Navigate to your blood-bank-dapp directory and run:

npx hardhat run scripts/deploy.js --network localhost

IMPORTANT: After deployment, you will see FileIntegrity deployed to address: <YOUR_CONTRACT_ADDRESS>. Copy this address immediately.

Go back to your app.py file, find the line CONTRACT_ADDRESS = os.getenv('CONTRACT_ADDRESS', 'YOUR_CONTRACT_ADDRESS_HERE'), and replace 'YOUR_CONTRACT_ADDRESS_HERE' with the address you copied. Save app.py.

Start IPFS Daemon:
Open your third terminal.

If using IPFS Desktop, simply launch the application. The daemon will start automatically.

If using IPFS CLI, navigate to any directory (doesn't have to be blood-bank-dapp) and run:

ipfs daemon

Keep this terminal open. It will show IPFS logs.

Run Python Backend Server:
Open your fourth terminal. Navigate to your blood-bank-dapp directory.
If you created a virtual environment, activate it:

# On macOS/Linux:
source venv/bin/activate
# On Windows:
venv\Scripts\activate

Then run the Flask application:

python app.py

Keep this terminal open. It will show Flask server logs.

Open the Frontend in Your Browser:
Finally, open the hema.html file in your web browser. You can typically do this by double-clicking the file in your file explorer, or by dragging it into your browser, or by using File > Open File in your browser.

The application should load, fetch any existing records, and be ready for you to upload and verify files!

Usage
Upload Blood Bank Record:

Select a file using the "Choose File" button.

Enter relevant metadata (e.g., "Donor ID: D123, Blood Type: O+").

Click "Upload Record". An alert will confirm the upload and the record will appear in the "Stored Records" table.

Verify File Integrity:

Select the file you want to verify (it can be the original file or a suspected altered copy).

Click "Verify File".

The "Verify Result" section will indicate if the file's integrity is intact (hash matches a record on the blockchain) and display its details.

View Stored Records:

The "Stored Records" table automatically updates with all files that have been successfully uploaded and recorded on the blockchain. You can click on the IPFS CID to view the content directly on the IPFS gateway.

Smart Contract Details
The FileIntegrity.sol smart contract provides the core logic for interacting with the blockchain. It stores file hashes, IPFS CIDs, and associated metadata, ensuring that once a record is on the blockchain, it cannot be tampered with without detection.

uploadFile(string _fileId, string _fileName, string _fileHash, string _ipfsCid, string _metadata): Records file details on the blockchain.

verifyFile(string _fileId, string _providedFileHash): Checks if a given hash matches a stored hash for a specific file ID.

getFileDetails(string _fileId): Retrieves all recorded details for a file.

getAllFileIds(): Returns a list of all unique file IDs stored.

Contribution
Contributions are welcome! If you have suggestions for improvements or encounter issues, please feel free to open a pull request or submit an issue on the project's repository (if applicable).

License
This project is open-source and available under the MIT License.