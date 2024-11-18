![Texto Alternativo](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%202/bannerPrimeiroProjeto.jpg)

## Prática I

> Neste artigo, mergulharemos no mundo da programação em Assembly para Raspberry Pi, explorando um projeto prático e divertido: controlar um LED através da pressão de um botão!

### Inicio do programa

```arm32bits
.section .text

.globl _start

_start:

mov sp,#0x8000 
```

- **`.globl _start`**: Define o símbolo `_start` como global, tornando-o o ponto de entrada do programa. Em Assembly , isso também é chamado de "label". A label ficará visível  para o sistema por ter sido criada como global.
- **`_start`**: Inicio da label.
- **`mov sp,#0x8000`**: Endereço base do módulo GPIO.
  
### Criando alguns símbolos

Separei os símbolos em 3 blocos, pois acredito que ficarão mais organizados para os comentários posteriores. Você pode trata-los como constantes no algoritmo. 

```arm32bits
@=========================
@Primeiro bloco de símbolos
@=========================
.equ BASE_ADDR,0x3f200000 	
.equ GPFSEL0,  0x0
.equ GPFSEL1,  0x04		
.equ GPFSEL2,  0x08		
.equ GPSET0,   0x1c		
.equ GPCLR0,   0x28		
.equ GPLEV0,   0x34		
@=========================
@Segundo bloco de símbolos
@=========================
.equ CLEAR_BITS21_23,0xFF1FFFFF 
.equ SET_20_27,0x249249		
.equ SET_BIT27,0x8000000	
.equ half_second, 0x7a120	
.equ quarter_second, 0x3d090	
.equ eighth_second,0x1e848
@==========================
@Terceiro bloco de símbolos
@==========================
base .req r1			
ldr base,=BASE_ADDR		

offset .req r2			
mask .req r3			
i .req r4			
j .req r5			
return .req r0
```


>Primeiro bloco de símbolos:

- **`BASE_ADDR`:** Esta constante define o endereço base do barramento GPIO no seu dispositivo. Este é um endereço físico específico que você precisa encontrar na documentação do seu hardware.
- **`GPFSEL0 - GPFSEL2`:** Estas constantes definem os offsets dos registradores que controlam a função dos pinos GPIO. Cada registrador controla um grupo de 10 pinos GPIO. Para configurar um pino como entrada ou saída, você precisa escrever no registrador correspondente usando uma máscara de bits.
- **`GPSET0` e `GPCLR0`:** Estas constantes definem os offsets dos registradores usados para definir o nível lógico dos pinos GPIO para alto (3.3V) ou baixo (0V).
- **`GPLEV0`:** Esta constante define o offset do registrador usado para ler o nível lógico atual dos pinos GPIO.

>Segundo bloco de símbolos:

- **`CLEAR_BITS21_23`:** Esta constante define uma máscara de bits para limpar os bits 21 a 23. Esta máscara é usada para configurar o GPIO 17 como entrada.
- **`SET_20_27`:** Esta constante define uma máscara de bits para definir os bits 20 a 27. Esta máscara é usada para configurar os GPIO 20-27 como saída.
- **`SET_BIT27`:** Esta constante define uma máscara de bits para definir o bit 27. Esta máscara é usada para definir o GPIO 27 para alto (3.3V) ou baixo (0V). Este GPIO é geralmente usado como uma luz indicadora.
- **`half_second`, `quarter_second`, `eighth_second`:** Estas constantes definem valores hexadecimais para tempos específicos em milissegundos. Esses valores são usados para criar atrasos no código.

>Terceiro bloco de símbolos:

- **`base .req r1`:** Esta diretiva define um símbolo chamado **`base`** que é um alias para o registrador **`r1`**. Isso significa que você pode usar **`base`** e **`r1`** de forma intercambiável no código.
- **`ldr base,=BASE_ADDR`:** Esta instrução carrega o endereço base do barramento GPIO (**`BASE_ADDR`**) no registrador **`base`** (que é o mesmo que **`r1`**).
- **`offset .req r2`, `mask .req r3`, `i .req r4`, `j .req r5`, `return .req r0`:** Estas diretivas definem aliasses para outros registradores: **`offset`** para **`r2`**, **`mask`** para **`r3`**, **`i`** para **`r4`**, **`j`** para **`r5`** e **`return`** para **`r0`**.

### Mãos a obra!

