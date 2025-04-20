![BannerAula1](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%201/rpiBareBanner.jpg)

## 🚀 Bare Metal!

Começamos nossa jornada Bare Metal com o Raspberry Pi 3 Model B! Neste ambiente, não usamos um sistema operacional completo — mas sim um conjunto mínimo de arquivos essenciais para executar nosso código Assembly diretamente no hardware.

> [!NOTE]
> Esses arquivos estão disponíveis na pasta Arquivos do repositório. Alternativamente, você pode extraí-los diretamente do Raspberry Pi OS.

## 🪶 Depenando o Raspbian

"Depenar" aqui significa otimizar: vamos extrair apenas o que importa do Raspberry Pi OS para rodar nosso código Assembly com agilidade e controle total.

### ✅ Pré-requisitos

- Acesse o site oficial do Raspberry Pi
- Baixe o instalador e grave o Raspberry Pi OS (32 bits) em um cartão SD.

> [!CAUTION]
> Precisa de ajuda com o instalador? Um guia passo a passo está em desenvolvimento!

## 📦 Arquivos essenciais

Após gravar o sistema no cartão SD (ejetado automaticamente), conecte-o novamente e copie os seguintes arquivos da raiz:

### 🧩 bootcode.bin

- Inicializa o hardware básico (memória, CPU, periféricos).
- Carrega o bootloader.
- Verifica o cartão SD.

### 🧠 fixup.dat

- Define endereços e tamanhos de memória.
- Configurações como clock e RAM.

### 🔧 start.elf

- Prepara o ambiente para execução
- Carrega e transfere o controle para seu código Assembly

> Esses três arquivos são suficientes para iniciar seu projeto Bare Metal.

### 😼 Autor

🐈‍⬛ @leonardoalvessousa

### 📄 Licença

Distribuído sob a GNU General Public License v3

### 🎁 Apoie o projeto

    Compartilhe com seus amigos 📢

    Pague uma cerveja ao autor 🍺
