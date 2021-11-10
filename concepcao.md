# Concepção

Neste projeto, estaremos realizando a implementação da Domótica em uma maquete, no intúito de exemplificar como é possível utilizar deste recurso para tornar o ambiente mais confortável, prático, seguro e econômico. Para isto, teremos à disposição alguns componentes eletrônicos que serão responsáveis pela automação da residência. Estes componentes poderão realizar as seguintes funcionalidades no ambiente:

- Monitorar a temperatura e a umidade do local, passando estas informações ao usuário, e acionando o condicionador de ar no caso de estar muito calor.
- Controlar a iluminação do ambiente a partir da sensibilidade de presença ou movimentação.
- Detectar vazamento de gás inflamável ou fumaça, abrindo as janelas para evitar o acúmulo, soando um alarme e informado ao usuário o que está acontecendo.
- Disponibilizar o controle do portão eletrônico do local.
- Verificar por proximidade e presença se há alguém no portão aguardando para entrar e acionar a campainha para notificar o usuário. Tornando possível que ele abra ou não o portão no caso de vizitantes.
- Detectar se está chovendo e automaticamente fechar as janelas e notificar o usuário sobre a chuva para que ele lembre de tirar suas roupas do varal.
- Detectar movimentação no local quando o usuário não está presente, caso ocorra, acionar o alarme enviar notificação, no intúito de previnir atividade suspeita na residência.
- Abrir as cortinas ao amanhecer e ativar o despertador, para auxiliar que o usuário acorde na hora que deseja.
- Verificar se o portão está aberto por mais de 5 minutos, para evitar que o usuário deixe aberto por esquecimento e acabe facilitando um cenário de invasão ao local.
- Controlar a iluminação do jardim para que as luzes fiquem acesas somente durante à noite.
- Monitorar a umidade do solo do jardim, para sinalizar quando é necessário ligar o sistema de irrigação. 

### Os componentes eletrônicos em questão estão listados abaixo:


**-Placa MEGA 2560 R3** (Arduino)
Basicamente será o “tradutor” dos sinais, ele irá receber os sinais a partir de suas entradas e, dependendo a entrada que receber o sinal, uma saída específica será ativada.

**-Sensor de Umidade e Temperatura DHT11**
Realiza a aferição da Humidade e da temperatura do ambiente em que está localizado e envia os dados a partir de uma saída serial de uma via.

**-Sensor de presença e movimento PIR**
Faz a leitura de movimentação no ambiente e coloca a saída em “alto” por um curto período de tempo quando detecta movimentação no local.

**-Sensor de gás MQ-2 inflamável e fumaça**
Realiza a leitura da concentração de gás e fumaça no ar, caso esta concentração esteja acima de um limite definido a partir do potenciômetro, ele coloca a saída digital D0 em nível “Alto”

**-Micro Servo SG92R 9g TowerPro** (Motor)
Motor com engrenagens que irá ligar ao receber uma alimentação.

**-Módulo Sensor de Umidade/Nível Água Chuva**
Sensor que indica se o nível de água já chegou a um limite estipulado.

**-Módulo Relé 5 V e um Canal**
Relé que será responsável para ligar o motor.

**-Sensor ultrasônico HC-SR04**
Sensor de distância, para calcular com precisão distâncias entre 2 e 4 cm.

**-Módulo Matriz de LED 8×8 com MAX7219**
Placa de leds que poderá eventualmente mostrar alguma mensagem mais simples.

**-Buzzer passivo**
Será o “alto falante” do sistema, para emitir sons quando se achar conveniente.

**-Display LCD 16×2 I2C Backlight Azul**
Poderá receber alguma mensagem que seja mais longa do arduíno e colcocar na tela.

## Referências
PLACA Mega 2560 R3 Arduino. Disponível em: <https://www.usinainfo.com.br/placas-arduino/placa-mega-2560-r3-arduino-cabo-usb-3629.html>. Acesso em: 02 nov. 2021.
FILIPEFLOP. Filipeflop. Disponível em: <https://www.filipeflop.com/>. Acesso em: 02 nov. 2021.
ELETRÔNICA CASTRO. MODULO SENSOR DE UMIDADE NIVEL AGUA CHUVA ARDUINO. Disponível em: <https://www.eletronicacastro.com.br/arduino/14906-modulo-sensor-de-umidade-nivel-agua-chuva-ardui-0000000149068.html>. Acesso em: 02 nov. 2021.
