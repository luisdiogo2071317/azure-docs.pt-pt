---
title: Migrar clusters do Apache Hadoop no local para o Azure HDInsight - práticas recomendadas de migração de dados
description: Conheça práticas recomendadas com uma migração de dados para migrar clusters do Hadoop no local para Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: hrasheed
ms.openlocfilehash: 95b753a3be824b5815a70fee84913f1c129f2605
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221947"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---data-migration-best-practices"></a>Migrar clusters do Apache Hadoop no local para o Azure HDInsight - práticas recomendadas de migração de dados

Este artigo oferece recomendações para a migração de dados para o Azure HDInsight. É parte de uma série que fornece as práticas recomendadas para ajudar a migrar sistemas de Apache Hadoop no local para Azure HDInsight.

## <a name="migrate-data-from-on-premises-to-azure"></a>Migrar dados no local para o Azure

Existem duas opções principais para migrar os dados no local para o ambiente do Azure:

1.  Transferir dados através de rede com TLS
    1.  Pela internet
    2.  ExpressRoute
2.  O envio de dados
    1.  Importar / exportar do serviço
        - SATA interno HDDs ou SSDs apenas
        - Encriptados em descanso (AES-128 / AES-256)
        - Tarefa de importação pode ter até 10 discos
        - Disponível em todas as regiões públicas & GA
    1.  Data Box
        - Até 80 TB de dados por caixa de dados
        - Encriptados em descanso (AES-256)
        - Utilize protocolos NAS e suporte a ferramentas de cópia de dados comuns
        - Hardware e resistente
        - Disponível nos E.U.A. apenas & pré-visualização pública

A tabela a seguir com duração de transferência de dados aproximada com base na largura de banda de rede e de volume de dados. Use uma caixa de dados se a migração de dados poderá demorar a demorar mais de três semanas.

|**Dados. de defeitos**|**Largura de banda de rede**|||
|---|---|---|---|
|| **45 Mbps (T3)**|**100 Mbps**|**1 Gbps**|**10 Gbps**
|1 TB|2 dias|1 dia| 2 horas|14 minutos|
|10 TB|22 dias|10 dias|1 dia|2 horas|
|35 TB|dias de 76|dias de 34|3 dias|8 horas|
|80 TB|dias 173|dias de 78|8 dias|19 horas|
|100 TB|dias 216|97 dias|10 dias|1 dia|
|200 TB|um ano|dias 194|dias de 19|2 dias|
|500 TB|3 anos|um ano|dias de 49|5 dias|
|1 PB|6 anos|3 anos|97 dias|10 dias|
|2 PB|há 12 anos|5 anos|dias 194|dias de 19|

Ferramentas nativas para o Azure, como DistCp, Azure Data Factory e AzureCp, podem ser usadas para transferir dados através da rede. A ferramenta de terceiros que wandisco também pode ser utilizado com o mesmo objetivo. Kafka Mirrormaker e Sqoop podem ser utilizado para transferência de dados em curso no local para sistemas de armazenamento do Azure.

## <a name="performance-considerations-when-using-apache-distcp"></a>Considerações de desempenho ao utilizar o Apache DistCp

DistCp é um projeto do Apache que utiliza uma tarefa de MapReduce mapa para transferir dados, manipular os erros e recuperar esses erros. Atribui uma lista de ficheiros de origem para cada tarefa de mapa. A tarefa de mapa, em seguida, copia todos os seus ficheiros atribuídos para o destino. Existem várias técnicas podem melhorar o desempenho de DistCp.

### <a name="increase-the-number-of-mappers"></a>Aumentar o número de Mapeadores

