# OMNI | ET Finance Pro V4.0

## Resumo da evolução

A aplicação existente `omnitst.html` foi evoluída sem criar uma aplicação paralela. A identidade visual azul da OMNI | ET foi preservada, o funcionamento local foi mantido e o fluxo de PIN continua disponível. A interface passou a ter navegação inferior orientada para telefone, cartões de leitura rápida, hierarquia visual melhorada e ecrãs adicionais agrupados em **Mais ferramentas**.

## O que foi alterado

O dashboard agora separa explicitamente capital total, capital disponível, capital na rua, lucro realizado, lucro projetado e total a receber. Também apresenta contratos ativos, contratos em dia, atrasados, próximos do vencimento, taxa de recuperação, taxa de atraso, capital em risco, utilização do capital e percentagem disponível. Foi incluído um gráfico local de evolução da carteira e uma área de saúde da carteira com alertas simples.

A gestão de contratos passou a suportar data do contrato, data de vencimento, taxa configurável, observações, pré-visualização do cálculo, número de contrato, estado, saldo, percentagem paga e histórico detalhado. Os cálculos utilizam arredondamento monetário a duas casas decimais e distinguem principal de juros.

O registo de pagamentos suporta pagamentos parciais e totais. Cada pagamento guarda data, valor, saldo antes, saldo depois e observação. O sistema cria também um movimento de entrada e conclui automaticamente o contrato quando o saldo chega a zero.

Foi criada uma ficha individual de cliente com total emprestado, total pago, dívida, lucro gerado, quantidade de contratos, contratos ativos, contratos concluídos, atrasados e pagamentos. A classificação interna é derivada exclusivamente do histórico registado e permite revisão posterior através do próprio histórico; não é usada para aprovar ou recusar crédito automaticamente.

Foram adicionadas a central de vencimentos, o livro de movimentos, relatórios por dia/semana/mês/ano, simulador sem criação automática de contrato, recibo imprimível ou guardável como PDF através da impressão do navegador, mensagens WhatsApp editáveis antes da abertura, arquivamento seguro e filtros operacionais.

## Migração e estrutura dos dados

A versão antiga utilizava as chaves `omin_db_v3` e `omin_config_v3`. A V4.0 não apaga nem substitui a chave antiga. Na primeira execução, os contratos antigos são normalizados para a nova estrutura e guardados em `omin_db_v4`. Antes da migração é criada uma cópia local em `omin_db_v3_backup_before_v4` e também uma cópia com timestamp quando o navegador permite.

A nova base contém `clients`, `contracts`, `movements`, `auditLog`, `settings`, `version` e `lastSavedAt`. Cada contrato conserva compatibilidade com nomes antigos como `cliente`, `telefone`, `dataVencimento`, `valorEmprestado`, `valorJuros`, `valorTotal`, `valorPago`, `saldo` e `historicoPagamentos`. Backups V3 em array e backups V4 em objeto são aceites no restauro.

## Backup e restauração

O exportador gera ficheiros com nomes como `OMNI_ET_BACKUP_2026-08-30.json`, contendo clientes, contratos, pagamentos, movimentos, configurações e histórico de auditoria. Antes de substituir os dados durante um restauro, a base atual é guardada numa cópia local `omin_db_v4_backup_before_restore_<timestamp>`. A restauração requer confirmação explícita. O backup automático local é atualizado depois de cada gravação através da chave `omin_auto_backup_v4`.

A eliminação definitiva foi colocada numa zona crítica, exige escrever `APAGAR` e cria uma cópia antes de remover a base ativa. O comportamento normal de remoção foi substituído por arquivamento, preservando o histórico financeiro.

## Segurança, privacidade e offline

O PIN existente foi preservado e pode ser alterado por um campo de quatro dígitos. Existe bloqueio automático configurável, bloqueio manual, ocultação visual de valores, confirmação de ações críticas e armazenamento local. As consultas, criações, pagamentos, histórico, dashboard, simulador e relatórios não dependem de internet. O WhatsApp só abre uma comunicação quando o utilizador confirma e edita a mensagem.

## Testes realizados

Foi feita validação sintática do JavaScript incorporado com `node --check`, confirmando que o ficheiro é interpretável. A aplicação foi aberta em viewport móvel e verificada visualmente no ecrã de login, no dashboard e no formulário de novo contrato. O PIN padrão `0000` foi validado, o cálculo de teste com capital de Kz 50.000,00 e taxa de 25% apresentou juros de Kz 12.500,00 e total de Kz 62.500,00, e foram confirmadas a presença dos módulos de vencimentos, movimentos, relatórios, simulador, WhatsApp, backup, restauro e arquivamento. Durante o teste dos movimentos foi encontrado e corrigido um erro de apresentação que mostrava `[object Object]` no título do movimento; o rótulo passou a usar corretamente o tipo do movimento.

A validação final de dados reais deve ser executada no telefone que contém a base V3.1, porque o armazenamento local é específico do navegador/dispositivo. Recomenda-se exportar um backup antes da primeira abertura da V4.0 e verificar alguns clientes e contratos existentes após a migração.

## Melhorias futuras recomendadas

Como próximos passos, a aplicação pode evoluir para IndexedDB com versionamento de transações, histórico de capital por período calculado a partir de movimentos, permissões por utilizador, sincronização opcional cifrada entre dispositivos e geração de PDF totalmente offline com uma biblioteca embebida. Estas extensões devem ser feitas apenas depois de confirmar a migração no telefone de produção.

**Ficheiro principal:** `omnitst.html`  
**Versão entregue:** `OMNI | ET Finance Pro V4.0`

> Nota operacional: a versão anterior continua preservada através das chaves locais V3 e das cópias de segurança automáticas de migração. Não foram removidas funcionalidades existentes de login, pesquisa, edição, cobrança, histórico ou WhatsApp.

