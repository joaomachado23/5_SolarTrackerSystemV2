# 5_SolarTrackerSystemV2

O objetivo deste trabalho era a conceção de um protótipo funcional a apresentar no final do semestre, à escolha de cada grupo, tendo obrigatoriamente de integrar conteúdos lecionados em 3 das 4 UC do mesmo semestre: Controlo Digital, Energias Renováveis e Mobilidade Elétrica, Sistemas de Automação e Sistemas Embebidos. Contrariamente à primeira versão deste projeto (Solar Tracker System), foi agora permitido utilizar tanto componentes analógicos como “digitais”, nomeadamente microcontroladores.

O nosso grupo decidiu dar continuidade ao projeto do semestre anterior (STS), aprimorando-o e simplificando-o com recurso a microcontroladores, apenas adicionando funcionalidades que acrescentariam valor num uso real. O Solar Tracker System (STS) manteve a mesma estrutura física, mas passou a incluir mais sensores, comunicação sem fios via MQTT entre uma ESP32 e um website em localhost, além de um novo controlo para o conversor CC-CC Boost baseado em MPPT P&O (Maximum Power Point Tracking - Perturbe & Observe).

Funcionalidades principais implementadas:

- Sensores

  - 3 LDRs para orientação dos painéis.
  - Sensor de temperatura e humidade (DHT11).
  - Sensor de posição (potenciómetro multivoltas).
  - 2 sensores de fim de curso.
  - Sensor de corrente com amplificação (INA126P + LM741).
  - Sensor de tensão escalonado para ADC.

- Controlo dos motores

  - Dois motores de passo Nema 17, controlados por drivers DRV8825.
  - Eixo azimute com fins de curso mecânicos para limitar rotações.
  - Eixo altitude com controlo via potenciómetro e posição de referência para o período noturno.
  - Posicionamento automático dos painéis em modo “home” quando não existe luminosidade suficiente.

- Conversores e gestão de energia

  - Conversor Boost desenvolvido de raiz, controlado por PWM da STM32 e com algoritmo MPPT P&O (Perturbação e Observação).
  - Conversor Buck (módulo XL4015) para regulação de tensão de carregamento de baterias.
  - Sistema de armazenamento com 2 células 18650 em série (3.7 V, 2500 mAh cada).
  - Battery Management System (BMS) para proteção contra sobrecarga, sobredescarga e picos de tensão/corrente.
  - Regulador linear 7805 para alimentação estável da placa de desenvolvimento STM32, a partir das baterias.

- Arquitetura de software

  - STM32 responsável por:

    - Leitura dos sensores (tensão, corrente, temperatura, humidade, LDRs).
    - Algoritmo MPPT para extração da máxima potência dos PV.
    - Controlo dos motores com base na comparação dos LDRs e histerese.

  - ESP32 responsável por:

    - Comunicação Wi-Fi e servidor MQTT.
    - Envio dos dados recolhidos para o Node-Red, onde foram tratados e exibidos numa dashboard HMI acessível via browser.

- Comunicação e interface

  - Dashboard no Node-Red mostrando em tempo real:
    
    - Últimos valores de tensão, corrente, temperatura e humidade.
    - Histórico da última hora.

  - Comunicação assíncrona via MQTT (publisher/subscriber).

Neste repositório, é possível encontrar:

1. Um PDF com toda a documentação do projeto: design, simulação e conceção.
2. Um vídeo que mostra o resultado geral do projeto.
3. Um vídeo que demonstra o funcionamento do conversor Boost com MPPT.
4. Um vídeo que mostra o STS a orientar-se em direção à maior luminosidade e a converter energia.

Este trabalho foi fundamental para consolidar conhecimentos em várias áreas da eletrónica e automação, unindo analógico e digital. Desafiante, mas extremamente gratificante.

Um grande obrigado aos colegas que me acompanharam nesta jornada: Diego Brandão, Henrique Magalhães, José Leite, Rui Pedroso e Tiago Pereira.
