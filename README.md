# 🌡️ Projeto de Monitoramento Ambiental com Arduino 🛠️

Este projeto é um sistema de monitoramento ambiental baseado em Arduino Uno, que coleta e exibe dados de temperatura, umidade e luminosidade. Os dados são apresentados em um display LCD e armazenados na EEPROM para consulta futura. O sistema também inclui alertas visuais e sonoros para indicar condições fora dos limites aceitáveis.

## 🛠️ Componentes Utilizados

- **Microcontrolador:** Arduino Uno.
- **Sensores:**
  - 🌡️ DHT22 (Temperatura e Umidade).
  - 💡 LDR (Sensor de Luminosidade).
- **Atuadores:**
  - 🟢 LEDs (Vermelho, Amarelo, Verde).
  - 🔊 Buzzer.
- **Exibição:** 🖥️ Display LCD 16x2 com interface I2C.
- **Armazenamento:**
  - 💾 EEPROM.
  - 🕒 RTC DS1307 para registro de data e hora.
- **Outros:**
  - 🔘 Pushbutton para alternância de exibição.
  - 🔌 Resistores de 1000Ω para LEDs.
  - 🧵 Breadboard para conexão dos componentes.

## 🚀 Funcionalidades

O sistema realiza a leitura de dados de temperatura, umidade e luminosidade por meio dos sensores DHT22 e LDR, respectivamente. Esses dados são processados e exibidos em um display LCD 16x2, permitindo ao usuário alternar entre as informações de temperatura, umidade e luminosidade utilizando um botão. O sistema também emite alertas visuais (através de LEDs) e sonoros (com um buzzer) quando os valores medidos estão fora dos limites pré-definidos. Além disso, os dados são armazenados na EEPROM sempre que estão fora dos limites, permitindo a consulta histórica dos registros.

## 📊 Especificações Técnicas Detalhadas

### 🌡️ Sensor DHT22 (Temperatura e Umidade)
- **Função:** Mede temperatura e umidade do ambiente.
- **Faixa de medição:**
  - Temperatura: -40°C a 80°C.
  - Umidade: 0% a 100%.
- **Precisão:**
  - Temperatura: ±0,5°C.
  - Umidade: ±2 a 5%.
- **Conexão:** Utiliza pino digital 9 do Arduino.

### 💡 Sensor LDR (Luminosidade)
- **Função:** Mede a intensidade da luz ambiente.
- **Faixa de medição:** 0% a 100% (mapeado de 0 a 1023).
- **Conexão:** Pino A0 do Arduino.

### 🖥️ Display LCD 16x2 (Interface I2C)
- **Função:** Exibir informações coletadas dos sensores.
- **Interface:** Comunicação via I2C.
- **Conexão:**
  - SDA: Pino A4.
  - SCL: Pino A5.

### 🟢 LEDs Indicadores
- **Função:** Alertar visualmente sobre as condições ambientais.
- **Cores e Significado:**
  - Verde: Condições normais.
  - Amarelo: Temperatura fora do intervalo seguro (15°C a 25°C).
  - Vermelho: Umidade fora do intervalo seguro (30% a 60%).
- **Conexões:**
  - LED Verde: Pino 6.
  - LED Amarelo: Pino 7.
  - LED Vermelho: Pino 8.

### 🔊 Buzzer
- **Função:** Emitir alertas sonoros quando valores ultrapassam os limites estabelecidos.
- **Conexão:** Pino 13 do Arduino.

### 🔘 Botão (Pushbutton)
- **Função:** Alternar entre as medições exibidas no LCD.
- **Conexão:** Pino 12 do Arduino.

### 💾 EEPROM (Memória não volátil)
- **Função:** Armazenar dados críticos quando fora dos limites estabelecidos.
- **Capacidade:** Até 100 registros.
- **Estrutura de armazenamento:**
  - 10 bytes por registro, contendo:
    - Timestamp (data e hora).
    - Temperatura.
    - Umidade.
    - Luminosidade.

### 🕒 RTC DS1307 (Relógio de Tempo Real)
- **Função:** Registrar data e hora de cada leitura.
- **Interface:** Comunicação via I2C.
- **Conexão:**
  - SDA: Pino A4.
  - SCL: Pino A5.

## 🧠 Lógica de Funcionamento

