# O que é o loki

O Loki é um sistema de agregação de log inspirado no Prometheus. 

Ele foi projetado para ser muito econômico e fácil de operar comparado a outros sistemas de agregação de logs, o Loki indexa um conjunto de rótulos para cada fluxo de logs, em vez de indexar o contexto dos logs.

# Arquitetura

<img src="images/grafana-loki-work.png" height="150">


Como podemos ver na imagem acima temos alguns componentes que precisamos conhecer.

do lado esquerdo nós temos o node, o node  ou nó é uma máquina de processamento físico virtual. Esse nó ele pode ser a uma padrao ou múltiplas máquinas controladas pelo control plaine do cubernets.

dentro desse nó nós temos containers rodando de forma atômica.

e quandomtrmosm esses containers rodando nós temos um fluxo de saida chamada de stdout. 

para quem não conhece o STD out, representa um monitor de saída padrão de interface do usuário e nessa interface nós temos todas as saídas da aplicação em format de texto. 
em outras palavras o famoso println, console.log.

adicionar imagem do console

temos também o promtail  que é um coletor de log feito apenas para o lock,ele utiliza o conceito de service Discovery o mesmo que serviço do Prometheus (ele Auto descobre os logs dos containers) e tem a finalidade de coletar esses logs automaticamente e enviar para o Loki

por fim temos o grandioso Loki que serve para armazenar indexar os logs 
, o Loki é semelhante a
ao elastic City mas de certa forma ele é mais rápido para configurar e possui os mesmos recursos.

para exploração dos logs Nós podemos usar o logql que é a linguagem de consulta do loc e  para visualizar essas consultas tem umas duas opções:o grafana de forma visual um logcli para visualizar pelo terminal (sem sombras de dúvida prefiro o grafana)