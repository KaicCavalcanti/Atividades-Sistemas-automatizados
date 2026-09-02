# Sistema Automatizado de Monitoramento - Semana 2

Protótipo de uma estação de monitoramento desenvolvida no Wokwi utilizando Arduino Uno, integrando entradas digitais, analógicas e medição de temperatura.

## 1. Mapa de Pinos e Componentes
* **Pushbutton (D2):** Atua como entrada digital (chave/sensor discreto) com resistor de pull-up interno.
* **LED + Resistor de 220 Ω (D8):** Atua como atuador/sinalizador visual de acionamento do botão.
* **Potenciômetro (A0):** Simula uma variável analógica contínua (ex: pressão ou nível).
* **DHT22 (D4):** Simula a medição de temperatura do processo.

## 2. Regras de Decisão e Prioridade
O sistema avalia os estados de forma hierárquica:
1. **FALHA DE SENSOR:** Acionada se a leitura de temperatura for inválida (`isnan`).
2. **ALARME:** Acionado se a temperatura for $\ge 40^\circ\text{C}$ ou o potenciômetro for $\ge 750$.
3. **ATENÇÃO:** Acionado se a temperatura for $\ge 30^\circ\text{C}$ ou o potenciômetro for $\ge 400$.
4. **NORMAL:** Ocorre apenas quando todas as variáveis estão dentro das faixas normais.

## 3. Casos de Teste Executados
| Caso | Potenciômetro | Temperatura | Botão | Resposta Esperada | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **T1** | 200 | 25 °C | Livre | NORMAL; LED off | Aprovado |
| **T2** | 500 | 25 °C | Livre | ATENÇÃO | Aprovado |
| **T3** | 800 | 25 °C | Livre | ALARME | Aprovado |
| **T4** | 200 | 35 °C | Livre | ATENÇÃO | Aprovado |
| **T5** | 200 | 45 °C | Livre | ALARME | Aprovado |
| **T6** | 200 | 25 °C | Pressionado | NORMAL; LED on | Aprovado |
| **T7** | — | Inválida | Livre | FALHA DE SENSOR | Aprovado |

## 4. Limitações da Simulação
A simulação comprova a coerência lógica da aquisição e tomada de decisão, mas **não valida** aspectos físicos industriais como ruído elétrico, tempo de resposta real, calibração metrológica, grau de proteção mecânica ou compatibilidade eletromagnética.

## 5. Instruções de Reprodução
1. Acesse o [Wokwi](https://wokwi.com) e crie um projeto com Arduino Uno.
2. Adicione a biblioteca **DHT sensor library** (Adafruit).
3. Monte o circuito conforme o mapeamento de pinos descrito e cole o código presente no arquivo `sketch.ino`.
4. Inicie a simulação e utilize o Monitor Serial (9600 baud) para acompanhar os estados.
