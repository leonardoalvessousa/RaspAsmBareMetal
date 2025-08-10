![BannerAula1](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%201/rpiBareBanner.jpg)

## 🚀 Bare Metal!

We begin our Bare Metal journey with the Raspberry Pi 3 Model B!
In this environment, we don’t use a full operating system — instead, we rely on a minimal set of essential files to run our Assembly code directly on the hardware.

> \[!NOTE]
> These files are available in the *Arquivos* (Files) folder of the repository.
> Alternatively, you can extract them directly from Raspberry Pi OS.

## 🪶 Stripping Down Raspbian

Here, “stripping down” means optimizing: we’ll extract only what matters from Raspberry Pi OS to run our Assembly code with speed and full control.

### ✅ Prerequisites

* Access the official Raspberry Pi website.
* Download the installer and flash Raspberry Pi OS (32-bit) to an SD card.

> \[!CAUTION]
> Need help with the installer? A step-by-step guide is in development!

## 📦 Essential Files

After flashing the system to the SD card (which will be automatically ejected), reconnect it and copy the following files from the root directory:

### 🧩 bootcode.bin

* Initializes the basic hardware (memory, CPU, peripherals).
* Loads the bootloader.
* Checks the SD card.

### 🧠 fixup.dat

* Defines memory addresses and sizes.
* Settings such as clock and RAM.

### 🔧 start.elf

* Prepares the environment for execution.
* Loads and hands over control to your Assembly code.

> These three files are enough to start your Bare Metal project.

### 😼 Author

🐈‍⬛ @leonardoalvessousa

### 📄 License

Distributed under the GNU General Public License v3

### 🎁 Support the Project

```
Share with your friends 📢

Buy the author a beer 🍺
```

---
