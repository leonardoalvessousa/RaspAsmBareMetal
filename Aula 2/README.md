![BannerAula2](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%202/bannerPrimeiroProjeto.jpg)

---

## 💡 Practice I – First Project

In this hands-on exercise with Assembly for Raspberry Pi, you’ll control an LED using a physical button — a fun and direct introduction to the Bare Metal world!

---

## 🔧 Program Start

```armasm
.section .text
.globl _start

_start:
    mov sp, #0x8000 
```

* `.globl _start`: Defines the program entry point.
* `_start`: Main label.
* `mov sp, #0x8000`: Initializes the stack.

---

## 📌 Symbols and Constants

The symbols are organized into three blocks for easier reading and code maintenance.

```armasm
@ Block 1 – Addresses and registers
.equ BASE_ADDR, 0x3f200000 	
.equ GPFSEL0, 0x0
.equ GPFSEL1, 0x04		
.equ GPFSEL2, 0x08		
.equ GPSET0, 0x1c		
.equ GPCLR0, 0x28		
.equ GPLEV0, 0x34		

@ Block 2 – Masks and delays
.equ CLEAR_BITS21_23, 0xFF1FFFFF 
.equ SET_20_27, 0x249249		
.equ SET_BIT27, 0x8000000	
.equ half_second, 0x7a120	
.equ quarter_second, 0x3d090	
.equ eighth_second, 0x1e848

@ Block 3 – Register aliases
base .req r1			
ldr base, =BASE_ADDR		

offset .req r2			
mask .req r3			
i .req r4			
j .req r5			
return .req r0
```

---

## ⚙️ GPIO Configuration

```armasm
@ GPIO 17 as input
ldr offset, =GPFSEL1
ldr mask, =CLEAR_BITS21_23
str mask, [base, offset]

@ GPIOs 20–27 as output
ldr mask, =SET_20_27
ldr offset, =GPFSEL2
str mask, [base, offset]
```

---

## 🔁 Main Loop

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

* Creates a delay, reads the button, and turns the LED on or off based on the returned value.

---

## 🧠 Auxiliary Functions

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

* Reads the input pin and returns `1` if pressed, `0` otherwise.

---

### 💡 Turn LED On/Off

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

### ⚡ turn\_on / turn\_off (any pin)

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

## 🛠️ Generating the `.img` File

Convert your code into an executable file for the Raspberry Pi:

```bash
# 1. Assemble Assembly into object
arm-none-eabi-as -g -o output.o input.s

# 2. Link object into ELF
arm-none-eabi-ld output.o -o output.elf

# 3. Convert ELF into binary
arm-none-eabi-objcopy output.elf -O binary kernel.img
```

> Copy `kernel.img` to the root of the SD card.

> \[!CAUTION]
> ⚠️ **Avoid this warning:**
> `"Warning: end of file not at end of a line; newline inserted"`
> End your code with a newline.

---

## 😼 Author

🐈‍⬛ [@leonardoalvessousa](https://github.com/leonardoalvessousa)

---

## 📄 License

Distributed under the [GNU General Public License v3](https://www.gnu.org/licenses/gpl-3.0.html)

---

## 🎁 Support the Project

* Share with your friends 📢
* Buy the author a beer \[🍺]

---
