## Diminuindo prioridade da memoria swap

Embora não seja obrigatória em micros com um volume suficiente de memória RAM, a partição swap é sempre recomendada, pois permite que o sistema disponha de uma área adicional para situações onde precisa de uma quantidade muito grande de memória RAM, como por exemplo ao editar vídeos.

A propensão do sistema a utilizar memória swap é configurável através de uma opção do kernel, a "vm.swappiness", que aceita valores de 0 a 100, sendo que um valor baixo orienta o sistema a usar swap apenas quando não houver mais memória disponível e um valor mais alto faz com que o sistema a utilize de maneira mais liberal, movendo arquivos e bibliotecas que não estão sendo usados.

O default na maioria das distribuições é "60", o que faz com que o sistema use um pouco de swap mesmo quando tem memória de sobra disponível. Você pode evitar isso alterando o valor para "20" (ou menos, de acordo com o gosto do freguês).

Para isso, execute, como root o comando:

`sysctl vm.swappiness=20`

Para que a alteração se torne permanente, edite o arquivo `/etc/sysctl.conf` e adicione a linha "vm.swappiness=20". Este arquivo contém variáveis para o kernel, que são carregadas durante o boot. 

## Ativa e Desativa o Swap

`swapoff -a` - desabilita "todas" as partições swap em /etc/fstab

`swapon -a` - habilita "todas" as partições swap em /etc/fstab
