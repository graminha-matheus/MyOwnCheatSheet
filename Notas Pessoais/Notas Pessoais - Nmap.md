## Notas Pessoais - Nmap

#### Escaneamento TCP

O "TCP Scan", ou escaneamento TCP (`-sT`), é um dos tipos de escaneamento presentes no Nmap. Ele toma princípios do **three-way handshake** para o seu funcionamento. 

O **three-way handshake** é um processo realizado para o estabelecimento de uma conexão TCP segura entre dois endpoints. Um dos endpoints envia um request TCP, um bit SYN. O outro endpoint então responde com outro bit SYN, juntamente de um bit ACK. Por fim, o endpoint inicial responde com um bit ACK.

O escaneamento TCP funciona dessa forma no Nmap. Ao realizá-lo, ele envia um request TCP (um bit SYN) à porta especificada. Caso ele **NÃO** obtenha resposta, ou seja, envie a request a uma porta fechada, ele responderá com um bit RST (Reset), e a conexão será encerrada. Dessa forma, o Nmap consegue estabelecer que aquela porta está **FECHADA**. 

Entretanto, se a porta responder com SYN-ACK, o Nmap a marca como **ABERTA**, e finaliza a conexão enviando o último bit ACK.

Porém, existe uma "terceira" forma de resposta. E se a porta estiver atrás de um firewall? 

Ao enviar um bit SYN, o Nmap não receberá nenhuma resposta, já que, geralmente, firewalls estão programados para "dropar" os pacotes enviados diretamente. Ao não receber nenhuma resposta, o Nmap marca automaticamente aquela porta como **FILTRADA**.

A título de curiosidade, é extremamente fácil de fazer com que o firewall filtre pacotes e responda com o bit RST. Usando o IPTables do Linux:

`iptables -I INPUT -p tcp --dport <port> -j REJECT --reject-with tcp-reset`

Isso torna a leitura do alvo bem mais complicada, se não impossível.

---

#### Escaneamento SYN

O escaneamento SYN (`-sS`) funciona de forma semelhante ao escaneamento TCP, com a diferença de ser mais "furtivo".  Quando o servidor envia um SYN ACK de volta, o escaneamento SYN responde com RST, cortando o estabelecimento da conexão. Visualmente, seria assim:

<img src="https://i.imgur.com/cPzF0kU.png" title="" alt="" data-align="center">

Para quem está realizando uma intrusão, existem alguns benefícios, dentre eles:

- Pode ser usado para passar por sistemas mais antigos, que costumavam desconsiderar "three-way handshakes" incompletos, sendo esse o motivo dos escaneamentos SYN serem considerados "furtivos". Entretanto, isso não é comum hoje em dia;

- Escaneamentos SYN não são "logados" por aplicações em portas abertas, já que o padrão é considerar apenas conexões bem sucedidas (após a realização do 3WH);

- Já que a conexão realizada pelo escaneamento SYN é incompleta, ela é bem mais rápida.

Também existem algumas desvantagens:

- Esses escaneamentos necessitam de permissão super-usuário (`sudo`), já que, diferentemente dos escaneamentos TCP, elas precisam criar raw packets (?), e essa é uma permissão que somente super-usuários possuem;

- Serviços instáveis podem ser derrubados por um escaneamento SYN. Isso é um enorme problema caso o cliente/requerente tenha oferecido um ambiente de testes que está na produção (ou seja, disponível para acesso no client-side).

No geral, é bem vantajoso usar o escaneamento SYN e, por isso, ele é o escaneamento padrão usado pelo Nmap **caso o escaneamento seja realizado com permissões de super-usuário**. Caso contrário, é utilizado o escaneamento TCP.

---

#### Escaneamento UDP

