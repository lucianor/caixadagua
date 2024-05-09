# Objetivo
Fazer um sensor de medida de volume de caixa d'água, incluindo compras dos itens, instalação, configuração e rodar. O projeto original é do [Fórum HomeAssistant Brasil](https://homeassistantbrasil.com.br/t/medindo-o-volume-da-caixa-de-agua/3668)

## Pré-Requisitos
- [ ] Conexão de internet com sinal Wi-Fi chegando até a caixa d'água
- [ ] Ponto de energia elétrica junto a caixa d'água
## Itens necessários
- [ ] **NodeMCU ESP32** que irá realizar a leitura desse sensor, calcular o volume e expor esses dados no HomeAssistant
- [ ] **Sensor ultrassônico** (JSN-SR04T ou JSN-Y2-1) para emitir um pulso que irá medir a distância da tampa da caixa até o nível da água
- [ ] Fios DuPont fêmea para fêmea para ligar o NodeMCU ao sensor ultrâssonico
- [ ] Carregador de celular simples para dar energia ao NodeMCU e cabo micro USB
- [ ] Furadeira e serra-copo 5mm para fazer os furos do sensor na tampa da caixa
- [ ] Caixa hermética para colocar os itens acima caso a caixa d'água seja externa
- [ ] Opcional: cola quente para fixar o sensor na tampa.
### Instalação e configuração
- [ ] Carregar o software no nodeMCU através de um cabo USB
- [ ] Instalar o sensor na tampa da caixa d'agua
- [ ] Ligar os fios do sensor ao nodeMCU, e do carregador do nodeMCU a ele
- [ ] Fazer a instalação do tailscale para monitoramento remoto

# Passo 1 - Itens