Durante a inicialização, o sistema exibe uma mensagem no LCD e toca uma melodia no buzzer. O RTC é configurado para a data e hora atuais. Em seguida, o sistema realiza a leitura de dados dos sensores a cada segundo, calculando a média das últimas 5 leituras para temperatura e umidade. Os dados são exibidos no LCD, com a possibilidade de alternar entre as informações utilizando o botão. Caso os valores estejam fora dos limites, os alertas são acionados, com os LEDs e o buzzer indicando a condição anormal. Os dados fora dos limites são armazenados na EEPROM com um timestamp para consulta futura.

## 📚 Bibliotecas Utilizadas

- **LiquidCrystal_I2C:** Responsável pelo controle do display LCD 16x2 via comunicação I2C.
- **DHT:** Biblioteca utilizada para fazer a leitura dos dados do sensor DHT22.
- **RTClib:** Gerencia o módulo RTC DS1307.
- **EEPROM:** Permite o armazenamento dos dados coletados na memória não volátil do Arduino.

## 🔌 Esquema de Conexões

O esquema de conexões define a interligação dos componentes ao Arduino Uno, assegurando a comunicação correta entre sensores, atuadores e módulos.

- **I2C:** Utilizado pelo display LCD 16x2 e RTC DS1307 nos pinos A4 (SDA) e A5 (SCL).
- **Pinos Digitais:** Controlam LEDs, buzzer e botão de navegação.
- **Pinos Analógicos:** Leitura do sensor LDR para medir luminosidade.

## 📖 Manual de Operação

O manual de operação fornece instruções sobre como instalar, configurar e utilizar o sistema de monitoramento ambiental. Ele também inclui orientações para solução de problemas e manutenção preventiva.

## 💻 Código Fonte

O código fonte do projeto está disponível no arquivo `Liquid_I2c.ino`. Ele inclui a lógica de funcionamento do sistema, a leitura dos sensores, a exibição dos dados no LCD e o armazenamento na EEPROM.

### Código Comentado

```cpp
// Inclusão das bibliotecas necessárias
#include <Wire.h> // Biblioteca para comunicação I2C
#include <RTClib.h> // Biblioteca para o módulo RTC DS1307
#include <LiquidCrystal_I2C.h> // Biblioteca para o display LCD 16x2 com I2C
#include <DHT.h> // Biblioteca para o sensor DHT22
#include <EEPROM.h> // Biblioteca para a EEPROM

// Definição dos pinos e constantes
#define DHTPIN 9 // Pino onde o DHT22 está conectado
#define DHTTYPE DHT11 // Tipo de sensor DHT
#define LOG_OPTION 1 // Opção para ativar a leitura do log
#define SERIAL_OPTION 0 // Opção de comunicação serial: 0 para desligado, 1 para ligado
#define UTC_OFFSET 0.83333333 // Ajuste de fuso horário para UTC-3

#define col 16 // Número de colunas do display LCD
#define lin 2 // Número de linhas do display LCD
#define ende 0x27 // Endereço I2C do display LCD

// Definição das notas musicais para a melodia de inicialização
#define NOTE_B0  31
#define NOTE_C1  33
#define NOTE_CS1 35
#define NOTE_D1  37
#define NOTE_DS1 39
#define NOTE_E1  41
#define NOTE_F1  44
#define NOTE_FS1 46
#define NOTE_G1  49
#define NOTE_GS1 52
#define NOTE_A1  55
#define NOTE_AS1 58
#define NOTE_B1  62
#define NOTE_C2  65
#define NOTE_CS2 69
#define NOTE_D2  73
#define NOTE_DS2 78
#define NOTE_E2  82
#define NOTE_F2  87
#define NOTE_FS2 93
#define NOTE_G2  98
#define NOTE_GS2 104
#define NOTE_A2  110
#define NOTE_AS2 117
#define NOTE_B2  123
#define NOTE_C3  131
#define NOTE_CS3 139
#define NOTE_D3  147
#define NOTE_DS3 156
#define NOTE_E3  165
#define NOTE_F3  175
#define NOTE_FS3 185
#define NOTE_G3  196
#define NOTE_GS3 208
#define NOTE_A3  220
#define NOTE_AS3 233
#define NOTE_B3  247
#define NOTE_C4  262
#define NOTE_CS4 277
#define NOTE_D4  294
#define NOTE_DS4 311
#define NOTE_E4  330
#define NOTE_F4  349
#define NOTE_FS4 370
#define NOTE_G4  392
#define NOTE_GS4 415
#define NOTE_A4  440
#define NOTE_AS4 466
#define NOTE_B4  494
#define NOTE_C5  523
#define NOTE_CS5 554
#define NOTE_D5  587
#define NOTE_DS5 622
#define NOTE_E5  659
#define NOTE_F5  698
#define NOTE_FS5 740
#define NOTE_G5  784
#define NOTE_GS5 831
#define NOTE_A5  880
#define NOTE_AS5 932
#define NOTE_B5  988
#define NOTE_C6  1047
#define NOTE_CS6 1109
#define NOTE_D6  1175
#define NOTE_DS6 1245
#define NOTE_E6  1319
#define NOTE_F6  1397
#define NOTE_FS6 1480
#define NOTE_G6  1568
#define NOTE_GS6 1661
#define NOTE_A6  1760
#define NOTE_AS6 1865
#define NOTE_B6  1976
#define NOTE_C7  2093
#define NOTE_CS7 2217
#define NOTE_D7  2349
#define NOTE_DS7 2489
#define NOTE_E7  2637
#define NOTE_F7  2794
#define NOTE_FS7 2960
#define NOTE_G7  3136
#define NOTE_GS7 3322
#define NOTE_A7  3520
#define NOTE_AS7 3729
#define NOTE_B7  3951
#define NOTE_C8  4186
#define NOTE_CS8 4435
#define NOTE_D8  4699
#define NOTE_DS8 4978
#define PAUSE 0

int tempo = 144; // Tempo da melodia

// Inicialização dos objetos
DHT dht(DHTPIN, DHTTYPE); // Inicializa o sensor DHT
LiquidCrystal_I2C lcd(ende, col, lin); // Inicializa o display LCD
RTC_DS1307 RTC; // Inicializa o módulo RTC

// Variáveis globais
float lastAvgTemp; // Armazena a última média de temperatura
float lastAvgHumd; // Armazena a última média de umidade
float tempReadings[5]; // Array para armazenar as últimas 5 leituras de temperatura
float humdReadings[5]; // Array para armazenar as últimas 5 leituras de umidade
int currentIndex = 0; // Índice para rastrear a posição atual no array de leituras
int lightLevel; // Nível de luz
int valorLDR; // Valor lido do LDR

// Definição dos pinos
const int pinLDR = A0; // Pino do LDR
const int ledRed = 8; // Pino do LED vermelho
const int ledYel = 7; // Pino do LED amarelo
const int ledGre = 6; // Pino do LED verde
const int pinBUZ = 13; // Pino do buzzer
const int pinBut = 12; // Pino do botão

// Configurações da EEPROM
const int maxRecords = 100; // Número máximo de registros
const int recordSize = 10; // Tamanho de cada registro em bytes
int startAddress = 0; // Endereço inicial da EEPROM
int endAddress = maxRecords * recordSize; // Endereço final da EEPROM
int currentAddress = 0; // Endereço atual da EEPROM

int lastLoggedMinute = -1; // Último minuto registrado

// Triggers de temperatura, umidade e luminosidade
float trigger_t_min = 15.0; // Valor mínimo de temperatura
float trigger_t_max = 25.0; // Valor máximo de temperatura
float trigger_u_min = 30.0; // Valor mínimo de umidade
float trigger_u_max = 60.0; // Valor máximo de umidade
float trigger_l_min = 0.0; // Valor mínimo de luminosidade
float trigger_l_max = 30.0; // Valor máximo de luminosidade

int contador = 0; // Contador para alternar entre as telas

// Função de setup
void setup() {
  Serial.begin(115200); // Inicia a comunicação serial

  lcd.init(); // Inicia o display LCD
  lcd.backlight(); // Liga a luz de fundo do display
  lcd.clear(); // Limpa o display

  dht.begin(); // Inicia o sensor DHT

  Wire.begin(); // Inicia a comunicação I2C

  if (!RTC.begin()) { // Verifica se o RTC está conectado
    Serial.println("Erro: RTC não encontrado.");
    while (1);
  }

  if (!RTC.isrunning()) { // Verifica se o RTC está funcionando
    Serial.println("RTC não está rodando, ajustando para a data/hora compilada...");
    RTC.adjust(DateTime(F(__DATE__), F(__TIME__))); // Ajusta o RTC para a data e hora da compilação
  }

  EEPROM.begin(); // Inicia a EEPROM

  pinMode(pinBUZ, OUTPUT); // Configura o pino do buzzer como saída

  // Configura os pinos dos LEDs como saída
  pinMode(ledRed, OUTPUT);
  pinMode(ledYel, OUTPUT);
  pinMode(ledGre, OUTPUT);

  pinMode(pinLDR, INPUT); // Configura o pino do LDR como entrada
  pinMode(pinBut, INPUT_PULLUP); // Configura o pino do botão como entrada com pull-up

  booting(); // Exibe a mensagem de inicialização
  delay(1000); // Aguarda 1 segundo
  lcd.clear(); // Limpa o display
  lcd.setCursor(3, 0);
  lcd.print("Loading..."); // Exibe uma mensagem de carregamento
  delay(2000); // Aguarda 2 segundos
  lcd.clear(); // Limpa o display
  lcd.setCursor(0, 0);
  lcd.print("Hold the button"); // Exibe uma mensagem para segurar o botão

  // Limpa a EEPROM
  for (int i = 0; i < EEPROM.length(); i++) {
    EEPROM.write(i, 0); // Escreve 0 em cada posição da EEPROM
  }
}

// Função principal de loop
void loop() {
  int valorLDR = analogRead(pinLDR); // Lê o valor do LDR
  lightLevel = map(valorLDR, 1023, 500, 0, 100); // Converte o valor do LDR para porcentagem

  float temp = dht.readTemperature(); // Lê a temperatura
  float humid = dht.readHumidity(); // Lê a umidade

  verifyLDR(); // Verifica o nível de luz e ajusta os LEDs

  // Verifica se os LEDs de alerta podem ser desligados
  if (digitalRead(ledYel) == HIGH || digitalRead(ledRed) == HIGH && valorLDR < 30) {
    if (15 < lastAvgTemp && lastAvgTemp < 25) {
      digitalWrite(ledYel, LOW); // Desliga o LED amarelo
    }
    if (30 < lastAvgHumd && lastAvgHumd < 60) {
      digitalWrite(ledRed, LOW); // Desliga o LED vermelho
    }
  }

  // Verifica se o buzzer pode ser desligado
  if (digitalRead(ledYel) == LOW && digitalRead(ledRed) == LOW && valorLDR < 30) {
    noTone(pinBUZ); // Desliga o buzzer
  }

  // Armazena as leituras atuais nos arrays
  tempReadings[currentIndex] = temp;
  humdReadings[currentIndex] = humid;

  currentIndex = (currentIndex + 1) % 5; // Atualiza o índice para a próxima leitura

  if (currentIndex == 4) {
    tenthRead(); // Calcula a média das últimas 5 leituras
    show_data(); // Exibe os dados no LCD
  }

  DateTime now = RTC.now(); // Obtém a data e hora atual

  // Ajusta o fuso horário
  int offsetSeconds = UTC_OFFSET * 3600; // Converte horas para segundos
  now = now.unixtime() + offsetSeconds; // Adiciona o deslocamento ao tempo atual
  DateTime adjustedTime = DateTime(now); // Converte o novo tempo para DateTime

  if (LOG_OPTION) get_log(); // Exibe o log se a opção estiver ativa

  // Verifica se o minuto atual é diferente do último minuto registrado
  if (adjustedTime.minute() != lastLoggedMinute) {
    lastLoggedMinute = adjustedTime.minute();

    // Verifica se os valores estão fora dos limites
    if (temp < trigger_t_min || temp > trigger_t_max || humid < trigger_u_min || humid > trigger_u_max || lightLevel < trigger_l_min || lightLevel > trigger_l_max) {
      // Converte os valores para inteiros
      int tempInt = (int)(temp * 100);
      int humiInt = (int)(humid * 100);
      int lumiInt = (int)(lightLevel * 100);

      // Armazena os dados na EEPROM
      EEPROM.put(currentAddress, now.unixtime());
      EEPROM.put(currentAddress + 4, tempInt);
      EEPROM.put(currentAddress + 6, humiInt);
      EEPROM.put(currentAddress + 8, lumiInt);

      // Atualiza o endereço para o próximo registro
      getNextAddress();
    }
  }

  if (SERIAL_OPTION) {
    // Exibe a data e hora no monitor serial
    Serial.print(adjustedTime.day());
    Serial.print("/");
    Serial.print(adjustedTime.month());
    Serial.print("/");
    Serial.print(adjustedTime.year());
    Serial.print(" ");
    Serial.print(adjustedTime.hour() < 10 ? "0" : ""); // Adiciona zero à esquerda se hora for menor que 10
    Serial.print(adjustedTime.hour());
    Serial.print(":");
    Serial.print(adjustedTime.minute() < 10 ? "0" : ""); // Adiciona zero à esquerda se minuto for menor que 10
    Serial.print(adjustedTime.minute());
    Serial.print(":");
    Serial.print(adjustedTime.second() < 10 ? "0" : ""); // Adiciona zero à esquerda se segundo for menor que 10
    Serial.print(adjustedTime.second());
    Serial.print("\n");
  }

  delay(1000); // Aguarda 1 segundo antes da próxima leitura
}

// Função para calcular a média das últimas 5 leituras
void tenthRead() {
  float sumTemp = 0;
  float sumHumd = 0;

  for (int i = 0; i < 5; i++) {
    sumTemp += tempReadings[i];
    sumHumd += humdReadings[i];
  }

  lastAvgTemp = sumTemp / 5;
  lastAvgHumd = sumHumd / 5;
}

// Função para exibir os dados no LCD
void show_data() {
  if (digitalRead(pinBut) == LOW) {
    delay(25); // Debounce do botão

    if (contador == 3) {
      contador = 0; // Reinicia o contador
    }

    contador += 1; // Incrementa o contador
  }

  if (contador == 1) {
    show_temp(); // Exibe a temperatura
  } else if (contador == 2) {
    show_humid(); // Exibe a umidade
  } else if (contador == 3) {
    show_light(); // Exibe a luminosidade
  }
}

// Função para exibir a temperatura no LCD
void show_temp() {
  if ((lastAvgTemp < 15 || lastAvgTemp > 25) && lastAvgTemp != 0) {
    digitalWrite(ledYel, HIGH); // Liga o LED amarelo
    digitalWrite(ledGre, LOW); // Desliga o LED verde
    tone(pinBUZ, 500); // Toca o buzzer
  }
  lcd.clear(); // Limpa o display
  term_image(lastAvgTemp); // Exibe a imagem correspondente à temperatura
  lcd.setCursor(4, 0);
  lcd.print("Temp: " + String(lastAvgTemp, 1) + String((char)223) + "C"); // Exibe a temperatura
  loop(); // Retorna ao loop principal
}

// Função para exibir a umidade no LCD
void show_humid() {
  if ((lastAvgHumd < 30 || lastAvgHumd > 60) && lastAvgHumd != 0) {
    digitalWrite(ledRed, HIGH); // Liga o LED vermelho
    digitalWrite(ledGre, LOW); // Desliga o LED verde
    tone(pinBUZ, 400); // Toca o buzzer
  }
  lcd.clear(); // Limpa o display
  humid_image(lastAvgHumd); // Exibe a imagem correspondente à umidade
  lcd.setCursor(4, 0);
  lcd.print("Humid: " + String(lastAvgHumd, 1) + "%"); // Exibe a umidade
}

// Função para exibir a luminosidade no LCD
void show_light() {
  lcd.clear(); // Limpa o display
  light_image(); // Exibe a imagem correspondente à luminosidade
  lcd.setCursor(4, 0);
  lcd.print("Light: " + String(lightLevel) + "%"); // Exibe a luminosidade
}

// Função para exibir a imagem correspondente à temperatura no LCD
void term_image(float temp) {
  byte term0x1[] = { B00000, B00000, B00000, B00010, B00001, B00000, B00010, B00001 };
  byte term0x2[] = { B00000, B00100, B01010, B01010, B01110, B01010, B01110, B01010 };
  byte term0x3[] = { B00000, B00000, B00000, B01000, B10000, B00000, B01000, B10000 };
  byte term1x1[] = { B00000, B00000, B00100, B00010, B00000, B00000, B00000, B00000 };
  byte term1x2[] = { B01110, B01010, B01010, B10001, B10001, B10001, B01110, B00000 };
  byte term1x3[] = { B00000, B00000, B00100, B01000, B00000, B00000, B00000, B00000 };

  if (temp < 15) {
    messageTemplate("Low"); // Exibe "Low" para temperatura baixa
  } else if (temp > 25) {
    messageTemplate("High"); // Exibe "High" para temperatura alta
  } else {
    messageTemplate("OK"); // Exibe "OK" para temperatura normal
  }

  lcd.createChar(0, term0x1);
  lcd.setCursor(0, 0);
  lcd.write(0);

  lcd.createChar(1, term0x2);
  lcd.setCursor(1, 0);
  lcd.write(1);

  lcd.createChar(2, term0x3);
  lcd.setCursor(2, 0);
  lcd.write(2);

  lcd.createChar(3, term1x1);
  lcd.setCursor(0, 1);
  lcd.write(3);

  lcd.createChar(4, term1x2);
  lcd.setCursor(1, 1);
  lcd.write(4);

  lcd.setCursor(2, 1);
  lcd.write(5);
}

// Função para exibir a imagem correspondente à umidade no LCD
void humid_image(float humid) {
  byte humid0x2[] = { B00000, B00000, B00100, B00100, B01110, B01110, B11111, B11111 };
  byte humid1x1[] = { B00001, B00001, B00001, B00001, B00000, B00000, B00000, B00000 };
  byte humid1x2[] = { B11111, B11111, B11111, B11111, B11111, B01110, B00000, B00000 };
  byte humid1x3[] = { B10000, B10000, B10000, B10000, B00000, B00000, B00000, B00000 };

  if (humid < 30) {
    messageTemplate("Low"); // Exibe "Low" para umidade baixa
  } else if (humid > 60) {
    messageTemplate("High"); // Exibe "High" para umidade alta
  } else {
    messageTemplate("OK"); // Exibe "OK" para umidade normal
  }

  lcd.createChar(0, humid0x2);
  lcd.setCursor(1, 0);
  lcd.write(0);

  lcd.createChar(1, humid1x1);
  lcd.setCursor(0, 1);
  lcd.write(1);

  lcd.createChar(2, humid1x2);
  lcd.setCursor(1, 1);
  lcd.write(2);

  lcd.createChar(3, humid1x3);
  lcd.setCursor(2, 1);
  lcd.write(3);
}

// Função para exibir a imagem correspondente à luminosidade no LCD
void light_image() {
  byte lamp0x1[] = { B00000, B00000, B00001, B00000, B00000, B00000, B00000, B00011 };
  byte lamp0x2[] = { B00000, B00000, B00001, B10000, B00011, B00111, B01111, B01111 };
  byte lamp0x3[] = { B00000, B00000, B10001, B00010, B11000, B11100, B11110, B11110 };
  byte lamp0x4[] = { B00000, B00000, B00000, B00000, B00000, B00000, B00000, B11000 };
  byte lamp1x1[] = { B00000, B00000, B00000, B00000, B00001, B00000, B00000, B00000 };
  byte lamp1x2[] = { B01111, B00111, B00011, B10011, B00011, B00000, B00000, B00000 };
  byte lamp1x3[] = { B11110, B11100, B11000, B11001, B11000, B00000, B00000, B00000 };
  byte lamp1x4[] = { B00000, B00000, B00000, B00000, B10000, B00000, B00000, B00000 };

  if (lightLevel >= 0 && lightLevel < 30) {
    messageTemplate("OK"); // Exibe "OK" para luminosidade normal
  } else if (lightLevel >= 30) {
    messageTemplate("High"); // Exibe "High" para luminosidade alta
  } else {
    messageTemplate("Low"); // Exibe "Low" para luminosidade baixa
  }

  lcd.createChar(0, lamp0x1);
  lcd.setCursor(0, 0);
  lcd.write(0);

  lcd.createChar(1, lamp0x2);
  lcd.setCursor(1, 0);
  lcd.write(1);

  lcd.createChar(2, lamp0x3);
  lcd.setCursor(2, 0);
  lcd.write(2);

  lcd.createChar(3, lamp0x4);
  lcd.setCursor(3, 0);
  lcd.write(3);

  lcd.createChar(4, lamp1x1);
  lcd.setCursor(0, 1);
  lcd.write(4);

  lcd.createChar(5, lamp1x2);
  lcd.setCursor(1, 1);
  lcd.write(5);

  lcd.createChar(6, lamp1x3);
  lcd.setCursor(2, 1);
  lcd.write(6);

  lcd.createChar(7, lamp1x4);
  lcd.setCursor(3, 1);
  lcd.write(7);
}

// Função para verificar o nível de luz e ajustar os LEDs
void verifyLDR() {
  if (lightLevel >= 30) {
    digitalWrite(ledRed, HIGH); // Liga o LED vermelho
    digitalWrite(ledGre, LOW); // Desliga o LED verde

    if (lastAvgTemp >= 15 && lastAvgTemp <= 25) {
      digitalWrite(ledYel, LOW); // Desliga o LED amarelo
      noTone(pinBUZ); // Desliga o buzzer
    }
    tone(pinBUZ, 600); // Toca o buzzer
  } else if (lightLevel < 50 && lightLevel >= 30) {
    digitalWrite(ledYel, HIGH); // Liga o LED amarelo
    digitalWrite(ledGre, LOW); // Desliga o LED verde

    if (lastAvgHumd >= 30 && lastAvgHumd <= 60) {
      digitalWrite(ledRed, LOW); // Desliga o LED vermelho
      noTone(pinBUZ); // Desliga o buzzer
    }
    tone(pinBUZ, 400); // Toca o buzzer
  } else {
    if (lastAvgHumd >= 30 && lastAvgHumd <= 60 && lastAvgTemp >= 15 && lastAvgTemp <= 25) {
      digitalWrite(ledGre, HIGH); // Liga o LED verde
      digitalWrite(ledRed, LOW); // Desliga o LED vermelho
      digitalWrite(ledYel, LOW); // Desliga o LED amarelo
      noTone(pinBUZ); // Desliga o buzzer
    }
  }
}

// Função para exibir uma mensagem no LCD
void messageTemplate(String msg) {
  lcd.setCursor(4, 1);
  lcd.print(msg);
}

// Função para atualizar o endereço da EEPROM
void getNextAddress() {
  currentAddress += recordSize;
  if (currentAddress >= endAddress) {
    currentAddress = 0; // Volta para o começo se atingir o limite
  }
}

// Função para exibir o log armazenado na EEPROM
void get_log() {
  Serial.println("Data stored in EEPROM:");
  Serial.println("Timestamp\t\tTemperature\tHumidity\tLuminosity");

  for (int address = startAddress; address < endAddress; address += recordSize) {
    long timeStamp;
    int tempInt, humiInt, lumiInt;

    // Ler dados da EEPROM
    EEPROM.get(address, timeStamp);
    EEPROM.get(address + 4, tempInt);
    EEPROM.get(address + 6, humiInt);
    EEPROM.get(address + 8, lumiInt);

    // Converter valores
    float temperature = tempInt / 100.0;
    float humidity = humiInt / 100.0;
    float luminosity = lumiInt / 100.0;

    // Verificar se os dados são válidos antes de imprimir
    if (timeStamp != 0xFFFFFFFF && temperature != 0 && humidity != 0 && luminosity != 0) {
      DateTime dt = DateTime(timeStamp);
      Serial.print(dt.timestamp(DateTime::TIMESTAMP_FULL));
      Serial.print("\t");
      Serial.print(temperature);
      Serial.print(" C\t\t");
      Serial.print(humidity);
      Serial.print(" %\t\t");
      Serial.print(luminosity);
      Serial.println(" %");
    }
  }
}
```

## 🎮 Simulação
O projeto pode ser simulado no Wokwi. O esquema de conexões e o código estão disponíveis no arquivo diagram.json.

## 🏁 Considerações Finais
Este projeto fornece uma solução robusta para monitoramento ambiental, com capacidade de alerta e registro histórico. Ele pode ser facilmente expandido para incluir mais sensores ou funcionalidades, como envio de dados para a nuvem ou integração com outros sistemas.

## 👥 Autores
- Aline Cristina Ribeiro de Barros – RA: 081230021
- Luis Gustavo de Oliveira Carneiro – RA: 081230029
- Roger Rocha da Silva – RA: 081230045
- João Victor Pereira Andrade – RA: 081230010
- Ezequiel Rodrigues Pereira – RA: 081230008

Março, 2025.
