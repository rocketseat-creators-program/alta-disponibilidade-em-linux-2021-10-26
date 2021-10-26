# Instalando e Configurando o DRBD no Ubuntu
Aqui vamos aprender a instalar e configurar o DRBD no Ubuntu. 
Na apresentação foi utilizado Ubuntu 18 e, embora os passos sejam os mesmos, é necessário atentar-se ao versionamento correto dos pacotes utilizados para evitar quaisquer problemas.

- Adicionando o Repositório do `DRBD`

  - Adicionar o Repo:

```
sudo add-apt-repository ppa:linbit/linbit-drbd9-stack
```
  - Atualizar os Pacotes:

```
sudo apt-get update
```

- Instalando os Pacotes do Repo
  
```
apt install drbd-utils drbd-dkms
```

Em outras versões de Linux, tal qual CentOS o comando acima pode variar:
```
yum install drbd kmod-drbd
```

Ou ainda no Suse Linux:
```
yast -i drbd
```

- Verificando a Instalação

```
sudo drbdadm --version
```

A saída esperada é a versão do DRBD instalada:
```
DRBDADM_VERSION=9.5.0
```

- Editando o Arquivo global_common.conf

Substitua todo o conteúdo do arquivo /etc/drbd.d/global_common.conf por este:

```
global {
   usage-count yes;
}
common {
   net {
      protocol C;
   }
}
```

- Crie o seu Arquivo de Resource

Crie um arquivo em /etc/drbd.d/meuRes.res com o seguinte conteúdo:

```
resource meuRes {
  on server01 {
   device    /dev/drbd0;
   disk      /dev/sdb1;
   address   10.1.1.31:7789;
   meta-disk internal;
  }
  on server02 {
   device    /dev/drbd0;
   disk      /dev/sdb1;
   address   10.1.1.32:7789;
   meta-disk internal;
  }
}
```
Onde:

* `server01` é o nome do servidor primário (saída do comando `uname -n`, no meu caso, server01 e server02 respectivamente)
* `device` é o disco virtual que será utilizado para armazenamento
* `disk` é o disco físico (o HDD) que será utilizado para armazenamento
* `address` é o endereço IP do servidor primário seguido por `:7789` que é a porta default do DRBD
* `meta-disk` é o disco onde o DRBD armazenará sua metadata

- Crie os Metadados

Em ambos os servidores, rode o comando:

```
sudo drbdadm create-md meuRes
```

- Ative o Resource

Em ambos os servidores, rode o comando:

```
sudo drbdadm up  meuRes
```

- Avalie o status

Em ambos os servidores, rode o comando:

```
sudo drbdadm status
```

A saída esperada é:

```
meuRes role:Secondary
  disk:Inconsistent
  bob role:Secondary`
    disk:Inconsistent
```

- Após a sincronia, force o servidor Primário a se tornar o tal:

APENAS no servidor primário, rode o comando:

```
sudo drbdadm primary --force meuRes
````

- Formate o disco virtual no servidor Primário:

APENAS no servidor primário, rode o comando:

```
sudo mkfs.ext4 /dev/drbd0
```

- Teste o disco após a formatação:

APENAS no servidor primário, rode o comando:

```
sudo mount /dev/drbd0 /montagens/drbd0/
```

E avalie se o mesmo está disponível