# TP09 · Redes industriais — célula heterogênea

UC Sistemas Automatizados · Semana 9 · Prof. Calvetti · USJT 2026.2

## 1. Objetivo, cenário e requisitos dominantes
Projetar e documentar a rede industrial de uma planta com **Célula A** (CLP, IHM, 2 remotas e 3 inversores), **Célula B** (controlador legado Modbus RTU) e **supervisão** (servidor SCADA na sala de controle).

Condições: 120 m entre células e sala · EMI elevada perto dos motores · parada de até 15 min aceitável · 4 novos nós no próximo semestre.

| ID | Requisito | Consequência de projeto |
|---|---|---|
| R01 | Integrar o legado sem substituí-lo | Gateway serial-Ethernet (GW-B) |
| R02 | E/S com atualização cíclica | Rede industrial compatível (PROFINET na Célula A) |
| R03 | 120 m e EMI elevada | Fibra entre áreas |
| R04 | Crescimento de 4 nós | Faixas de endereço reservadas |
| R05 | Recuperação em 15 min | Backup e sobressalente testados |
| R06 | Falhas contidas por célula | Segmentação por zona (VLAN 10/30/40) |

## 2. Diagrama
![Arquitetura](evidencias/arquitetura.png)

Fonte editável: `diagrama/rede.drawio` · versão de leitura: `diagrama/rede.pdf`.

## 3. Inventário de nós, zonas e interfaces
| ID | Zona | Tipo | Interface | Endereço | Nome PROFINET | Função | Crit. |
|---|---|---|---|---|---|---|---|
| SCADA | Supervisão | Servidor SCADA | Ethernet | 10.20.10.20/24 | - | Supervisão e histórico; lê tags do CLP-A | Média |
| SW-SUP | Supervisão | Switch gerenciável | Gestão Ethernet | 10.20.10.2/24 | - | Comutação da zona de supervisão (VLAN 10); ponta da fibra para a Célula A | Média |
| SW-A | Célula A | Switch gerenciável | Gestão Ethernet | 10.20.30.2/24 | - | Núcleo da Célula A (VLAN 30, estrela); fibra para SW-SUP e GW-B | Alta |
| CLP-A | Célula A | CLP (controlador) | PROFINET | 10.20.30.10/24 | clp-a | Controle da Célula A | Alta |
| HMI-A | Célula A | IHM | Ethernet | 10.20.30.20/24 | - | Operação local da Célula A | Média |
| RIO-A1 | Célula A | E/S remota | PROFINET | 10.20.30.31/24 | rio-a1 | Sensores e atuadores | Alta |
| RIO-A2 | Célula A | E/S remota | PROFINET | 10.20.30.32/24 | rio-a2 | Sensores e atuadores | Alta |
| INV-A1 | Célula A | Inversor (acionamento) | PROFINET | 10.20.30.41/24 | inv-a1 | Acionamento de motor | Alta |
| INV-A2 | Célula A | Inversor (acionamento) | PROFINET | 10.20.30.42/24 | inv-a2 | Acionamento de motor | Alta |
| INV-A3 | Célula A | Inversor (acionamento) | PROFINET | 10.20.30.43/24 | inv-a3 | Acionamento de motor | Alta |
| GW-B | Célula B | Gateway Ethernet ↔ Modbus RTU | Ethernet | 10.20.40.10/24 | - | Converte Ethernet/TCP em Modbus RTU para o legado | Alta |
| CTRL-B | Célula B | Controlador legado | Modbus RTU (RS-485) | Modbus end. 5 | - | Controle da Célula B (escravo Modbus) | Alta |

## 4. Mapa de endereçamento e reservas
Sub-redes /24: Supervisão `10.20.10.0` (VLAN 10), Célula A `10.20.30.0` (VLAN 30), Célula B `10.20.40.0` (VLAN 40). Gateway padrão de cada VLAN: `.1`. Endereço Modbus (serial) não pertence à rede IP: o GW-B faz a ponte.

