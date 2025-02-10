![BannerAula2](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%202/bannerPrimeiroProjeto.jpg)

---

## Prática I

Neste artigo, mergulharemos no mundo da programação em Assembly para Raspberry Pi, explorando um projeto prático e divertido: controlar um LED através da pressão de um botão!

---

## Prática I

> Neste artigo, mergulharemos no mundo da programação em Assembly para Raspberry Pi, explorando um projeto prático e divertido: controlar um LED através da pressão de um botão!

### Início do Programa

```arm32bits
.section .text
.globl _start

_start:
    mov sp, #0x8000 
```

- *.globl _start*: Define o símbolo _start como global, tornando-o o ponto de entrada do programa. Em Assembly, isso também é chamado de "label".
- *_start*: Início da label.
- *mov sp, #0x8000*: Define o endereço base do módulo GPIO.

### Símbolos e Constantes

Para melhor organização, os símbolos foram divididos em três blocos. Eles funcionam como constantes no algoritmo.

```arm32bits
@=========================
@ Primeiro bloco de símbolos
@=========================
.equ BASE_ADDR, 0x3f200000 	
.equ GPFSEL0, 0x0
.equ GPFSEL1, 0x04		
.equ GPFSEL2, 0x08		
.equ GPSET0, 0x1c		
.equ GPCLR0, 0x28		
.equ GPLEV0, 0x34		

@=========================
@ Segundo bloco de símbolos
@=========================
.equ CLEAR_BITS21_23, 0xFF1FFFFF 
.equ SET_20_27, 0x249249		
.equ SET_BIT27, 0x8000000	
.equ half_second, 0x7a120	
.equ quarter_second, 0x3d090	
.equ eighth_second, 0x1e848

@=========================
@ Terceiro bloco de símbolos
@=========================
base .req r1			
ldr base, =BASE_ADDR		

offset .req r2			
mask .req r3			
i .req r4			
j .req r5			
return .req r0
```

#### Explicação dos Blocos

1. *Primeiro Bloco*:
   - *BASE_ADDR*: Endereço base do barramento GPIO.
   - *GPFSEL0 - GPFSEL2*: Offsets dos registradores que controlam a função dos pinos GPIO.
   - *GPSET0 e GPCLR0*: Offsets dos registradores para definir o nível lógico dos pinos.
   - *GPLEV0*: Offset do registrador para ler o nível lógico dos pinos.

2. *Segundo Bloco*:
   - *CLEAR_BITS21_23*: Máscara para limpar os bits 21-23, configurando o GPIO 17 como entrada.
   - *SET_20_27*: Máscara para configurar os GPIOs 20-27 como saída.
   - *SET_BIT27*: Máscara para definir o GPIO 27 como alto ou baixo.
   - *half_second, quarter_second, eighth_second*: Valores hexadecimais para criar atrasos.

3. *Terceiro Bloco*:
   - *base .req r1*: Alias para o registrador r1, que armazena o endereço base do GPIO.
   - *offset .req r2, *mask .req r3**, etc.: Aliases para outros registradores usados no código.

### Configuração dos GPIOs

```arm32bits
@=================================================
@ Configurar o GPIO 17 como entrada
@=================================================
ldr offset, =GPFSEL1
ldr mask, =CLEAR_BITS21_23
str mask, [base, offset]

@=================================================
@ Configurar os GPIOs 20-27 como saída
@=================================================
ldr mask, =SET_20_27
ldr offset, =GPFSEL2
str mask, [base, offset]
```

- *GPIO 17 como entrada*: Limpa os bits 21-23 no registrador GPFSEL1.
- *GPIOs 20-27 como saída*: Define os bits 20-27 no registrador GPFSEL2.

### Loop Principal

```arm32bits
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

- *input_loop*: Loop principal que verifica o estado do GPIO 17.
- *Wait*: Função que cria um atraso de 30.000 microssegundos.
- *getInput*: Função que lê o estado do GPIO 17.
- *turn_on_indicator* e *turn_off_indicator*: Rotinas para acender ou apagar o LED.

### Funções Auxiliares

#### getInput

```arm32bits
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

- Lê o estado do pino GPIO especificado e retorna 1 (alto) ou 0 (baixo).

#### turn_on_indicator e turn_off_indicator

```arm32bits
@=============================
@ Rotina II: Acender o LED
@=============================
turn_on_indicator:
    ldr offset, =GPSET0
    ldr mask, =SET_BIT27
    str mask, [base, offset]
    bl turn_on
    b input_loop

@=============================
@ Rotina III: Apagar o LED
@=============================
turn_off_indicator:
    ldr offset, =GPCLR0
    ldr mask, =SET_BIT27
    str mask, [base, offset]
    bl turn_off
    b input_loop


- *turn_on_indicator*: Acende o LED (GPIO 27).
- *turn_off_indicator*: Apaga o LED (GPIO 27).

#### turn_on e turn_off

arm32bits
@=============================
@ Rotina IV: Acender um pino
@=============================
turn_on:
    ldr offset, =GPSET0
    mov mask, #1
    lsl mask, r0
    str mask, [base, offset]
    mov pc, lr

@=============================
@ Rotina V: Apagar um pino
@=============================
turn_off:
    ldr offset, =GPCLR0
    mov mask, #1
    lsl mask, r0
    str mask, [base, offset]
    mov pc, lr

```

- *turn_on*: Acende um pino GPIO específico.
- *turn_off*: Apaga um pino GPIO específico.

### Preparando o Arquivo .img

Para executar o código, é necessário convertê-lo em um arquivo .img que o Raspberry Pi possa reconhecer. Siga os passos abaixo:

1. *Assembly para Objeto*:
   bash
   arm-none-eabi-as -g -o NAMEOBJ NAMES_ASM
   

2. *Objeto para Executável*:
   bash
   arm-none-eabi-ld NAMEOBJ -o NAMES_ELF
   

3. *Executável para Imagem*:
   bash
   arm-none-eabi-objcopy NAMES_ELF -O binary NAME_BIN
   

4. *Salvando no SD Card*:
   - Renomeie o arquivo .bin para kernel.img.
   - Copie-o para a raiz do SD Card.
   - Insira o SD Card no Raspberry Pi e veja o projeto funcionar!

> [!CAUTION]
> *Evitando o Erro*: "Warning: end of file not at end of a line; newline inserted"  
> Certifique-se de que o arquivo de código termine com uma quebra de linha para evitar esse aviso.

---

## 😼 Autor
    @leonardoalvessousa
    
## 📄 Licença

   >GNU GENERAL PUBLIC LICENSE Version 3


## 🎁 Expressões de gratidão

- Conte a outras pessoas sobre este projeto 📢;
- Pague uma cerveja para o autor **[🍺](https://nubank.com.br/cobrar/f7g6w/6755dd2c-8e3d-4c14-9976-b1afefc8ae07)**;
