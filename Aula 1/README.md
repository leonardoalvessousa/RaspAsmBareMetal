![BannerAula1](https://raw.githubusercontent.com/leonardoalvessousa/RaspAsmBareMetal/refs/heads/main/Aula%201/rpiBareBanner.jpg)

## Bare Metal!

Para iniciar a jornada no mundo do Bare Metal com o Raspberry Pi 3 Model B, é crucial entender que, embora não seja necessário um sistema operacional completo, alguns arquivos essenciais são indispensáveis para que nosso código em Assembly funcione corretamente.

> [!NOTE]
> Esses arquivos essenciais para o Bare Metal podem ser encontrados na pasta Arquivos. Caso prefira uma alternativa mais segura, você pode obtê-los diretamente do *Raspberry Pi OS*!

### Depenando o Raspbian!

O termo "Depenando" descreve perfeitamente o processo de otimização que realizaremos. Essa "depenação" nos permitirá criar um sistema enxuto e pronto para receber nossos algoritmos em Assembly, explorando todo o potencial do hardware.

#### Pré-requisitos

Para começar, acesse o **[RaspberrySite](https://www.raspberrypi.com/software/)** e baixe o instalador para formatar seu cartão SD com o *Raspberry Pi OS (32 bits)*.

> [!CAUTION]
> Em dúvida sobre como usar o instalador? Um material detalhado está em desenvolvimento para guiá-lo passo a passo!

#### Separando os arquivos

Após conectar o cartão SD ao computador (o instalador o ejetará automaticamente ao finalizar), vamos ao processo de "depenagem". Navegue até a raiz do cartão SD e copie os seguintes arquivos:

raiz SD Card
bootcode.bin


- *Inicializar o hardware*: Configura os componentes básicos, como memória, processador e periféricos.
- *Carregar o bootloader*: Prepara o sistema para carregar o sistema operacional.
- *Verificar o cartão SD*: Confirma a presença dos arquivos necessários para o boot.

raiz SD Card
fixup.dat


- *Endereços de memória*: Define endereços específicos para cada componente do hardware.
- *Tamanho da memória*: Indica a memória disponível para cada componente.
- *Configurações de hardware*: Armazena informações como frequência do clock e quantidade de RAM.

raiz SD Card
start.elf


- *Inicializar o ambiente de execução*: Configura o processador e a memória para o código em Assembly.
- *Carregar o código em Assembly*: Prepara o ambiente para executar seu projeto Bare Metal.
- *Transferir o controle para o seu código*: Após carregar, o controle é passado para o seu código, permitindo a execução das instruções definidas.

---

## 😼 Autor

🐈‍⬛ @leonardoalvessousa

## 📄 Licença

> General Public License (GPL)

## 🎁 Expressões de gratidão

- Conte a outras pessoas sobre este projeto 📢;
- Pague uma cerveja para o autor **[🍺](https://nubank.com.br/cobrar/f7g6w/6755dd2c-8e3d-4c14-9976-b1afefc8ae07)**;
