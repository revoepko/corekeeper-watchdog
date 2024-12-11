# **Core Keeper Dedicated Server Monitoring Guide**

---

## **1. System Preparation**

### 1.1 Environment Details

This guide is tailored for `Ubuntu 24.04.1 LTS` and `Python`.

The code is named `watchcore`, developed with `ChatGPT`.

---

### **2. Setting Up watchcore**

The watchcore script is provided in the repository.

#### 2.1 Install Required Python Modules
The required Python modules for running the script can be installed using the `requirements.txt` file in the repository.

Run the following command:

```bash
pip3 install -r requirements.txt
```

#### 2.2 Make watchcore Executable
After installing the required modules, make the `watchcore` script executable by running the following command:

```bash
chmod +x watchcore
```
This will allow you to execute the script directly from the command line.

---

### **3. Systemd Service Setup**

#### 3.1 Creating a Systemd Service File

Create a service file to manage `watchcore` with `systemd`.

##### 3.1.1 Service File Creation

Add the following content to `/etc/systemd/system/watchcore.service`:

```bash
sudo vi /etc/systemd/system/watchcore.service
```

**Service File Content**

```ini
[Unit]
Description=Core Keeper Dedicated Server Watchdog
After=network.target

[Service]
Type=simple
User=corekeeper
WorkingDirectory=/home/corekeeper/watchcore
ExecStart=/home/corekeeper/watchcore/watchcore
KillMode=process
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

##### 3.1.2 Reloading and Enabling the Service

Reload systemd and enable the service:

```bash
sudo systemctl daemon-reload
sudo systemctl start watchcore.service
sudo systemctl enable watchcore.service
```

---

### **4. Execution Flow**

The operation of `watchcore` is as follows:

1. `watchcore` parses the journal logs of `corekeeper` and processes them.
2. The processed logs are sent to `Discord` via Webhook.
3. If an issue occurs with `corekeeper`, `watchcore` updates the sent message.
   - Updated fields include **Server ID**, **Server Status** and **Last  Server Start Time**.
4. If `watchcore` is stopped, it updates the sent message.
   - Updated field is **Watchcore Status**, and other status information is removed.