```arm32bits
@=================================================
@Defina o GPIO 17 como entrada
@=================================================
ldr offset,=GPFSEL1
ldr mask,=CLEAR_BITS21_23
str mask,[base,offset]
@=================================================
@Defina os pinos GPIO 20 a 27 como saída
@=================================================
ldr mask,=SET_20_27
ldr offset,=GPFSEL2
str mask,[base,offset]
```

>Configurar o GPIO 17 como entrada:

- `ldr offset,=GPFSEL1`: Carrega o offset do registrador GPFSEL1 em `offset` (r2). O registrador GPFSEL1 controla a função dos pinos GPIO 10-19, incluindo o GPIO 17.
- `ldr mask,=CLEAR_BITS21_23`: Carrega a máscara `CLEAR_BITS21_23` em `mask` (r3). Esta máscara limpa os bits 21 a 23, o que configura o GPIO 17 como entrada.
- `str mask,[base,offset]`: Escreve a máscara `mask` no registrador GPFSEL1. O endereço do registrador é calculado adicionando o endereço base (`base`) e o offset (`offset`).

 >Configurar os GPIO 20 a 27 como saída:*

- `ldr mask,=SET_20_27`: Carrega a máscara `SET_20_27` em `mask` (r3). Esta máscara define os bits 20 a 27, o que configura os GPIO 20 a 27 como saída.
- `ldr offset,=GPFSEL2`: Carrega o offset do registrador GPFSEL2 em `offset` (r2). O registrador GPFSEL2 controla a função dos pinos GPIO 20-29.
- `str mask,[base,offset]`: Escreve a máscara `mask` no registrador GPFSEL2. O endereço do registrador é calculado adicionando o endereço base (`base`) e o offset (`offset`).

### Primeiro loop

```arm32bit
input_loop:
	mov r0,#30000		
	bl Wait			

	mov mask,#17		
	push {mask}			
	bl getInput		

	cmp r0,#1		
	bne turn_off_indicator 
	beq turn_on_indicator

	b input_loop
```  

 - **`input_loop:`:** Esta label marca o início do loop principal do programa.
 - **`mov r0,#30000`:** Carrega o valor 30000 (30.000 microssegundos) em `r0`. Este valor será usado como argumento para a função `Wait`. Ele será trabalhado mais tarde.
 - `bl Wait`: Chama a função `Wait`, que espera por 30.000 microssegundos.
 - **`mov mask,#17`:** Carrega o valor 17 em `mask` (r3). Este valor representa o número do pino GPIO que será lido (GPIO 17).
 - **`push {mask}`:** Empilha o valor de `mask` (r3) na pilha. Isso é feito para que o valor de `mask` possa ser passado para a função `getInput`.
 - **`bl getInput`:** Chama a função `getInput`, que lê o estado do pino GPIO especificado em `mask` (r3). O resultado da leitura é armazenado em `r0`.
 - **`cmp r0,#1`:** Compara o valor em `r0` com 1. Se o pino GPIO estiver alto, `r0` será igual a 1. Se o pino GPIO estiver baixo, `r0` será diferente de 1.
 - **`bne turn_off_indicator`:** Se `r0` for diferente de 1 (pino GPIO está baixo), pula para o rótulo `turn_off_indicator`.
 - **`beq turn_on_indicator`:** Se `r0` for igual a 1 (pino GPIO está alto), pula para o rótulo `turn_on_indicator`.
 - **`b input_loop`:** Salta para o início do loop `input_loop`.

### Vamos as rotinas (I)