Diferentemente do TCP, as conexões UDP são "stateless", ou seja, não dependem de um estabelecimento firme de conexão. O UDP basicamente envia os pacotes e torce para que eles cheguem onde tem que chegar. Isso o deixa muito rápido, porém muito inseguro, também. E essa "incerteza" em relação ao destino dos pacotes é o que torna o escaneamento UDP bem mais devagar que sua contraparte. 

No Nmap, o escaneamento UDP é feito com o comando `-sU`. 

Geralmente, no envio de um pacote a uma porta UDP aberta, o Nmap não obterá nenhuma resposta. Quando isso acontece, ele marca a porta como `open|filtered`. isso significa que ele suspeita que a porta está aberta, porém também pode estar sendo filtrada por um firewall. Em casos raros, o UDP responderá, e o Nmap marcará a porta como `open`. Geralmente não há resposta e, nesse caso, o Nmap reenviará o request para ter certeza do resultado final. Caso ele, novamente, não obtenha resposta, a porta será marcada como `open|filtered`, e o Nmap seguirá em frente.

Caso o pacote seja enviado a uma porta UDP fechada, o alvo deve responder com um pacote ICMP (ping), informando que aquela porta está fechada. Desta forma, o Nmap a indica como tal, e segue em frente.

Por conta dessa dificuldade de saber do estado das portas, o escaneamento UDP é muito mais devagar que os outros (aproximadamente 20 minutos para escanear 1000 portas, com uma boa conexão). Portanto, ao executar um escaneamento UDP, é sempre bom usar as flags `--top-ports <número>` para escanear um número definido de portas das mais conhecidas, diminuindo bastante o tempo necessário para o escaneamento.

Exemplo:

`nmap -sU --top-ports 20 <alvo>` escaneará 20 das portas mais conhecidas.

---

#### Escaneamentos NULL, FIN e Xmas

Os escaneamentos NULL, FIN e Xmas são pouco usados, e considerados ainda mais furtivos que o escaneamento SYN.

##### NULL

O escanamento NULL é um tipo de escaneamento onde o request TCP é enviado sem quaisquer flags (SYN, ACK). Caso a porta esteja fechada, o alvo deve responder com a flag RST.

<img src="https://i.imgur.com/ABCxAwf.png" title="" alt="" data-align="center">

##### FIN

O escaneamento FIN funciona *quase* igual ao NULL, com a diferença de que ele é enviado com a flag FIN, que é usada geralmente para fechar/**fin**alizar conexões.

Novamente, caso a porta esteja fechada, a resposta deve ser uma flag RST.

<img src="https://i.imgur.com/gIzKbEk.png" title="" alt="" data-align="center">

##### Xmas

O escaneamento Xmas, assim como os outros dois, manda um pacote TCP malformado e espera uma resposta RST de portas fechadas. Tem esse nome pois suas flags (FIN, PSH, URG) o dão uma aparência de "árvore de natal piscante" quando vistas como captura de pacotes no Wireshark.

<img src="https://i.imgur.com/gKVkGug.png" title="" alt="" data-align="center">

A resposta que esses escaneamentos terão de portas **abertas** será semelhante à do escaneamento UDP. Geralmente, quando o alvo recebe esses pacotes TCP malformados, ele não responde nada. Por isso, as únicas possíveis respostas desses escaneamentos serão `open|filtered`, `closed` ou `filtered`.

Apesar do RFC 793 estabelecer que a resposta ao envio de pacotes TCP malformados devem ser **RST** para portas fechadas e **sem resposta** para portas abertas, alguns sistemas em específico respondem **sempre** com a flag RST, **sempre** resultando na resposta `closed`.

E qual o objetivo desses escaneamentos, afinal? Evasão de firewall. A maior parte dos firewalls estão configurados para bloquear pacotes com a flag SYN em portas bloqueadas para tal. Com isso, ele ignoraria um pacote sem essa flag, e o pacote TCP malformado poderia, tranquilamente, passar pelo firewall. Entretanto, a maior parte das soluções IDS (?) hoje em dia já conhecem essa técnica, então ela nem sempre funcionará.
