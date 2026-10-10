# Semana 7 - Atividade Prática P07: Programação Sequencial de CLP

## Descrição do Projeto
Esta atividade prática consiste na implementação e validação de um programa em linguagem Ladder no **PLC Simulator Online** para controle temporizado de partida de motor (`MOTOR_DELAY`) com interrupção de segurança.

## Requisitos de Funcionamento
1. O acionamento da partida exige que o botão de emergência (`EMG_OK`) e o botão de parada (`STOP_OK`) estejam ativos (nível lógico 1).
2. Ao pressionar `START`, o temporizador `T1` (do tipo **TON**) inicia a contagem do tempo predefinido ($PT = 3\text{s}$).
3. A saída `MOTOR_DELAY` liga somente após a conclusão dos 3 segundos ($ET = 3\text{s}$ e $Q = 1$).
4. A perda de sinal de `EMG_OK` ou `STOP_OK` interrompe a temporização imediatamente e desliga a saída.

---

## Mapeamento de Variáveis (Tags)

| Nome da Tag | Tipo | Descrição / Função |
| :--- | :--- | :--- |
| `EMG_OK` | `Bool` | Botão de emergência (Permissivo de Segurança) |
| `STOP_OK` | `Bool` | Botão de parada geral (Permissivo) |
| `START` | `Bool` | Comando de partida do processo |
| `T1` | `Timer` | Temporizador ao ligar (TON) com $PT = 3\text{s}$ ($3000\text{ms}$) |
| `MOTOR_DELAY` | `Bool` | Saída digital para acionamento do motor |

---

## Estrutura Lógica em Ladder

* **Rung 1:** `EMG_OK AND STOP_OK AND START` ➔ Habilita o bloco `TON T1`.
* **Rung 2:** Contato `T1.Q` ➔ Comanda a bobina de saída `MOTOR_DELAY`.

---

## Matriz de Testes Executados (T01 a T04)

| ID | Ação no Simulador | Resultado Esperado | Resultado Obtido | Situação |
| :--- | :--- | :--- | :--- | :--- |
| **T01** | Pressionar e manter `START` por 3s | $Q = 1$ e `MOTOR_DELAY = 1` | $Q = 1$ e `MOTOR_DELAY = 1` | Aprovado |
| **T02** | Tornar `STOP_OK = 0` durante a contagem $ET$ | $ET = 0$ e `MOTOR_DELAY = 0` | $ET = 0$ e `MOTOR_DELAY = 0` | Aprovado |
| **T03** | Tornar `EMG_OK = 0` com o motor já ligado | `MOTOR_DELAY = 0` | `MOTOR_DELAY = 0` | Aprovado |
| **T04** | Normalizar falhas sem pressionar `START` | `MOTOR_DELAY = 0` | `MOTOR_DELAY = 0` | Aprovado |

---

## Estrutura de Evidências

```text
semana-07/
├── README.md
├── casos_teste.csv
└── capturas/
    ├── T01_motor_ligado_3s.png
    ├── T02_falha_stop.png
    ├── T03_falha_emergencia.png
    └── T04_retorno_sem_start.png
