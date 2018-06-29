---
title: Copiar dados para o Azure Data Lake armazenamento Gen2 Preview utilizando Distcp | Microsoft Docs
description: Utilizar o Distcp ferramenta para copiar dados de e para pré-visualização do Data Lake armazenamento Gen2
services: storage
documentationcenter: ''
author: seguler
manager: jahogg
editor: seguler
ms.component: data-lake-storage-gen2
ms.service: storage
ms.devlang: na
ms.topic: how-to
ms.date: 06/27/2018
ms.author: seguler
ms.openlocfilehash: 2a958ceb0b3a1db9d06d045a8161fa6cd3ef5aba
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059931"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-storage-gen2-preview"></a>Utilizar o Distcp para copiar dados entre o Blobs Storage do Azure e pré-visualização do Data Lake armazenamento Gen2

Se tiver um cluster do HDInsight com acesso à pré-visualização do Azure Data Lake armazenamento Gen2, pode utilizar as ferramentas do ecossistema de Hadoop, como o Distcp para copiar dados **de e para** um armazenamento de cluster do HDInsight (WASB) para uma Data Lake armazenamento Gen2 com capacidade conta. Este artigo fornece instruções sobre como utilizam a ferramenta de Distcp.

## <a name="prerequisites"></a>Pré-requisitos

* **Uma subscrição do Azure**. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Uma conta de armazenamento do Azure com a funcionalidade de armazenamento do Azure Data Lake (pré-visualização) ativada**. Para obter instruções sobre como criar um, consulte [TODO](quickstart-create-account.md)
* **Cluster do HDInsight Azure** com acesso a uma conta de armazenamento do Data Lake. Consulte [clusters de utilização do Azure Data Lake armazenamento Gen2 com o Azure HDInsight](use-hdi-cluster.md). Certifique-se de que ativar o ambiente de trabalho remoto para o cluster.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>Utilizar o Distcp de um cluster do HDInsight com Linux

Um cluster do HDInsight é fornecido com o utilitário Distcp, que pode ser utilizado para copiar dados de diferentes origens para um cluster do HDInsight. Se tiver configurado o cluster do HDInsight para utilizar o Blob Storage do Azure e o armazenamento do Azure Data Lake em conjunto, o utilitário de Distcp pode ser utilizado fora-de-a-box para copiar dados entre bem. Nesta secção, vamos ver como utilizar o utilitário de Distcp.

1. Do ambiente de trabalho, utilize o SSH para ligar ao cluster. Consulte [ligar a um cluster do HDInsight baseado em Linux](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). Execute os comandos a partir da linha de SSH.

2. Verifique se pode aceder a Blobs de armazenamento do Azure (WASB). Execute o seguinte comando:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    A saída deve fornecer uma lista de conteúdo no storage blob.

3. Da mesma forma, verifique se pode aceder à conta de armazenamento do Data Lake do cluster. Execute o seguinte comando:

        hdfs dfs -ls abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/

    A saída deve fornecer uma lista de ficheiros/pastas na conta de armazenamento do Data Lake.

4. Utilize o Distcp para copiar dados de WASB para uma conta de armazenamento do Data Lake.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder

    O comando copia os conteúdos do **/exemplo dados/gutenberg/** pasta no armazenamento de BLOBs para **/myfolder** na conta de armazenamento do Data Lake.

5. Da mesma forma, utilize o Distcp para copiar dados da conta de armazenamento do Data Lake ao Blob Storage (WASB).

        hadoop distcp abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    O comando copia os conteúdos do **/myfolder** na conta do Data Lake Store para **/exemplodados/gutenberg/** pasta WASB.

## <a name="performance-considerations-while-using-distcp"></a>Considerações sobre o desempenho ao utilizar o DistCp

Uma vez granularidade mais baixa do DistCp é um ficheiro único, definir o número máximo de cópias em simultâneo é o parâmetro mais importante para otimizá-lo contra o armazenamento do Data Lake. Número de cópias em simultâneo é controlado ao definir o número de mappers (Estou ') parâmetro na linha de comandos. Este parâmetro especifica o número máximo de mappers que são utilizados para copiar dados. Valor predefinido é 20.

**Exemplo**

    hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg abfs://<filesystem_name>@<storage_account_name>.dfs.core.windows.net/myfolder -m 100

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Como posso determinar o número de mappers utilizar?

Eis algumas orientações que poderá utilizar.

* **Passo 1: Determinar o total de memória YARN** -o primeiro passo consiste em determinar a memória YARN disponível para o cluster em que executou a tarefa de DistCp. Esta informação está disponível no portal do Ambari associado com o cluster. Navegue para YARN e ver o separador de cálculo de configurações para ver a memória YARN. Para obter a memória total do YARN, multiplique a YARN de memória por nó com o número de nós que tem no seu cluster.

* **Passo 2: Calcular o número de mappers** -o valor de **m** é igual ao quociente de memória total do YARN dividido pelo tamanho do contentor YARN. As informações de tamanho do contentor YARN estão disponíveis no portal do Ambari. Navegue para YARN e ver o separador de folhas. É apresentado o tamanho de contentor do YARN nesta janela. A equação de chegam o número de mappers (**m**) é

        m = (number of nodes * YARN memory for each node) / YARN container size

**Exemplo**

Vamos assumir que tem um 4 D14v2s nós no cluster e está a tentar a transferência de 10 TB de dados de 10 pastas diferentes. Cada uma das pastas contém variando quantidades de dados e os tamanhos de ficheiro dentro de cada pasta são diferentes.

* Total de memória YARN - do Ambari portal a determinar se a memória YARN é 96 GB para um nó de D14. Por isso, é o total de memória YARN para o cluster de quatro nós: 

        YARN memory = 4 * 96GB = 384GB

* Número de mappers - portal do Ambari a determinar se o tamanho de contentor do YARN é 3072 para um nó de cluster D14. Por isso, é o número de mappers:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Se outras aplicações estão a utilizar memória, em seguida, pode optar por utilizar apenas uma parte da memória do seu cluster YARN para DistCp.

### <a name="copying-large-datasets"></a>Copiar grandes conjuntos de dados

Quando o tamanho do conjunto de dados sejam movidos é grande (por exemplo, > 1 TB) ou se tiver muitos pastas diferentes, deve considerar a utilização de várias tarefas de DistCp. É provável que não existem ganhos de desempenho, mas propaga terminar as tarefas de modo a que o se qualquer tarefa falhar, apenas terá de reiniciar essa tarefa específica em vez da tarefa de toda.

### <a name="limitations"></a>Limitações

* DistCp tenta criar mappers são semelhantes de tamanho para otimizar o desempenho. Aumento do número de mappers poderá não sempre aumentar o desempenho.

* DistCp está limitado a apenas um mapeador por ficheiro. Por conseguinte, não deve ter mais mappers superior ao número de ficheiros. Uma vez que o DistCp só pode atribuir um mapeador para um ficheiro, isto limita a quantidade de concorrência que pode ser utilizada para copiar ficheiros grandes.

* Se tiver um pequeno número de ficheiros grandes, deve dividi-los em segmentos de ficheiro de 256 MB para lhe dar mais simultaneidade potencial. 
