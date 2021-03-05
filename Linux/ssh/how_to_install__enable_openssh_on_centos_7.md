# How To Install / Enable OpenSSH On CentOS 7

### Introduction

`Secure Shell (SSH)` is a cryptographic protocol that allows a client to interact with a remote server in a secure environment.

High-level encryption protects the exchange of sensitive information and allows flie trans or issue commands on remote machines securely.

Learn how to enable SSH on CentOS 7 by following the instructions in this short tutorial.

SSH software packages are included on CentOS by default. However, if these packages are not present on your system, easily install them by completing Step 1, outlined below.

### Step 1: Install OpenSSH Server

```bash
sudo yum –y install openssh-server openssh-clients
```

This command installs both the OpenSSH client applications, as well as the OpenSSH server daemon, `sshd`.

### Step 2: Starting SSH Service

```bash
sudo systemctl start sshd
```

### Step 3: Check sshd status

```bash
sudo systemctl status sshd
```

### Step 4: Enable OpenSSH Service

Enable SSH to start automatically after each system reboot by using the systemctl command:

```bash
sudo systemctl enable sshd
```

To disable SSH after reboot enter:

```bash
sudo systemctl disable sshd
```

To stop the SSH daemon enter:

```bash
sudo systemctl stop sshd
```

