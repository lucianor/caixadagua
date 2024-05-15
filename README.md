# Objetivo
Criar um guia prático de como instalar um sensor de volume de caixa d'água, de forma a poder monitorar pelo celular em momentos de falta de água ou catástrofe. Esse guia deverá incluir quais itens a serem comprados, quais os passos a serem seguidos para instalação e configuração. O projeto original é do [Fórum HomeAssistant Brasil](https://homeassistantbrasil.com.br/t/medindo-o-volume-da-caixa-de-agua/3668), mas fiz modificações para simplicidade.

O local onde está a caixa d'água precisa ter uma tomada para conectar o sensor, e também sinal Wi-Fi, pois as informações de volume serão transmitidas pela internet - ou seja, se você tiver falta de energia e/ou internet, não será possível visualizar os dados de volume da caixa. Também é importante saber qual disjuntor liga e desliga esse sensor, caso seja necessário reiniciá-lo - nem todas as caixas d'água tem acesso fácil.

# Passo 1 - Itens

## Itens necessários
- [ ] **NodeMCU v3** que irá realizar a leitura desse sensor, calcular o volume e expor esses dados no HomeAssistant
- [ ] **Sensor ultrassônico** (JSN-SR04T ou JSN-Y2-1) para emitir um pulso que irá medir a distância da tampa da caixa até o nível da água
- [ ] Fios DuPont fêmea para fêmea para ligar o NodeMCU ao sensor ultrâssonico
- [ ] Carregador de celular simples para dar energia ao NodeMCU e cabo micro USB
- [ ] Furadeira e serra-copo 5mm para fazer os furos do sensor na tampa da caixa


