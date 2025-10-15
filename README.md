# Sistema de Irrigação Inteligente - FarmTech
## FIAP - Trabalho Um Mapa do Tesouro

Este projeto consiste em um sistema de irrigação automatizado e inteligente, desenvolvido para otimizar o consumo de água em culturas específicas, simulando a tomada de decisão agronômica com base em parâmetros ambientais e de nutrientes.

O sistema utiliza o microcontrolador **ESP32** para monitorar continuamente o ambiente e as condições simuladas do solo, acionando uma bomba d'água (via Relé) apenas quando todas as condições ideais são atendidas.

---

## 🔗 Links do Projeto

* **Demonstração em Vídeo (YouTube):** [Assista ao Vídeo](https://youtu.be/SaqpoRK0JMY)
* **Simulação (Wokwi):** [Acesse o Projeto](https://wokwi.com/projects/444556662174638081)

## 📸 Diagrama do Projeto

![Imagem do Sistema de Irrigação na simulação Wokwi](https://github.com/user-attachments/assets/a785a918-7fec-470b-859a-3909f4349ebe)

---

## Objetivo

O principal objetivo é demonstrar um ciclo de decisão completo para a irrigação da cultura de **TOMATE**, garantindo que a bomba só seja acionada se as seguintes condições forem verdadeiras:

1.  A **Umidade do Solo** estiver abaixo do limite mínimo: `< 60%`.
2.  O **pH do Solo (simulado)** estiver dentro da faixa ideal: `Entre 6.0 e 6.8`.
3.  Os **Nutrientes-chave (P e K)** estiverem presentes (simulados via botões).

---

## Componentes de Hardware

| Componente | Função | Especificidade |
| :--- | :--- | :--- |
| **Microcontrolador** | ESP32 | Responsável pelo processamento e controle. |
| **Sensor de Umidade** | DHT22 | Mede a umidade do ar (simulando a umidade do solo). |
| **Sensor de Luz** | LDR | Utilizado para simular a leitura do pH do solo. |
| **Atuador** | Relé (Ativo Baixo) | Controla o acionamento da Bomba de Água. |
| **Botões (x3)** | Botões Tácteis | Simulam a presença dos nutrientes **N**, **P** e **K**. |

---

## Diagrama de Conexões

O projeto utiliza a seguinte configuração de pinos do ESP32, conforme definido no arquivo `main.cpp`:

| Componente | Sinal / Pino (Código) | Pino do ESP32 | Tipo | Notas |
| :--- | :--- | :--- | :--- | :--- |
| **Sensor DHT22** | `PIN_DHT22` | **GPIO 4** | Digital | Leitura de Umidade. |
| **Sensor LDR** | `PIN_LDR` | **GPIO 34** | Analógico | Simulação de Leitura de pH. |
| **Botão N** | `PIN_BOTAO_N` | **GPIO 13** | Digital | Entrada (INPUT). Conectado ao GND. |
| **Botão P** | `PIN_BOTAO_P` | **GPIO 32** | Digital | Entrada (INPUT). Conectado ao GND. |
| **Botão K** | `PIN_BOTAO_K` | **GPIO 14** | Digital | Entrada (INPUT). Conectado ao GND. |
| **Relé/Bomba** | `PIN_RELE` | **GPIO 27** | Saída | Ativo Baixo (`LOW` = Liga). |

---

## 🧠 Lógica de Funcionamento e Decisão

O sistema opera em um ciclo contínuo (`loop`), verificando as condições a cada **2 segundos (`delay(2000)`)** após um período de estabilização (*debounce*) de 50ms para garantir a precisão da leitura dos botões.

### 1. Parâmetros da Cultura (Tomate)

O sistema é calibrado para a cultura do Tomate, com os seguintes limites:

| Parâmetro | Limite Ideal | Condição para Ligar a Bomba |
| :--- | :--- | :--- |
| **Umidade do Solo** | `< 60.0%` | Umidade Baixa. |
| **pH do Solo (Simulado)** | `6.0` a `6.8` | pH na Faixa Ideal. |
| **Nutrientes (P e K)** | Ambos `SIM` | Presença de Nutrientes Chave. |

### 2. Algoritmo de Decisão

A bomba de irrigação é ativada se, e somente se, **TODAS** as três condições a seguir forem verdadeiras:
