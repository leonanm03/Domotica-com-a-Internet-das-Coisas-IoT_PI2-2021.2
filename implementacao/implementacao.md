# Implementação

Para dar continuidade ao projeto à partir das ideias que temos na [concepção](./concepcao.md) e das necessidades apontadas em [design](./design.md), se torna necessário agora "por a mão na massa" e começar a concretizar nosso protótipo utilizando os componentes eletrônicos disponíveis, programando eles na IDE do arduino e posicionando eles no nosso modelo.


## Testes

Antes de efetivamente colocar os componentes no modelo, também é necessário qu sejam realizados testes, para se familiarizar com com eles e verificar se estão funcionando de forma correta. Logo, para cada componente isolado, foi utilizado um codigo de programção na IDE do arduino para verificar o seu funcionamento.

Seguem abaixo os links de acesso para a testagem de cada componente:

- [Módulo Sensor de Umidade/Nível Água Chuva](testes/agua_sensor/agua_sensor.md)
- [Buzzer passivo](testes/buzzer/buzzer.md)
- [Sensor de Umidade e Temperatura DHT11](testes/dht11/dht11.md)
- [Sensor de presença e movimento PIR](testes/pir/pir.md)
- [Sensor de gás MQ-2 inflamável e fumaça](testes/mq2/mq2.md)
- [Micro Servo SG92R 9g TowerPro](testes/servo/servo.md)
- [Módulo Relé 5 V](testes/rele/rele.md)
- [Sensor ultrasônico HC-SR04](testes/hcsr04/hcsr04.md)
- [Módulo Matriz de LED 8×8](testes/matriz_led/matriz_led.md)
- [Display LCD](testes/display_lcd/display_lcd.md)
- [LDR](testes/ldr/ldr.md)

## Interações

Após testar os componentes do projeto e entender melhor como eles funcionam, foi possível então começar a fazer com que eles se relacionassem, criando novos códigos e adicionando tarefas na IDE do arduíno para que a partir de alguns comandos enviados pela comunicação serial ou então pela leitura dos sensores, acabassem ocasionando em respostas em outros componentes, como por exemplo um LED acendendendo, uma mensagem na matriz de LEDs ou no display LCD, um servo abrindo um portão ou janela, ou o buzzer emitindo o alarme sonoro. Abaixo estão listadas as interações que foram individualmente construídas e testadas.

- [Sistema de detecção de chuva](interacoes/chuva/chuva.md)
 
 
 
## Referências
