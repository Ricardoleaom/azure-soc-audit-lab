Monitorando acessos no Azure (Lab de auditoria)
Nesse laboratório eu quis resolver um problema comum: como saber se alguém mexeu em arquivos importantes da empresa sem autorização? Usei o Azure para criar um ambiente seguro e conseguir rastrear tudo o que acontece lá dentro.

O que eu fiz na prática:
Organizei os acessos: Não deixei todo mundo entrar em tudo. Usei o esquema de "Privilégio Mínimo" (RBAC). Só quem precisa trabalhar com os arquivos tem a chave da pasta.

Liguei a "caixa preta": Ativei os Logs de Diagnóstico. Muita gente esquece disso, mas é o que permite saber não só quem entrou na pasta, mas quem baixou ou leu um arquivo específico.

Criei um filtro de busca: Usei o KQL (que é tipo um comando de busca do Azure) para gerar uma lista automática. Com um clique, eu consigo ver o horário, o IP da pessoa e o que exatamente ela acessou.

Por que isso é importante?
Hoje em dia, com as leis de proteção de dados (LGPD), as empresas precisam provar que os dados estão seguros. Esse projeto mostra como eu consigo dar essa transparência para um time de segurança.

A query que usei para filtrar os acessos:
```kusto
StorageBlobLogs
| where TimeGenerated > ago(24h)
| where OperationName == "GetBlob"
| project TimeGenerated, OperationName, ObjectKey, CallerIpAddress, AuthenticationType
