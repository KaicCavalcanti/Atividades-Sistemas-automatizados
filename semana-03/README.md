# Semana 3 - Sinais analógicos e digitais

## Links dos Artefatos
* **Projeto Wokwi:** [Inserir link público da simulação]
* **Google Planilhas:** [Inserir link compartilhável da planilha]

## Tabela de E/S
| Tag | Endereço | Tipo | Faixa | Função |
| :--- | :--- | :--- | :--- | :--- |
| `BTN_COMANDO` | D2 | DI | HIGH/LOW | Ler comando do usuário |
| `LED_ALARME` | D8 | DO | OFF/ON | Sinalizar perigo ($\ge 85\%$) |
| `POT_BRUTO` | A0 | AI | 0-1023 | Adquirir variável simulada |
| `TEMP_PCT` | Memória | REAL | 0-100% | Decidir estados e registrar |

## Amostras Coletadas (Evidências da Planilha)
| Amostra | Bruto ADC | Percentual (%) | Temperatura (°C) | Corrente (mA) | Tensão (V) | Estado |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | 512 | 50,05% | 50,05 °C | 12,01 mA | 5,00 V | NORMAL |
| 2 | 800 | 78,20% | 78,20 °C | 16,51 mA | 7,82 V | ATENÇÃO |
| 3 | 450 | 43,99% | 43,99 °C | 11,04 mA | 4,40 V | NORMAL |
| 4 | 1000 | 97,75% | 97,75 °C | 19,64 mA | 9,78 V | PERIGO |
| 5 | 50 | 4,89% | 4,89 °C | 4,78 mA | 0,49 V | NORMAL |
| 6 | 100 | 9,78% | 9,78 °C | 5,56 mA | 0,98 V | NORMAL |
| 7 | 980 | 95,80% | 95,80 °C | 19,33 mA | 9,58 V | PERIGO |
| 8 | 700 | 68,43% | 68,43 °C | 14,95 mA | 6,84 V | NORMAL |
| 9 | 22 | 2,15% | 2,15 °C | 4,34 mA | 0,22 V | NORMAL |
| 10 | 133 | 13,00% | 13,00 °C | 6,08 mA | 1,30 V | NORMAL |

## Conclusão e Limitações
O protótipo validou com sucesso a cadeia de aquisição analógica, o escalonamento linear, a simulação de laços industriais (4-20 mA e 0-10 V) e a priorização de falhas. O comportamento dos dados demonstra a transição correta entre os estados NORMAL, ATENÇÃO e PERIGO conforme a variação do sinal de entrada. Ressalta-se que a simulação via Wokwi e planilha valida exclusivamente a lógica computacional e os cálculos matemáticos, não atestando restrições físicas reais como ruído eletromagnético ou tolerância de componentes.
