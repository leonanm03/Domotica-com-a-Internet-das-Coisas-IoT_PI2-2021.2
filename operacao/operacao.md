# Operação
Neste momento, após passar por todas as etapas do projeto, é possível agora ver tudo funcionando. 

Esta é uma lista das funcionalidades do projeto.
- O sistema possiu 4 botões que são enviados pelo teclado do computador ao sistema:

  - **a** - a letra "a" é o botão responsável resetar as informações responsáveis mostrar mensagens e acionar alarmes no sistema. Logo, ao receber um som de campainha e não querer abrir o portão, pode-se usar esta opção para desligar o alarme. Outro momento interessante para o uso desta letra, é quando o sistema de vazamento de gás dispara o alarme. Como a intenção é fazer com que usuário vá verificar o vazamento de gás, os alarmes são irão parar após apertar esta letra.
  - **d** - com a letra "d", é possível ativar/desativar o despertador do sistema, o qual irá despertarao ao amanhecer, abrindo janelas, enviando uma mensagem de "BOM DIA" ao usuário, e ativando o alarme.
  - **j** - "j" é a letra responsável por abrir/fechar a janela, sem depender que um evento específico aconteça para que consiga abrir ou fechar.
  - **p** - "p" é a opção do portão, mandando este comando, o portão irá abrir ou fechar. Caso a campainha tenha sido ativada no portão, ao se apertar esta letra, o alarme também será desligado, para desativar a campainha. 
- Há um sistema de monitaramento de gás, quando a concentração de gás no local chega a uma nível considerado perigoso ao sistema, a janela é automaticamente aberta, para evitar o acúmulo do gás, o alarme é acionado, e tanto na matriz de LED quando no display LCD, estarão aparecendo mensagens que alertam sobre o gás. Para desativar estas mensagens, é necessário parar o vazamento para depois desligar os alarmes com a letra "a".
- Junto com o portão, há dois sensores, um de proximidade e um de presença. Quando há presença no portão, e essa presença está próxima o suficiente para se garantir que seja um visitante, o alarme é acionado para funcionar como campainha, para informar ao usuário que há visitas. Desta forma o usuário pode ou não abrir o portão com a letra "p", ou então apenas parar a campainha com a letra "a".
- Na parte da sala, há outro detector de presença, que serve para acender a luz quando é detectada presença/movimentação no local.
- No quarto, há um sistema de controle de temperadura, há um sensor de umidade e tempetura que está sendo utilizado para monitorar a temperatura do ambiente, quando a temperatura passa de um limite que foi configurado no código do sistema, o arduíno manda um sinal para o relé fazer com que o cooler(ar condicionado) seja ligado.
- Na rua fica um sensor de água, o qual será responsável por realizar o monitoramento de quando está chovendo. Quando chover, o sensor irá detectar a água e entõ irá fechar a janela, para que não molhe dentro de casa, aciona uma luz de alerta próximo ao display LCD, que mostrará uma mensagem de alerta sobre a chuva.
- Também na área externa fica um sensor de luminosidade, que irá fazer com que a luz do jardim acenda quando está escuro (de noite). E apaga a mesma luz quando amanhece. caso o despertador estiver ativado e estiver de dia, a janela abre, a matriz de led mostra uma mensagem de bom dia, e o alarme começa a tocar.

Segue abaixo um vídeo demonstrativo com todos os sistemas funcionando.

Link do vídeo: https://youtu.be/kQbmYIa3Vvo
