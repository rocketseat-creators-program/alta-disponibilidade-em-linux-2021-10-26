# alta-disponibilidade-em-linux-2021-10-26
Uma abordagem DevOps utilizando DRBD e Heartbeat para atingir alta disponibilidade


![](https://storage.googleapis.com/golden-wind/experts-club/capa-github.svg)

# Alta Disponibilidad em Linux

Nessa aula vamos aprender a utilizar o `DRBD` (Distributed Replicated Block Device) e `Heartbeat` para alcançar Alta Disponibilidade em um ambiente Linux.

![high availability logo](.github/assets/Heartbeat.png)

![drbd logo](.github/assets/DRBD.png)


Utilizaremos como base um sistema que depende pesadamente na estrutura de arquivos, sendo que para seu perfeito funcionamento todos os arquivos devem estar íntegros e sincronizados entre os computadores elegíveis para substituição em caso de falha catastrófica ou indisponibilidade do computador Primário.

## Exemplos

A aula irá mostrar como instalar e configurar o `DRBD` para que os computadores tenham um ou mais discos sincronizados a qualquer tempo:

- [Instalando e Configurando DRBD](./examples/drbd.md)

Posteriormente, mostrará como instalar e configurar o `Heartbeat` para executar serviços ao detectar a perda do computador Primário:

- [Instalando e Configurando Heartbeat](./examples/heartbeat.md)

## Referências
- https://linbit.com/drbd/
- http://www.linux-ha.org/wiki/Heartbeat
- https://wiki.ubuntu.com/UbuntuHighAvailabilityTeam/Heartbeat
- https://ubuntu.com/server/docs/ubuntu-ha-drbd

## Expert

| [<img src="https://avatars.githubusercontent.com/u/4620702?v=4" width="75px">](https://github.com/alexvenom) |
| :-: |
| [Alex Kusmenkovsky](https://github.com/alexvenom) |
