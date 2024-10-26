![RPIamb.jpg](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%20Zero/RPIamb.jpg)


>Este material detalha a configuração do ambiente de desenvolvimento em seu computador para projetos "Bare Metal" voltado o Raspberry Pi.

 > [!NOTE]
> No contexto do Raspberry Pi, "Bare Metal" significa que você está trabalhando diretamente com o hardware do Raspberry Pi, sem a camada de abstração de um sistema operacional completo como Ubuntu.


## 🚀 Vamos começar!

Antes de tudo, vamos verificar se sua distribuição Linux já possui os pré-requisitos para trabalhar com o Raspberry Pi usando ASM ARM.

```
gcc --version
```
> Saída:
> gcc (Debian 12.2.0-14) 12.2.0
   Copyright (C) 2022 Free Software Foundation, Inc.
   
```
vim --version
```
>Saída:
>VIM - Vi IMproved 9.0 (2022 Jun 28, compiled May 04 2023 10:24:44)

### 🔧 Resolvendo os pré-requisitos

Esses pré-requisitos serão úteis não apenas para esta prática, mas também para futuras demonstrações. Vamos aproveitar e instalá-los agora mesmo no terminal!


###### GCC (Compilador)

O GCC (GNU Compiler Collection) é um compilador de código aberto fundamental para o Linux. Ele permite que você transforme código-fonte escrito em linguagens como C, C++ e Fortran em programas executáveis que podem ser executados no sistema.

```
sudo apt install build-essential
```

```
sudo apt-get install manpages-dev
```

```
gcc --version
```


###### Make

O Make é uma ferramenta de linha de comando no Linux que automatiza o processo de construção de softwares, gerenciando a compilação e a ligação de arquivos de código-fonte para criar um programa executável.

> [!CAUTION]
> O make vem padrão junto a instalação do GCC.


###### VIM (Nosso bloco de notas)

O Vim é um editor de texto poderoso e altamente personalizável, amplamente utilizado no Pinguim.

```
sudo apt install vim
```


###### arm-none-eabi

É um prefixo de ferramenta usado para identificar um conjunto de ferramentas de desenvolvimento de software (compilador, linker, etc.) que são projetadas para gerar código para a arquitetura ARM, especificamente para sistemas embarcados que não possuem um sistema operacional completo (como o Raspberry Pi).

```
sudo apt-get remove binutils-arm-none-eabi gcc-arm-none-eabi
```

```
sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
```

```
sudo apt-get update
```
 
```
sudo apt-get install gcc-arm-none-eabi
```

```
sudo apt-get install gdb-arm-none-eabi
```

> Manjaro
```
sudo apt-get install arm-none-eabi-gcc
```

```
sudo apt-get install arm-none-eabi-gdb
```


## ✒️ Autor
🐈‍⬛ @leonardoalvessousa
  
## 🎁 Expressões de gratidão

* Conte a outras pessoas sobre este projeto 📢;
* Convide alguém da equipe para uma cerveja 🍺;
* Um agradecimento publicamente 🫂;
* etc.


---
