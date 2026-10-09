# Feature 1: Fetch Salesforce Accounts using Basic Authentication

This branch demonstrates how to connect MuleSoft to Salesforce using **Basic Authentication (Username, Password, and Security Token)**, paired with MuleSoft's **Secure Configuration Properties Extension** to safely store sensitive credentials.

---

## 🚀 Project Overview

* **Trigger:** HTTP GET Request (`/sf/accounts`)
* **Salesforce Operation:** Query (`SELECT Id, Name, CreatedDate FROM Account`)
* **Security:** Encrypted passwords and security tokens using Blowfish/CFB mode.
* **Output:** JSON array containing Salesforce account records.

---

## ⚙️ Configuration Files Setup

Make sure your configuration files are located under `src/main/resources/`:

### 1. Non-Sensitive Configuration (`config-dev.yaml`)
```yaml
sfdc:
  username: "your-salesforce-user-name"
  url: "https://login.salesforce.com/services/Soap/u/63.0"
```

### 2. Encrypted Secure Configuration (`secure-config-dev.yaml`)
```yaml
sfdc:
  password: "![ENCRYPTED_PASSWORD_HERE]"
  token: "![ENCRYPTED_TOKEN_HERE]"
```

---

## 🔒 Secure Property Global Configuration

In Anypoint Studio, configure your **Secure Properties Config** global element to match these settings (as shown in the configuration reference):

* **Name:** `Secure_Properties_Config`
* **File:** `secure-config-${env}.yaml` (where `${env}` resolves to `dev`)
* **Key:** `${spKey}`
* **Algorithm:** `Blowfish`
* **Mode:** `CFB`

---

## 🏃‍♂️ How to Run the Project Locally

Because this project uses secure properties, you must pass your decryption key as a VM argument when running the application.

1. In Anypoint Studio, go to your Run Configuration.
2. Under **VM arguments**, add your encryption key:
   ```bash
   -DspKey=ABCVhPpYMhSrAXYZ -Denv=dev
   ```
3. Run the Mule Application.

---

## 🧪 Testing the Endpoint

Send a `GET` request using Postman or your browser:

```http
http://localhost:8081/sf/accounts
```

### Expected Response Format:
```json
[
  {
    "Id": "001XXXXXXXXXXXXXXX",
    "Name": "Sample Account Corp",
    "CreatedDate": "2026-06-01T12:00:00.000Z"
  }
]
```

---

## 🛠️ Troubleshooting: SOAP API Login Error

If you recently created a new Salesforce account, you might encounter this error when trying to connect:

```text
org.mule.runtime.api.connection.ConnectionException: SOAP API login() is disabled by default in this org. Contact the org administrator to enable SOAP API login()
```

### Why this happens:
This error occurs because you are using MuleSoft's Basic Authentication connection provider, which relies on the legacy Salesforce SOAP API `login()` method. Salesforce disables SOAP API `login()` by default in all new orgs created from Winter '26 onward and plans to fully retire it in Summer '27.

### Solution:

* **Option 1: Switch to OAuth 2.0 (Recommended):** Look ahead to our upcoming feature branch where we implement token-based OAuth flows.
* **Option 2: Re-enable SOAP API Login (Standard Orgs Only):** If you are using a standard production, sandbox, or developer org that allows modifications, a Salesforce Administrator can temporarily turn this setting back on:
  1. Log in to Salesforce.
  2. Navigate to **Setup**.
  3. In the Quick Find box, type **User Interface** and select it.
  4. Check the box for **Enable SOAP API login()**.
  5. *Note:* Users must also have the **Use Any API Auth** permission assigned to their profile or permission set to authenticate successfully.

---

## 📂 Navigation

* **Previous Branch:** `0-hello-world`
* **Next Branch:** `feature/2-oauth2-flow`
* [Back to Main Repository README](../README.md)