## Onde Comprar
- NodeMCU: [Eletrogate](https://www.eletrogate.com/modulo-Wi-fi-esp8266-nodemcu-v3-lolin) | [Casa da Robótica](https://www.casadarobotica.com/internet-das-coisas/placas/esp/placa-esp-nodemcu-v3-Wi-fi-802-11-b-g-n) | [MercadoLivre](https://www.mercadolivre.com.br/esp8266-modulo-Wi-fi-esp8266-nodemcu-v3-ch340/p/MLB34953815?item_id=MLB3679118579&from=gshop&matt_tool=40343894&matt_word=&matt_source=google&matt_campaign_id=14303413655&matt_ad_group_id=133855953276&matt_match_type=&matt_network=g&matt_device=c&matt_creative=584156655519&matt_keyword=&matt_ad_position=&matt_ad_type=pla&matt_merchant_id=735128761&matt_product_id=MLB34953815-product&matt_product_partition_id=2268053647630&matt_target_id=aud-1966873223882:pla-2268053647630&cq_src=google_ads&cq_cmp=14303413655&cq_net=g&cq_plt=gp&cq_med=pla&gad_source=1&gclid=CjwKCAjwi_exBhA8EiwA_kU1Mp58IvvsEqdQ8Qpfu_QRCcwdVrg21JKlLZBRf2RdG427uj4FqwGQOxoCk2cQAvD_BwE) | [AliExpress](https://www.aliexpress.com/item/1005005977505151.html?spm=a2g0o.productlist.main.1.6b881568z6HXO3&algo_pvid=3d980182-4747-48c2-b768-fa3630e00195&algo_exp_id=3d980182-4747-48c2-b768-fa3630e00195-0&pdp_npi=4%40dis%21BRL%2136.71%2111.38%21%21%2149.52%2115.35%21%4021032dcb17151818189177583ec215%2112000035141219305%21sea%21BR%211907945182%21&curPageLogUid=LMfOcIXKtVlR&utparam-url=scene%3Asearch%7Cquery_from%3A)
- Sensor Ultrassônico [Eletrogate](https://www.eletrogate.com/sensor-de-distancia-ultrassonico-jsn-sr04t-a-prova-dagua-modulo) | [Casa da Robótica](https://www.casadarobotica.com/sensores-e-modulos/sensores/movimento-e-proximidade/sensor-ultrassonico-de-distancia-jsn-sr04m-a-prova-d-agua) | [MercadoLivre](https://produto.mercadolivre.com.br/MLB-2132613896-modulo-sensor-ultrassnico-impermeavel-jsn-sr04taj-sr04m-_JM#is_advertising=true&position=2&search_layout=grid&type=pad&tracking_id=94fd3a97-9fc7-46d4-8c5a-6ab6fcebf61e&is_advertising=true&ad_domain=VQCATCORE_LST&ad_position=2&ad_click_id=ZjYxY2I0NjktNTI1My00YTA2LWI2N2ItM2FlZTA5NGM1YWRh) | [AliExpress](https://www.aliexpress.com/item/1005005995118143.html?spm=a2g0o.productlist.main.3.49f632dbFzaqVy&algo_pvid=622ac9e1-a2b9-4503-874d-5cb7d8005776&algo_exp_id=622ac9e1-a2b9-4503-874d-5cb7d8005776-1&pdp_npi=4%40dis%21BRL%2123.38%2115.54%21%21%2131.54%2120.96%21%402101e9ec17151819516342394edb60%2112000035288838952%21sea%21BR%211907945182%21&curPageLogUid=ZOIxaVGXOsXz&utparam-url=scene%3Asearch%7Cquery_from%3A)
- Fios DuPont: [EletroGate](https://www.eletrogate.com/jumpers-femea-femea-20-unidades-de-20-cm) | [Casa da Robótica](https://www.casadarobotica.com/prototipagem-e-ferramentas/prototipagem/cabos/20x-cabo-jumper-femea-x-femea-10-cm) | [MercadoLivre](https://produto.mercadolivre.com.br/MLB-4568483108-40-cabos-jumper-femea-femea-f-f-fxf-20cm-arduino-protoboard-_JM?vip_filters=shipping:fulfillment#position=30&search_layout=stack&type=item&tracking_id=94cea785-a682-4b01-a78e-9829bf5d14f7) | [AliExpress](https://www.aliexpress.com/item/32661309874.html?spm=a2g0o.productlist.main.3.6155hPfbhPfbO3&algo_pvid=62220dc5-d55b-4ef1-ab40-244b6155c782&algo_exp_id=62220dc5-d55b-4ef1-ab40-244b6155c782-1&pdp_npi=4%40dis%21BRL%216.54%216.54%21%21%211.22%211.22%21%402101f04d17151822093485980ef3c8%2166291977410%21sea%21BR%211907945182%21&curPageLogUid=nYAbEDcfU2aV&utparam-url=scene%3Asearch%7Cquery_from%3A)

# Passo 2 - Instalação e Configuração

## NodeMCU

O primeiro passo é fazer o carregamento do software ao NodeMCU, para que ao você chegar para a instalação completa, basta ligar a tomada e pronto. Para isso, será necessário instalar o Python e instalar o esphome para carregar o software. Você só precisará rodar o esphome novamente para calibrar as dimensões da sua caixa d'água, ou ainda quando você mudar o nome da rede ou a senha do seu Wi-Fi.

1) Instalar o Python no Windows e adicionar o python na variável de ambiente PATH - instruções [aqui](https://dicasdeprogramacao.com.br/como-instalar-o-python-no-windows-10/#google_vignette)
2) Abrir um prompt de comando do Windows (instruções acima) e copiar e colar o texto abaixo e dar ENTER
```
pip install tornado esptool esphome dashboard config
```
3) Ao final irá exibir uma mensagem XXXX. Agora é necessário carregar o dashboard do esphome:
```
esphome dashboard config --open-ui
```
4) No navegador que abrir, clicar no botão "+ Add Device" na parte inferior a direita
5) Conectar um cabo micro USB ao NodeMCU e ao computador onde você instalou o esphome
6) Selecionar "Physically connected to this device" - se o seu dispositivo não for encontrado, procure usar outro cabo micro USB, nem todos possuem a função de transmitir dados além de energia
7) Isso irá abrir uma nova configuração de dispositivo onde você irá carregar esse código. Altere a sua RedeWi-fi e SenhaWi-fi. O demais pode deixar como está.
```
substitutions:
  #Configurações do dispositivo:
  hostname: 'caixaagua' #Hostname do dispositivo na rede
  PrefixoNome: "Caixa Água - "
  
  #Configurações da Rede:
  RedeWifi: !secret wifi_ssid #Nome da rede wifi que o dispositivo irá se conectar
  SenhaWifi: !secret wifi_password #Senha da rede wifi que o dispositivo irá se conectar
  SenhaWifiReconfig: !secret SenhaWifiReconfig #Senha do AP Wifi para reconfiguração do wifi do dispositivo
  EndConfig: '192.168.0.66' #192.168.1.50 #Endereço para configuração (IP que o esp está acessível atualmente na rede), especialmente usado quando quer alterar o hostname via OTA
  WifiOculto: 'False' #Se a rede wifi está oculta defina como 'True'
  WifiFastConnect: 'True' #Se o esp realizará a conexão à rede wifi sem escanear as redes disponíveis defina como 'True'

  #Pinos do Sensor
  SensorTriggerPin: D6
  SensorEchoPin: D7
  
  #INFORMAÇÕES FÍSICAS DA CAIXA E DA INSTALAÇÃO DO SENSOR (medidas em metros):
  #Diametro interno da base da caixa (fundo)
  DD1: "1.17"
  #Diametro interno da borda superior sem a tampa
  DM: "1.34"
  #Distância entre o fundo da caixa e a borda sem a tampa 
  HB: "0.84" # MEDIDO 
  #Distância entre o fundo da caixa e a borda superior da tampa (onde o sensor será instalado)
  HT: "0.95" # REVISAR
  #Distância entre o fundo da caixa e o nível da base da tubulação de saída (altura da coluna de água do nível morto) -- CALC
  HMT: "0.07" # REVISAR
  #Distância entre o fundo da caixa e o nível máximo normal (altura da água onde a boia é fechada)
  HM: "0.67" # MEDIDO
  #Distância entre o sensor e o nível máximo normal (altura onde a boia é fechada até o sensor)
  HS: "0.28" # REVISAR
  #Distância entre o fundo da caixa e o nível de transbordamento (ou nível do extravasor/ladrão, o que o ocorrer primeiro)
  HMA: "0.78" # REVISAR

esphome:
  name: $hostname
  platform: ESP8266
  board: nodemcuv2
  
wifi:
  networks:
  - ssid: ${RedeWifi}
    password: ${SenhaWifi}
    hidden: ${WifiOculto}
  fast_connect: ${WifiFastConnect}
  use_address: ${EndConfig}

#Habilita a atualização de firmware por OTA
ota:
  password: ${SenhaWifiReconfig}
  
#Habilita as mensagens de logs pela porta serial e via MQTT
logger:
  level: ERROR
  logs:
    app: ERROR
    sensor: ERROR

#Habilita a api para comunicar com o Home Assistant
api:
  encryption:
    key: 'YGMhClFcQ6wOhwLM69E2ZNm1Vxa6LQE23OGGSnV8xo0='

switch:
############################# SENSORES DEVICE ##################################
  #Comando reinicilizar esp remotamente
  - platform: restart
    id: ${hostname}_restart
    name: ${PrefixoNome} Reiniciar
    icon: mdi:restart

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
############################ IMPORT VARIAVÉIS HA ###############################
  - platform: homeassistant
    id: AMIN
    name: ${PrefixoNome} Nível Mínimo de Alerta
    internal: true #false Apenas para debug
    entity_id: input_number.caixa_agua_alerta_nivel_minimo
    accuracy_decimals: 2

  - platform: homeassistant
    id: AMAX
    name: ${PrefixoNome} Nível Máximo de Alerta
    internal: true #false Apenas para debug
    entity_id: input_number.caixa_agua_alerta_nivel_maximo
    accuracy_decimals: 2

############################ SENSOR ULTRASONICO ################################
  - platform: ultrasonic
    id: HMED
    trigger_pin:
      number: ${SensorTriggerPin}
      inverted: true
    echo_pin: 
      number: ${SensorEchoPin}
    name: ${PrefixoNome} Distância Medida Sensor
    accuracy_decimals: 6
    timeout: 4m
    update_interval: 30s
    filters:
      - median:
          window_size: 5
          send_every: 3
          send_first_at: 2
    #pulse_time: 10us


#################### INFORMAÇÕES FÍSICAS DA INSTALAÇÃO #########################
  #Diametro interno da base da caixa (fundo)
  - platform: template
    id: DD1
    name: ${PrefixoNome} Diâmentro Interno Base
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${DD1};
    accuracy_decimals: 4
    update_interval: 30s

  #Diametro interno da borda superior sem a tampa
  - platform: template
    id: DM
    name: ${PrefixoNome} Diâmetro Interno Superior
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${DM};
    accuracy_decimals: 4
    update_interval: 30s

  #Distância entre o fundo da caixa e a borda sem a tampa
  - platform: template
    id: HB
    name: ${PrefixoNome} Altura Interna até a borda 
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HB};
    accuracy_decimals: 4
    update_interval: 30s

  #Distância entre o fundo da caixa e a borda superior da tampa (onde o sensor será instalado)
  - platform: template
    id: HT
    name: ${PrefixoNome} Altura com Tampa
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HT};
    accuracy_decimals: 4
    update_interval: 30s
    
  #Distância entre o fundo da caixa e o nível da base da tubulação de saída (altura da coluna de água do nível morto)
  - platform: template
    id: HMT
    name: ${PrefixoNome} Coluna de Água Nível Morto
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HMT};
    accuracy_decimals: 4
    update_interval: 30s
  
  #Distância entre o fundo da caixa e o nível máximo normal (altura da água onde a boia é fechada)
  - platform: template
    id: HM
    name: ${PrefixoNome} Coluna de Água Útil
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HM};
    accuracy_decimals: 4
    update_interval: 30s
  
  #Distância entre o sensor e o nível máximo normal (altura onde a boia é fechada até o sensor)
  - platform: template
    id: HS
    name: ${PrefixoNome} Distância Sensor Nível Máximo
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HS};
    accuracy_decimals: 4
    update_interval: 30s

  #Distância entre o fundo da caixa e o nível de transbordamento (ou nível do extravasor/ladrão, o que o ocorrer primeiro)
  - platform: template
    id: HMA
    name: ${PrefixoNome} Coluna de Água Transbordamento
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return ${HMA};
    accuracy_decimals: 4
    update_interval: 30s
    
######################### CÁLCULOS AUXILIARES ##################################
  #Coluna de água medida (descontado a distância do sensor)
  - platform: template
    id: H
    name: ${PrefixoNome} Coluna de Água Atual
    internal: false
    unit_of_measurement: m
    lambda: return (id(HM).state+id(HS).state-id(HMED).state);
    accuracy_decimals: 4
    update_interval: 30s
    
  #Raio menor do reservatório
  - platform: template
    id: R1
    name: ${PrefixoNome} Raio Menor 
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(DD1).state/2);
    accuracy_decimals: 4
    update_interval: 30s
  
  #Raio maior máximo do reservatório
  - platform: template
    id: RM
    name: ${PrefixoNome} Raio Maior Máximo Físico
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(DM).state/2);
    accuracy_decimals: 4
    update_interval: 30s
  
  #Raio maior máximo de acordo com o nível máximo de água
  - platform: template
    id: R2M
    name: ${PrefixoNome} Raio Maior com Nível Máximo
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(R1).state+(id(HM).state*tan(atan(((id(DM).state-id(DD1).state)/2)/(id(HB).state)))));
    accuracy_decimals: 4
    update_interval: 30s
  
  #Raio maior atual do reservatório (de acordo com o nível da água)
  - platform: template
    id: R2
    name: ${PrefixoNome} Raio Maior com o Nível Atual
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(R1).state+(id(H).state*tan(atan(((id(DM).state-id(DD1).state)/2)/(id(HB).state)))));
    accuracy_decimals: 4
    update_interval: 30s
  
  #Raio maior o volume morto
  - platform: template
    id: RHMT
    name: ${PrefixoNome} Raio Maior no Nível Mínimo
    internal: true #false Apenas para debug
    unit_of_measurement: m
    lambda: return (id(R1).state+(id(HMT).state*tan(atan(((id(DM).state-id(DD1).state)/2)/(id(HB).state)))));
    accuracy_decimals: 4
    update_interval: 30s
    
########################## CÁLCULOS DOS VOLUMES#################################
  #Volume com o reservatório cheio (altura da borda)
  - platform: template
    id: VTT
    name: ${PrefixoNome} Volume com o Reservatório Cheio
    internal: true
    unit_of_measurement: litros
    lambda: return ((1.047197*id(HB).state)*((id(RM).state*id(RM).state)+(id(RM).state*id(R1).state)+(id(R1).state*id(R1).state))*1000)-id(VMT).state;
    accuracy_decimals: 0
    update_interval: 30s

  #Volume de água do volume morto (abaixo da saída)
  - platform: template
    id: VMT
    name: ${PrefixoNome} Volume Morto
    internal: false
    unit_of_measurement: litros
    lambda: return ((1.047197*id(HMT).state)*((id(RHMT).state*id(RHMT).state)+(id(RHMT).state*id(R1).state)+(id(R1).state*id(R1).state))*1000);
    accuracy_decimals: 0
    update_interval: 30s

  #Volume de água com o nível máximo
  - platform: template
    id: VT
    name: ${PrefixoNome} Volume com o Nível Máximo
    internal: false
    unit_of_measurement: litros
    lambda: return ((1.047197*id(HM).state)*((id(R2M).state*id(R2M).state)+(id(R2M).state*id(R1).state)+(id(R1).state*id(R1).state))*1000)-id(VMT).state;
    accuracy_decimals: 0
    update_interval: 30s
    
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
    update_interval: 30s

  #Volume Percentual
  - platform: template
    id: VAGUAP
    name: ${PrefixoNome} Volume Percentual
    unit_of_measurement: "%"
    lambda: return ((id(VAGUA).state / id(VT).state)*100);
    accuracy_decimals: 1
    update_interval: 30s
```
7) Após isso, clicar em Install. Irá exibir várias mensagens e deverá demorar uns 5 a 10 minutos para finalizar. Quando terminar, irá exibir a mensagem XXXXX
8) Desconectar o NodeMCU do seu computador, e ligar ele ao carregador USB simples. ELe irá piscar apenas um led quando for ligado. Volte ao dashboard e verifique após alguns minutos que o dispositivo aparece como ONLINE.
9) Pronto, o NodeMCU está pronto para conectar ao seu Wi-Fi, ler os dados do sensor e enviar aos dados ao MQTT.

