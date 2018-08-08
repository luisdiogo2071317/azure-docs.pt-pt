---
title: Analisar dados do sensor utilizando o Hive e do Hadoop - Azure HDInsight
description: Saiba como analisar dados de sensores, utilizando a consola de consulta do Hive com HDInsight (Hadoop), em seguida, visualize os dados no Microsoft Excel com PowerView.
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.topic: conceptual
ms.date: 04/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 412942aa41e7884c6315d921b0b272b033386d17
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590250"
---
# <a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Analisar dados de sensores utilizando a consola de consulta do Hive do Hadoop no HDInsight

Saiba como analisar dados de sensores, utilizando a consola de consulta do Hive com HDInsight (Hadoop), em seguida, visualize os dados no Microsoft Excel com o Power View.

> [!IMPORTANT]
> Os passos neste documento funcionam apenas com clusters do HDInsight baseado em Windows. HDInsight só está disponível no Windows para versões mais baixos do que o HDInsight 3.4. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).


Neste exemplo, utilizar o Hive para processar dados históricos e identificar problemas com os sistemas de aquecimento... e ar condicionado. Especificamente, identificar os sistemas não são capazes de forma fiável manter uma determinada temperatura, efetuando as seguintes tarefas:

* Crie tabelas do HIVE para consultar dados armazenados em ficheiros de valores separados por vírgulas (CSV).
* Crie consultas do HIVE para analisar os dados.
* Para obter os dados analisados, utilize o Microsoft Excel para ligar ao HDInsight.
* Para visualizar os dados, utilize o Power View.

![Um diagrama da arquitetura da solução](./media/apache-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a>Pré-requisitos

* Um cluster do HDInsight (Hadoop): veja [criar clusters Hadoop no HDInsight](../hdinsight-hadoop-provision-linux-clusters.md) para obter informações sobre como criar um cluster.
* O Microsoft Excel 2013

  > [!NOTE]
  > Microsoft Excel é utilizado para visualização de dados com [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).

* [Controlador Microsoft Hive ODBC](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="to-run-the-sample"></a>Para executar o exemplo

1. A partir de seu navegador da web, navegue para o seguinte URL: 

         https://<clustername>.azurehdinsight.net

    Substitua `<clustername>` pelo nome do seu cluster do HDInsight.

    Quando lhe for pedido, autentique com o nome de utilizador de administrador e a palavra-passe que utilizou durante o aprovisionamento este cluster.

2. Da página da web que se abre, clique nas **Galeria de introdução de introdução** separador e, no **soluções com dados de exemplo** categoria, clique no **análise de dados de Sensor** exemplo.

    ![Obter imagem da Galeria de introdução](./media/apache-hive-analyze-sensor-data/getting-started-gallery.png)

3. Siga as instruções fornecidas na página da web para concluir o exemplo.
