# Scrot

### Screenshot da tela toda
manda para ~/imagens/shots e mostra com o gpicview

`scrot '%Y-%m-%d-%H:%M:%S_$wx$h_screenshot.png' -e 'mv $f ~/imagens/shots/; gpicview ~/imagens/shots/$f'`

### Seleciona a área com o mouse

`scrot -s '%Y-%m-%d-%H:%M:%S_$wx$h_screenshot.png' -e 'mv $f ~/imagens/shots/; gpicview ~/imagens/shots/$f' `
