# 01. Hello World - MuleSoft HTTP Listener

This branch contains a foundational **Hello World** MuleSoft application demonstrating how to configure an HTTP Listener using externalized properties via a YAML configuration file. 

It serves as the starting baseline for the Salesforce & MuleSoft integration portfolio.

---

## 🚀 Project Overview

* **Trigger:** HTTP GET Request (`/hello`)
* **Configuration:** Externalized `config.yaml` for host and port management.
* **Output:** A simple JSON or text response confirming the application is running.

---

## 🛠️ Prerequisites

* [Anypoint Studio](https://www.mulesoft.com/platform/studio) (v7.x or later)
* Mule Runtime Engine 4.x
* Postman or any API testing client (or a web browser)

---

## ⚙️ Configuration Setup

1. **Verify the Configuration File:**
   Ensure your configuration file exists under `src/main/resources/`:
   ```yaml
   # config-dev.yaml
   http:
     host: "0.0.0.0"
     port: "8081"