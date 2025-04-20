![BannerAula2](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%202/bannerPrimeiroProjeto.jpg)

---

## 💡 Prática I – Primeiro Projeto

Neste exercício prático com Assembly para Raspberry Pi, você vai controlar um LED utilizando um botão físico — uma introdução divertida e direta ao mundo Bare Metal!

---

## 🔧 Início do Programa

```armasm
.section .text
.globl _start

_start:
    mov sp, #0x8000 
```

- `.globl _start`: Define o ponto de entrada do programa.
- `_start`: Label principal.
- `mov sp, #0x8000`: Inicializa a pilha.

---

## 📌 Símbolos e Constantes

Os símbolos estão organizados em três blocos, facilitando a leitura e manutenção do código.

```armasm
@ Bloco 1 – Endereços e registradores
.equ BASE_ADDR, 0x3f200000 	
.equ GPFSEL0, 0x0
.equ GPFSEL1, 0x04		
.equ GPFSEL2, 0x08		
.equ GPSET0, 0x1c		
.equ GPCLR0, 0x28		
.equ GPLEV0, 0x34		

@ Bloco 2 – Máscaras e delays
.equ CLEAR_BITS21_23, 0xFF1FFFFF 
.equ SET_20_27, 0x249249		
.equ SET_BIT27, 0x8000000	
.equ half_second, 0x7a120	
.equ quarter_second, 0x3d090	
.equ eighth_second, 0x1e848

@ Bloco 3 – Aliases de registradores
base .req r1			
ldr base, =BASE_ADDR		

offset .req r2			
mask .req r3			
i .req r4			
j .req r5			
return .req r0
```

---

## ⚙️ Configuração dos GPIOs

```armasm
@ GPIO 17 como entrada
ldr offset, =GPFSEL1
ldr mask, =CLEAR_BITS21_23
str mask, [base, offset]

@ GPIOs 20–27 como saída
ldr mask, =SET_20_27
ldr offset, =GPFSEL2
str mask, [base, offset]
```

---

## 🔁 Loop Principal

```armasm
input_loop:
    mov r0, #30000		
    bl Wait			

    mov mask, #17		
    push {mask}			
    bl getInput		

    cmp r0, #1		
    bne turn_off_indicator 
    beq turn_on_indicator

    b input_loop
```

- Cria um delay, lê o botão e liga/desliga o LED com base no valor retornado.

---

## 🧠 Funções Auxiliares

### 🔍 `getInput`

```armasm
getInput:
    push {r7, lr}
    mov r7, sp
    push {r1-r5}

    ldr r4, [r7, #8]
    argument .req r4
    result .req r5

    mov mask, #1
    lsl mask, argument		
    ldr base, =BASE_ADDR	
    ldr offset, =GPLEV0		
    ldr result, [base, offset]		
    tst result, mask
    moveq r0, #0		 
    movne r0, #1		

    .unreq argument
    .unreq result

    pop {r1-r5}				
    pop {r7, lr}
    add sp, sp, #4		
    mov pc, lr
```

- Lê o pino de entrada e retorna `1` se pressionado, `0` caso contrário.

---

### 💡 Acender/Apagar o LED

```armasm
turn_on_indicator:
    ldr offset, =GPSET0
    ldr mask, =SET_BIT27
    str mask, [base, offset]
    bl turn_on
    b input_loop

turn_off_indicator:
    ldr offset, =GPCLR0
    ldr mask, =SET_BIT27
    str mask, [base, offset]
    bl turn_off
    b input_loop
```

---

### ⚡ turn_on / turn_off (qualquer pino)

```armasm
turn_on:
    ldr offset, =GPSET0
    mov mask, #1
    lsl mask, r0
    str mask, [base, offset]
    mov pc, lr

turn_off:
    ldr offset, =GPCLR0
    mov mask, #1
    lsl mask, r0
    str mask, [base, offset]
    mov pc, lr
```

---

## 🛠️ Gerando o Arquivo `.img`

Transforme seu código em um arquivo executável para o Raspberry Pi:

```bash
# 1. Compilar Assembly para objeto
arm-none-eabi-as -g -o output.o input.s

# 2. Linkar objeto para ELF
arm-none-eabi-ld output.o -o output.elf

# 3. Converter ELF para binário
arm-none-eabi-objcopy output.elf -O binary kernel.img
```

> Copie o `kernel.img` para a raiz do cartão SD.

> [!CAUTION]
> ⚠️ **Evite o aviso:**  
> `"Warning: end of file not at end of a line; newline inserted"`  
> Finalize seu código com uma quebra de linha.

---

## 😼 Autor

🐈‍⬛ [@leonardoalvessousa](https://github.com/leonardoalvessousa)

---

## 📄 Licença

Distribuído sob a [GNU General Public License v3](https://www.gnu.org/licenses/gpl-3.0.html)

---

## 🎁 Apoie o projeto

- Compartilhe com seus amigos 📢  
- Pague uma cerveja ao autor [🍺]