```arm32bits
getInput:
	push {r7,lr}
	mov r7,sp
	push {r1-r5}

	ldr r4,[r7,#8]

	argument .req r4
	result .req r5

	mov mask,#1
	lsl mask,argument		
		
	ldr base,=BASE_ADDR	
	ldr offset,=GPLEV0		
	ldr result,[base,offset]		
	tst result,mask
	moveq r0,#0		 
	movne r0,#1		

	.unreq argument
	.unreq result

	pop {r1-r5}				
	pop {r7,lr}
	add sp,sp,#4		
	mov pc,lr
```

 **`getInput:`:** A função `getInput` é responsável por ler o estado de um pino GPIO específico.
 **`push {r7,lr}`:** Salva os registradores `r7` (ponteiro da pilha) e `lr` (endereço de retorno) na pilha. Isso é feito para garantir que esses registradores não sejam sobrescritos durante a execução da função.
 **`mov r7,sp`:** Move o endereço do topo da pilha para `r7`. Isso é usado para acessar os argumentos da função na pilha.
 **`push {r1-r5}`:** Salva os registradores `r1` a `r5` na pilha. Isso é feito para garantir que os valores nesses registradores não sejam sobrescritos durante a execução da função.
 **`ldr r4,[r7,#8]`:** Carrega o valor do primeiro argumento da função na pilha para o registrador `r4`. O primeiro argumento da função é o número do pino GPIO a ser lido.
 **`argument .req r4`:** Define um alias chamado `argument` para o registrador `r4`. Isso torna o código mais fácil de ler.
 **`result .req r5`:** Define um alias chamado `result` para o registrador `r5`. Isso torna o código mais fácil de ler.
 **`mov mask,#1`:** Carrega o valor 1 em `mask` (r3).
 **`lsl mask,argument`:** Desloca o valor em `mask` para a esquerda pelo número de bits especificado em `argument` (r4). Isso cria uma máscara de bits para ler o estado do pino GPIO especificado.
 **`ldr base,=BASE_ADDR`:** Carrega o endereço base do barramento GPIO em `base` (r1).
 **`ldr offset,=GPLEV0`:** Carrega o offset do registrador `GPLEV0` em `offset` (r2). `GPLEV0` contém o estado atual de todos os pinos GPIO.
 **`ldr result,[base,offset]`:** Carrega o valor do registrador `GPLEV0` em `result` (r5).
 **`tst result,mask`:** Realiza uma operação AND entre `result` (r5) e `mask` (r3). Se o resultado for diferente de zero, a flag de zero não é definida. Se o resultado for zero, a flag de zero é definida.
 **`moveq r0,#0`:** Se a flag de zero estiver definida (resultado do AND foi zero), move o valor 0 para `r0`. Isso indica que o pino GPIO está baixo.
 **`movne r0,#1`:** Se a flag de zero não estiver definida (resultado do AND foi diferente de zero), move o valor 1 para `r0`. Isso indica que o pino GPIO está alto.
 **`.unreq argument`:** Remove o alias `argument` para o registrador `r4`.
 **`.unreq result`:** Remove o alias `result` para o registrador `r5`.
 **`pop {r1-r5}`:** Restaura os valores dos registradores `r1` a `r5` da pilha.
 **`pop {r7,lr}`:** Restaura os valores de `r7` e `lr` da pilha.
 **`add sp,sp,#4`:** Limpa o argumento na pilha, movendo o ponteiro da pilha 4 bytes para cima.
 **`mov pc,lr`:** Retorna para o local de chamada, movendo o endereço de retorno (lr) para `pc`.

### Rotinas II e III

```arm32bits
@=============================
@Rotina II
@=============================
turn_on_indicator:
	ldr offset,=GPSET0
	ldr mask,=SET_BIT27
	str mask,[base,offset]
	bl turn_on
    b input_loop
@=============================
@Rotina III
@=============================
turn_off_indicator:
	ldr offset,=GPCLR0
	ldr mask,=SET_BIT27
	str mask,[base,offset]
	bl turn_off
    b input_loop
```

> Rotina II

**`turn_on_indicator`:** Esta função é responsável por acender a luz indicadora.
 - `ldr offset,=GPSET0`: Carrega o offset do registrador `GPSET0` em `offset` (r2). `GPSET0` é usado para definir o nível lógico dos pinos GPIO para alto (3.3V).
-  `ldr mask,=SET_BIT27`: Carrega a máscara `SET_BIT27` em `mask` (r3). Esta máscara define o bit 27, que corresponde ao pino GPIO 27, que é a luz indicadora.
- `str mask,[base,offset]`: Escreve a máscara `mask` no registrador `GPSET0`. Isso define o pino GPIO 27 para alto (3.3V), acendendo a luz indicadora.
- `bl turn_on`: Chama a função `turn_on`. Esta função pode ser usada para realizar outras ações relacionadas a acender a luz indicadora, como ativar um atraso ou executar uma animação.
- `b input_loop`: Salta para o início do loop `input_loop`.

> Rotina III

