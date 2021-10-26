# Instalando e Configurando o Heartbeat no Ubuntu
Aqui vamos aprender a instalar e configurar o Heartbeat no Ubuntu. 
Na apresentação foi utilizado Ubuntu 18 e, embora os passos sejam os mesmos, é necessário atentar-se ao versionamento correto dos pacotes utilizados para evitar quaisquer problemas.

- Atualizar os Pacotes:

```
sudo apt-get update
```

- Instalando os Pacotes do Repo
  
```
apt-get install heartbeat
```

Em outras versões de Linux, tal qual CentOS o comando acima pode variar:
```
yum install heartbeat
```

Ou ainda no Suse Linux:
```
yast -i heartbeat
```

- Editando o Arquivo /etc/heartbeat/ha.cf:

Em AMBAS as máquinas, substitua todo o conteúdo do arquivo /etc/heartbeat/ha.cf por este:

```
logfacility     local0
keepalive 2
deadtime 10
bcast   eth0
node server1 server2
```

Onde:
* `logfacility` onde o Syslog gravará suas mensagens
* `keepalive` é o intervalo entre os pacotes do Heartbeat para avaliação da disponibilidade
* `deadtime` especifica o quão rápido o Heartbeat decidirá que um servidor está indisponível
* `bcast` é a interface de rede usada para avaliação
* `node` são cada um dos nodes (nomes das máquinas, a saída do comando `uname -n`) que farão parte do grupo

- Editando o arquivo /etc/heartbeat/haresources:

Em AMBAS as máquinas, edite o arquivo /etc/heartbeat/haresources com o seguinte conteúdo:

```
server1  IPaddr::192.168.0.174/24/eth0 nginx
```
Onde:

* `server01` é o nome do servidor primário (saída do comando `uname -n`, no meu caso, server01. Note que este é o nome do server primário e deve ser O MESMO em ambas as máquinas.
* `IPaddr::XX.XX.XX.XX/YY/zzz` uma string composta por `IPaddr::` seguido do IP da máquina, no meu caso, 192.168.0.174, seguido por `/`, seguido pela máscara de subrede, no meu caso 24, seguido por `/`, seguido pela interface de rede, no meu czso eth0
* `nginx` é serviço que deseja iniciar/parar, no meu caso nginx mas pode ser qualquer serviço ou mesmo uma lista de serviços

- Editando o arquivo /etc/heartbeat/authkeys:

Em ambos os servidores, edite o arquivo /etc/heartbeat/authkeys com o seguinte conteúdo:

```
auth 3
3 md5 algumaStringRandomica
```

- Proteja o arquivo /etc/heartbeat/authkeys:

Em ambos os servidores, rode o comando:

```
chmod 600 /etc/heartbeat/authkeys
```

Isso impedirá que outros usuários acessem o conteúdo deste arquivo, protegendo a sua string de autenticação.

- Inicie o Serviço:

Em ambos os servidores, rode o comando:

```
/etc/init.d/heartbeat start
```

- Avalie a Disponibilidade:

Em ambos os servidores, rode o comando:

```
/etc/init.d/heartbeat status
```

A saída esperada é que o serviço esteja rodando em ambos os servidores e, ao parar o serviço no servidor primário, o serviço configurado deverá ser iniciado no servidor secundário automaticamente.