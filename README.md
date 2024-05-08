# Objetivo
Fazer um sensor de medida de volume de caixa d'água, incluindo compras dos itens, instalação, configuração e rodar. O projeto original é do [Fórum HomeAssistant Brasil](https://homeassistantbrasil.com.br/t/medindo-o-volume-da-caixa-de-agua/3668)

São vários passos, e pode dar problemas em muitos lugares, mas vou tentar ajudar o máximo possível. Os passos e itens são seguintes:
1) Pré-Requisitos
- [ ] Conexão de internet com sinal Wi-Fi chegando até a caixa d'água
- [ ] Ponto de energia elétrica para conectar os itens
- [ ] Conhecimentos em informática básicos e falta de medo de seguir esse passo a passo
- [ ] Saber ligar um fio em outro, usar uma furadeira
2) Compras
- [ ] **Sensor ultrassônico** (JSN-SR04T ou JSN-Y2-1) para emitir um pulso que irá medir a distância da tampa da caixa até o nível da água
- [ ] **NodeMCU ESP32** que irá realizar a leitura desse sensor, calcular o volume e expor esses dados no HomeAssistant
- [ ] **SBC (single board computer)** para rodar o HomeAssistant (opcional, pode usar um computador existente que fique ligado 24x7)
- [ ] Fios DuPont fêmea para ligar o NodeMCU ao sensor ultrâssonico
- [ ] Carregador de celular simples para dar energia ao NodeMCU e cabo micro USB
- [ ] Carregador de celular capaz de 5V3A para o SBC (opcional) e cabo USB-C
- [ ] Furadeira e serra-copo 5mm para fazer os furos do sensor na tampa da caixa
- [ ] Caixa hermética para colocar os itens acima caso a caixa d'água seja externa
- [ ] Opcional: cola quente para fixar o sensor na tampa.
3) Instalação e configuração
- [ ] Instalar o sensor na caixa d'agua
- [ ] Fazer a instalação dos softwares necessários no SBC - sistema operacional, HomeAssistant e esphome - e configuração (passo mais demorado)
- [ ] Carregar o software no nodeMCU através de um cabo USB
- [ ] Ligar os fios do sensor ao nodeMCU, e do carregador do nodeMCU a ele
- [ ] Fazer a instalação do tailscale para monitoramento remoto

## Passo 1 - Compras

### AliExpress
- [Cabos DuPont](https://www.aliexpress.com/item/32661309874.html?spm=a2g0o.productlist.main.3.6155hPfbhPfbO3&algo_pvid=62220dc5-d55b-4ef1-ab40-244b6155c782&algo_exp_id=62220dc5-d55b-4ef1-ab40-244b6155c782-1&pdp_npi=4%40dis%21BRL%216.54%216.54%21%21%211.22%211.22%21%402101f04d17151822093485980ef3c8%2166291977410%21sea%21BR%211907945182%21&curPageLogUid=nYAbEDcfU2aV&utparam-url=scene%3Asearch%7Cquery_from%3A)
- [Sensor Ultrassônico](https://www.aliexpress.com/item/1005005995118143.html?spm=a2g0o.productlist.main.3.49f632dbFzaqVy&algo_pvid=622ac9e1-a2b9-4503-874d-5cb7d8005776&algo_exp_id=622ac9e1-a2b9-4503-874d-5cb7d8005776-1&pdp_npi=4%40dis%21BRL%2123.38%2115.54%21%21%2131.54%2120.96%21%402101e9ec17151819516342394edb60%2112000035288838952%21sea%21BR%211907945182%21&curPageLogUid=ZOIxaVGXOsXz&utparam-url=scene%3Asearch%7Cquery_from%3A)
- [NodeMCU](https://www.aliexpress.com/item/1005005977505151.html?spm=a2g0o.productlist.main.1.6b881568z6HXO3&algo_pvid=3d980182-4747-48c2-b768-fa3630e00195&algo_exp_id=3d980182-4747-48c2-b768-fa3630e00195-0&pdp_npi=4%40dis%21BRL%2136.71%2111.38%21%21%2149.52%2115.35%21%4021032dcb17151818189177583ec215%2112000035141219305%21sea%21BR%211907945182%21&curPageLogUid=LMfOcIXKtVlR&utparam-url=scene%3Asearch%7Cquery_from%3A)
- [SBC OrangePi Zero 3](https://www.aliexpress.com/item/1005006329934692.html?spm=a2g0o.productlist.main.1.54b01cac7sJiXl&algo_pvid=317102aa-d101-4e06-a9ec-69675bb5650c&algo_exp_id=317102aa-d101-4e06-a9ec-69675bb5650c-0&pdp_npi=4%40dis%21BRL%21332.41%21122.46%21%21%21448.42%21165.20%21%402101ef6817151816290552000ed1c2%2112000036790204831%21sea%21BR%211907945182%21&curPageLogUid=So6MjdYQww5p&utparam-url=scene%3Asearch%7Cquery_from%3A)