| Ativo | Zona | Interface | Endereço | Faixa reservada |
|---|---|---|---|---|
| SCADA | Supervisão | Ethernet | 10.20.10.20/24 | .20–.29 servidores |
| SW-SUP | Supervisão | Gestão Ethernet | 10.20.10.2/24 | .1–.9 infraestrutura |
| SW-A | Célula A | Gestão Ethernet | 10.20.30.2/24 | .1–.9 infraestrutura |
| CLP-A | Célula A | PROFINET | 10.20.30.10/24 | .10–.19 controle |
| HMI-A | Célula A | Ethernet | 10.20.30.20/24 | .20–.29 operação |
| RIO-A1 | Célula A | PROFINET | 10.20.30.31/24 | .30–.39 E/S |
| RIO-A2 | Célula A | PROFINET | 10.20.30.32/24 | .30–.39 E/S |
| INV-A1 | Célula A | PROFINET | 10.20.30.41/24 | .40–.49 drives |
| INV-A2 | Célula A | PROFINET | 10.20.30.42/24 | .40–.49 drives |
| INV-A3 | Célula A | PROFINET | 10.20.30.43/24 | .40–.49 drives |
| GW-B | Célula B | Ethernet | 10.20.40.10/24 | .10–.19 controle |
| CTRL-B | Célula B | Modbus RTU (RS-485) | Modbus end. 5 | 1–9 legado |
| EXP-A | Célula A | — | 10.20.30.50-59/24 | .50–.59 expansão |
| EXP-B | Célula B | — | 10.20.40.20-29/24 | .20–.29 expansão |

Verificação de duplicidade: coluna *Duplicidade* da aba **Nós** (`=SE(CONT.SE(...)>1;"DUPLICADO";"OK")`) → 0 duplicados.

## 5. Justificativa de topologias, meios e protocolos
**Topologia:** estrela por zona (cada ativo em porta própria do switch): facilita achar a falha e crescer. Anel não se justifica: a parada tolerada é 15 min e não há requisito de continuidade sem interrupção.

**Matriz protocolo-aplicação** (nota 1–5 × peso; detalhes e justificativas por nota na aba **Matriz**):

| Critério | Peso | Modbus RTU | PROFINET | Ethernet comum |
|---|---|---|---|---|
| Compatibilidade | 5 | 3 | 5 | 4 |
| Prazo cíclico | 5 | 2 | 5 | 2 |
| Diagnóstico | 4 | 2 | 5 | 3 |
| Distância e EMI | 4 | 3 | 4 | 4 |
| Custo de integração | 3 | 5 | 3 | 4 |
| Expansão | 3 | 2 | 5 | 4 |
| **Total ponderado** |  | **66** | **110** | **82** |

PROFINET venceu para a Célula A. Modbus RTU é **eliminado** na Célula A (não entrega ciclo/diagnóstico para E/S e inversores PROFINET), mas é **mantido** no trecho legado porque é o único protocolo do CTRL-B. Ethernet comum fica só como transporte TCP/IP entre SCADA e CLP-A. A soma não autoriza usar um protocolo em dispositivo que não o suporte.

### Justificativa por trecho
**Célula A (CLP-A, RIO-A1/A2, INV-A1/A3, HMI-A ↔ SW-A)** → PROFINET em estrela, cobre blindado.  
*Requisito:* Atualização cíclica de E/S + diagnóstico por dispositivo (R02).  
*Por quê:* Todos os dispositivos declaram PROFINET; a estrela dá um enlace por nó e facilita achar a falha; PROFINET foi a maior nota ponderada (110 de 120).  
*Risco/mitigação:* SW-A é ponto único de falha → fonte protegida, config versionada, switch sobressalente testado (parada tolerada: 15 min).  
*Teste:* T01, T06

**Entre áreas (SW-SUP ↔ SW-A) e SW-A ↔ GW-B** → Fibra óptica.  
*Requisito:* 120 m + EMI elevada perto dos motores (R03).  
*Por quê:* Fibra é imune a EMI, isola eletricamente as zonas e atende a distância; cobre ficaria exposto ao ruído.  
*Risco/mitigação:* Rompimento isola o SCADA das células → controle local continua; alarme de link down; 2ª rota é extensão opcional.  
*Teste:* T04

**Supervisão (SCADA ↔ CLP-A)** → Ethernet TCP/IP segmentada (VLAN 10 / VLAN 30).  
*Requisito:* Supervisão com diagnóstico e falhas contidas por zona (R06).  
*Por quê:* O SCADA só precisa ler tags (prazo folgado); VLANs separam supervisão e células e limitam broadcast e propagação de falha.  
*Risco/mitigação:* Fluxos entre VLANs só os necessários (SCADA ↔ CLP-A / GW-B); IP duplicado coberto por controle de configuração.  
*Teste:* T03, T04

**Célula B (GW-B ↔ CTRL-B)** → Modbus RTU sobre RS-485 via gateway.  
*Requisito:* Integrar o legado sem substituí-lo (R01).  
*Por quê:* O CTRL-B só fala Modbus RTU. O gateway converte para Ethernet sem estender o barramento serial até a supervisão. Endereço 5, timeout e estado seguro documentados.  
*Risco/mitigação:* GW-B é ponto único para o legado → watchdog, backup de config e sobressalente; timeout invalida dado e aciona estado seguro.  
*Teste:* T02