## Sensor ultrassônico

O sensor é composto do sensor em si e uma placa de controle. Agora é necessário usar uma furadeira para instalar esse sensor na tampa e conectar os fios. Não é necessário um computador para esse passo.
1) Fazer um furo com uma serra-copo de 5mm na tampa da caixa d'água
2) Passar o sensor por esse furo e fixar com cola-quente (opcional) ou outra cola (silicone ou PVC). Aguarde a cola secar, para que não respingue nada no sensor ou dentro da caixa d'água.
3) Esse sensor possui um cabo preto que deve ser conectado na placa de controle em um encaixe branco com 2 pinos.
4) Localize no NodeMCU os pinos laterais, encontre aqueles com as inscrições D6 e D7, assim como pinos GND e 5V (existem múltiplos, podem usar qualquer um deles)
5) Agora use os fios DuPont, encaixando a parte preta em cada um dos conectores no NodeMCU e no sensor: 5V para 5V, D6 para Trig, D7 para Echo e GND para GND
![image](https://github.com/lucianor/caixadagua/assets/4389998/8fd804d3-a7e6-4288-8583-1138f4f4e083)

6) Pronto, você tem um sensor conectado a sua placa, essa placa conectada ao NodeMCU. Agora o NodeMCU deve ser ligado ao carregador de celular para tudo funcionar. Aproveite e fixe todo o conjunto em algum local dentro da sua instalação, ou até mesmo dentro de uma caixa hermética caso necessário, para mantê-lo protegido bem como evitar que qualquer fio seja desconectado. O NodeMCU ficará ligado a tomada 100% do tempo e transmitindo as informações de volume a cada 5 minutos.

### Acessando os dados

1) TODO: adicionar

### Calibração

Os dados acima consideram uma caixa de 1.000 litros. Porém, você pode instalar esse sensor em qualquer reservatório, ajustando os parâmetros.
