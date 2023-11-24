# Projeto de Monitoramento de IoT :hearts: :drop_of_blood: :cloud:

## Descrição da Solução :right_anger_bubble:

Nossa solução emprega Arduino Cloud, uma infraestrutura baseada na nuvem, desenvolvida pela Arduino. Esta plataforma é especialmente concebida para simplificar e otimizar a criação de aplicações no crescente campo da Internet das Coisas (IoT). Um de seus grandes atrativos é a capacidade de conectar dispositivos Arduino à internet com facilidade e eficiência. Além disso, a plataforma permiti a construção de dashboards personalizados. Estes painéis são essenciais para o monitoramento e controle de dispositivos IoT, proporcionando uma visão clara e atualizada dos dados em tempo real, o que é crucial para a tomada de decisões informadas e rápidas.

Planejamos integrar o ESP32 com o sensor MAX30100. Esta combinação tem como objetivo captar informações vitais como batimentos cardíacos e oxigenação do sangue, fundamentais para o monitoramento da saúde em tempo real. O ESP32, um microcontrolador por seu baixo custo e eficiência energética, é amplamente reconhecido em aplicações IoT. Sua versatilidade e desempenho confiável o tornam ideal para nossa aplicação. O sensor MAX30100, por outro lado, é um módulo de alta precisão projetado para monitoramento de sinais biométricos. Ele capta com precisão as variações do volume sanguíneo na microvasculatura, permitindo a análise de batimentos cardíacos e os níveis de oxigênio no sangue.

Essa combinação entre o ESP32 e o MAX30100 não apenas garante a coleta eficiente de dados biométricos, mas também facilita a integração desses dados com a plataforma Arduino Cloud. Isso abre caminho para a análise avançada de dados de saúde e a possibilidade de intervenções rápidas em situações críticas, transformando o modo como monitoramos e reagimos às necessidades de saúde em tempo real.

## Link Youtube 💻:
[Youtube]()
Precisa Terminar

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

### 4- Selecionar o Dispositivo

<div align="center">
    <img height src="https://cdn.discordapp.com/attachments/946052411984842782/1177402934456238090/image.png?ex=657260ee&is=655febee&hm=b26207f3793f9f3d3ca63ae2cb053ee401cbb010cef6569fbfd875aaa11c26bc&"/>
</div>

### 5- Configurar o Dispositivo

<div style="display: flex; justify-content: space-around;">
    <img src="https://cdn.discordapp.com/attachments/946052411984842782/1177403381191540836/image.png?ex=65726159&is=655fec59&hm=79e7fe3ee453735d1e6becc19bc4c5ac25469cd15d187ba57a2b3f9b74c6eb64&" alt="Imagem 1" style="width: 30%; height: auto;">
    <img src="https://cdn.discordapp.com/attachments/946052411984842782/1177403864916439052/image.png?ex=657261cc&is=655feccc&hm=7f4c5f0b5608e2e572deec3aa07928730f99f6e73f3cbd81572156fadf804b42&" alt="Imagem 2" style="width: 30%; height: auto;">
    <img src="https://cdn.discordapp.com/attachments/946052411984842782/1177404052158558238/image.png?ex=657261f9&is=655fecf9&hm=c2a698ab8140a9dbeef40a3386c7c9c615ea6546fccf27ec91c6b7f465bd7625&" alt="Imagem 3" style="width: 30%; height: auto;">
</div>

### 6- Salvar a Secret Key

<div align="center">
    <img height src="https://cdn.discordapp.com/attachments/946052411984842782/1177404350923030598/image.png?ex=65726240&is=655fed40&hm=37ca8ec283fae52dbfdac4e924c114a1f92e47eb5216a64a5d3a71238261bbf6&"/>
</div>

### 7- Configurar a rede do ESP32
<div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177405392750399488/image.png?ex=65726338&is=655fee38&hm=5b77ce79794d8b4817ae008e27ca12fc39e19abc9f885f9eee55a0d3ec9b1053&=&format=webp&width=1282&height=701"/>
</div>

### 8- Colocar `nome` e `senha` da rede wifi e a `secret key`
<div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177405694182441041/image.png?ex=65726380&is=655fee80&hm=f673de7ef38df70c0707bff1b1ee6b228bdf7d2359e2db226fac6766fc216c9f&=&format=webp"/>
</div>

### 9- Ir em Sketch e adicionar o código

<div align="center">
    <img height src="https://cdn.discordapp.com/attachments/946052411984842782/1177406559031140352/image.png?ex=6572644e&is=655fef4e&hm=4d746fbcb941ffdbfe010ce767d87ff78f5b21d4d0da5cd7c633459fb71dcd2f&"/>
</div>

### 10- Instalar o [Arduino Create Agent](https://support.arduino.cc/hc/en-us/articles/360014869820-Install-the-Arduino-Create-Agent)

### 11- Conectar o circuito ao computador/notebook

### 12- Passar o código para o `ESP32`

<div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177407627404263584/image.png?ex=6572654d&is=655ff04d&hm=af1e9f6aa9265766d9bb538df6e162c29a361ebe865ae47479004492a0888572&=&format=webp"/>
</div>

**Obervação:** Logo após clicar no botão de passar o código, segure o botão `BOOT` no ASP32 até o código ser passado 100%

### 13- Criação do DashBoard - Na página Home clique em `Dashboards`

<div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177409772853674004/image.png?ex=6572674d&is=655ff24d&hm=78cc4c902cd24fdf5619cf9f38eabecc445a04f84d7cadc72d2daa84fd3ade6a&=&format=webp"/>
</div>

### 14- Clicar em `Create DASHBOARD`

<div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177410300039921674/image.png?ex=657267ca&is=655ff2ca&hm=e103ba0f9bf34e3ecb11990a58bb8882007f9df0c05897a0b6e84ee8f2a1650e&=&format=webp"/>
</div>

### 15- Adicionar Gauge(batimentos cardíacos) e Percentage(oxigenação do sangue)

<div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177410657495294012/image.png?ex=6572681f&is=655ff31f&hm=627cae0f953bfe90151e67a150dcf9afe3ea4913f5abb83283fea051d3b73479&=&format=webp"/>
</div>

### 16- Devemos configurar o Gauge e Percentage linkando com as variáveis que criamos anteriormente

<div align="center">
    <img height src="https://cdn.discordapp.com/attachments/946052411984842782/1177411280395571210/image.png?ex=657268b4&is=655ff3b4&hm=7f286c3909089bb317c843ec6703bf86bd9ac65751ad8fa21a31603eecf18e64&"/>
</div>

<div align="center">
    <img height src="https://media.discordapp.net/attachments/946052411984842782/1177411885344243753/image.png?ex=65726944&is=655ff444&hm=feb84fe9aa4121938e3c432f07d8891f99a1c5f0fa993728ca7bf4b29263f294&=&format=webp&width=1257&height=701"/>
</div>

### Se você quiser entender como o código funcionar assista o vídeo disponiblizado no início deste documento.
### Caso tenha dúvidas específicas, você pode acessar a documentação do [Arduino Cloud](https://docs.arduino.cc/arduino-cloud/)
















