---
title: Instalação publicado aplicação - Datameer - Azure HDInsight | Microsoft Docs
description: Instalar e utilizar a aplicação de Hadoop do Datameer por terceiros.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 9eef1760b7cee3bbdf33122514669b38b0b4d9db
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31400848"
---
# <a name="install-published-application---cask-data-application-platform-cdap"></a>Instalar a aplicação publicada - Cask dados aplicação plataforma (CDAP)

Este artigo descreve como instalar e executar o [CDAP](http://cask.co/products/cdap/) publicado a aplicação de Hadoop no Azure HDInsight. Para obter uma descrição geral da plataforma de aplicação do HDInsight e uma lista de disponíveis independentes de Software (ISV) aplicações publicadas, consulte [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-cdap"></a>Sobre CDAP

Desenvolver aplicações do Hadoop pode ser um desafio.  Há um grande e crescente número de extensões de tecnologia de Hadoop, que pode demorar algum tempo para integrar. Fluxo de dados de monitorização e recolha de métricas podem exigir a criação de uma solução separada.

### <a name="how-does-cdap-help"></a>Como ajuda CDAP?

A plataforma de aplicação de dados de Cask (CDAP) é uma plataforma de integração de macrodados. CDAP permite-lhe concentrar-se na criação de aplicações, em vez de na infraestrutura subjacente.

CDAP utiliza conceitos de alto nível e abstrações são familiares para os programadores. Estes abstrações ocultar complexidades dos sistemas internos e encorajar reusability das soluções.

Uma extensão CDAP chamado [Cask Hydrator](http://cask.co/products/hydrator/) fornece uma interface de utilizador para desenvolver e gerir pipelines de dados. Um pipeline de dados é composto por várias * plug-ins que efetuem tarefas como a aquisição de dados, transformação, análise e pós-cópia de execução de operações.

Cada plug-in CDAP tem uma interface bem definida para que as diferentes tecnologias de avaliação é apenas um fim de substituir um plug-in com outra, sem ter de touch o resto da aplicação.

CDAP *pipelines* fornecer um fluxo pictorial alto nível dos dados na sua aplicação. Esta visualização apresenta o fluxo de ponto-a-ponto dos dados da ingestão, através de transformações de dados e análises e, finalmente, para um dados externos armazenar.

O seguinte exemplo de um pipeline de dados ingere dados twitter em tempo real, em seguida, filtra algumas tweets com base em critérios predefinidos. O pipeline de dados transformações de dados não processados tweet e armazenam projetos que num formato mais legível, em seguida, agrupa os tweets, de acordo com um conjunto de valores e escreve os resultados num HBase.

![Pipeline CDAP](./media/hdinsight-apps-install-cask/pipeline.png)

Este pipeline ponto-a-ponto é compilada com a **Cask Hydrator IU**, utilizando a funcionalidade de interface e arrastar e largar de plug-in para ligações de formulário entre cada fase. Pode isolar e modificar a funcionalidade de cada plug-in de forma independente. Utilizar CDAP, pipelines semelhantes podem ser criadas e validadas nas horas. No mundo de Hadoop típico, a construir essas soluções poderá demorar vários dias.

CDAP também fornece uma extensão chamada [Cask controlador](http://cask.co/products/tracker/) para visualmente os dados de rastreio que passa através da aplicação. Adiciona o controlador de cask *governação de dados* para o sistema para que os recursos de dados formally são geridos em toda a aplicação. Pode controlar linhagem de cada ponto de dados, recolher a métrica relevante e o registo de dados em todo o processo de auditoria.

Eis uma ilustração como dados é que fluem no pipeline acima:

![Controlador CDAP](./media/hdinsight-apps-install-cask/tracker.png)

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação no novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Camada do cluster: Standard
* Tipo de cluster: HBase
* Versão do cluster: 3.4, 3.5

## <a name="install-the-cdap-published-application"></a>Instalar o CDAP publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações de ISV disponíveis, leia o artigo [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-cdap"></a>Iniciar CDAP

1. Após a instalação, inicie CDAP do seu cluster no portal do Azure acedendo ao **definições** painel, em seguida, selecionar **aplicações** sob o **geral** categoria. O painel aplicações instaladas apresenta uma lista de todas as aplicações instaladas.

    ![Aplicação CDAP instalado](./media/hdinsight-apps-install-cask/cdap-app.png)

2. Quando seleciona CDAP, verá uma ligação para a página web e ponto final de HTTP e também o caminho do ponto final SSH. Selecione a hiperligação de página Web.

3. Quando lhe for pedido, introduza as credenciais de administrador do cluster.

    ![Autenticação](./media/hdinsight-apps-install-cask/auth.png)

4. Depois de iniciarem sessão, verá a home page Cask CDAP GUI.

    ![Home page do cask GUI](./media/hdinsight-apps-install-cask/gui.png)

5. Para explorar a interface CDAP, clique o **Cask mercado** ligação menu na parte superior da página.

    ![Ligação de mercado cask](./media/hdinsight-apps-install-cask/cask-market.png)

6. Selecione **exemplo de registo de acesso** da lista.

    ![Exemplo de registo de acesso](./media/hdinsight-apps-install-cask/market-log-sample.png)

7. Clique em **carga** para confirmar.

    ![Clique em carga](./media/hdinsight-apps-install-cask/market-load.png)

8. É apresentada uma vista dos dados de exemplo incluído. Selecione **seguinte**.

    ![Exemplo de registo de acesso - ver dados](./media/hdinsight-apps-install-cask/market-view-data.png)

9. Selecione **fluxo** como o tipo de destino, introduza um nome de destino, em seguida, selecione **concluir**.

    ![Exemplo de registo de acesso - destino selecione](./media/hdinsight-apps-install-cask/market-destination.png)

10. Depois do datapack foi carregada com êxito, selecione **ver detalhes de fluxo**.

    ![Datapack carregado com êxito](./media/hdinsight-apps-install-cask/market-view-details.png)

11. Para ativar os metadados para o espaço de nomes, selecione **ativar** no separador de utilização na página de detalhes de registo de acesso.

    ![Exemplo de registo de acesso - carregado - ativar metadados de](./media/hdinsight-apps-install-cask/log-loaded.png)

12. Depois de metadados estiver ativado, verá um gráfico que apresenta informações de mensagens de auditoria.

    ![Exemplo de registo de acesso - metadados ativada](./media/hdinsight-apps-install-cask/log-metadata.png)

13. Para explorar os dados de registo, selecione o **explorar** ícone na parte superior da página.

    ![Aceder ao registo de exemplo - explorar](./media/hdinsight-apps-install-cask/log-explore.png)

14. Pode ver uma consulta SQL de exemplo. Pode modificar como pretendido, em seguida, selecione **executar**.

    ![Exemplo de registo de acesso - explorar o conjunto de dados com uma consulta](./media/hdinsight-apps-install-cask/log-query.png)

15. Depois de concluída a consulta, selecione o **vista** ícone sob a coluna de ações.

    ![Exemplo de registo de acesso - consulta de vista concluída](./media/hdinsight-apps-install-cask/log-query-view.png)

16. Pode ver os resultados da consulta.

    ![Exemplo de registo de acesso - resultados da consulta](./media/hdinsight-apps-install-cask/log-query-results.png)

## <a name="next-steps"></a>Passos Seguintes

* [Documentação de cask](http://cask.co/resources/documentation/).
* [Instalar aplicações HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada no HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux através da ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilize nós de limite vazio no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder a clusters do HDInsight e para testar e alojamento de aplicações do HDInsight.