## AliExpress
- [NodeMCU](https://www.aliexpress.com/item/1005005977505151.html?spm=a2g0o.productlist.main.1.6b881568z6HXO3&algo_pvid=3d980182-4747-48c2-b768-fa3630e00195&algo_exp_id=3d980182-4747-48c2-b768-fa3630e00195-0&pdp_npi=4%40dis%21BRL%2136.71%2111.38%21%21%2149.52%2115.35%21%4021032dcb17151818189177583ec215%2112000035141219305%21sea%21BR%211907945182%21&curPageLogUid=LMfOcIXKtVlR&utparam-url=scene%3Asearch%7Cquery_from%3A)
- [Sensor Ultrassônico](https://www.aliexpress.com/item/1005005995118143.html?spm=a2g0o.productlist.main.3.49f632dbFzaqVy&algo_pvid=622ac9e1-a2b9-4503-874d-5cb7d8005776&algo_exp_id=622ac9e1-a2b9-4503-874d-5cb7d8005776-1&pdp_npi=4%40dis%21BRL%2123.38%2115.54%21%21%2131.54%2120.96%21%402101e9ec17151819516342394edb60%2112000035288838952%21sea%21BR%211907945182%21&curPageLogUid=ZOIxaVGXOsXz&utparam-url=scene%3Asearch%7Cquery_from%3A)
- [Cabos DuPont](https://www.aliexpress.com/item/32661309874.html?spm=a2g0o.productlist.main.3.6155hPfbhPfbO3&algo_pvid=62220dc5-d55b-4ef1-ab40-244b6155c782&algo_exp_id=62220dc5-d55b-4ef1-ab40-244b6155c782-1&pdp_npi=4%40dis%21BRL%216.54%216.54%21%21%211.22%211.22%21%402101f04d17151822093485980ef3c8%2166291977410%21sea%21BR%211907945182%21&curPageLogUid=nYAbEDcfU2aV&utparam-url=scene%3Asearch%7Cquery_from%3A)

# Passo 2 - Instalação e Configuração

## NodeMCU

O primeiro passo é fazer o carregamento do software ao nodeMCU, para que ao você chegar para a instalação completa, basta ligar a tomada e pronto. Para isso, será necessário instalar o software esphome
1) Instalar o Python no Windows
2) Abrir um prompt de comando do Windows e digitar. Irá mostrar algumas mensagens mas ao final deverá exibir ,,,,,,,,
```
pip install tornado esptool
```
2) Carregar o dashboard do esphome:
```
esphome dashboard config --open-ui
```
3) No navegador que abrir, clicar no botão "+ Add Device" ao topo e direita
4) Conectar um cabo micro USB ao NodeMCU e ao computador onde você instalou o esphome
5) Selecionar "Physically connected to this device" - se o seu dispositivo não for encontrado, procure um outro cabo microUSB, nem todos possuem a função de transmitir dados além de energia
6) Isso irá abrir uma nova configuração de dispositivo onde você irá carregar esse código. Altere a sua RedeWifi e SenhaWifi. O demais pode deixar como está.
```
substitutions:
  #Configurações da placa:
  Plataforma: ESP8266
  TipoPlaca: d1_mini
  
  #Configurações do dispositivo:
  hostname: 'caixaagua' #Hostname do dispositivo na rede
  PrefixoNome: "Caixa Água - "
  
  #Configurações da Rede:
  RedeWifi: !secret RedeWifi #Nome da rede wifi que o dispositivo irá se conectar
  SenhaWifi: !secret  SenhaWifi #Senha da rede wifi que o dispositivo irá se conectar
  SenhaWifiReconfig: !secret SenhaWifiReconfig #Senha do AP Wifi para reconfiguração do wifi do dispositivo
  EndConfig: ${hostname}.local #192.168.1.50 #Endereço para configuração (IP que o esp está acessível atualmente na rede), especialmente usado quando quer alterar o hostname via OTA
  WifiOculto: 'False' #Se a rede wifi está oculta defina como 'True'
  WifiFastConnect: 'False' #Se o esp realizará a conexão à rede wifi sem escanear as redes disponíveis defina como 'True'

  #Senhas
  SenhaAPI: !secret SenhaAPI #Senha para adicionar o esp no HA
  SenhaOTA: !secret SenhaOTA #Senha para atualizar o firmware do esp via OTA
  
  #Pinos do Sensor
  SensorTriggerPin: D6
  SensorEchoPin: D7
  
  #INFORMAÇÕES FÍSICAS DA CAIXA E DA INSTALAÇÃO DO SENSOR (medidas em metros):
  #Diametro interno da base da caixa (fundo)
  DD1: "0.95"
  #Diametro interno da borda superior sem a tampa
  DM: "1.22"
  #Distância entre o fundo da caixa e a borda sem a tampa 
  HB: "0.58"
  #Distância entre o fundo da caixa e a borda superior da tampa (onde o sensor será instalado)
  HT: "0.72"
  #Distância entre o fundo da caixa e o nível da base da tubulação de saída (altura da coluna de água do nível morto)
  HMT: "0.038"
  #Distância entre o fundo da caixa e o nível máximo normal (altura da água onde a boia é fechada)
  HM: "0.54"
  #Distância entre o sensor e o nível máximo normal (altura onde a boia é fechada até o sensor)
  HS: "0.22"
  #Distância entre o fundo da caixa e o nível de transbordamento (ou nível do extravasor/ladrão, o que o ocorrer primeiro)
  HMA: "0.55"

#Logger
  LoggerUART: UART0_SWAP
  LoggerLevel: DEBUG

esphome:
  name: $hostname
  platform: ${Plataforma}
  board: ${TipoPlaca}
  
wifi:
  networks:
  - ssid: ${RedeWifi}
    password: ${SenhaWifi}
    hidden: ${WifiOculto}
  fast_connect: ${WifiFastConnect}
  use_address: ${EndConfig}
  
  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: WIFI
    password: ${SenhaWifiReconfig}

#Habilita um AP Wifi para reconfigurar em caso de perda de conexão com a rede configurada
captive_portal:

#Habilita a atualização de firmware por OTA
ota:
  password: ${SenhaOTA}
  
#Habilita as mensagens de logs pela porta serial e via MQTT
logger:
  hardware_uart: ${LoggerUART}
  level: ${LoggerLevel}

#Habilita a api para comunicar com o Home Assistant
api:
  password: ${SenhaAPI}

switch:
############################# SENSORES DEVICE ##################################
  #Comando reinicilizar esp remotamente
  - platform: restart
    id: ${hostname}_restart
    name: ${PrefixoNome} Reiniciar
    icon: mdi:restart

text_sensor:
############################# SENSORES DEVICE ##################################
  #Informações da conexão wifi
  - platform: wifi_info
    #Endereço IP
    ip_address:
      id: ${hostname}_IP
      name: ${PrefixoNome} Endereço IP
      icon: mdi:ip-network
    #Nome da Rede
    ssid:
      id: ${hostname}_SSID
      name: ${PrefixoNome} Rede Wifi
      icon: mdi:wifi
  #Informação da versão da compilação
  - platform: version
    id: ${hostname}_versao
    name: ${PrefixoNome} Versão
    icon: mdi:information

binary_sensor:
############################# SENSORES DEVICE ##################################
  #Status (conectado ou desconectado)
  - platform: status
    id: ${hostname}_status
    name: ${PrefixoNome} Status
    device_class: connectivity

############################# ALERTA DE NIVEIS #################################
  #Nível mínimo de alerta
  - platform: template
    id: NAMIN
    name: ${PrefixoNome} Nível Mínimo de Alerta
    ##internal: True
    lambda: |-
      if (id(VAGUAP).state <= id(AMIN).state) {
        return true;
      } else {
        return false;
      }

  #Nível máximo de alerta
  - platform: template
    id: NAMAX
    name: ${PrefixoNome} Nível Máximo de Alerta
    ##internal: True
    lambda: |-
      if (id(VAGUAP).state >= id(AMAX).state) {
        return true;
      } else {
        return false;
      }
      
  #Erro medição
  - platform: template
    id: ERRO_MED
    name: ${PrefixoNome} Erro Medição
    #internal: True
    lambda: |-
      if (((id(HMED).state>((id(HM).state+id(HS).state)))) or (id(HMED).state<id(HS).state)) {
        return true;
      } else {
        return false;
      }

sensor:
############################ SENSOR ULTRASONICO ################################
  - platform: ultrasonic
    id: HMED
    trigger_pin: ${SensorTriggerPin}
    echo_pin: ${SensorEchoPin}
    name: ${PrefixoNome} Distância Medida Sensor
    internal: false
    accuracy_decimals: 6
    update_interval: 0.5s
    filters:
      - median:
          window_size: 20
          send_every: 20
          send_first_at: 20
      
#################### INFORMAÇÕES FÍSICAS DA INSTALAÇÃO #########################
  #Diametro interno da base da caixa (fundo)
  - platform: template
    id: DD1
    name: ${PrefixoNome} Diâmentro Interno Base
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${DD1};
    accuracy_decimals: 4
    update_interval: 1s

  #Diametro interno da borda superior sem a tampa
  - platform: template
    id: DM
    name: ${PrefixoNome} Diâmetro Interno Superior
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${DM};
    accuracy_decimals: 4
    update_interval: 1s

  #Distância entre o fundo da caixa e a borda sem a tampa
  - platform: template
    id: HB
    name: ${PrefixoNome} Altura Interna até a borda 
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HB};
    accuracy_decimals: 4
    update_interval: 1s

  #Distância entre o fundo da caixa e a borda superior da tampa (onde o sensor será instalado)
  - platform: template
    id: HT
    name: ${PrefixoNome} Altura com Tampa
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HT};
    accuracy_decimals: 4
    update_interval: 1s
    
  #Distância entre o fundo da caixa e o nível da base da tubulação de saída (altura da coluna de água do nível morto)
  - platform: template
    id: HMT
    name: ${PrefixoNome} Coluna de Água Nível Morto
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HMT};
    accuracy_decimals: 4
    update_interval: 1s
  
  #Distância entre o fundo da caixa e o nível máximo normal (altura da água onde a boia é fechada)
  - platform: template
    id: HM
    name: ${PrefixoNome} Coluna de Água Útil
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HM};
    accuracy_decimals: 4
    update_interval: 1s
  
  #Distância entre o sensor e o nível máximo normal (altura onde a boia é fechada até o sensor)
  - platform: template
    id: HS
    name: ${PrefixoNome} Distância Sensor Nível Máximo
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HS};
    accuracy_decimals: 4
    update_interval: 1s

  #Distância entre o fundo da caixa e o nível de transbordamento (ou nível do extravasor/ladrão, o que o ocorrer primeiro)
  - platform: template
    id: HMA
    name: ${PrefixoNome} Coluna de Água Transbordamento
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HMA};
    accuracy_decimals: 4
    update_interval: 1s
    
######################### CÁLCULOS AUXILIARES ##################################
  #Coluna de água medida (descontado a distância do sensor)
  - platform: template
    id: H
    name: ${PrefixoNome} Coluna de Água Atual
    internal: false
    unit_of_measurement: m
    lambda: return (id(HM).state+id(HS).state-id(HMED).state);
    accuracy_decimals: 4
    update_interval: 1s
    
  #Raio menor do reservatório
  - platform: template
    id: R1
    name: ${PrefixoNome} Raio Menor 
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(DD1).state/2);
    accuracy_decimals: 4
    update_interval: 1s
  
  #Raio maior máximo do reservatório
  - platform: template
    id: RM
    name: ${PrefixoNome} Raio Maior Máximo Físico
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(DM).state/2);
    accuracy_decimals: 4
    update_interval: 1s
  
  #Raio maior máximo de acordo com o nível máximo de água
  - platform: template
    id: R2M
    name: ${PrefixoNome} Raio Maior com Nível Máximo
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(R1).state+(id(HM).state*tan(atan(((id(DM).state-id(DD1).state)/2)/(id(HB).state)))));
    accuracy_decimals: 4
    update_interval: 1s
  
  #Raio maior atual do reservatório (de acordo com o nível da água)
  - platform: template
    id: R2
    name: ${PrefixoNome} Raio Maior com o Nível Atual
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(R1).state+(id(H).state*tan(atan(((id(DM).state-id(DD1).state)/2)/(id(HB).state)))));
    accuracy_decimals: 4
    update_interval: 1s
  
  #Raio maior o volume morto
  - platform: template
    id: RHMT
    name: ${PrefixoNome} Raio Maior no Nível Mínimo
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(R1).state+(id(HMT).state*tan(atan(((id(DM).state-id(DD1).state)/2)/(id(HB).state)))));
    accuracy_decimals: 4
    update_interval: 1s
    
########################## CÁLCULOS DOS VOLUMES#################################
  #Volume com o reservatório cheio (altura da borda)
  - platform: template
    id: VTT
    name: ${PrefixoNome} Volume com o Reservatório Cheio
    internal: true
    unit_of_measurement: litros
    lambda: return ((1.047197*id(HB).state)*((id(RM).state*id(RM).state)+(id(RM).state*id(R1).state)+(id(R1).state*id(R1).state))*1000)-id(VMT).state;
    accuracy_decimals: 0
    update_interval: 1s

  #Volume de água do volume morto (abaixo da saída)
  - platform: template
    id: VMT
    name: ${PrefixoNome} Volume Morto
    internal: false
    unit_of_measurement: litros
    lambda: return ((1.047197*id(HMT).state)*((id(RHMT).state*id(RHMT).state)+(id(RHMT).state*id(R1).state)+(id(R1).state*id(R1).state))*1000);
    accuracy_decimals: 0
    update_interval: 1s

  #Volume de água com o nível máximo
  - platform: template
    id: VT
    name: ${PrefixoNome} Volume com o Nível Máximo
    internal: false
    unit_of_measurement: litros
    lambda: return ((1.047197*id(HM).state)*((id(R2M).state*id(R2M).state)+(id(R2M).state*id(R1).state)+(id(R1).state*id(R1).state))*1000)-id(VMT).state;
    accuracy_decimals: 0
    update_interval: 1s
    
  #Volume de Água
  - platform: template
    id: VAGUA
    name: ${PrefixoNome} Volume
    unit_of_measurement: litros
    lambda: |-
      if ( (((1.047197*id(H).state)*((id(R2).state*id(R2).state)+(id(R2).state*id(R1).state)+(id(R1).state*id(R1).state))*1000)-id(VMT).state) <0 ) {
        return 0;
      } else if  (  (((1.047197*id(H).state)*((id(R2).state*id(R2).state)+(id(R2).state*id(R1).state)+(id(R1).state*id(R1).state))*1000)-id(VMT).state) > id(VTT).state) {
        return id(VTT).state;
      } else {
        return (((1.047197*id(H).state)*((id(R2).state*id(R2).state)+(id(R2).state*id(R1).state)+(id(R1).state*id(R1).state))*1000)-id(VMT).state);
      }
    accuracy_decimals: 0
    update_interval: 1s

  #Volume Percentual
  - platform: template
    id: VAGUAP
    name: ${PrefixoNome} Volume Percentual
    unit_of_measurement: "%"
    lambda: return ((id(VAGUA).state / id(VT).state)*100);
    accuracy_decimals: 1
    update_interval: 1s
```
7) Clicar em load, vai fazer um monte de coisas e ao final vai exibir
8) Desconectar o NodeMCU do seu computador, e ligar ele ao carregador USB simples. ELe irá piscar apenas um led quando for ligado, e após alguns minutos, deverá aparecer no dashboard como ONLINE.
9) Pronto, o nodeMCU está pronto para conectar ao seu wifi, ler os dados do sensor e enviar aos dados ao MQTT.

