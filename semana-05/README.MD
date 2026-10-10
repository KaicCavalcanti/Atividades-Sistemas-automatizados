# TP05 - Sistemas sequenciais e GRAFCET/SFC

## Objetivo
Modelar e validar um semáforo veicular temporizado usando GRAFCET (Atividade 24 - obrigatória).

## Convenções
- Etapa inicial com borda dupla (E0).
- Etapas numeradas (E0, E1, E2), cada uma comandando uma única cor.
- Ações em retângulos laterais ligados à etapa.
- Transições como barras horizontais, com receptividades temporizadas ao lado.

## Artefatos
- Fonte editável: `grafcet.drawio`
- Diagrama exportado: `semaforo.png`
- Testes: `casos_teste.csv`
- Notas: `NOTAS.md`

## Tabela de etapas e ações
| Etapa | Situação | Ação |
|-------|----------|------|
| E0 (inicial) | Vermelho | VERMELHO = 1 |
| E1 | Verde | VERDE = 1 |
| E2 | Amarelo | AMARELO = 1 |

## Tabela de transições
| Transição | De → para | Receptividade |
|-----------|-----------|---------------|
| T0 | E0 → E1 | tempo de E0 ≥ 8 s |
| T1 | E1 → E2 | tempo de E1 ≥ 10 s |
| T2 | E2 → E0 | tempo de E2 ≥ 3 s |

## Decisões e hipóteses
- Origem do tempo: o relógio de cada etapa começa quando ela é ativada.
- Estado seguro/inicial: após a inicialização, apenas o vermelho fica aceso (E0).
- Não há seleção nem paralelismo neste modelo, então não existe conflito de prioridade.
- Sem sensores de entrada: a evolução depende apenas do tempo.

## Percurso manual (Atividade 27)
| Passo | Marcação | Evento ou tempo | Transição | Nova marcação |
|-------|----------|-----------------|-----------|---------------|
| 0 | E0 | 7,9 s | nenhuma | E0 |
| 1 | E0 | 8,0 s | T0 | E1 |
| 2 | E1 | 9,9 s | nenhuma | E1 |
| 3 | E1 | 10,0 s | T1 | E2 |
| 4 | E2 | 3,0 s | T2 | E0 |

## Resultados
Todos os cenários de `casos_teste.csv` (antes do limite, no limite e depois do tempo) foram percorridos manualmente e passaram.
