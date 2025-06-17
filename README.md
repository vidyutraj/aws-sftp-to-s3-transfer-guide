# Using SFTP over an SSH encrypted connection to get content into an S3 bucket by configuring an AWS Transfer Server

SFTP (SSH File Transfer Protocol) is a secure file transfer protocol that provides a reliable and encrypted way to transfer files between systems. It operates over SSH (Secure Shell) and offers several advantages over traditional FTP, including:
- Encrypted data transfer for enhanced security
- Built-in authentication and access control
- Support for file operations like upload, download, rename, and delete
- Ability to resume interrupted transfers
- Compatibility with various operating systems and clients

This guide will walk you through setting up an AWS Transfer Server to enable secure SFTP transfers directly to Amazon S3 buckets.

## Goals
The primary objectives of this project are to:
1. Set up an AWS Transfer Family server that supports SFTP protocol
2. Configure secure SSH-based authentication for file transfers
3. Establish a direct connection between the SFTP server and an S3 bucket
4. Enable secure file transfers from external hosts to the S3 bucket
5. Implement proper access controls and security measures

### AWS Services We'll Use
To accomplish these goals, we'll work with the following AWS services:
- **Amazon S3**: For storing the transferred files
- **AWS Transfer Family**: To provide the SFTP server endpoint
- **IAM (Identity and Access Management)**: 
  - For creating roles and policies
  - Managing user permissions
  - Controlling access to S3 buckets

## Setup Steps

### 1. S3 Bucket Creation
First, we can create our target S3 bucket. This will be the location where our SFTP uploads will go. I will be using the demo-bucket-1092 for this lab.

<img width="1705" alt="Screen Shot 2025-06-17 at 9 16 13 AM" src="https://github.com/user-attachments/assets/0e8015f5-d3e1-43c7-a977-c5d1d207381a" />

### 2. IAM

Once we've created our S3 bucket, we now need to set up an IAM role to enable to enable a trust relationship. This gives the users permissions to do this SFTP action. 

Navigate to IAM --> Go to Roles --> Create role

<img width="962" alt="Screen Shot 2025-06-17 at 9 17 13 AM" src="https://github.com/user-attachments/assets/bdb0844c-c240-4aeb-a11d-0faa5fe9677f" />
<img width="1728" alt="Screen Shot 2025-06-17 at 9 17 32 AM" src="https://github.com/user-attachments/assets/7e0da5b5-2c8c-474a-aa77-b4d734ae492c" />



We can select AWS service for the "Trusted entity type" and select EC2 for the use case.

<img width="1102" alt="Screen Shot 2025-06-17 at 9 17 51 AM" src="https://github.com/user-attachments/assets/79d60297-d8bd-4b08-bf0c-bc538279f41d" />
<img width="1080" alt="Screen Shot 2025-06-17 at 9 18 07 AM" src="https://github.com/user-attachments/assets/48f73f01-41da-417c-a169-28c14d9c5e7e" />


placehlder

