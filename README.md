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



