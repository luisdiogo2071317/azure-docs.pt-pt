---
title: Migrar clusters do Apache Hadoop no local para o Azure HDInsight - segurança e as melhores práticas de DevOps
description: Saiba mais segurança e melhores práticas do DevOps para migrar clusters do Hadoop no local para Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 1a1cf731678ef7678b740020a4d61725f9a2b32a
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221953"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---security-and-devops-best-practices"></a>Migrar clusters do Apache Hadoop no local para o Azure HDInsight - segurança e as melhores práticas de DevOps

Este artigo oferece recomendações de segurança e de DevOps nos sistemas do Azure HDInsight. É parte de uma série que fornece as práticas recomendadas para ajudar a migrar sistemas de Apache Hadoop no local para Azure HDInsight.

## <a name="use-the-enterprise-security-package-to-secure-and-govern-the-cluster"></a>Utilizar o Enterprise Security Package para proteger e regular o cluster

O pacote de segurança da empresa (ESP) suporta a autenticação baseada no Active Directory, o suporte de multiutilizador e o controlo de acesso baseado em funções. Com a opção de ESP escolhida, cluster do HDInsight está associado ao domínio do Active Directory e o administrador da empresa pode configurar o controlo de acesso baseado em funções (RBAC) para a segurança do Hive com o Apache Ranger. O administrador também pode auditar o acesso a dados por funcionários e quaisquer alterações efetuadas às políticas de controlo de acesso.

Recursos do ESP estão atualmente em pré-visualização e estão disponíveis apenas nos seguintes tipos de cluster: Apache Hadoop, Apache Spark, Apache HBase, Apache Kafka e Apache Interactive Query.

Utilize os seguintes passos para implementar o cluster do HDInsight associados a um domínio:

- Implementar o Azure Active Directory (AAD), transmitindo o nome de domínio
- Implementar os serviços de domínio do Azure Active Directory (AAD DS)
- Criar a rede Virtual e sub-rede necessária
- Implementar uma VM na rede Virtual para gerir DS do AAD
- Associar a VM ao domínio
- Instalar o AD e as ferramentas DNS
- Que o administrador de DS do AAD criar uma unidade organizacional (UO)
- Ativar o LDAPS para DS do AAD
- Crie uma conta de serviço no Azure Active Directory com o delegado de leitura e escrita permissão ao administrador para a UO, para que ele pode. Esta conta de serviço pode, em seguida, associar máquinas ao domínio e coloque principais de máquina na UO. Ele também pode criar principais de serviço na UO que especificou durante a criação do cluster.

    > [!Note]
    > A conta de serviço não tem de ser a conta de administrador de domínio do AD

- Implemente o cluster de HDInsight ESP, definindo os seguintes parâmetros:
    - **Nome de domínio**: O nome de domínio que está associada com o Azure AD DS.
    - **Nome de utilizador de domínio**: A conta de serviço no domínio do Azure AD DS DC gerido que criou na secção anterior, por exemplo: `hdiadmin@contoso.onmicrosoft.com`. Este utilizador de domínio será o administrador deste cluster do HDInsight.
    - **Palavra-passe do domínio**: A palavra-passe da conta de serviço.
    - **Unidade organizacional**: O nome único da UO que pretende utilizar com o cluster do HDInsight, por exemplo: `OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com`. Se essa UO não existir, o cluster de HDInsight tenta criar a UO usando os privilégios da conta de serviço.
    - **URL de LDAPS**: por exemplo, `ldaps://contoso.onmicrosoft.com:636`.
    - **Grupo de utilizadores de acesso**: os grupos de segurança cujos usuários que quer sincronizar para o cluster, por exemplo: `HiveUsers`. Se pretender especificar múltiplos grupos de utilizador, separadas por ponto e vírgula ";". O grupo (s) tem de existir no diretório antes de criar o cluster do ESP.

Para obter mais informações, veja os artigos seguintes:

- [Uma introdução à segurança do Hadoop com clusters do HDInsight associados a um domínio](../domain-joined/apache-domain-joined-introduction.md)
- [Planear clusters do Hadoop associados a um domínio do Azure no HDInsight](../domain-joined/apache-domain-joined-architecture.md)
- [Configurar um cluster do HDInsight associados a um domínio com o Azure Active Directory Domain Services](../domain-joined/apache-domain-joined-configure-using-azure-adds.md)
- [Sincronizar utilizadores do Azure Active Directory para um cluster do HDInsight](../hdinsight-sync-aad-users-to-cluster.md)
- [Configurar políticas do Hive no HDInsight associado a um domínio](../domain-joined/apache-domain-joined-run-hive.md)
- [Execute o Apache Oozie no Hadoop do HDInsight associados a um domínio clusters](../domain-joined/hdinsight-use-oozie-domain-joined-clusters.md)

## <a name="implement-end-to-end-enterprise-security-management"></a>Implementar a gestão de segurança de ponta a ponta enterprise

Ponto a segurança da empresa de ponto pode ser obtido usando os controles seguintes:

- **Pipeline de dados particulares e protegidos** (segurança de nível de perímetro):
    - Segurança em nível de perímetro pode ser alcançada através de redes virtuais do Azure, os grupos de segurança de rede e o serviço de Gateway

- **Autenticação e autorização de acesso a dados**
    - Crie o cluster HDI associados a um domínio com o Azure Active Directory Domain Services. (Pacote de segurança empresarial)
    - Utilizar Ambari para fornecer acesso baseado em funções aos recursos do cluster para os utilizadores do AD
    - Utilizar o Apache Ranger para definir políticas de controlo de acesso para o Hive na mesa / coluna / nível de linha.
    - Acesso SSH para o cluster pode ser restringido apenas ao administrador.

- **Auditoria**
    - Ver e comunicar todo o acesso aos dados e recursos de cluster do HDInsight.
    - Ver e comunicar todas as alterações para as políticas de controlo de acesso

- **Encriptação**
    - Encriptação do lado do servidor transparente com chaves geridas pela Microsoft ou as chaves geridas pelo cliente.
    - Na encriptação de trânsito através de encriptação do lado do cliente, https e TLS

Para obter mais informações, veja os artigos seguintes:

- [Descrição geral de redes virtuais do Azure](../../virtual-network/virtual-networks-overview.md)
- [Descrição geral de grupos de segurança de rede do Azure](../../virtual-network/security-overview.md)
- [Peering de rede Virtual do Azure](../../virtual-network/virtual-network-peering-overview.md)
- [Guia de segurança do armazenamento do Azure](../../storage/common/storage-security-guide.md)
- [Encriptação do serviço de armazenamento do Azure em repouso](../../storage/common/storage-service-encryption.md)

## <a name="use-monitoring--alerting"></a>Utilização de monitorização e alertas

Para obter mais informações, consulte o artigo:

[Descrição Geral do Azure Monitor](../../azure-monitor/overview.md)

## <a name="upgrade-clusters"></a>Atualizar clusters

Atualize regularmente para a versão mais recente do HDInsight para tirar partido das funcionalidades mais recentes. Os seguintes passos podem ser utilizados para atualizar o cluster para a versão mais recente:

- Crie um novo cluster de HDI de teste com a versão HDI mais recente disponível.
- Teste no novo cluster para se certificar de que as cargas de trabalho e tarefas funcionam conforme esperado.
- Modificar tarefas ou aplicações ou cargas de trabalho conforme necessário.
- Fazer backup de todos os dados transitórios armazenados localmente em nós do cluster.
- Elimine o cluster existente.
- Crie um cluster do HDInsight versão mais recente na mesma sub-rede VNET, utilizar o mesmo arquivo de dados e metadados de predefinição que o cluster anterior.
- Importe todos os dados transitórios que foi feitos backup.
- Iniciar tarefas/continuam a utilizar o novo cluster de processamento.

Para obter mais informações, consulte o artigo: [cluster HDInsight de atualizar para uma nova versão](../hdinsight-upgrade-cluster.md)

## <a name="patch-cluster-operating-systems"></a>Sistemas de operativos do cluster de patch

