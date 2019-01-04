---
title: Utilizar o Hive com o Apache Hadoop para análise de registos de Web site - Azure HDInsight
description: Saiba como utilizar o Apache Hive com HDInsight para analisar registos de Web sites. Irá utilizar um ficheiro de registo como entrada para uma tabela do HDInsight e utilizar o HiveQL para consultar os dados.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/17/2016
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 30bfaad8fcc1a837a37689280149a6dbe20b7c1d
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628156"
---
# <a name="use-apache-hive-with-windows-based-hdinsight-to-analyze-logs-from-websites"></a>Utilizar o Apache Hive com o HDInsight baseado em Windows para analisar os registos de Web sites
Saiba como utilizar o HiveQL com o HDInsight para analisar os registos a partir de um site. Análise de registos de Web site pode ser utilizado para segmentar o público-alvo com base em atividades semelhantes, categorizar os visitantes do site por dados demográficos e para obter o conteúdo vista, o Web sites, eles vêm e assim por diante.

> [!IMPORTANT]  
> Os passos neste documento funcionam apenas com clusters do HDInsight baseado em Windows. HDInsight só está disponível no Windows para versões mais baixos do que o HDInsight 3.4. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).

Neste exemplo, utilize um cluster do HDInsight para analisar ficheiros de registo do Web site para obter informações sobre a frequência de visitas ao Web site de Web sites externos num dia. Também gerar um resumo dos erros de Web site que os utilizadores experienciam. Saiba como:

* Ligar a um armazenamento de Blobs do Azure, que contém ficheiros de registo do Web site.
* Crie tabelas do HIVE para consultar esses registos.
* Crie consultas do HIVE para analisar os dados.
* Utilizar o Microsoft Excel para ligar ao HDInsight (com conectividade de banco de dados abertos (ODBC) para recuperar os dados analisados.

![HDI. Samples.Website.Log.Analysis](./media/apache-hive-analyze-website-log/hdinsight-weblogs-sample.png)

## <a name="prerequisites"></a>Pré-requisitos
* Tem de ter aprovisionado um cluster do Apache Hadoop no HDInsight do Azure. Para obter instruções, consulte [Provision HDInsight Clusters](../hdinsight-hadoop-provision-linux-clusters.md).
* Tem de ter o Microsoft Excel 2013 ou o Excel 2010 instalado.
* Tem de ter [Microsoft Hive ODBC Driver](https://www.microsoft.com/download/details.aspx?id=40886) para importar dados de Hive para o Excel.

## <a name="to-run-the-sample"></a>Para executar o exemplo
1. Partir do [portal do Azure](https://portal.azure.com/), partir do Startboard (se o tiver afixado o cluster existe), clique no mosaico de cluster no qual pretende executar o exemplo.
2. Do painel do cluster, em **ligações rápidas**, clique em **Dashboard do Cluster**e, em seguida, a partir do **Dashboard do Cluster** painel, clique em **Cluster do HDInsight Dashboard**. Em alternativa, pode abrir o dashboard diretamente utilizando a seguinte URL:

         https://<clustername>.azurehdinsight.net

    Quando lhe for pedido, autentique com o nome de utilizador de administrador e a palavra-passe que utilizou durante o aprovisionamento do cluster.
3. Da página da web que se abre, clique nas **Galeria de introdução de introdução** separador e, no **soluções com dados de exemplo** categoria, clique no **Website Log Analytics** exemplo.
4. Siga as instruções fornecidas na página da web para concluir o exemplo.

## <a name="next-steps"></a>Passos Seguintes
Experimente o exemplo a seguir: [Análise de dados do sensor utilizando o Hive com HDInsight](apache-hive-analyze-sensor-data.md).

[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md
