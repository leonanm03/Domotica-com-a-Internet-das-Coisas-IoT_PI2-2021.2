# Implementação

Para dar continuidade ao projeto à partir das ideias que temos na [concepção](../concepcao.md) e das necessidades apontadas em [design](../design/design.md), se torna necessário agora "por a mão na massa" e começar a concretizar nosso protótipo utilizando os componentes eletrônicos disponíveis, programando eles na IDE do arduino e posicionando eles no nosso modelo.


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
- [Sistema de campainha e controle do portão](interacoes/controle_portao/controle_portao.md)
- [Sistema de controle de temperatura](interacoes/temperatura/temperatura.md)
- [Sistema de monitoramento de gás](interacoes/gas/gas.md)
- [Iluminação por detecção de movimento](interacoes/luz/luz.md)
- [Depertador e iluminação do jardim](interacoes/sensor_luz/sensor_luz.md)
 
 
 ## Código final
 Após a implementação de todas as interações. Chegou então o momento e colocar todas juntas em um único código para ter o sistema inteiro funcionando. Realizando estas junções, apareceu também a necessidade de fazer alguns ajustes no código afim evitar alguns bugs e priorizar algumas tarefas. Montando o circuito da mesma forma que a [maquete eletrônica](../design/mqt_eletronica.png), e utilizando o código disponibilizado abaixo. teremos então todo o projeto funcionando.
 
 código final:
 ```C
 // Definição dos pinos
#define PINO_SENSOR_AGUA A1   // pino sensor agua
#define LUZ_AGUA 52           // pino LED da agua
#define JANELAPIN 10          // pino servo janela
#define PINO_BUZZER 12        // pino buzzer
#define PORTAOPIN 11          // pino servo portao
#define pirPORTAO 53          // pino sensor PIR portao
#define pino_trigger 49       // trigger portao HC-SR04
#define pino_echo 51          // echo portao HC-SR04
#define DHT11PIN A0           // pino DHT11
#define DHTTYPE  DHT11        // modelo DHT
#define RELE 13               // pino relé
#define pirCASA 47            // pino sensor PIR interno 
#define LUZ_CASA 50           // pino LED iluminação interna
#define ENTRADA_GAS A3        // pino do sensor de gás
#define MCLK  2               // CLK matriz de led
#define MCS 3                 // CS matriz de led
#define MDIN 23               // DIN matriz de led
#define PINO_LDR A2           // pino sensor LDR
#define LED_JARDIM 48         // pino LED do jardim

//bibliotecas
#include <Servo.h> // servo motor
#include <Wire.h> // LCD
#include <LiquidCrystal_I2C.h> // LCD
#include <Ultrasonic.h>  // sensor ultrassonico
#include <DHT.h> //  DHT
#include <MatrizLed.h> // matriz de led

MatrizLed matriz; // nomeando matriz de led
Servo JANELA; // Nomeando o servo da janela
Servo PORTAO; // Nomeando o servo do portao
LiquidCrystal_I2C lcd(0x27, 20, 4);// Define o endereço LCD para 0x27 para um display de 16 caracteres e 2 linhas
Ultrasonic ultrasonic(pino_trigger, pino_echo); // Inicializa o sensor de proximidade
DHT dht(DHT11PIN, DHTTYPE);   // inicializa o DHT11

#define DEBUG // testes de funcionamento do código

/*  Variáveis para daterminar a frequência
    que as tarefas irão ler os dados,
    "periodo" : o tempo em que vai ler novamente
    "tempo" :  marca o momento da última leitura*/
const unsigned long periodo_tarefa_agua = 1000;     // sensor de agua
unsigned long tempo_tarefa_agua = millis();
const unsigned long periodo_tarefa_buzzer = 1000;  // buzzer
unsigned long tempo_tarefa_buzzer = 0;
const unsigned long periodo_tarefa_temp = 1000;  // controle de temperatura
unsigned long tempo_tarefa_temp = millis();
const unsigned long periodo_tarefa_gas = 500;  // vazamento de gás
unsigned long tempo_tarefa_gas = millis();
const unsigned long periodo_tarefa_portao = 1000; // abre/fecha portão
unsigned long tempo_tarefa_portao = millis();
const unsigned long periodo_tarefa_janela = 1000;  // abre/fecha janela
unsigned long tempo_tarefa_janela = millis();
const unsigned long periodo_tarefa_visita = 500;  // presença no portão
unsigned long tempo_tarefa_visita = millis();
const unsigned long periodo_tarefa_lcd = 2000;    // lcd
unsigned long tempo_tarefa_lcd = millis();
const unsigned long periodo_tarefa_luzdm = 500;  // luz por detecção de movimento
unsigned long tempo_tarefa_luzdm = millis();
const unsigned long periodo_tarefa_matriz = 2000; // matriz de led
unsigned long tempo_tarefa_matriz = millis();
const unsigned long periodo_tarefa_ldr = 1000;    // sensor LDR
unsigned long tempo_tarefa_ldr = millis();

/* Variáveis de manipulação das tarefas */
int posp = 0;                // posição servo portão
int pos = 0;                 // posição servo janela
bool abre_portao = false;    // portao aberto/fechado
bool abre_janela = false;    // janela aberta/fechada
bool ligar_LCD = false;      // ligar LCD
bool chuva = false;          // define se está chovendo
int estado_alarme = LOW;     // variável do alarme
bool ligar_alarme = false;   // variável do alarme
int aSensor;                 // sensor de gás
bool ligar_matriz = false;   // ligar a matriz de LED
bool gas = false;            // tarefas relacionadas ao gás
bool despertador = false;    // despertador no LDR
bool dia = false;            // indica se é dia ou noite

// Função setup é executada apenas uma vez
void setup() {
  Serial.begin(9600);  // inicia a comunicação serial a 9600 bits por segundo
  while (!Serial);

  /* configurando entradas de dados digitais */
  pinMode (pirPORTAO, INPUT);     // PIR do portão
  pinMode (pirCASA, INPUT);       // PIR interno

  /* configurando saídas de dados*/
  pinMode (LUZ_AGUA, OUTPUT);     // LED da agua
  pinMode (PINO_BUZZER, OUTPUT);  // Buzzer
  pinMode (RELE, OUTPUT);         // Relé
  pinMode (LUZ_CASA, OUTPUT);     // LED interno
  pinMode (LED_JARDIM, OUTPUT);   // LED do jardim

  JANELA.attach(JANELAPIN);       //Porta onde o servo da janela está conectado
  PORTAO.attach(PORTAOPIN);       //Porta onde o servo do portão está conectado

  digitalWrite(PINO_BUZZER, HIGH ); // inicia o buzzer desligado

  lcd.init();       // inicializa o LCD
  lcd.backlight();  // liga a luz de fundo

  dht.begin();      // inicializa a classe do dht

  matriz.begin(MDIN, MCLK, MCS, 2); // dataPin, clkPin, csPin, numero de matrizes de 8x8
  matriz.rotar(false);              // Caso ocorra falha

  PORTAO.write(posp);  //coloca o portão na posição inicial (fechado)
  JANELA.write(pos);   // coloca a janela na posição inicial (fechada)
}


/* Função loop() é responsável por escalonar as tarefas.
   Essa função é executada eternamente enquanto o Arduino estiver  energizado */
void loop() {
  tarefa_buzzer();
  tarefa_portao();
  tarefa_janela();
  tarefa_matriz();
  tarefa_lcd();
  tarefa_ldr();
  tarefa_agua();
  tarefa_visita();
  tarefa_temp();
  tarefa_luzdm();
  tarefa_gas();
  tarefa_serial();
}

void tarefa_agua() {
  unsigned long tempo_atual = millis();
  int valorSensor;

  if (tempo_atual - tempo_tarefa_agua > periodo_tarefa_agua) {
    tempo_tarefa_agua = tempo_atual;

    valorSensor = analogRead(PINO_SENSOR_AGUA);

#ifdef DEBUG
    Serial.print("Agua : ");  // printa na tela o valor que o sensor de água está enviando
    Serial.println(valorSensor);
#endif

    if (valorSensor > 300) {    // Quando detecta água (300) abre janela, mostra no LCD e acende o LED da agua
      digitalWrite(LUZ_AGUA, HIGH);
      ligar_LCD = true;
      abre_janela = false;
      chuva = true;
    }
    else {  // se não, desliga o LED da água e tira a mensagem do LCD
      digitalWrite(LUZ_AGUA, LOW);
      chuva = false;
    }
  }
}

void tarefa_serial() {  // comandos do teclado para manipulação do sistema

  /* Caso tenha recebido algum dado do PC */
  if (Serial.available()) {
    char dado_recebido = Serial.read();

    /* Depuração */
    Serial.print("Recebido:");
    Serial.println(dado_recebido);

    if (dado_recebido == 'a') { // reseta as variáveis relacionadas a, matriz, lcd e buzzer
      if (ligar_alarme == true)
        ligar_alarme = false;
      if (ligar_matriz == true)
        ligar_matriz = false;
      if (gas == true)
        gas = false;
      if (despertador == true)
        despertador = false;
    }

    if (dado_recebido == 'p') {// abre ou fecha o portão
      if (ligar_alarme == true)
        ligar_alarme = false;
      if (abre_portao == true)
        abre_portao = false;
      else
        abre_portao = true;
    }

    if (dado_recebido == 'j') { // abre ou fecha a janela
      if (abre_janela == true)
        abre_janela = false;
      else
        abre_janela = true;
    }
    if (dado_recebido == 'd') { // liga ou desliga o despertador
      if (despertador == true) {
        despertador = false;
        Serial.println("despertador desativado");
      }
      else {
        despertador = true;
        Serial.println("despertador ativado");
      }
    }

  }
}

void tarefa_portao() {  // abre o fecha o portão de acordo com a variável global "abre_portao"
  unsigned long tempo_atual = millis();

  /* Hora de executar essa tarefa */
  if (tempo_atual - tempo_tarefa_portao > periodo_tarefa_portao) {
    tempo_tarefa_portao = tempo_atual;

    if (abre_portao == true) {
      if (posp < 90) {
        for (posp = 0; posp <= 90; posp += 1) {
          // Troca de posição
          PORTAO.write(posp);
          // Aguarda 10 ms
          delay(10);
        }
      }
    }
    else {
      if (posp > 0) {
        for (posp = 90; posp >= 0; posp -= 1) {
          // Troca de posição
          PORTAO.write(posp);
          // Aguarda 10 ms
          delay(10);
        }
      }
    }
  }
}

void tarefa_janela() {  // abre ou fecha a janela de acordo com a variável "abre_janela"

  unsigned long tempo_atual = millis();

  /* Hora de executar essa tarefa */
  if (tempo_atual - tempo_tarefa_janela > periodo_tarefa_janela) {
    tempo_tarefa_janela = tempo_atual;

    if (abre_janela == true) {
      if (pos < 180) {
        for (pos = 0; pos <= 180; pos += 1) {
          // Troca de posição
          JANELA.write(pos);
          // Aguarda 10 ms
          delay(10);
        }
      }
    }
    else {
      if (pos > 0) {
        for (pos = 180; pos >= 0; pos -= 1) {
          // Troca de posição
          JANELA.write(pos);
          // Aguarda 10 ms
          delay(10);
        }
      }
    }
  }
}

void tarefa_visita() {    // ativa o alarme quando há alguém próximo ao portão
  unsigned long tempo_atual = millis();

  /* Hora de executar essa tarefa */
  if (tempo_atual - tempo_tarefa_visita > periodo_tarefa_visita) {
    tempo_tarefa_visita = tempo_atual;
    
    float cmMsec;
    cmMsec = ultrasonic.distanceRead(CM);

    // Se houver movimento próximo do portão
    if ((digitalRead(pirPORTAO) == HIGH) && cmMsec <= 5 ) {
      // Enviar para monitor serial
      Serial.println("Presença detectada no portão");
      ligar_alarme = true;
    }
  }
}

void tarefa_lcd() { // mostra as mensagens de gás ou chuva de acordo com a prioridade

  unsigned long tempo_atual = millis();

  /* Hora de executar essa tarefa */
  if (tempo_atual - tempo_tarefa_lcd > periodo_tarefa_lcd) {
    tempo_tarefa_lcd = tempo_atual;

    if (ligar_LCD == true) {
      lcd.clear();
      if ( gas == true) { // tarefa de gás tem maior prioridade
        lcd.setCursor(1, 0);
        // Imprime uma mensagem no LCD
        lcd.print("Vazamento de");
        // Posição do cursor
        lcd.setCursor(1, 1);
        // Imprime uma mensagem no LCD
        lcd.print("gas, cuidado!");
      }
      else if ( chuva == true) {
        lcd.setCursor(1, 0);
        // Imprime uma mensagem no LCD
        lcd.print("Chuva, olhe a");
        // Posição do cursor
        lcd.setCursor(1, 1);
        // Imprime uma mensagem no LCD
        lcd.print("roupa no varal!");
      }
    }
  }
}

void tarefa_buzzer() { // manipulação do buzzer pela variável "ligar_alarme"

  unsigned long tempo_atual = millis();

  /* Hora de executa essa tarefa */
  if (tempo_atual - tempo_tarefa_buzzer > periodo_tarefa_buzzer) {
    tempo_tarefa_buzzer = tempo_atual;

    if (ligar_alarme == true) {

      if (estado_alarme == HIGH) {
        estado_alarme = LOW;
        tone(PINO_BUZZER, 2000);
      }
      else {
        estado_alarme = HIGH;

        /* Depende do Buzzer:
           Se acionado com NPN, use apenas noTone(PINO_BUZZER);
           Se acionado com PNP, use abaixo para deixar o pino em nível alto.
            noTone(PINO_BUZZER);
            digitalWrite(PINO_BUZZER, HIGH);
        */
        noTone(PINO_BUZZER);
        digitalWrite(PINO_BUZZER, HIGH);
      }
    }
    else {
      noTone(PINO_BUZZER);
      digitalWrite(PINO_BUZZER, HIGH);
    }
  }
}

void tarefa_temp() { // tarefa responsável por ligar o cooler quando a temperatura está alta
  unsigned long tempo_atual = millis ();

  //Hora de enviar os dados caso tenha passado 2000 ms
  if (tempo_atual - tempo_tarefa_temp > periodo_tarefa_temp) {

    tempo_tarefa_temp = tempo_atual;
    // Lê a temperatura em Celsius
    float t = dht.readTemperature();

    // Verifica se alguma leitura falhou
    if (isnan(t)) {
      Serial.println(F("Falha ao ler o sensor DHT!"));
      return;
    }

#ifdef DEBUG
    Serial.print("temperatura celsius : ");
    Serial.println(t);
#endif

    // Envia para o computador (serial) os dados
    if (t > 28)
      digitalWrite(RELE, LOW);
    else
      digitalWrite(RELE, HIGH);
  }
}

void tarefa_luzdm() { // liga/desliga de acordo com a detecção de movimento

  unsigned long tempo_atual = millis();

  /* Hora de executar essa tarefa */
  if (tempo_atual - tempo_tarefa_luzdm > periodo_tarefa_luzdm) {

    tempo_tarefa_luzdm = tempo_atual;

    // Se houver movimento
    if (digitalRead(pirCASA) == HIGH)
      digitalWrite(LUZ_CASA, HIGH); // acender luz
    else
      digitalWrite(LUZ_CASA, LOW); //apagar luz
  }
}

void tarefa_gas() { // tarefa responsável pelo monitoramento de gás
  unsigned long tempo_atual = millis ();

  //Hora de enviar os dados caso tenha passado 1000 ms
  if (tempo_atual - tempo_tarefa_gas > periodo_tarefa_gas) {
    tempo_tarefa_gas = tempo_atual;

    // Faz a leitura do sensor
    aSensor = analogRead(ENTRADA_GAS);


#ifdef DEBUG
    Serial.print("Leitura do gas: ");
    Serial.println(aSensor);
    Serial.println();
#endif

    if (aSensor >= 400) {   // quando há uma concentração perigosa (400) de gás.
      ligar_alarme = true;  
      ligar_matriz = true;
      ligar_LCD = true;
      gas = true;           // ativa mensagem de gás
      abre_janela = true;

    }
  }
}

void tarefa_matriz() {  // gerencia as mensagens da matriz "GAS" e "BOM DIA" de acordo com prioridade
  unsigned long tempo_atual = millis ();

  //Hora de enviar os dados caso tenha passado 1000 ms
  if (tempo_atual - tempo_tarefa_matriz > periodo_tarefa_matriz) {
    tempo_tarefa_matriz = tempo_atual;

    matriz.borrar();
    if (ligar_matriz == true) {
      if (gas == true) {
        matriz.escribirFraseScroll("GAS", 65);
      }
      else if ((despertador == true) && (dia == true) ) {
        matriz.escribirFraseScroll("BOM DIA", 65);
      }
    }
  }
}

void tarefa_ldr() { // tarefa do sensor de luz
  unsigned long tempo_atual = millis();

  int valorLDR;

  /* Hora de enviar os dados analógicos caso tenha passado 1000 ms */
  if (tempo_atual - tempo_tarefa_ldr > periodo_tarefa_ldr) {
    tempo_tarefa_ldr = tempo_atual;

    valorLDR = analogRead(PINO_LDR);

#ifdef DEBUG
    Serial.print("Valor : ");
    Serial.println(valorLDR);
#endif

    //Acender o LED de acordo com o valor da entrada analógica
    if (valorLDR < 400) {  //  liga o led do jardim se a claridade for baixa (400)
      dia = false;
      digitalWrite(LED_JARDIM, HIGH);
    }
    else { // desliga o led se a claridade estiver adequada
      dia = true;
      digitalWrite(LED_JARDIM, LOW);
      ligar_matriz = true;

      if (despertador == true) {  // se o despertador está ativado, abre a janela e liga o alarme
        abre_janela = true;
        ligar_alarme = true;
      }
    }
  }
} 
 ```
