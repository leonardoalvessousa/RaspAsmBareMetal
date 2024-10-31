![Texto Alternativo](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%201/rpiBareBanner.jpg)

## Bare Metal!

>Para iniciar a jornada no mundo do Bare Metal com o Raspberry Pi 3 Model B, é crucial entender que, apesar de não precisarmos de um sistema operacional completo, alguns arquivos essenciais são necessários para que nosso código em Assembly funcione. 

> [!NOTE]
> Esses arquivos essenciais para o Bare Metal podem ser encontrados na pasta `Arquivos`. No entanto, se você não se sentir confortável em baixar arquivos de um repositório no GitHub, existe uma solução simples e segura: A própria Raspberry Pi OS!
### Depenando o Raspbian!

O termo "Depenando" é, de fato, bastante icônico e descreve perfeitamente o que faremos a seguir. Essa "depenação" nos permitirá ter um sistema enxuto e otimizado, pronto para receber nossos algoritmos em Assembly e explorar todo o potencial do hardware. 

#### Pré-requisitos

Consulte **[RaspberrySite](https://www.raspberrypi.com/software/)** para obter o instalador para formatar e obter a SO `Raspberry Pi OS (32 bits)` em seu cartão SD!

> [!CAUTION]
> Ficou em dúvida com os tutoriais disponíveis sobre o instalador? Não se preocupe! Um material esta em desenvolvimento sobre esse assunto.

#### Separando os arquivos

Conecte seu cartão SD ao computador, isso porque o instalador vai ejetar automaticamente ao fim da instalação da SO. E vamos ao processo de "depenagem"!  

Ele será reconhecido como um pendrive, então vamos navegar até a raiz do cartão SD. Agora vamos apenas pesquisar e copiar os seguintes arquivos:

```raiz SD Card
bootcode.bin
```

- **Inicializar o hardware**: O `bootcode.bin` contém a configuração dos componentes básicos do hardware do Raspberry Pi, como a memória, o processador e os periféricos.
- **Carregar o bootloader**: Carrega o bootloader, que é um programa menor que prepara o sistema para carregar o sistema operacional.
- **Verificar o cartão SD**: Verifica se o cartão SD está presente e se contém os arquivos necessários para o boot.

```raiz SD Card
fixup.dat
```

- **Endereços de memória**: Define os endereços de memória específicos para cada componente do hardware, como a memória RAM, o processador e os periféricos.
- **Tamanho da memória**: Indica o tamanho da memória disponível para cada componente.
- **Configurações de hardware**: Armazena informações sobre as configurações de hardware, como a frequência do clock do processador e a quantidade de memória RAM.

```raiz SD Card
start.elf
```

- **Inicializar o ambiente de execução**: O `start.elf` configura o ambiente de execução básico para o seu código em Assembly, incluindo a inicialização do processador e a configuração da memória.
- **Carregar o código em Assembly**:  Carrega o seu código em Assembly, que é o coração do seu projeto Bare Metal!
- **Transferir o controle para o seu código**: Após carregar o seu código, o `start.elf` transfere o controle para ele, permitindo que o seu código execute as instruções que você definiu.

## 😼 Autor

🐈‍⬛ @leonardoalvessousa

## 📄 Licença

> MIT license
## 🎁 Expressões de gratidão

- Conte a outras pessoas sobre este projeto 📢;
- Convide o autor para uma cerveja 🍺;
- Um agradecimento publicamente ou citação 🫂;
- etc.