DistCp tenta criar tarefas de mapa, de modo a que cada um deles aproximadamente copia o mesmo número de bytes. Por predefinição, as tarefas de DistCp utilizam 20 mapeadores. Com mais Mapeadores para Distcp (com o estou ' parâmetro na linha de comando) aumenta o paralelismo durante o processo de transferência de dados e diminui o tamanho de transferência de dados. No entanto, existem dois aspetos a considerar ao aumento do número de Mapeadores:

1. Granularidade de mais baixa do DistCp é um único arquivo. Especificar um número de Mapeadores mais do que o número de ficheiros de origem não ajuda e irá perder os recursos de cluster disponível.
1. Considere a memória de Yarn disponível no cluster para determinar o número de Mapeadores. Cada tarefa de mapa é iniciada como um contêiner de Yarn. Partindo do princípio que não existem outras cargas de trabalho pesadas estão em execução no cluster, o número de Mapeadores pode ser determinado pela seguinte fórmula: m = (número de nós de trabalho \* memória YARN para cada nó de trabalho) / tamanho do contentor do YARN. No entanto, se outros aplicativos estiver usando a memória, em seguida, selecione utilizar apenas uma parte da memória YARN para as tarefas de DistCp.

### <a name="use-more-than-one-distcp-job"></a>Utilizar mais do que uma tarefa de DistCp

Quando o tamanho do conjunto de dados sejam movidos é maior do que 1 TB, utilize mais do que uma tarefa de DistCp. Usando mais do que um trabalho limita o impacto das falhas. Se qualquer tarefa falhar, apenas terá de reiniciar esse trabalho específico em vez de todas as tarefas.

### <a name="consider-splitting-files"></a>Considere a divisão de ficheiros

Se existir um pequeno número de ficheiros grandes, considere dividi-los em segmentos de ficheiro de 256 MB para obter mais simultaneidade potencial com mais Mapeadores.

### <a name="use-the-strategy-command-line-parameter"></a>Utilize o parâmetro de linha de comandos de "estratégia"

Considere a utilização de `strategy = dynamic` parâmetro na linha de comandos. O valor predefinido da `strategy` parâmetro é `uniform size`, caso em que cada mapa aproximadamente copia o mesmo número de bytes. Quando este parâmetro for alterado para `dynamic`, o ficheiro de listagem é dividido em vários "segmentos-files". O número de ficheiros de bloco é um múltiplo do número de mapas. Cada tarefa de mapa é atribuída um dos arquivos de segmentos. Depois de todos os caminhos num segmento são processados, o segmento atual é eliminado e é obtido um novo bloco. O processo continua até que não existem mais segmentos estão disponíveis. Essa abordagem "dynamic" permite que mais rápido do mapa-tarefas consumir mais caminhos que os mais lentos, assim, acelerar a tarefa de DistCp geral.

### <a name="increase-the-number-of-threads"></a>Aumentar o número de threads

Se aumentar o `-numListstatusThreads` parâmetro melhora o desempenho. Este parâmetro controla o número de threads a utilizar para a criação de listagem de ficheiros e 40 é o valor máximo.

### <a name="use-the-output-committer-algorithm"></a>Utilizar o algoritmo de committer de saída

Veja se passando o parâmetro `-Dmapreduce.fileoutputcommitter.algorithm.version=2` melhora o desempenho de DistCp. Algoritmo de committer esta saída tem otimizações em torno de escrever ficheiros de saída para o destino. O comando seguinte é um exemplo que mostra o uso de parâmetros diferentes:

```bash
hadoop distcp -Dmapreduce.fileoutputcommitter.algorithm.version=2 -numListstatusThreads 30 -m 100 -strategy dynamic hdfs://nn1:8020/foo/bar wasb://<container_name>@<storage_account_name>.blob.core.windows.net/foo/
```

## <a name="metadata-migration"></a>Migração de metadados

### <a name="hive"></a>Hive

O metastore hive pode ser migrado usando os scripts ou utilizando a replicação de DB.

#### <a name="hive-metastore-migration-using-scripts"></a>Migração de metastore Hive através de scripts

- Gere o DDLs do Hive a partir de metastore de Hive no local. Este passo pode ser feito usando um [script de bash de wrapper](https://github.com/hdinsight/hdinsight.github.io/blob/master/hive/hive-export-import-metastore.md)
- Editar a DDL gerada para substituir o url HDFS com urls do ADLS/WASB/ABFS
- Execute a DDL atualizada no metastore do HDI cluster
- Certifique-se de que a versão de metastore Hive é compatível entre no local e na cloud

#### <a name="hive-metastore-migration-using-db-replication"></a>Migração de metastore Hive utilizando a replicação de DB

- Configurar a replicação de base de dados entre locais metastore de Hive DB e HDI metastore DB
- Utilize "MetaTool do Hive" para substituir o url HDFS com urls do ADLS/WASB/ABFS, por exemplo:

```bash
./hive --service metatool -updateLocation hdfs://nn1:8020/ wasb://<container_name>@<storage_account_name>.blob.core.windows.net/
```

### <a name="ranger"></a>Ranger

- Exportar políticas do Ranger no local para arquivos xml
- Transformar no local com base no HDFS caminhos específicos para WASB/ADLS usando uma ferramenta como XSLT
- importar as políticas de sessão no Ranger em execução no HDI

## <a name="next-steps"></a>Passos Seguintes

Leia o artigo seguinte nesta série:

- [Práticas recomendadas de segurança e DevOps no local para a migração do Azure HDInsight Hadoop](apache-hadoop-on-premises-migration-best-practices-security-devops.md)