
---

#  Lab Summary: Encrypting and Decrypting Data Using AWS KMS and the AWS Encryption CLI

In this lab, I used AWS Key Management Service (KMS) together with the AWS Encryption CLI to encrypt and decrypt a plaintext file. The objective was to understand how client‑side encryption works, how a KMS key is referenced using its ARN, and how encryption context and metadata are used to secure data.  

The workflow included:
- Viewing a plaintext file  
- Creating an output directory  
- Setting a shell variable for the KMS key ARN  
- Encrypting the file using the AWS Encryption CLI  
- Decrypting the encrypted file  
- Verifying the decrypted output  

This lab demonstrated how KMS integrates with client‑side tools to protect data at rest and how envelope encryption is applied in practice.

---

#  Challenges Caused by the Lab Instructions and Environment

These challenges come directly from the **lab design**, the **AWS Academy environment**

---

## **1. The Lab Assumes the AWS Encryption CLI Is Preinstalled**
The instructions immediately use:

```
aws-encryption-cli --encrypt ...
```

However, on the AWS Academy File Server, the CLI is **not installed by default**.  
I must install it manually, but the lab does not mention this step.

**Impact:**  
The first CLI commands fail until the tool is installed.

---

## **2. The Lab Environment Uses Python 3.7, but the CLI Depends on Older SDK Versions**
The File Server runs **Python 3.7**, which is no longer supported by the latest AWS Encryption SDK.  
Only older versions of the CLI (e.g., **3.0.0**) work correctly.

**Impact:**  
I must install a specific older version, even though the lab does not mention version compatibility.

---

## **3. The Lab Uses Home Directory Paths (`~/metadata`, `~/output/.`) Without Explaining Behavior**
The instructions use:

```
--metadata-output ~/metadata
--output ~/output/.
```

But the File Server environment sometimes interprets these paths differently depending on:
- Shell behavior  
- CLI version  
- Whether the directory already exists  

Impact:  
I may see unexpected output locations or overwritten files.

---

## **4. The Lab Does Not Explain the CLI’s Automatic File Naming**
The lab expects the decrypted file to be named:

```
secret1.txt.decrypted
```

But the CLI actually produces:

```
secret1.txt.encrypted.decrypted
```

This is normal behavior for the CLI version compatible with Python 3.7.

**Impact:**  
I could think the decryption failed because the filename does not match the lab instructions.

---

## **5. The Lab Does Not Mention Deprecation Warnings**
Every encryption and decryption command prints:

```
CryptographyDeprecationWarning
```

This is expected in Python 3.7, but the lab does not warn me about it.

**Impact:**  
Warnings may be mistaken for errors.

---

## **6. The Lab Does Not Explain the Required Metadata Flag**
The CLI requires **one of these**:

- `--metadata-output <path>`  
- `--suppress-metadata-output`

The lab includes the metadata flag for encryption, but **not for decryption**, even though the CLI requires it for both.

**Impact:**  
Decryption fails unless I add the metadata flag manually.

---