### Verificação simplificada do ciclo serial (hipótese didática)
Duas consultas Modbus com 45 ms de requisição+resposta e 20 ms de intervalo cada: 2 × (45 + 20) = 130 ms; com margem de 25 %: 162,5 ms < 500 ms → orçamento atendido **nesta hipótese**. O valor real precisa ser medido (pior caso, retransmissões, carga do gateway, timeout).

## 6. Pontos únicos de falha e mitigações
| ID | Elemento | Falha | Impacto | Tempo tol. | Mitigação | Teste |
|---|---|---|---|---|---|---|
| R01 | SW-A (e sua fonte) | Sem energia / defeito | Célula A isolada: CLP-A, RIO, INV, HMI e GW-B ficam sem rede | 15 min | Fonte protegida, configuração salva e versionada, switch sobressalente compatível | T06 |
| R02 | GW-B | Travamento | CTRL-B (legado) indisponível; Célula A segue operando | 15 min | Watchdog, backup de configuração, gateway sobressalente; timeout aciona estado seguro | T02 |
| R03 | Fibra SW-SUP ↔ SW-A | Rompimento | SCADA sem as células; controle local permanece | 15 min | Alarme de link down, rota/fibra reserva em duto distinto (extensão), peças de reposição | T04 |
| R04 | Plano de endereçamento | IP duplicado | Comunicação instável de um ou mais nós | Imediato (corrigir) | Controle de configuração, verificação COUNTIF na planilha, faixas reservadas por função | T03 |
| R05 | SW-SUP | Sem energia | SCADA perde o caminho até as células; controle local permanece | 15 min | Fonte protegida e switch sobressalente; células operam sem supervisão | —(proposto) |
| R06 | Configuração dos equipamentos | Perda / versão divergente | Reposição lenta, estourando os 15 min tolerados | 15 min | Backup versionado de switches, gateway e CLP; restauração testada em sobressalente | —(proposto) |

## 7. Casos de teste e problemas corrigidos
Detalhes (estado inicial, estímulo, como observar) na aba **Testes** e em `testes/casos_teste.csv`.

| ID | Cenário | Esperado | Obtido (documental) | Situação |
|---|---|---|---|---|
| T01 | Todos os enlaces ativos | Todos os nós previstos acessíveis | Inventário (12 ativos) e diagrama (12 ativos) coincidem, com os mesmos IDs e endereços | Aprovado (documental) |
| T02 | Gateway B desconectado | Somente o legado fica indisponível | No diagrama o GW-B é um nó-folha ligado a SW-A por enlace próprio; nenhum nó da Célula A depende dele | Aprovado (documental) |
| T03 | IP duplicado na Célula A | Conflito identificado e corrigido | Em cópia de teste da planilha a fórmula marcou DUPLICADO nas duas linhas e a planilha final tem 0 duplicados | Aprovado (documental) |
| T04 | Fibra até a supervisão removida | Controle local permanece ativo; SCADA indica a perda | O tráfego CLP-A ↔ RIO/INV/HMI-A passa só pelo SW-A; a fibra L02 é apenas o caminho da supervisão | Aprovado (documental) |
| T05 | Novo nó adicionado | Usa a faixa reservada, sem renumerar ativos | Faixa 10.20.30.50-59 livre na planilha e no diagrama; cabem os 4 nós previstos | Aprovado (documental) |
| T06 | SW-A sem energia (extra) | Célula A isolada e alarmada; reposição de fonte/switch em até 15 min | Só dá para comprovar em bancada: depende do sobressalente e do backup de configuração | Pendente (bancada) |

- Problema corrigido/verificado: a fórmula de duplicidade foi testada numa cópia da planilha, repetindo um IP da Célula A; ela marcou as duas linhas como `DUPLICADO`. Na planilha final não há duplicidade.

## 8. Limites da validação documental
Diagrama e planilha **documentam** a arquitetura; não simulam tráfego nem comprovam tempo de ciclo, interoperabilidade ou disponibilidade. Os testes marcados "Aprovado (documental)" são inspeção dos artefatos; T06 e as medições reais de tempo, perda e recuperação exigem bancada ou simulador de protocolo. IPs, nomes e tempos são valores didáticos.

## Estrutura do repositório
```
TP09/
├── README.md
├── diagrama/rede.drawio
├── diagrama/rede.pdf
├── planilha/TP09-rede.xlsx
├── evidencias/arquitetura.png
├── evidencias/matriz.png
└── testes/casos_teste.csv
```
