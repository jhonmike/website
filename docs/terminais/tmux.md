# TMUX

    quando ler <Prefix> será o padrão Ctrl + b

## Window

    <Prefix> c => para criar uma nova 'window'
    <Prefix> % => para dividir uma 'window' verticalmente
    <Prefix> " => para dividir uma 'window' horizontalmente
    <Prefix> & => para fechar uma 'window'
    <Prefix> . => move a window para algum indice ainda não utilizado
    <Prefix> 0 a 9 => abre a 'window' com o numero digitado
    <Prefix> w => abre a 'window' a partir de uma lista
    <Prefix> n => (next) abre a próxima 'window'
    <Prefix> p => (previous) abre a 'window' anterior

## Pane (split window)

    <Prefix> x => para fechar uma 'pane', pergunta de confirmação
    <Prefix> ! => para fechar uma 'pane', SEM pergunta de confirmação
    <Prefix> o => para ir para a proxima 'pane'
    <Prefix> Up ou Left ou Right ou Down => para ir para a 'pane' 'apontada'
    <Prefix> z => maximiza a 'pane' atual, se executar novamente volta a exibir as outras
    <Prefix> { ou } => mover 'pane' de lugar
    <Prefix> ; => alterana para a ultima pane
    <Prefix> Alt(segura) setas; => redimensiona o tamanho da pane

## Commands

    <Prefix> : => para digitar o comando a ser executado, ex: 'kill-window'
    <Prefix> f => procura o termo em todas as 'window' e 'panes'
    <Prefix> d => 'destaca' da sessão, mas mantem a sessão aberta
    <Prefix> ? => exibe uma lista de comandos que podem ser utilizados

## Sessions

para listar sessões:

tmux list-sessions

para retornar a uma sessão aberta:

tmux attach -t 0  #=> Ou o nome da sessão

para destruir uma session:

tmux kill-session -t 0  #=> Ou o nome da sessão

para destruir TODAS as sessões abertas:

tmux kill-server

para criar uma nova sessão (new-session ou alias new):

tmux new -s nome-da-sess
