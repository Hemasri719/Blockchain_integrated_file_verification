Detailed Project Report: Blood Bank File Integrity System
1. Introduction
The Blood Bank File Integrity System is a decentralized application (dApp) designed to enhance the security and trustworthiness of sensitive blood bank records. In critical sectors like healthcare, ensuring the immutability and integrity of data is paramount. This project addresses this need by leveraging blockchain technology (Ethereum) and a decentralized file storage system (IPFS) to create a transparent, tamper-proof, and verifiable record-keeping solution for blood bank data.

The system allows authorized personnel to upload digital blood bank records, which are then hashed and stored on IPFS, with their unique identifiers and cryptographic hashes permanently recorded on the Ethereum blockchain. This setup ensures that once a record is registered, any subsequent alteration to the original file can be immediately detected, providing a robust mechanism for maintaining data integrity and auditability.

2. Technologies Used
This project integrates several cutting-edge technologies to achieve its decentralized and secure objectives:

Frontend: HTML, CSS, JavaScript (ES6+)

Styling: Custom CSS for a clean, responsive, and modern user interface, including a dark mode toggle.

Interactivity: JavaScript handles user interactions, form submissions, and asynchronous communication with the backend.

Backend: Python with Flask

Flask: A lightweight and flexible Python web framework for handling API requests.

Flask-CORS: Enables Cross-Origin Resource Sharing for seamless communication between the frontend and backend.

Web3.py: A Python library for interacting with the Ethereum blockchain, allowing the backend to send transactions and read contract data.

hashlib: Python's built-in library for cryptographic hashing (SHA256) of file contents.

uuid: For generating universally unique identifiers for files.

ipfshttpclient: A Python client library for interacting with the IPFS HTTP API to add and retrieve files from the IPFS network.

Blockchain: Ethereum (Smart Contracts with Solidity)

Solidity: The programming language used to write the FileIntegrity.sol smart contract.

Hardhat: An Ethereum development environment used for compiling, deploying, and testing the smart contract on a local blockchain network.

Decentralized Storage: IPFS (InterPlanetary File System)

A peer-to-peer network protocol for storing and sharing data in a distributed file system. It provides content-addressable storage, meaning data is retrieved by its content hash (CID), ensuring data integrity and uniqueness.

3. System Architecture
The project follows a standard dApp architecture comprising three main layers:

Frontend (User Interface):

An HTML page (hema.html) with embedded CSS and JavaScript.

Provides forms for uploading new blood bank records and verifying existing ones.

Displays a table of all recorded files, including their hashes, IPFS CIDs, and blockchain-recorded metadata.

Communicates with the Python backend via HTTP requests.

Backend (API Layer - Python Flask):

A Flask application (app.py) that acts as an intermediary between the frontend and the blockchain/IPFS.

API Endpoints:

/upload (POST): Receives a file and metadata, hashes the file, uploads it to IPFS, and then records the file's metadata, hash, and IPFS CID on the Ethereum blockchain.

/verify (POST): Receives a file, hashes it, and compares this hash against records stored on the blockchain to verify integrity.

/records (GET): Retrieves all file records (hashes, CIDs, metadata) from the blockchain and sends them to the frontend for display.

Blockchain Interaction: Uses web3.py to interact with the deployed FileIntegrity.sol smart contract, sending transactions (for uploads) and making call (read) requests (for verification and retrieving records).

IPFS Interaction: Uses ipfshttpclient to add files to the local IPFS daemon, obtaining their Content Identifiers (CIDs).

Blockchain (Ethereum Smart Contract - Solidity):

The FileIntegrity.sol smart contract is deployed on an Ethereum network (typically a local development network like Hardhat for testing).

FileRecord Struct: Defines the structure for each record, including fileName, fileHash, ipfsCid, uploader address, timestamp, and metadata.

fileRecords Mapping: Stores FileRecord structs, mapped by a unique fileId.

fileIds Array: Maintains a list of all fileIds for iterating through records.

uploadFile Function: A public function that records the file's details on the blockchain. This transaction is immutable and publicly verifiable.

verifyFile Function: A view function that compares a provided hash with a stored hash for a given file ID.

getFileDetails Function: A view function to retrieve all stored details for a specific file ID.

getAllFileIds Function: A view function to retrieve all existing file IDs.

IPFS (Decentralized Storage):

Acts as the content-addressable storage layer. When a file is uploaded, its actual content is stored on the IPFS network (via a local IPFS daemon).

The IPFS CID (Content Identifier) is then stored on the blockchain, creating an immutable link to the decentralized content.

