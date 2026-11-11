# TP08 - Supervisório do Reservatório R-101

**UC:** Sistemas Automatizados | **Aluno:** Kaic Cavalcanti Pereira

## Objetivo
Prototipar uma interface homem-máquina (HMI) para acompanhar o nível do reservatório R-101, observar o estado da bomba P-101 e diagnosticar alarmes e comunicação.

## Como abrir
Abra `prototipo/supervisorio-r101.html` em um navegador (Chrome, Edge ou Firefox). Não exige internet nem instalação. Use as abas Processo, Alarmes e Manutenção e o seletor **Cenário de teste**.

**Importante:** esta versão é uma demonstração funcional em HTML. O material da disciplina solicita um protótipo construído no **Figma**, com link de visualização testado em janela anônima. Para aderência literal à ferramenta, reproduza as três telas e ligações no Figma e substitua o campo abaixo.

**Link Figma:** PENDENTE - inserir após criação e publicação.

## Telas e navegação
- **Processo:** R-101, nível e qualidade, modos, comunicação, bomba P-101, resumo de alarmes e comandos.
- **Alarmes:** A01–A04 com prioridade, fonte, mensagem, condição, estado de reconhecimento e ação recomendada.
- **Manutenção:** diagnóstico de comunicação/qualidade, permissivos, último comando e último retorno, papéis de acesso.
- Todas as telas permitem voltar à visão Processo; menu superior permite ir diretamente às demais.

## Estados e decisões
- Normal: 58%, qualidade GOOD, comunicação GOOD; comandos apenas se permissivos e perfil forem válidos.
- Atenção: 78%, alarme H (A01), limite H = 75%.
- HH: 92%, alarme HH (A02), limite HH = 90%.
- Falha: COMM_PLC=BAD; nível exibido como `---`, qualidade UNKNOWN e todos os comandos bloqueados.
- P-101: solicitação de parada passa por confirmação e depois exibe separadamente `Solicitação enviada` e retorno `P_101.STATUS=PARADA`.
- Reconhecimento de alarme não remove a condição ativa. Cor é acompanhada de texto.

## Tags
Consulte `testes/tags.csv` (seis tags, tipo, unidade, acesso e descrição). Controle e intertravamento pertencem ao CLP, não à HMI.

## Perfis e permissivos
Os perfis Operador e Manutenção são demonstrativos. Os comandos de operação exigem Operador, comunicação GOOD, modo REMOTO/MANUAL e permissivos positivos. A visão de Manutenção apresenta diagnóstico, sem ignorar intertravamentos.

## Testes
Consulte `testes/casos_teste.csv`. Cinco testes obrigatórios T01–T05 e um adicional T06 são reproduzíveis pelo seletor de cenário. Os resultados registrados são verificações funcionais do HTML, **não** testes de navegação do Figma em Present. Reexecutar no Figma e registrar evidências após a conversão.

## Limitações
Simulação didática, sem CLP, comunicação industrial, banco histórico, autenticação real ou atuação física. Valores e timestamps são fictícios. O seletor de cenários é um instrumento de demonstração e não um comando real de processo.

## Evidências pendentes para submissão no formato pedido
1. Capturas de Processo normal, HH e falha de comunicação **no Figma**.
2. Link público de visualização do protótipo no Figma.
3. Testes T01–T05 executados em Present com resultados e correções reais.
4. Publicação desta pasta TP08 no GitHub e teste anônimo do link.
