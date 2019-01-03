---
title: Instalar a aplicação publicada - Datameer - Azure HDInsight
description: Instalar e utilizar a aplicação do Hadoop de terceiros Datameer.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 715e536d7356a4e37f512027a23236b1fd37cbac
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2018
ms.locfileid: "53651304"
---
# <a name="install-published-application---cask-data-application-platform-cdap"></a>Instalar a aplicação publicada - Cask Data Application Platform (CDAP)

Este artigo descreve como instalar e executar o [CDAP](https://cask.co/products/cdap/) publicado [Apache Hadoop](https://hadoop.apache.org/) aplicação no Azure HDInsight. Para uma visão geral da plataforma de aplicações do HDInsight e uma lista de disponíveis independentes fabricante de Software (ISV) a aplicações publicadas, consulte [instalar aplicações do Apache Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-cdap"></a>Sobre CDAP

Desenvolvimento de aplicativos no Hadoop pode ser um desafio.  Existe um número grande e crescente de extensões de tecnologia do Hadoop, o que pode demorar algum tempo a integrar. Fluxo de dados de monitorização e recolha de métricas podem exigir a criação de uma solução separada.

### <a name="how-does-cdap-help"></a>Como ajudar CDAP?

O Cask Data Application Platform (CDAP) é uma plataforma de integração para grandes volumes de dados. CDAP permite-lhe concentrar-se na criação de aplicativos, em vez de na infraestrutura subjacente.

CDAP utiliza conceitos de alto nível e abstrações que familiares aos desenvolvedores. Essas abstrações ocultar as complexidades de sistemas internos e incentive a reutilização de soluções.

Uma extensão CDAP chamado [Cask Hydrator](https://cask.co/products/hydrator/) fornece uma interface do usuário para desenvolver e gerir pipelines de dados. Um pipeline de dados é composto por várias * plug-ins que realizarem tarefas como operações pós-execução, análise, transformação e aquisição de dados.

Cada plug-in do CDAP tem uma interface bem definida para que avaliar tecnologias diferentes é apenas uma questão de substituir um plug-in por outro, sem precisar tocar o restante do aplicativo.

CDAP *pipelines* fornecer um fluxo de pictóricos de alto nível dos dados na sua aplicação. Esta visualização mostra o fluxo de ponto-a-ponto de dados de ingestão, por meio de transformações de dados e análises e finalmente num dados externos armazenar.

O exemplo seguinte de um pipeline de dados ingere dados do twitter em tempo real, em seguida, filtra alguns tweets com base nos critérios predefinidos. O pipeline de dados transforma dados não processados de tweet e armazenam os projetos que os dados num formato mais legível, em seguida, agrupa os tweets, de acordo com um conjunto de valores e grava os resultados num HBase.

![Pipeline CDAP](./media/hdinsight-apps-install-cask/pipeline.png)

Este pipeline ponto a ponto é criado com o **Cask Hydrator IU**, usando sua funcionalidade de interface e arrastar-e-soltar de plug-in para ligações de formulário entre cada fase. Pode isolar e modificar a funcionalidade de cada plug-in de forma independente. Utilizar CDAP, pipelines semelhante podem ser criados e validados em horas. No mundo típico do Hadoop, a construção de tais soluções pode demorar dias.

CDAP também fornece uma extensão chamada [Cask controlador](https://cask.co/products/tracker/) para visualmente os dados de rastreio à medida que fluem através da aplicação. Adiciona o cask Tracker *governação de dados* no sistema, de modo a que recursos de dados são geridos formalmente em todo o aplicativo. Pode controlar a linhagem de cada ponto de dados, coletar métricas relevantes e o registo de dados em todo o processo de auditoria.

Segue-se obter uma ilustração como dados estão a ser encaminhados no pipeline acima:

![Controlador CDAP](./media/hdinsight-apps-install-cask/tracker.png)

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação num novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Escalão do cluster: Standard
* Tipo de cluster: HBase
* Versão do cluster: 3.4, 3.5

## <a name="install-the-cdap-published-application"></a>Instalar o CDAP publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações disponíveis do ISV, leia [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-cdap"></a>Iniciar CDAP

1. Após a instalação, inicie CDAP do seu cluster no portal do Azure ao aceder a **definições** painel, em seguida, selecionar **aplicativos** sob o **geral** categoria. O painel aplicações instaladas apresenta uma lista de todas as aplicações instaladas.

    ![Aplicação CDAP instalado](./media/hdinsight-apps-install-cask/cdap-app.png)

2. Quando seleciona CDAP, verá um link para a página da web e ponto final de HTTP e também o caminho do ponto final SSH. Selecione a ligação de página Web.

3. Quando lhe for pedido, introduza as credenciais de administrador de cluster.

    ![Autenticação](./media/hdinsight-apps-install-cask/auth.png)

4. Depois de iniciar sessão, verá a home page do Cask CDAP GUI.

    ![Home page do cask GUI](./media/hdinsight-apps-install-cask/gui.png)

5. Para explorar a interface CDAP, clique nas **Cask mercado** link de menu na parte superior da página.

    ![Ligação de mercado cask](./media/hdinsight-apps-install-cask/cask-market.png)

6. Selecione **amostra de registo de acesso** da lista.

    ![Amostra de registo de acesso](./media/hdinsight-apps-install-cask/market-log-sample.png)

7. Clique em **carga** para confirmar.

    ![Clique em carregar](./media/hdinsight-apps-install-cask/market-load.png)

8. É apresentada uma vista dos dados de exemplo incluídos. Selecione **seguinte**.

    ![Amostra de registo de acesso - ver dados](./media/hdinsight-apps-install-cask/market-view-data.png)

9. Selecione **Stream** como o tipo de destino, introduza um nome de destino, em seguida, selecione **concluir**.

    ![Amostra de registo de acesso - destino selecione](./media/hdinsight-apps-install-cask/market-destination.png)

10. Após o datapack terem sido carregado com êxito, selecione **ver detalhes do Stream**.

    ![Datapack carregado com êxito](./media/hdinsight-apps-install-cask/market-view-details.png)

11. Para ativar os metadados para o espaço de nomes, selecione **ativar** no separador de utilização na página de detalhes do registo de acesso.

    ![Amostra de registo de acesso - carregada - ativar metadados](./media/hdinsight-apps-install-cask/log-loaded.png)

12. Depois de metadados está ativado, verá um gráfico, exibindo informações de mensagens de auditoria.

    ![Metadados de amostra de registo de acesso - ativada](./media/hdinsight-apps-install-cask/log-metadata.png)

13. Para explorar os dados de registo, selecione o **explorar** ícone na parte superior da página.

    ![Aceder ao registo de exemplo - explorar](./media/hdinsight-apps-install-cask/log-explore.png)

14. Veja uma consulta SQL de exemplo. Fique à vontade para modificá-lo como pretendido, em seguida, selecione **Execute**.

    ![Amostra de registo de acesso - explorar o conjunto de dados com uma consulta](./media/hdinsight-apps-install-cask/log-query.png)

15. Depois da consulta estiver concluída, selecione o **vista** ícone na coluna ações.

    ![Amostra de registo de acesso - consulta de vista concluída](./media/hdinsight-apps-install-cask/log-query-view.png)

16. Ver os resultados da consulta.

    ![Exemplo de registo de acesso – resultados da consulta](./media/hdinsight-apps-install-cask/log-query-results.png)

## <a name="next-steps"></a>Passos Seguintes

* [Documentação de cask](https://cask.co/resources/documentation/).
* [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada HDInsight.
* [Publicar aplicações HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar aplicações HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux com ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilizar nós de extremidade vazios no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder aos clusters do HDInsight e de teste e alojamento de aplicações do HDInsight.
