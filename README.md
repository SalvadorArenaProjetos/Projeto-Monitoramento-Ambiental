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
  - Vermelho: Umidade fora do intervalo seguro (30% a 50%).
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

Durante a inicialização, o sistema exibe uma mensagem no LCD e toca uma melodia no buzzer. O RTC é configurado para a data e hora atuais. Em seguida, o sistema realiza a leitura de dados dos sensores a cada segundo, calculando a média das últimas 10 leituras para temperatura e umidade. Os dados são exibidos no LCD, com a possibilidade de alternar entre as informações utilizando o botão. Caso os valores estejam fora dos limites, os alertas são acionados, com os LEDs e o buzzer indicando a condição anormal. Os dados fora dos limites são armazenados na EEPROM com um timestamp para consulta futura.

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

O código fonte do projeto está disponível no arquivo `sketch.ino`. Ele inclui a lógica de funcionamento do sistema, a leitura dos sensores, a exibição dos dados no LCD e o armazenamento na EEPROM.

## 🎮 Simulação

O projeto pode ser simulado no Wokwi. O esquema de conexões e o código estão disponíveis no arquivo `diagram.json`.

## 🏁 Considerações Finais

Este projeto fornece uma solução robusta para monitoramento ambiental, com capacidade de alerta e registro histórico. Ele pode ser facilmente expandido para incluir mais sensores ou funcionalidades, como envio de dados para a nuvem ou integração com outros sistemas.

## 👥 Autores

- Aline Cristina Ribeiro de Barros – RA: 081230021
- Luis Gustavo de Oliveira Carneiro – RA: 081230029
- Roger Rocha da Silva – RA: 081230045
- João Victor Pereira Andrade – RA: 081230010
- Ezequiel Rodrigues Pereira – RA: 081230008

Março, 2025