Como um serviço gerido do Hadoop HDInsight assume o controlo de aplicação de patches de SO das VMs utilizados pelos clusters do HDInsight.

Para obter mais informações, consulte o artigo: [aplicação de patches de SO para HDInsight](../hdinsight-os-patching.md)

## <a name="post-migration"></a>Pós-migração

1. **Correção dos aplicativos** - iterativamente efetue as alterações necessárias para os trabalhos, processos e scripts
2. **Executar testes de** - iterativamente execução funcional e desempenho testes
3. **Otimizar** – resolva problemas de desempenho com base nos resultados de teste acima e, em seguida, testar novamente para confirmar as melhorias de desempenho.

## <a name="appendix-gathering-details-to-prepare-for-a-migration"></a>Apêndice: detalhes para se preparar para uma migração de recolha

Esta seção fornece questionários de modelo para o ajudar a reunir informações importantes sobre:

- A implementação no local.
- Detalhes do projeto.
- Requisitos do Azure.

### <a name="on-premises-deployment-questionnaire"></a>Questionário de implementação no local

| **Pergunta** | **Exemplo** | **Resposta** |
|---|---|---|
|**Tópico**: **ambiente**|||
|Tipo de distribuição de cluster|Hortonworks, Cloudera, MapR| |
|Versão de distribuição do cluster|HDP 2.6.5, CDH 5.7|
|Grandes componentes do sistema ao meio ambiente de dados|HDFS, Yarn, Hive, LLAP, Impala, Kudu, HBase, Spark, MapReduce, Kafka, Zookeeper, Solr, Sqoop, Oozie, Ranger, Atlas, Falcon, Zeppelin, R|
|Tipos de cluster|Hadoop, Spark, Confluent Kafka, Storm, Solr|
|Número de clusters|4|
|Número de nós do mestre|2|
|Número de nós de trabalho|100|
|Número de nós de extremidade| 5|
|Total de espaço em disco|100 TB|
|Configuração do nó principal|m/y, cpu, disco, etc.|
|Configuração de nós de dados|m/y, cpu, disco, etc.|
|Configuração de nós de extremidade|m/y, cpu, disco, etc.|
|Encriptação de HDFS?|Sim|
|Elevada Disponibilidade|HA do HDFS, Metastore HA|
|Recuperação após desastre / cópia de segurança|Cluster de cópia de segurança?|  
|Sistemas que são dependentes do Cluster|SQL Server, Teradata, Power BI, MongoDB|
|Integrações de terceiros|Tableau, GridGain, Qubole, Informatica, Splunk|
|**Tópico**: **segurança**|||
|Segurança de perímetro|Firewalls|
|Cluster de autenticação e autorização|Active Directory, Ambari, Cloudera o Gestor de diretório, sem autenticação|
|Controlo de acesso do HDFS|  Manual, ssh utilizadores|
|Ramo de registo de autenticação e autorização|Sentry, LDAP, AD com o Kerberos, o Ranger|
|Auditoria|Ambari, navegador Cloudera, Ranger|
|Monitorização|Graphite, recolhidos, statsd, Telegraf, InfluxDB|
|Alertas|Kapacitor, Prometheus, Datadog|
|Duração da retenção de dados| 3 anos, 5 anos|
|Administradores de cluster|Administrador de único, vários administradores|

### <a name="project-details-questionnaire"></a>Questionário de detalhes do projeto