## Sensor ultrassônico

O sensor comprado virá com 1 ou 2 peças pretas, que são o sensor em si, e uma placa de controle. Para o sensor em si:
1) Fazer um furo com uma serra-copo de 5mm na tampa da caixa d'água
2) Passar o sensor por esse furo e fixar com cola-quente (opcional) ou outra cola (silicone ou PVC). Aguarde a cola secar, para que não caia nada que possa obstruir a frente do sensor ou respingar na caixa
3) Esse sensor deverá ser conectado na placa de controle, em um encaixe branco com 2 pinos. Faça esse encaixe.
4) Localize no NodeMCU os pinos D6 e D7, assim como os pinos GND e 5V (pode usar qualquer um deles)
5) Agora use o cabo DuPont para conectar os fios da seguinte forma: 5V para 5V, D6 para Trig, D7 para Echo e GND para GND
![image](https://github.com/lucianor/caixadagua/assets/4389998/8fd804d3-a7e6-4288-8583-1138f4f4e083)

6) Com o sensor conectada a placa, e a placa conectada ao nodeMCU, basta ligar o NodeMCU ao carregador de celular e fixar ele em algum local dentro da sua instalação, ou até mesmo dentro de uma caixa hermética caso necessário. O nodeMCU ficará ligado a tomada 100% do tempo, e você sempre poderá repetir os passsos acima do esphome quando necessário.


### Acessando os dados

1) TODO: adicionar
