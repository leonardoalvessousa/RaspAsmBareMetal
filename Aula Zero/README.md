![RPIamb.jpg](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%20Zero/RPIamb.jpg)

---

## ⚙️ Lesson Zero – Setting Up the Environment

> This guide shows how to prepare your Linux environment to develop *Bare Metal* projects for Raspberry Pi using ARM Assembly.

---

## 🚀 Let’s Get Started!

First, let’s check if you already have the basic packages installed.

### Checking GCC

```bash
gcc --version
```

**Expected output:**

```
gcc (Debian 12.2.0-14) 12.2.0
```

### Checking VIM

```bash
vim --version
```

**Expected output:**

```
VIM - Vi IMproved 9.0
```

---

## 🔧 Installing Prerequisites

### 🧱 GCC (Compiler)

GCC is essential for compiling programs on Linux.

```bash
sudo apt install build-essential
sudo apt-get install manpages-dev
```

Then, confirm the installation:

```bash
gcc --version
```

---

### 🔄 Make

`make` automates project compilation. It comes with `build-essential`, so no extra steps are needed.

> \[!CAUTION]
> If you want to make sure:
>
> ```bash
> sudo apt install make
> ```

---

### 📝 VIM (Text Editor)

A fast and handy editor for editing your `.s` files.

```bash
sudo apt install vim
```

---

### 🛠️ arm-none-eabi

This toolchain compiles code for ARM systems without an operating system — perfect for *bare metal* Raspberry Pi development.

#### 1. Remove old versions

```bash
sudo apt-get remove binutils-arm-none-eabi gcc-arm-none-eabi
```

#### 2. Add repository (Ubuntu/Debian)

```bash
sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
sudo apt-get update
```

#### 3. Install compiler and debugger

```bash
sudo apt-get install gcc-arm-none-eabi
sudo apt-get install gdb-arm-none-eabi
```

#### 💡 Manjaro users

```bash
sudo pacman -S arm-none-eabi-gcc
sudo pacman -S arm-none-eabi-gdb
```

---

## ✅ Verify the Installation

```bash
arm-none-eabi-gcc --version
arm-none-eabi-gdb --version
```

If both respond with the correct versions, you’re ready to start coding!

---

## 😼 Author

🐈‍⬛ [@leonardoalvessousa](https://github.com/leonardoalvessousa)

---

## 🎁 Support the Project

* Share with fellow low-level enthusiasts 📢
* Buy the author a beer: \[🍺]

---