|**Pergunta**|**Exemplo**|**Resposta**|
|---|---|---|
|**Tópico**: **cargas de trabalho e a frequência**|||
|Tarefas de MapReduce|10 tarefas – duas vezes por dia||
|Tarefas do Hive|100 tarefas – cada hora||
|Tarefas de lote do Spark|50 tarefas – a cada 15 minutos||
|Tarefas de transmissão em fluxo do Spark|5 tarefas – a cada 3 minutos||
|Tarefas de transmissão em fluxo estruturada|5 tarefas – a cada minuto||
|Tarefas de formação do modelo de ML|2 tarefas, uma vez numa semana||
|Linguagens de Programação|Python, Scala, Java||
|Scripts|Shell, Python||
|**Tópico**: **dados**|||
|Origens de dados|Arquivos simples, Json, Kafka, RDBMS||
|Orquestração de dados|Fluxos de trabalho do Oozie, ventilação||
|Em pesquisas de memória|Apache Ignite, Redis||
|Destinos de dados|HDFS, RDBMS, Kafka, MPP ||
|**Tópico**: **metadados**|||
|Tipo de BD do Hive|MySQL, Postgres||
|Não. de metastores do Hive|2||
|Não. de tabelas do Hive|100||
|Não. de políticas do Ranger|20||
|Não. dos fluxos de trabalho do Oozie|100||
|**Tópico**: **dimensionamento**|||
|Volume de dados, incluindo a replicação|100 TB||
|Volume de ingestão de diário|50 GB||
|Taxa de crescimento de dados|10% por ano||
|Taxa de crescimento de nós de cluster|5% por ano
|**Tópico**: **utilização do Cluster**|||
|Média de CPU de % utilizada|60%||
|Média % de memória utilizada|75%||
|Espaço em disco utilizado|75%||
|Média de rede de % utilizada|25%
|**Tópico**: **equipe**|||
|Não. de administradores|2||
|Não. de desenvolvedores|10||
|Não. dos utilizadores finais|100||
|Competências|Hadoop, Spark||
|Não. de recursos disponíveis para iniciativas de migração|2||
|**Tópico**: **limitações**|||
|Limitações atuais|Latência é elevada||
|Desafios atuais|Problema de simultaneidade||

### <a name="azure-requirements-questionnaire"></a>Questionário de requisitos do Azure

|**Tópico**: **infraestrutura** |||
|---|---|---|
|**Pergunta**|**Exemplo**|**Resposta**|
| Região preferencial|E.U.A Leste||
|VNet preferido?|Sim||
|HA / DR necessária?|Sim||
|Integração com outros serviços em nuvem?|ADF, cosmos DB||
|**Tópico**: **movimento de dados**  |||
|Preferência de carregamento inicial|DistCp, dados de caixa, ADF, WANDisco||
|Diferenças de transferência de dados|DistCp, o AzCopy||
|Transferência de dados incrementais em curso|DistCp, Sqoop||
|**Tópico**: **monitorização e alertas** |||
|Utilizar a monitorização de terceiros de monitorização do Azure & alertas integrar o Vs|Utilizar o Azure, monitorização e alertas||
|**Tópico**: **preferências de segurança** |||
|Pipeline de dados particulares e protegidos?|Sim||
|Cluster de associado de domínio (ESPP)?|     Sim||
|No local sincronização do AD para a Cloud?|     Sim||
|Não. de utilizadores do AD para sincronizar?|          100||
|OK para sincronizar as palavras-passe para a cloud?|    Sim||
|Apenas utilizadores de cloud?|                 Sim||
|MFA necessário?|                       Não|| 
|Requisitos de autorização de dados?|  Sim||
|Controlo de acesso baseado em funções?|        Sim||
|Auditoria necessário?|                  Sim||
|Encriptação de dados Inativos?|          Sim||
|Encriptação de dados em trânsito?|       Sim||
|**Tópico**: **preferências de Rearquitetura** |||
|Tipos de clusters específicos do cluster único vs|Tipos de clusters específicos||
|Armazenamento remoto do Vs de armazenamento colocalizados?|Armazenamento remoto||
|Tamanho de cluster mais pequeno que dados é armazenado remotamente?|Tamanho de cluster mais pequeno||
|Utilizar vários clusters menores em vez de um único cluster grande?|Utilizar vários clusters menores||
|Utilizar um metastore remoto?|Sim||
|Partilhar metastores entre clusters diferentes?|Sim||
|Eles possuem de desconstruir cargas de trabalho?|Substitua as tarefas do Hive com tarefas do Spark||
|Utilizar o ADF para orquestração de dados?|Não||
|HDI vs HDP no IaaS?|HDI||

## <a name="next-steps"></a>Passos Seguintes

- Leia mais sobre [4.0 do HDInsight](https://docs.microsoft.com/azure/hdinsight/hadoop/apache-hadoop-introduction)