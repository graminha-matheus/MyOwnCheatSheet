## Notas Pessoais - Networking

#### Modelo OSI

O modelo OSI (Open Systems Interconnection, ou Interconexão de Sistemas Abertos) é um modelo padronizado que representa a comunicação e conexão de dispositivos na rede, teoricamente. Ele é composto de 7 etapas:

- Aplicação;

- Apresentação;

- Sessão;

- Transporte;

- Rede;

- Enlace;

- Física;

**Aplicação:**

É a camada que fornece opções de rede para programas rodando no computador. Funciona quase que exclusivamente com aplicações, as fornecendo uma interface onde podem transmitir dados. Após ser passado para a camada de Aplicação, os dados são então enviados para a camada de Apresentação.

**Apresentação**

A camada de apresentação recebe dados da camada de aplicação. Esses dados geralmente vêm em um formato que é compreendido pela camada de aplicação, mas não necessariamente de uma forma que podem ser entendidos pela camada de aplicação do OUTRO computador (o que receberá os dados). A camada de apresentação é, então, responsável por traduzir os dados e cuidar de quaisquer outras coisas que transformem os dados de alguma forma, como encriptações ou compressões.

**Sessão:**

Após o recebimento dos dados formatados corretamente pela camada de apresentação, a camada de sessão verifica se pode realizar a conexão com a camada de sessão do OUTRO computador. Caso não possa, ela retorna um erro e não continua o processo. Caso SEJA possível, o trabalho da camada de sessão é manter a conexão ativa e cooperar com a OUTRA camada de sessão para sincronizar a conexão. Essa camada é bem importante, já que cada sessão criada é única e permite que hajam diversas outras sem que elas se interfiram e misturem os dados. Ou seja, é como se cada aba aberta do navegador fosse uma sessão diferente. Após a conexão entre o computador host e o remoto, os dados são passados à camada de transporte.

**Transporte**

Nesta camada, o protocolo de transmissão dos dados é escolhido - os dois mais comuns sendo o TCP (Transmission Control Protocol) e o UDP (User Datagram Protocol). A diferença entre os dois é basicamente **Segurança vs Velocidade**.

O protocolo TCP estabelece uma conexão estrita com o computador que deseja a conexão, garantindo uma transmissão segura a uma velocidade aceitável, onde pacotes perdidos são retransmitidos. Já o protocolo UDP basicamente "joga" os pacotes em cima do computador. Se ele os recebeu ou não, é problema *dele*. Ou seja, é bem mais rápido que o protocolo TCP, porém menos seguro.

O protocolo TCP é utilizado em downloads, uploads, acessos a websites (entre outros), e o UDP é geralmente utilizado em streamings, onde a velocidade de transmissão pesa mais na balança do que a segurança dos pacotes.

Com os pacotes recebidos, e seus protocolos determinados, a camada de transporte divide eles em pequenos pedacinhos - chamados de *segmentos* pelo protocolo TCP, e *datagramas* pelo protocolo UDP. Esses pedacinhos tornam a transferência mais fácil de ser realizada.

**Rede**

A camada de Rede é responsável por localizar o computador de destino, para onde os dados serão enviados. A camada então escolhe a melhor (mais rápida) rota que será percorrida para alcançar o IP de destino. É utilizado o endereçamento de redes (como endereços IP), que são, basicamente, formas de categorizar e organizar redes. O modo de endereçamento mais comum hoje em dia é o IPv4.

**Enlace**

A camada de enlace foca no endereço FÍSICO da transmissão. Ao receber o pacote, a camada atribui a ele o endereço físico do endpoint (o recipiente dos dados), o Endereço MAC (Media Access Control, ou Controle de Acesso a Mídia), que é atribuído e FIXADO a qualquer dispositivo de rede. O Enlace também tem outras duas responsabilidades: apresentar o dado de uma forma "Ok" para que ele seja transmitido, e verificar se ele está corrompido antes de enviá-lo.

**Física**

Essa é a camada física, onde todos os pulsos elétricos que compõem o envio de dados são enviados e recebidos. Ela também é responsável pelo envio e recebimento de dados binariamente.

<img src="https://qph.fs.quoracdn.net/main-qimg-c39d533111fda5c465b8cb49c8ff3af3" title="" alt="" data-align="center">

![](C:\Users\Graminha\AppData\Roaming\marktext\images\2021-03-27-19-56-07-image.png)

---

#### TCP/IP

O modelo TCP/IP é bem semelhante ao modelo OSI, e representa melhor a "realidade".

Diferente do OSI, ele apresenta 4 camadas:

- Aplicação;

- Transporte;

- Internet;

- Interface Física de Rede;

Alguns lugares escolhem "quebrar" a camada 4 em duas: Enlace e Física. Os dois tipos são aceitos, porém a única que é **oficialmente ** reconhecida é a original, apresentada acima.

O modelo recebe seu nome de duas importantes partes dele: o protocolo TCP (Transmission Control Protocol), que controla o fluxo de dados entre dois endpoints, e o IP (Internet Protocol), que controla o endereçamento de dispositivos em uma rede, e como pacotes são enviados e recebidos.

O protocolo TCP é baseado em comunicação, ou seja, ele só vai enviar dados após o estabelecimento de uma conexão firme e estável entre os dois endpoints. Esse estabelecimento é chamado de **three-way handshake**, em tradução livre, algo como "aperto de mão de três etapas/formas".

Quando seu computador quer estabelecer uma conexão, ele envia um bit SYN (synchronization/sincronização). O outro computador então responde com um bit ACK (acknowledged/confirmado ou compreendido), juntamente de outro bit SYN (SYN/ACK). E enfim, seu computador envia um bit ACK para confirmar que a conexão foi bem sucedida. Desta forma, o **three-way handshake** está concluído, e uma conexão estável foi estabelecida entre os dois computadores. Caso haja perda de pacotes, os mesmos são retransmitidos.

<img src="https://muirlandoracle.co.uk/wp-content/uploads/2020/03/image-2.png" title="" alt="The three way handshake" data-align="center">

<img src="file:///C:/Users/Graminha/AppData/Roaming/marktext/images/2021-03-27-21-48-58-image.png" title="" alt="" data-align="center">
