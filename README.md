# Dropbear-mipsel

Lightweight SSH server and client tools built for **MIPSEL**-based devices like the **JioFiber Router**.

This project provides precompiled binaries of [Dropbear SSH](https://matt.ucc.asn.au/dropbear/dropbear.html) — a small SSH server/client — for embedded Linux devices using the **mipsel architecture**.

---

## 📥 Downloads

Download the tools from the latest release here:  
➡️ [Releases](https://github.com/rabilrbl/dropbear-mipsel/releases/latest)

You can directly download individual files using this format:

```
https://github.com/rabilrbl/dropbear-mipsel/releases/latest/download/<filename>
```

Available files:

| File | Description |
|------|-------------|
| `dropbear` | Lightweight SSH server |
| `dbclient` | SSH client (like OpenSSH's `ssh`) |
| `dropbearkey` | Key generation tool |
| `dropbearconvert` | Converts keys between OpenSSH and Dropbear format |
| `scp` | Secure copy tool (to transfer files over SSH) |

---

## 🚀 How to Use on a JioFiber Router

> ⚠️ You must have root access to JioFiber router. Follow guide at https://github.com/JFC-Group/JF-Customisation

### 1. Download the required files

```sh
wget -O /flash2/dropbear --no-check-certificate https://github.com/rabilrbl/dropbear-mipsel/releases/latest/download/dropbear
wget -O /flash2/dropbearkey --no-check-certificate https://github.com/rabilrbl/dropbear-mipsel/releases/latest/download/dropbearkey
chmod +x /flash2/dropbear /flash2/dropbearkey
```

### 2. Generate an SSH key

```sh
/flash2/dropbearkey -t ed25519 -f /flash2/id_dropbear
```

This creates a secure private key at `/flash2/id_dropbear`.

### 3. Start the Dropbear SSH server

```sh
/flash2/dropbear -p 22 -r /flash2/id_dropbear
```

This starts the SSH server on port **22** using your generated key.

### 4. Allow SSH traffic through the firewall

```sh
/pfrm2.0/bin/iptables -I fwInBypass -p tcp --dport 22 -m ifgroup --ifgroup-in 0x1/0x1 -j ACCEPT
```

### 5. Connect to the Router via SSH

From your computer or phone (using an SSH client), run:

```sh
ssh root@<router-ip>
```

> Replace `<router-ip>` with your router’s actual IP address (e.g., `192.168.1.1`).

When prompted, enter the **root password** of your router to log in.

---

## 🔁 Auto-Start on Boot in JioFiber routers

To make Dropbear start automatically each time the router reboots:

Add command from step 3 and stpe 4 to file `/flash2/pfrm2.0/etc/customInit` and make that file executable.

This ensures the SSH server and firewall rule are applied every time the device boots.

---

## 🧠 What Each Tool Does

- **`dropbear`**: The SSH server that accepts incoming SSH connections.
- **`dbclient`**: A lightweight SSH client for connecting to other devices.
- **`dropbearkey`**: Generates the key file used by the server.
- **`dropbearconvert`**: Converts keys between OpenSSH and Dropbear formats.
- **`scp`**: Used for secure file transfer over SSH.

---

## ✅ Why Dropbear?

- Extremely lightweight — ideal for routers and embedded systems
- Compatible with OpenSSH clients and servers
- Simple to set up, even with limited resources

---

## 🙌 Credits

- Dropbear: https://github.com/mkj/dropbear
- Buildroot: https://github.com/JFC-Group/JF-Buildroot
