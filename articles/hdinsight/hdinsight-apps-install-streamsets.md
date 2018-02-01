---
title: "Instalação publicado aplicação - StreamSets de Recoletores de dados - Azure HDInsight | Microsoft Docs"
description: "Instalar e utilizar a aplicação de Hadoop do Recoletor de dados de StreamSets por terceiros."
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 90775452c58457ae8ecc73687a375606474158f5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="install-published-application---streamsets-data-collector"></a>Instalar a aplicação publicada - StreamSets de Recoletores de dados

Este artigo descreve como instalar e executar o [StreamSets de Recoletores de dados para o HDInsight](https://streamsets.com/) publicado a aplicação de Hadoop no Azure HDInsight. Para obter uma descrição geral da plataforma de aplicação do HDInsight e uma lista de disponíveis independentes de Software (ISV) aplicações publicadas, consulte [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-streamsets-data-collector"></a>Acerca de Recoletores de dados de StreamSets

O Recoletor de dados StreamSets implementa por cima de uma aplicação do Azure HDInsight. StreamSets de Recoletores de dados fornece um ambiente de desenvolvimento integrado completo (IDE) que permite-lhe estrutura, testa, implementa e gerir qualquer a qualquer ingestão de pipelines. Estes pipelines podem mesh dados de fluxo e batch e incluem uma variedade de transformações de na sequência, tudo sem ter de escrever código personalizado.

Recoletor de dados de StreamSets permite-lhe utilizar vários componentes de macrodados, tais como o HDFS, Kafka, Solr, Hive, HBASE e Kudu fluxos de dados de compilação. Assim que o Recoletor de dados de StreamSets está em execução num servidor edge, ou no seu cluster de Hadoop, obter monitorização para anomalias de dados e dados de operações de fluxo em tempo real. Esta monitorização inclui baseadas em limiares de alertas, deteção de anomalias e a remediação automática de registos de erro.

StreamSets de Recoletores de dados foi concebido para isolar logicamente cada fase num pipeline, pelo que pode satisfazer novos requisitos de negócio, arrastando no novo processadores e conectores sem codificação e com o período de indisponibilidade mínimo.

### <a name="streamsets-resource-links"></a>Ligações de recursos de StreamSets

* [Documentação](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html)
* [Blogue](https://streamsets.com/blog/)
* [Tutoriais](https://github.com/streamsets/tutorials)
* [Fórum de suporte para programadores](https://groups.google.com/a/streamsets.com/forum/#!forum/sdc-user)
* [Slack público StreamSets canal](https://streamsetters.slack.com/)
* [Código de origem](https://github.com/streamsets)

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação no novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Cluster tier(s): Standard ou Premium
* Versões de cluster: 3.5 e posterior

## <a name="install-the-streamsets-data-collector-published-application"></a>Instalar o Recoletor de dados StreamSets publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações de ISV disponíveis, leia o artigo [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-streamsets-data-collector"></a>Iniciar o Recoletor de dados de StreamSets

1. Após a instalação, pode iniciar StreamSets do seu cluster no portal do Azure acedendo ao **definições** painel, em seguida, selecionar **aplicações** sob o **geral** categoria. O painel aplicações instaladas apresenta uma lista de aplicações instaladas.

    ![Aplicação de StreamSets instalado](./media/hdinsight-apps-install-streamsets/streamsets.png)

2. Quando selecionar StreamSets de Recoletores de dados, é apresentada uma ligação para a página web e o caminho do ponto final SSH. Selecione a hiperligação de página Web.

3. Na caixa de diálogo de início de sessão, utilize as seguintes credenciais para iniciar sessão: `admin` e `admin`.

4. Na página de introdução, clique em **criar novo Pipeline**.

    ![Criar novo pipeline](./media/hdinsight-apps-install-streamsets/get-started.png)

5. Na janela do novo Pipeline, introduza um nome para o pipeline ("Olá mundo"), opcionalmente, introduza uma descrição e selecione **guardar**.

6. É apresentada a consola de Recoletores de dados. O painel de propriedades mostra propriedades de pipeline.
 
    ![Consola de Recoletor de dados](./media/hdinsight-apps-install-streamsets/pipeline-canvas.png)

7. Agora, está pronto a seguir a [StreamSets tutorial](https://streamsets.com/documentation/datacollector/latest/help/#Tutorial/Tutorial-title.html). O tutorial fornece instruções passo a passo para criar o seu primeiro pipeline.

## <a name="next-steps"></a>Passos Seguintes

* [Documentação de Recoletores de dados de StreamSets](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html#concept_htw_ghg_jq).
* [Instalar aplicações HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada no HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux através da ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilize nós de limite vazio no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder a clusters do HDInsight e para testar e alojamento de aplicações do HDInsight.
