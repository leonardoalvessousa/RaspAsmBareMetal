![RPIamb.jpg](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%20Zero/RPIamb.jpg)

---

## ⚙️ Aula Zero – Configurando o Ambiente

> Este guia mostra como preparar seu ambiente Linux para desenvolver projetos *Bare Metal* com Raspberry Pi usando Assembly ARM.

---

## 🚀 Vamos começar!

Primeiro, vamos conferir se você já tem os pacotes básicos instalados.

### Verificando GCC

```bash
gcc --version
```
**Saída esperada:**
```
gcc (Debian 12.2.0-14) 12.2.0
```

### Verificando VIM

```bash
vim --version
```
**Saída esperada:**
```
VIM - Vi IMproved 9.0
```

---

## 🔧 Instalando os pré-requisitos

### 🧱 GCC (Compilador)

O GCC é essencial para compilar programas no Linux.

```bash
sudo apt install build-essential
sudo apt-get install manpages-dev
```

Depois, confirme a instalação:

```bash
gcc --version
```

---

### 🔄 Make

O `make` automatiza a compilação de projetos. Ele já vem com o `build-essential`, então nada extra é necessário.

> [!CAUTION]
> Se preferir garantir:
> ```bash
> sudo apt install make
> ```

---

### 📝 VIM (Editor de texto)

Editor rápido e prático para editar seus arquivos `.s`.

```bash
sudo apt install vim
```

---

### 🛠️ arm-none-eabi

Esse conjunto de ferramentas compila código para sistemas ARM sem sistema operacional — perfeito para *bare metal* no Raspberry Pi.

#### 1. Remover versões antigas

```bash
sudo apt-get remove binutils-arm-none-eabi gcc-arm-none-eabi
```

#### 2. Adicionar repositório (Ubuntu/Debian)

```bash
sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
sudo apt-get update
```

#### 3. Instalar compilador e depurador

```bash
sudo apt-get install gcc-arm-none-eabi
sudo apt-get install gdb-arm-none-eabi
```

#### 💡 Usuários Manjaro

```bash
sudo pacman -S arm-none-eabi-gcc
sudo pacman -S arm-none-eabi-gdb
```

---

## ✅ Verifique a instalação

```bash
arm-none-eabi-gcc --version
arm-none-eabi-gdb --version
```

Se ambos responderem com as versões corretas, você está pronto para codar!

---

## 😼 Autor

🐈‍⬛ [@leonardoalvessousa](https://github.com/leonardoalvessousa)

---

## 🎁 Apoie o projeto

- Compartilhe com a galera que curte baixo nível 📢  
- Pague uma cerveja ao autor: [🍺]