**`turn_off_indicator`:** Esta função é responsável por apagar a luz indicadora.
- `ldr offset,=GPCLR0`: Carrega o offset do registrador `GPCLR0` em `offset` (r2). `GPCLR0` é usado para definir o nível lógico dos pinos GPIO para baixo (0V).
- `ldr mask,=SET_BIT27`: Carrega a máscara `SET_BIT27` em `mask` (r3). Esta máscara define o bit 27, que corresponde ao pino GPIO 27, que é a luz indicadora.
- `str mask,[base,offset]`: Escreve a máscara `mask` no registrador `GPCLR0`. Isso define o pino GPIO 27 para baixo (0V), apagando a luz indicadora.
- `bl turn_off`: Chama a função `turn_off`. Esta função pode ser usada para realizar outras ações relacionadas a apagar a luz indicadora, como ativar um atraso ou executar uma animação.
- `b input_loop`: Salta para o início do loop `input_loop`.

### Rotinas IV e V

```arm32bits
@=============================
@Rotina IV
@=============================
turn_on:
	@turns on led
	@accepts pin number to turn on from pin 0-31	
	ldr offset,=GPSET0
	mov mask,#1
	lsl mask,r0
	str mask,[base,offset]
	mov pc,lr
@=============================
@Rotina V
@=============================
turn_off:
	@turns off led
	@accepts  pin numnber to turn off from pin 0-31
	ldr offset,=GPCLR0
	mov mask,#1
	lsl mask,r0
	str mask,[base,offset]
	mov pc,lr
```

> Rotina IV

- **`turn_on`:** Esta função é responsável por acender um pino GPIO específico.
- `ldr offset,=GPSET0`: Carrega o offset do registrador `GPSET0` em `offset` (r2). `GPSET0` é usado para definir o nível lógico dos pinos GPIO para alto (3.3V).
- `mov mask,#1`: Carrega o valor 1 em `mask` (r3).
- `lsl mask,r0`: Desloca o valor em `mask` para a esquerda pelo número de bits especificado em `r0`. Isso cria uma máscara de bits para definir o pino GPIO especificado para alto (3.3V).
- `str mask,[base,offset]`: Escreve a máscara `mask` no registrador `GPSET0`. Isso define o pino GPIO especificado para alto (3.3V), acendendo o LED.
- `mov pc,lr`: Retorna para o local de chamada.

> Rotina V

**`turn_off`:** Esta função é responsável por apagar um pino GPIO específico.
- `ldr offset,=GPCLR0`: Carrega o offset do registrador `GPCLR0` em `offset` (r2). `GPCLR0` é usado para definir o nível lógico dos pinos GPIO para baixo (0V).
- `mov mask,#1`: Carrega o valor 1 em `mask` (r3).
- `lsl mask,r0`: Desloca o valor em `mask` para a esquerda pelo número de bits especificado em `r0`. Isso cria uma máscara de bits para definir o pino GPIO especificado para baixo (0V).
- `str mask,[base,offset]`: Escreve a máscara `mask` no registrador `GPCLR0`. Isso define o pino GPIO especificado para baixo (0V), apagando o LED.
- `mov pc,lr`: Retorna para o local de chamada.


> [!CAUTION]
> Evitando o Erro: "Warning: end of file not at end of a line; newline inserted" 
> 
> Ele significa que seu arquivo de código termina em um ponto que não é o final de uma linha. O compilador detecta isso e, para corrigir o problema, insere uma quebra de linha no final do arquivo.

## Preparando o arquivo .img
Para dar vida a este projeto, precisamos transformar nosso código ASM em um arquivo IMG. No entanto, essa conversão não é direta. É preciso percorrer um caminho de conversões até chegarmos a um arquivo que o sistema Bare Metal reconheça e execute.

> Assembly para Objeto:

```arm32bit
arm-none-eabi-as -g -o NAMEOBJ NAMES_ASM
``` 

> Objeto para Executável e vinculável:

```arm32bit
arm-none-eabi-ld NAMEOBJ -o NAMES_ELF
``` 

> Executável e vinculável para Imagem:

```arm32bit
arm-none-eabi-objcopy NAMES_ELF -O binary NAME_BIN
``` 

### Salvando o arquivo .img no SDcard

- Salve seu .img como `kernel.img`, posteriormente cole ele na raiz do SDcard!
- Agora com os arquivos no SDcard, insira o dispositivo na RPI3 e veja o projeto funcionar!

## 😼 Autor
    @leonardoalvessousa
  
## 🎁 Expressões de gratidão

* Conte a outras pessoas sobre este projeto 📢;
* Convide alguém da equipe para uma cerveja 🍺;
* Um agradecimento publicamente 🫂;
* etc.
