# Semana 04 - Sistemas Discretos e Lógica Combinacional (TP04)

## Descrição do Projeto
Implementação e validação da lógica de partida condicionada de máquina usando portas lógicas (CircuitVerse) e linguagem Ladder (PLC Simulator Online).

### Expressão Booleana
M = S AND G AND NOT E

## Evidências e Capturas de Tela

### 1. CircuitVerse
<img width="400" height="255" alt="circuitverse-partida-condicionada" src="https://github.com/user-attachments/assets/515872fa-5244-4038-9728-6b139a1de6b5" />

### 2. Diagrama Ladder
<img width="1346" height="106" alt="ladder-partida-condicionada" src="https://github.com/user-attachments/assets/e186ef06-5380-4f6c-a488-79590d508a96" />

## Tabela de Testes Combinacionais
| ID | S | G | E | M Esperado | M Obtido | Situação |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| P01 | 0 | 0 | 0 | 0 | 0 | PASSA |
| P02 | 0 | 0 | 1 | 0 | 0 | PASSA |
| P03 | 0 | 1 | 0 | 0 | 0 | PASSA |
| P04 | 0 | 1 | 1 | 0 | 0 | PASSA |
| P05 | 1 | 0 | 0 | 0 | 0 | PASSA |
| P06 | 1 | 0 | 1 | 0 | 0 | PASSA |
| P07 | 1 | 1 | 0 | 1 | 1 | PASSA |
| P08 | 1 | 1 | 1 | 0 | 0 | PASSA |
