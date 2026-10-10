# TP06 - CLP e Linguagem Ladder (Semana 6)
**UC Sistemas Automatizados - USJT**  
**Professor:** Prof. Calvetti  

---

## 1. Descrição do Projeto
Implementação e validação de um programa em **Linguagem Ladder (LD)** para partida direta de motor elétrico com retenção por **selo**, **parada prioritária**, permissivos de segurança (emergência e proteção térmica) e indicação luminosa de funcionamento.

A lógica foi desenvolvida e testada no **PLC Simulator Online** e estruturada segundo as recomendações da norma **IEC 61131-3:2025**.

---

## 2. Mapeamento de Entradas e Saídas (Tabela de Tags)

| Tag | Tipo | Descrição Lógica | Convenção Física / Operacional |
| :--- | :--- | :--- | :--- |
| **EMG_OK** | Entrada | Emergência liberada | `1` = Condição saudável / Liberada |
| **OL_OK** | Entrada | Proteção de sobrecarga (Relé térmico) | `1` = Condição saudável / Sem disparo |
| **STOP_OK** | Entrada | Botão de parada | `1` = Pressionado/Saudável em repouso (NF) |
| **START** | Entrada | Botão de partida/liga | `1` = Pressionado (NA) |
| **MOTOR** | Saída | Comando do contator principal do motor | `1` = Energizado / Ligado |
| **RUN** | Saída | Lâmpada indicadora de funcionamento | `1` = Sinalizador aceso |

---

## 3. Equação Booleana do Circuito
$$\text{MOTOR} = \text{EMG\_OK} \cdot \text{OL\_OK} \cdot \text{STOP\_OK} \cdot (\text{START} + \text{MOTOR})$$
$$\text{RUN} = \text{MOTOR}$$

> **Regra de Segurança:** Os contatos `EMG_OK`, `OL_OK` e `STOP_OK` estão posicionados em **série antes da derivação em paralelo** (`START` || `MOTOR`), garantindo prioridade absoluta de parada sobre o botão e sobre a retenção de memória (selo).

---

## 4. Estrutura do Programa Ladder (Rungs)

* **Rung 1 (Comando do Motor):**
  * Contatos normalmente abertos `EMG_OK`, `OL_OK` e `STOP_OK` em série.
  * Ramificação em paralelo contendo o contato de partida `START` e o contato de realimentação (selo) `MOTOR`.
  * Bobina de saída `MOTOR`.

* **Rung 2 (Sinalização de Funcionamento):**
  * Contato normalmente aberto `MOTOR` em série com a bobina de saída `RUN`.

---

## 5. Casos de Teste (Validação)

| ID | Condição Inicial | Ação no Simulador | Resultado Esperado | Resultado Obtido | Situação | Justificativa |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **T01** | `EMG_OK=1`, `OL_OK=1`, `STOP_OK=1`, `MOTOR=0` | `START=1` | `MOTOR=1`, `RUN=1` | `MOTOR=1`, `RUN=1` | **PASSA** | Caminho de partida completo e energizado |
| **T02** | `MOTOR=1`, `RUN=1` | `START=0` | `MOTOR=1`, `RUN=1` | `MOTOR=1`, `RUN=1` | **PASSA** | Contato MOTOR mantém a continuidade (selo) |
| **T03** | `MOTOR=1`, `RUN=1` | `STOP_OK=0` | `MOTOR=0`, `RUN=0` | `MOTOR=0`, `RUN=0` | **PASSA** | Parada abre o circuito antes do ramo de selo |
| **T04** | `MOTOR=1`, `RUN=1` | `EMG_OK=0` | `MOTOR=0`, `RUN=0` | `MOTOR=0`, `RUN=0` | **PASSA** | Emergência remove a continuidade de toda a linha |
| **T05** | `MOTOR=0`, `EMG_OK=0` | `START=1` | `MOTOR=0`, `RUN=0` | `MOTOR=0`, `RUN=0` | **PASSA** | Partida não vence o bloqueio de emergência |

---

## 6. Evidências da Simulação (Capturas de Tela)

Os registros gráficos das etapas de teste estão disponíveis no diretório `/capturas`:

1. `01_partida_pressionada.png` - Pulso no botão `START` e acionamento da saída `MOTOR`.
2. `02_bloqueio_emergencia.png` - Tentativa de partida com `EMG_OK=0` sem energizar o motor.

---

## 7. Link do Projeto no Simulador
* **Simulador Utilizado:** [PLC Simulator Online](https://plcsimulator.online/)
* **Link para Execução Direta:** (https://app.plcsimulator.online/sXQUm0Qbb0fPyWOoBLWF)

---

## 8. Limites do Modelo e Considerações