4. Core Features
Decentralized File Storage: Records are stored on IPFS, ensuring data availability and resistance to censorship.

Blockchain-Anchored Integrity: Cryptographic hashes and IPFS CIDs of records are immutably stored on the Ethereum blockchain, making any tampering immediately detectable.

File Uploads with Metadata: Users can upload files along with custom metadata (e.g., Donor ID, Blood Type) relevant to the blood bank record.

Real-time Verification: The system allows instant verification of any file's integrity against its blockchain-recorded hash.

Transparent Record Listing: All stored records are visible in a table format, showing key details like file name, hash, CID, uploader, and timestamp.

User-Friendly Interface: A responsive HTML interface with a dark mode option for enhanced user experience.

5. How it Works (Workflow)
5.1. File Upload Flow
User Action: The user selects a blood bank record file (e.g., a PDF, CSV, or text file) via the "Upload Blood Bank Record" section on hema.html and enters relevant metadata.

Frontend Processing: The JavaScript in hema.html captures the file and metadata from the form and sends them as FormData to the backend's /upload endpoint.

Backend Processing (app.py):

Receives the file and metadata.

Calculates the SHA256 hash of the file's content.

Connects to the local IPFS daemon and adds the file, receiving an IPFS CID in return.

Generates a unique fileId (UUID).

Constructs and sends a transaction to the Ethereum blockchain, calling the uploadFile function of the FileIntegrity smart contract. This transaction includes the fileId, fileName, fileHash, ipfsCid, uploader address, and metadata.

Waits for the transaction to be mined and confirmed on the blockchain.

Responds to the frontend with success status, fileId, fileHash, and ipfsCid.

Frontend Update: Upon successful upload, an alert confirms the upload details, and the "Stored Records" table is automatically refreshed to show the new record.

5.2. File Verification Flow
User Action: The user selects a file they wish to verify from their local system using the "Verify File Integrity" section on hema.html.

Frontend Processing: The JavaScript in hema.html captures the selected file and sends it as FormData to the backend's /verify endpoint.

Backend Processing (app.py):

Receives the file.

Calculates the SHA256 hash of this provided file's content.

Calls the getAllFileIds function on the smart contract to get a list of all recorded file IDs.

For each fileId, it calls getFileDetails on the smart contract to retrieve the fileHash stored on the blockchain.

Compares the hash of the provided file with each stored hash.

If a match is found, it indicates that the file's integrity is preserved and retrieves the associated metadata from the blockchain.

Responds to the frontend with isValid status and the record details if valid.

Frontend Update: The verifyResult section on hema.html updates to display whether the file was verified successfully, along with its details if a match was found, or a warning if not.

5.3. View Stored Records Flow
Initial Load/Refresh: When hema.html loads or fetchRecords() is called (e.g., after a successful upload).

Frontend Request: The JavaScript initiates a GET request to the backend's /records endpoint.

Backend Processing (app.py):

Calls getAllFileIds() on the smart contract to retrieve all unique file IDs.

For each fileId, it calls getFileDetails() on the smart contract to fetch the comprehensive details (fileName, fileHash, ipfsCid, uploader, timestamp, metadata).

Aggregates these details into a list of record objects.

Sends this list as a JSON response to the frontend.

Frontend Update: The JavaScript receives the list of records and dynamically populates the recordsBody table in hema.html, displaying all the immutably stored blood bank record details.

6. Future Enhancements
User Authentication/Authorization: Implement a robust authentication system (e.g., using Firebase Authentication or a custom JWT-based system) to restrict upload and sensitive operations to authorized users. Link blockchain transactions to authenticated user IDs.

Private Data Handling: For highly sensitive blood bank data, explore methods like symmetric encryption where the encryption key is managed by the user or a secure key management system, with only the encrypted content on IPFS.

Advanced IPFS Features: Implement features to fetch files directly from IPFS via the frontend using the ipfsCid (e.g., for viewing or downloading records).

Scalability for Records: For a very large number of records, optimize the getAllFileIds() and getFileDetails() calls by potentially implementing pagination or off-chain indexing solutions.

Decentralized Frontend Hosting: Host hema.html on IPFS itself to achieve a fully decentralized application.

Enhanced Error Handling & UI Feedback: Implement more sophisticated error messages and visual feedback in the frontend for a smoother user experience, rather than just alert().

Batch Uploads: Allow users to upload multiple files at once.

Search and Filter: Add functionality to search and filter records in the table based on file name, metadata, or uploader.

Network Selection: Allow users to select different Ethereum networks (testnets, mainnet) or provide their custom node URL.