<img width="958" alt="Screen Shot 2025-06-17 at 9 18 25 AM" src="https://github.com/user-attachments/assets/82d62f3b-8d50-4e6a-9f47-3fd4b1271f5a" />
<img width="1425" alt="Screen Shot 2025-06-17 at 9 18 49 AM" src="https://github.com/user-attachments/assets/01a5b251-ef1b-439a-8bbd-f4f80dca06f6" />
<img width="567" alt="Screen Shot 2025-06-17 at 9 19 11 AM" src="https://github.com/user-attachments/assets/587ddf84-cc81-4a83-a3ac-33878762ecb3" />
<img width="662" alt="Screen Shot 2025-06-17 at 9 19 19 AM" src="https://github.com/user-attachments/assets/01e1b4ff-b49c-4209-a759-85f3bba66a91" />
<img width="1442" alt="Screen Shot 2025-06-17 at 9 19 36 AM" src="https://github.com/user-attachments/assets/5bd58d0b-f1c4-4c54-bf44-9f2fdcbab323" />
<img width="932" alt="Screen Shot 2025-06-17 at 9 19 51 AM" src="https://github.com/user-attachments/assets/acc243d1-af53-476f-9a31-93937636b528" />
<img width="1253" alt="Screen Shot 2025-06-17 at 9 20 12 AM" src="https://github.com/user-attachments/assets/9a35ec3f-1f64-4a6f-b391-1214776ca3c4" />
<img width="830" alt="Screen Shot 2025-06-17 at 9 20 23 AM" src="https://github.com/user-attachments/assets/2111dc70-aa94-416c-8ede-019a2225f973" />
<img width="1377" alt="Screen Shot 2025-06-17 at 9 20 35 AM" src="https://github.com/user-attachments/assets/8ff66835-14dc-491c-910a-9daa2f5057c1" />
<img width="1024" alt="Screen Shot 2025-06-17 at 9 20 48 AM" src="https://github.com/user-attachments/assets/32bf9da2-c45f-4ddd-9903-903b6f00a58b" />
<img width="1001" alt="Screen Shot 2025-06-17 at 9 21 12 AM" src="https://github.com/user-attachments/assets/ec039c25-c09e-45d2-8e63-eddc9b97abf1" />
<img width="1385" alt="Screen Shot 2025-06-17 at 9 21 30 AM" src="https://github.com/user-attachments/assets/c41a6d6d-4148-4530-b41e-00572b6cc4af" />
<img width="1429" alt="Screen Shot 2025-06-17 at 9 21 40 AM" src="https://github.com/user-attachments/assets/b280fbd0-83db-45fd-bdbb-d5a34feeaacc" />
<img width="1410" alt="Screen Shot 2025-06-17 at 9 21 49 AM" src="https://github.com/user-attachments/assets/02c89d58-3a7b-4f33-8530-65fd0f146d20" />
<img width="21" alt="Screen Shot 2025-06-17 at 9 22 08 AM" src="https://github.com/user-attachments/assets/13d9c2af-d21d-4c22-bc7a-d282317fff48" />
<img width="1197" alt="Screen Shot 2025-06-17 at 9 22 15 AM" src="https://github.com/user-attachments/assets/f0cfe916-67d6-4414-9ce7-ff2380fe8681" />
<img width="369" alt="Screen Shot 2025-06-17 at 9 23 16 AM" src="https://github.com/user-attachments/assets/cac88882-683c-418d-94fc-727ca1a04b2e" />
<img width="682" alt="Screen Shot 2025-06-17 at 9 23 53 AM" src="https://github.com/user-attachments/assets/69185287-d4f3-445d-89c8-f300b947c44e" />
<img width="924" alt="Screen Shot 2025-06-17 at 9 26 28 AM" src="https://github.com/user-attachments/assets/d063ce14-6613-477c-8970-581adaa7e1cd" />
<img width="1656" alt="Screen Shot 2025-06-17 at 9 26 57 AM" src="https://github.com/user-attachments/assets/9f0b8442-bc78-4065-8414-730de571f6ec" />
<img width="1431" alt="Screen Shot 2025-06-17 at 9 27 21 AM" src="https://github.com/user-attachments/assets/1abea24c-7f67-4311-93c0-b8a2e3ebfd30" />
<img width="1320" alt="Screen Shot 2025-06-17 at 9 27 39 AM" src="https://github.com/user-attachments/assets/25850910-c715-4a8f-a850-fff7703b243c" />
<img width="851" alt="Screen Shot 2025-06-17 at 9 28 11 AM" src="https://github.com/user-attachments/assets/e78e0a63-0ecb-40c4-b00e-8eea585727ab" />
<img width="909" alt="Screen Shot 2025-06-17 at 9 33 53 AM" src="https://github.com/user-attachments/assets/522a81ac-c4d8-4a72-912e-2ab322e9b719" />
<img width="914" alt="Screen Shot 2025-06-17 at 9 34 03 AM" src="https://github.com/user-attachments/assets/f5ea179e-e902-4a80-a514-08d868d53fac" />
<img width="918" alt="Screen Shot 2025-06-17 at 9 34 55 AM" src="https://github.com/user-attachments/assets/e97703b7-bbfa-45f0-9e22-c5fa8846f130" />
<img width="1332" alt="Screen Shot 2025-06-17 at 9 35 24 AM" src="https://github.com/user-attachments/assets/da3d8230-9e3e-427d-a68c-400c7ef1e4e3" />

