# Projeto de Monitoramento de IoT :hearts: :drop_of_blood: :cloud:

## Descrição da Solução :right_anger_bubble:

Nossa solução irá utilizar a plataforma Arduino Cloud que é uma plataforma baseada na nuvem desenvolvida pela Arduino para facilitar 
a criação de aplicações de Internet das Coisas (IoT), a plataforma permite conectar facilmente dispositivos Arduino à internet, também, 
permite a criação de dashboards personalizados para monitorar e controlar dispositivos IoT, incluindo a visualização de dados em tempo real.
  
  Com isso, iremos conectar o Esp32 com o sensor MAX30100 para captar batimentos cardíacos e oxigenação do sangue, assim conseguiremos
monitorar a saúde de uma pessoa em tempo real.

  O ESP32 é um microcontrolador de baixo custo e com baixo consumo de energia, amplamente utilizado em aplicações de Internet das Coisas (IoT),
  com isso conseguiremos utilizar ele juntamente com o MAX30100.

  ## Arquitetura do Projeto 	:triangular_ruler:


  <div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177374432348610630/image.png?ex=65724663&is=655fd163&hm=561a0a09c818b197c3d558caff8e1017f85a4235f34763e71d64a7232416a17e&=&format=webp&width=1289&height=701"/>
  </div>

  ## Esquema Eletrônico :zap:

  <div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177380602564071544/image.png?ex=65724c22&is=655fd722&hm=af240b35b77d954645a69bf20365e6333fdcb9b955e4e755e103c81ec6a013d0&=&format=webp"/>
</div>

## Código-fonte :copyright::heavy_plus_sign::heavy_plus_sign:

  ```cpp
  #include "thingProperties.h"
#include <Wire.h>
#include "MAX30100_PulseOximeter.h"

#define REPORTING_PERIOD_MS     1000

PulseOximeter pox;

uint32_t tsLastReport = 0;

void onBeatDetected()
{
  Serial.println("Beat!");
}

void setup()
{
  Serial.begin(115200);
  initProperties();
  ArduinoCloud.begin(ArduinoIoTPreferredConnection);
  setDebugMessageLevel(2);
  ArduinoCloud.printDebugInfo();

  Serial.print("Initializing pulse oximeter..");

  if (!pox.begin()) {
    Serial.println("FAILED");
    for (;;);
  } else {
    Serial.println("SUCCESS");
  }

  pox.setOnBeatDetectedCallback(onBeatDetected);
}

void loop()
{
  ArduinoCloud.update();
  pox.update();

  if (millis() - tsLastReport > REPORTING_PERIOD_MS) {
    
    oxigenio = pox.getSpO2();
    batimentos = pox.getHeartRate();
    
    tsLastReport = millis();
  }
}

```

## Tutorial Arduino Cloud :page_with_curl:

<div align="center">
    <img height src="https://user-images.githubusercontent.com/77038120/174824422-29e21c5a-0a78-4424-ac6a-cf206aeb0d26.png"/>
</div>

### 1- Deverá criar uma conta no [Arduino Cloud](https://cloud.arduino.cc/)

### 2- Clicar em Things e depois em Create Thing
 <div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177399999177437344/image.png?ex=65725e32&is=655fe932&hm=8bce01ccdccce80a933b582a9a8fc8cf655de1c90fa617382a02a4887f4f04e5&=&format=webp&width=1385&height=701"/>
</div>

### 3- Adicionar as Variáveis

<div style="display: flex; justify-content: space-around;">
    <img src="https://cdn.discordapp.com/attachments/946052411984842782/1177401557642387546/image.png?ex=65725fa6&is=655feaa6&hm=4e0d8e6a7a4f4110c4c03536f0187bd6bec6b23b448f4d9bc18f6828f8d16b1e&" alt="Imagem 1" style="width: 30%; height: auto;">
    <img src="https://cdn.discordapp.com/attachments/946052411984842782/1177401655977844796/image.png?ex=65725fbd&is=655feabd&hm=761885c132737025bb71bf6320cf3a34211e1a5aca47f1b5cc864b3d80d65442&" alt="Imagem 2" style="width: 30%; height: auto;">
    <img src="https://cdn.discordapp.com/attachments/946052411984842782/1177401795471999006/image.png?ex=65725fdf&is=655feadf&hm=458c6385a5ec2c8ca7acc34d9a5a5a317854eb53f206f5e2132480becadf84cc&" alt="Imagem 3" style="width: 30%; height: auto;">
</div>



