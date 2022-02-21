# Sensor de gás MQ-2 inflamável e fumaça
O sensor MQ2 possui duas saídas, uma analógica e uma digital. na saída analógica, é detectada a concentração de gás ou fumaça no ambiente. Já a saída digital, é a saída que é configurada pelo pino trimpot, para mudar de valor quando a detecção de gás chegou em um limite (considerado perigoso para o ambiente).

No teste realizado, é colocado no monitor serial o comportamento de ambas as saídas. Para verificar o funcionamento do sensor, foram utilizados um fósforo para simular fumaça e um isqueiro para simular gás inflamável. Segue abaixo como foi o comportamento do sensor:

<img src = "mq2.jpeg" alt = "MQ2 circuito" width = "400" />

Nas imagens abaixo, seguem os resultados que apareceram no monitor serial.

1º: Quando o sensor não está recebendo estímulos / 2°: quando sensor está recebendo fumaça / 3°: quando o monitor está recendo gás
<img src = "mq2_limpo.jpeg" alt = "MQ2 limpo" width = "300" /> <img src = "mq2_fumaca.jpeg" alt = "MQ2 fumaça" width = "310" /> <img src = "mq2_gas.jpeg" alt = "MQ2 gas" width = "330" />







Tanto o código utilizado para realizar o teste quanto o tutorial de montagem, estão no repositório disponibilizado pelos professores neste link:
<https://github.com/LPAE/arduino_tutorial/tree/main/gas>
