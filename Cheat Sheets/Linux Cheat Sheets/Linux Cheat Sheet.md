# Linux Cheat Sheet

clear:
limpa a tela de comandos;

pwd: indica
a pasta atual;

ls:
lista o conteúdo da pasta atual;

 ls -l: mostra conteúdo extra;

 ls -l (arquivo): lista apenas o
arquivo alvo;

rm (alvo) :
apaga o arquivo/pasta alvo;

mkdir (nome) :
cria um novo diretório;

cd .. : volta
um “caminho” para trás;

cd / :
volta para a raíz;

vim (arquivo) **ou** vi
(arquivo) **ou** nano (arquivo) : abre o editor de texto
Vim/Vi/Nano;

cat (arquivo) :
apresenta o conteúdo do arquivo diretamente no terminal, sem a necessidade de
abrí-lo;

cat (arquivo) | more : “pausa”
a tela, enquanto apresenta o conteúdo do arquivo. Ao apertar espaço, ele passa
para a próxima página. Para sair, basta apertar **Q**;

cat (arquivo) | less :
possui a mesma função que o more, mas apresenta o arquivo de uma forma
mais flexível, que permite que você desça e suba a tela, no estilo de um editor
de textos;

cat (arquivo) | grep
(filtro) : filtra o conteúdo do arquivo, para apresentar apenas as linhas que possuam o
parâmetro do filtro;

head -(número de linhas)
(arquivo) : mostra o conteúdo do arquivo diretamente no terminal,
limitado pelo número de linhas escolhido previamente;

tail -(número de linhas)
(arquivo) : mostra o conteúdo do arquivo diretamente no terminal,
limitado pelo número de linhas escolhido previamente;

· **Exemplo**: head -4 arquivo apresentará as quatro **primeiras linhas** do arquivo, enquanto tail -4 arquivo apresentará as quatro **últimas linhas** do arquivo;

echo (texto) >>
(arquivo) : **ADICIONA** uma nova linha ao arquivo, com o texto
desejado;

echo (texto) > (arquivo) : **SUBSTITUI** todo o conteúdo do arquivo pelo texto desejado;

ifconfig :
apresenta as configurações de endereço IP da(s) sua(s) placa(s) de rede;

ifconfig (interface, como
por exemplo **eth0**) (IP desejado) netmask (máscara do IP desejado) :
configura o endereço IP;

ifconfig (interface) down:
desliga a interface especificada no comando;

ifconfig (interface) up: liga
a interface especificada no comando;

who :
mostra todos os logados no sistema;

whoami :
mostra qual seu usuário;

ps :
mostra os processos do usuário.

 ps -e : mostra todos os processos
do sistema.

(comando) –help :
mostra todas as opções possíveis com um determinado comando;

man (comando) :
abre um documento detalhado, que apresenta informações e opções disponíveis
para um determinado comando;

useradd (usuário) :
cria um novo usuário;

adduser (usuário) :
cria um usuário, pasta root, grupo e senha. É mais prático;

cat /etc/passwd :
mostra todos os usuários;

cat /etc/shadow : mostra
todos os usuários, mais suas senhas criptografadas

chown (usuário):(grupo) :
muda o dono do diretório atual para o indicado no comando.

chmod (parâmetro) **ou** chmod
(parâmetro 1) + (parâmetro 2) : muda as permissões, de acordo com o(s)
parâmetro(s) indicados no comando. Alguns dos parâmetros:

· rwx (read, write, execution – leitura,
escrita e execução);

· ugo (user, group, others – usuário, grupo,
outros);

· a (all

- todos);

Exemplo:

chmod g+x (arquivo) : modifica as permissões para que o grupo especificado seja
capaz de executar o arquivo indicado. O grupo ao qual a permissão está sendo
dada é indicado ao escrever-se o comando ls -l (arquivo alvo). O
símbolo + adiciona a permissão de execução à lista de permissões desse usuário em relação
a esse comando.

chmod g-x (arquivo) : modifica as permissões para que o grupo especificado não
seja mais capaz de executar o arquivo indicado. O grupo de qual a permissão
está sendo revogada é indicado ao escrever-se o comando ls -l (arquivo alvo). O
símbolo - remove a permissão de execução da lista de permissões desse usuário em relação
a esse comando.

chmod a+w (arquivo) :
modifica as permissões para que **todos** (all) sejam capazes de executar o
arquivo indicado.

export <variável>=<valor>: cria uma variável de ambiente (environment variable) e estabelece um valor à ela.

$<variável>: refere-se a uma variavel criada.

grep <palavra>: encontra a palavra especificada em um documento

    cat <arquivo> | grep <palavra>: procura a palavra especificada no arquivo parâmetro do comando cat.
