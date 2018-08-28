---
title: Instalar a aplicação publicada - StreamSets Data Collector - Azure HDInsight
description: Instalar e utilizar a aplicação do Hadoop de terceiros StreamSets Data Collector.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 72ace99a8124b0a288e8facf630e947151169d0b
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43106887"
---
# <a name="install-published-application---streamsets-data-collector"></a>Instalar a aplicação publicada - StreamSets Data Collector

Este artigo descreve como instalar e executar o [StreamSets Data Collector para HDInsight](https://streamsets.com/) publicado a aplicação do Hadoop no HDInsight do Azure. Para uma visão geral da plataforma de aplicações do HDInsight e uma lista de disponíveis independentes fabricante de Software (ISV) a aplicações publicadas, consulte [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-streamsets-data-collector"></a>Sobre o Recoletor de dados de StreamSets

O StreamSets Data Collector é implementado sobre a uma aplicação do Azure HDInsight. O StreamSets Data Collector fornece um ambiente completo de desenvolvimento integrado (IDE) que permite conceber, testar, implementar e gerir qualquer para qualquer pipelines de ingestão. Esses pipelines podem integrar dados de fluxo e lote e incluem uma variedade de transformações no fluxo, tudo sem ter de escrever código personalizado.

O StreamSets Data Collector permite-lhe fluxos de dados de compilação com numerosos componentes de macrodados, tais como o HDFS, Kafka, Solr, Hive, HBASE e Kudu. Quando o StreamSets Data Collector estiver em execução num servidor de borda ou no seu cluster do Hadoop, obtenha monitorização para anomalias de dados e dados de operações de fluxo em tempo real. Esta monitorização inclui baseados no limiar de alerta, deteção de anomalias e remediação automática de registos de erro.

O StreamSets Data Collector foi concebido para isolar logicamente de cada fase no pipeline, pelo que pode satisfazer todas as novas exigências de negócio soltando em novos processadores e conectores sem codificação e com o período de indisponibilidade mínimo.

### <a name="streamsets-resource-links"></a>Ligações de recursos de StreamSets

* [Documentação](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html)
* [Blogue](https://streamsets.com/blog/)
* [Tutoriais](https://github.com/streamsets/tutorials)
* [Fórum de suporte do Programador](https://groups.google.com/a/streamsets.com/forum/#!forum/sdc-user)
* [Canal do Slack público StreamSets](https://streamsetters.slack.com/)
* [Código-fonte](https://github.com/streamsets)

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação num novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Escalões de cluster:{0}<br>tipos de cluster: Standard ou Premium
* Versões de cluster: 3.5 e superior

## <a name="install-the-streamsets-data-collector-published-application"></a>Instalar o Recoletor de dados de StreamSets publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações disponíveis do ISV, leia [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-streamsets-data-collector"></a>Iniciar o Recoletor de dados de StreamSets

1. Após a instalação, pode iniciar StreamSets do seu cluster no portal do Azure ao aceder a **definições** painel, em seguida, selecionar **aplicativos** sob o **geral** categoria. O painel aplicações instaladas apresenta uma lista de aplicações instaladas.

    ![Aplicação de StreamSets instalado](./media/hdinsight-apps-install-streamsets/streamsets.png)

2. Quando seleciona o StreamSets Data Collector, verá um link para a página da web e o caminho do ponto final SSH. Selecione a ligação de página Web.

3. Na caixa de diálogo de início de sessão, utilize as seguintes credenciais para iniciar sessão: `admin` e `admin`.

4. Na página de introdução, clique em **criar novo Pipeline**.

    ![Criar novo pipeline](./media/hdinsight-apps-install-streamsets/get-started.png)

5. Na janela novo Pipeline, introduza um nome para o pipeline ("Hello World"), opcionalmente, introduza uma descrição e selecione **guardar**.

6. É apresentada a consola de Recoletores de dados. O painel de propriedades mostra as propriedades do pipeline.
 
    ![Consola do Recoletor de dados](./media/hdinsight-apps-install-streamsets/pipeline-canvas.png)

7. Agora, está pronto para seguir a [StreamSets tutorial](https://streamsets.com/documentation/datacollector/latest/help/#Tutorial/Tutorial-title.html). O tutorial fornece instruções passo a passo para criar seu primeiro pipeline.

## <a name="next-steps"></a>Passos Seguintes

* [Documentação de Recoletores de dados de StreamSets](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html#concept_htw_ghg_jq).
* [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada para o HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux com ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilizar nós de extremidade vazios no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder aos clusters do HDInsight e de teste e alojamento de aplicações do HDInsight.
