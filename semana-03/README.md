# Semana 3 — Sinais Analógicos e Digitais

## Links dos Artefatos

* **Projeto Wokwi:**
* **Google Planilhas:**

---

## Tabela de E/S

| Tag           | Endereço | Tipo | Faixa    | Função                      |
| ------------- | -------- | ---- | -------- | --------------------------- |
| `BTN_COMANDO` | D2       | DI   | HIGH/LOW | Ler comando do usuário      |
| `LED_ALARME`  | D8       | DO   | OFF/ON   | Sinalizar perigo (≥ 85%)    |
| `POT_BRUTO`   | A0       | AI   | 0–1023   | Adquirir variável simulada  |
| `TEMP_PCT`    | Memória  | REAL | 0–100%   | Decidir estados e registrar |

---

## Casos de Teste de Aquisição e Fronteira

| Caso   | Entrada / Condição                         | Resposta Esperada              | Status     |
| ------ | ------------------------------------------ | ------------------------------ | ---------- |
| **A1** | Botão liberado; potenciômetro no mínimo    | 0 (botão); próximo de 0 (ADC)  | ✅ Aprovado |
| **A2** | Botão pressionado; potenciômetro no mínimo | 1 (botão); próximo de 0 (ADC)  | ✅ Aprovado |
| **T1** | Percentual em -1%                          | FALHA (abaixo da faixa válida) | ✅ Aprovado |
| **T2** | Percentual em 0%                           | NORMAL (limite inferior)       | ✅ Aprovado |
| **T3** | Percentual em 70%                          | ATENÇÃO (limite de atenção)    | ✅ Aprovado |
| **T4** | Percentual em 85%                          | PERIGO (limite de perigo)      | ✅ Aprovado |
| **T5** | Percentual em 100,01%                      | FALHA (acima da faixa válida)  | ✅ Aprovado |

---

## Conclusão e Limitações

O protótipo validou com sucesso a cadeia de aquisição analógica, o escalonamento linear para grandezas de engenharia, a simulação de laços industriais **4–20 mA** e **0–10 V**, além da priorização de falhas de instrumento em relação aos estados de processo.

Ressalta-se que a simulação realizada por meio do **Wokwi** e da planilha valida exclusivamente a lógica computacional e os cálculos matemáticos.

Dessa forma, o protótipo não considera integralmente restrições presentes em aplicações físicas reais, como:

* Ruído eletromagnético;
* Tolerância de componentes elétricos;
* Variações dos sinais de entrada;
* Calibração metrológica;
* Condições reais de operação industrial.

Portanto, os resultados obtidos demonstram o funcionamento adequado da lógica proposta dentro do ambiente de simulação.
