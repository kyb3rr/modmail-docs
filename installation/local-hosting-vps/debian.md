---
description: Deploy Modmail on Debian / Raspberry Pi OS.
---

# Debian

{% hint style="warning" %}
For safety reasons, **DO NOT** install Modmail with a root user. A misbehaving or malicious plugin installed on your Modmail bot can easily access your entire system. If you are unsure how to create a new user on Linux, see [DigitalOcean’s tutorial: How To Create a New Sudo-enabled User](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-sudo-enabled-user-on-ubuntu-20-04-quickstart).
{% endhint %}

Raspberry Pi OS 11 Bullseye and Raspberry Pi OS 10 Buster are based on Debian 11 Bullseye and Debian 10 Buster respectively so you can essentially follow this guide if you're running any of the OS mentioned above.

## Prerequisites

1. Root access (**`sudo`**).
2. Minimum 1GB of RAM
3. At least 2GB available disk space.
4. Supported releases:&#x20;
   * Debian 11 Bullseye
   * Debian 10 Buster
   * Raspberry Pi OS 11 Bullseye
   * Raspberry Pi OS 10 Buster

## Dependencies

* Python 3.9 / 3.10
* Tools: `git`, `wget`, `nano`
* Additional Modmail requirements: `libcairo2-dev`, `libffi-dev`, `g++`

{% hint style="info" %}
All code blocks should be executed in bash and line by line unless specified otherwise.
{% endhint %}

To install these dependencies, we will be using **`apt`**.

### **Debian 11 Bullseye /** Raspberry Pi OS 11 Bullseye

```bash
sudo apt update
sudo apt -y install python3 python3-dev python3-venv python3-pip libcairo2-dev libffi-dev g++ git wget nano
```

At the time of writing, this will install Python 3.9 from Debian's repository.

### **Debian 10 Buster /** Raspberry Pi OS 10 Buster

You will need to manually compile Python 3.10 from source. Compiling Python may take a while (est. 5-10 minutes). Make sure to run line 2-7 all at once.

{% code lineNumbers="true" %}
```bash
sudo apt update && sudo apt upgrade -y  # Update and upgrade all packages
sudo apt install -y software-properties-common \
                    libcairo2-dev libffi-dev g++ \
                    git wget nano \
                    build-essential zlib1g-dev libncurses5-dev \
                    libgdbm-dev libnss3-dev libssl-dev \
                    libreadline-dev libffi-dev libsqlite3-dev libbz2-dev
wget https://www.python.org/ftp/python/3.10.9/Python-3.10.9.tgz
tar xzf Python-3.10.9.tgz
cd Python-3.10.9
./configure --enable-optimizations 
sudo make altinstall
```
{% endcode %}

After that, ensure `pip` is installed and updated for Python 3.10 with:

```
python3.10 -m ensurepip --upgrade
```

Then **log out and log back in** to continue the installation steps.

## Installing Bot

Clone and change directory into the Modmail folder with:

```bash
git clone https://github.com/modmail-dev/modmail
cd modmail
```

Inside the Modmail folder, Install `pipenv` and the bot dependencies with:&#x20;

```bash
python3.9 -m pip install pipenv
python3.9 -m pipenv install --python 3.9
```

{% hint style="info" %}
Replace <mark style="color:green;">`3.9`</mark> with <mark style="color:green;">`3.10`</mark> on the command above if you followed[ Debian 10 Buster](debian.md#debian-10-buster-raspberry-pi-os-10-buster) method previously.
{% endhint %}

Create a file named `.env` with `nano` and paste all the environmental variables (secrets) needed to run the bot via right-clicking in the nano editor. Refer to the steps in the [parent Installation page](../#preparing-your-environmental-variables) to find where to obtain these.

```bash
nano .env
```

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

After that, press `Ctrl+O` and `Enter` to save your changes. Exit the `nano` editor with `Ctrl+X`.

{% hint style="info" %}
If using the `nano` editor is a bit of a learning curve, you can always FTP into your server using software like [WinSCP](https://winscp.net/eng/index.php) to edit the `.env` file manually with your preferred GUI-based editor like Notepad.
{% endhint %}

After your `.env` file is ready, you can now go ahead and try running your bot with:

```bash
python3.9 -m pipenv run bot
```

{% hint style="info" %}
Replace <mark style="color:green;">`3.9`</mark> with <mark style="color:green;">`3.10`</mark> on the command above if you followed[ Debian 10 Buster](debian.md#debian-10-buster-raspberry-pi-os-10-buster) method previously.
{% endhint %}

If no error shows up, it means your bot is now running correctly. You can stop the bot from running with `Ctrl+C` to continue using your terminal.
