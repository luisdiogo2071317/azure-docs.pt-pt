---
title: Implementar e gerir topologias do Apache Storm no HDInsight do Azure
description: Saiba como implementar, monitorizar e gerir topologias do Apache Storm com o Dashboard do Storm no HDInsight baseado em Windows. Utilize as ferramentas Hadoop do Visual Studio.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 03/01/2017
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 322c7164c0ecda550bf1bfe6a55075759bf95735
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630521"
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a>Implementar e gerir topologias do Apache Storm no HDInsight baseado em Windows

O [Apache Storm](https://storm.apache.org/) Dashboard permite-lhe implementar facilmente e execução topologias do Apache Storm para o HDInsight cluster, utilizando o seu navegador da web. Também pode utilizar o dashboard para monitorizar e gerir topologias em execução. Se utilizar o Visual Studio, as ferramentas do HDInsight para Visual Studio fornecem recursos semelhantes no Visual Studio.

O Dashboard do Storm e as funcionalidades de Storm nas ferramentas do HDInsight baseiam-se na API de REST de Storm, o que pode ser utilizado para criar o seu próprio de monitorização e soluções de gestão.

> [!IMPORTANT]  
> Os passos neste documento exigem um Storm no cluster do HDInsight que utiliza o Windows como o sistema operativo. O Linux é o único sistema operativo utilizado na versão 3.4 ou superior do HDInsight. Para obter mais informações, veja [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight no Windows).
>
> Para obter informações sobre como implementar e gerir topologias do Storm num cluster do HDInsight que utilizam o Linux, consulte [implementar e gerir topologias do Apache Storm no HDInsight baseado em Linux](apache-storm-deploy-monitor-topology-linux.md)

## <a name="prerequisites"></a>Pré-requisitos

* **O Apache Storm no HDInsight** -veja [introdução ao Apache Storm no HDInsight](apache-storm-tutorial-get-started-linux.md) para obter os passos sobre como criar um cluster.

* Para o **Dashboard do Storm**: Um browser moderno que suporte HTML5.

* Para **Visual Studio** -Azure SDK 2.5.1 ou mais recente e as ferramentas do HDInsight para Visual Studio. Ver [começar a utilizar as ferramentas do HDInsight para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md) para instalar e configurar as ferramentas do HDInsight para Visual Studio.

    Uma das seguintes versões do Visual Studio:

  * O Visual Studio 2012 com atualização 4

  * Visual Studio 2013 com a atualização 4 ou Visual Studio 2013 Community

  * Visual Studio 2015 (qualquer edição)

  * Visual Studio 2017 (qualquer edição)

## <a name="storm-dashboard"></a>Dashboard do Storm

O Dashboard do Storm é uma página da web disponíveis no seu cluster do Storm. O URL é **https://&lt;clustername >.azurehdinsight.net/**, em que **clustername** é o nome do seu cluster Storm no HDInsight.

Na parte superior do Dashboard do Storm, selecione **submeter topologia**. Siga as instruções na página para executar uma topologia de exemplo ou para carregar e executar uma topologia que criou.

![a página de topologia de envio][storm-dashboard-submit]

### <a name="storm-ui"></a>IU do Storm

A partir do Dashboard do Storm, selecione o **IU do Storm** ligação. Esta ação apresenta informações sobre o cluster, além de qualquer topologias em execução.

![a IU do storm][storm-dashboard-ui]

> [!NOTE]  
> Com algumas versões do Internet Explorer, pode descobrir que a IU do Storm não atualiza após o primeiro visitaram-lo. Por exemplo, ele pode não mostrar as topologias de novo submetidas, ou mostrar uma topologia de como o Active Directory quando desativado anteriormente. A Microsoft está ciente deste problema e está trabalhando numa solução.

#### <a name="main-page"></a>Página principal

A página principal da IU do Storm fornece as seguintes informações:

* **Resumo do cluster**: Informações básicas sobre o cluster do Storm.

* **Resumida da topologia**: Uma lista de execução de topologias. Use os links nesta secção para ver mais informações sobre topologias específicas.

* **Resumo do supervisor**: Informações sobre o supervisor de Storm.

* **Configuração de nimbus**: Configuração de nimbus para o cluster.

#### <a name="topology-summary"></a>Resumida da topologia

Selecionar uma ligação a partir da **resumo da topologia** secção apresenta as seguintes informações sobre a topologia:

* **Resumida da topologia**: Informações básicas sobre a topologia.

* **Ações de topologia**: Ações de gestão que pode efetuar para a topologia.

  * **Ativar**: Retoma o processamento de uma topologia desativada.

  * **Desativar**: Coloca em pausa uma topologia em execução.

  * **Reequilibrar**: Ajusta o paralelismo da topologia. Deve rebalancear as topologias em execução depois de ter alterado o número de nós no cluster. Isso permite que a topologia ajuste o paralelismo para compensar o número de maior ou menor de nós do cluster.

      Para obter mais informações, consulte [compreender o paralelismo de uma topologia do Apache Storm](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

  * **Eliminar**: Termina uma topologia do Storm após o tempo limite especificado.

* **Estatísticas de topologia**: Estatísticas sobre a topologia. Utilize as ligações na **janela** coluna para definir o período de tempo para as entradas restantes na página.

* **Spouts**: Os spouts utilizados pela topologia. Use os links nesta secção para ver mais informações sobre spouts específicos.

* **Bolts**: Os bolts utilizados pela topologia. Use os links nesta secção para ver mais informações sobre bolts específicos.

* **Configuração da topologia**: A configuração da topologia selecionada.

#### <a name="spout-and-bolt-summary"></a>Spout e o resumo de Bolt

Selecionar um spout do **Spouts** ou **Bolts** secções apresenta as seguintes informações sobre o item selecionado:

* **Resumo dos componentes**: Informações básicas sobre o spout ou bolt.

* **Estatísticas de spout/Bolt**: Estatísticas sobre o spout ou bolt. Utilize as ligações na **janela** coluna para definir o período de tempo para as entradas restantes na página.

* **Estatísticas de entrada** (apenas bolt): Informações sobre os fluxos de entrada consumidos pelo bolt.

* **Estatísticas de saída**: Informações sobre os fluxos emitidos por esta spout ou bolt.

* **Executores**: Informações sobre as instâncias do spout ou bolt. Selecione o **porta** produzidos de entrada para um executor específico ver um log de informações de diagnóstico para esta instância.

* **Erros**: Qualquer informação de erro para este spout ou bolt.

## <a name="hdinsight-tools-for-visual-studio"></a>Ferramentas do HDInsight para o Visual Studio

O [ferramentas do HDInsight](https://azure.microsoft.com/resources/videos/hdinsight-tools-for-visual-studio/) pode ser utilizado para enviar C# ou topologias híbridas ao seu cluster do Storm. Os passos seguintes utilizam um aplicativo de exemplo. Para obter informações sobre como criar seu próprio topologias usando as ferramentas do HDInsight, consulte [desenvolver topologias c# com as ferramentas do HDInsight para Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md).

Utilize os seguintes passos para implementar um exemplo para Storm no cluster do HDInsight, em seguida, ver e gerir a topologia.

1. Se ainda não tiver instalado a versão mais recente das ferramentas do HDInsight para Visual Studio, consulte [começar a utilizar as ferramentas do HDInsight para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).

2. Abra o Visual Studio, selecione **arquivo** > **New** > **projeto**.

3. Na **novo projeto** caixa de diálogo caixa, expanda **instalado** > **modelos**e, em seguida, selecione **HDInsight**. Na lista de modelos, selecione **exemplo de Storm**. Na parte inferior da caixa de diálogo, escreva um nome para a aplicação.

    ![image](./media/apache-storm-deploy-monitor-topology/sample.png)

4. Na **Explorador de soluções**, clique com o botão direito no projeto e selecione **submeter para Storm no HDInsight**.

   > [!NOTE]  
   > Se lhe for pedido, introduza as credenciais de início de sessão para a sua subscrição do Azure. Se tiver mais de uma subscrição, inicie sessão no que contém o seu cluster Storm no HDInsight.

5. Selecione o Storm no cluster do HDInsight a partir da **Cluster do Storm** na lista pendente e, em seguida, selecione **submeter**. Pode monitorizar se a submissão for bem sucedida, utilizando o **saída** janela.

6. Quando a topologia foi submetida com êxito, o **topologias do Storm** para o cluster deve aparecer. Selecione a topologia da lista para ver informações sobre a topologia em execução.

    ![monitor do Visual studio](./media/apache-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]  
   > Também pode ver **topologias do Storm** partir **Explorador de servidores** ao expandir **Azure** > **HDInsight**e, em seguida, clicar com o botão direito um Storm num cluster do HDInsight e selecionando **Zobrazit Topologie Stormu**.

    Selecione a forma de spouts ou bolts para ver informações sobre estes componentes. É aberta uma janela nova para cada item selecionado.

   > [!NOTE]  
   > O nome da topologia é o nome da classe da topologia (neste caso, `HelloWord`,) com um carimbo anexado.

7. Do **resumo da topologia** visualizar, selecione **Kill** para parar a topologia.

   > [!NOTE]  
   > Topologias do Storm continuam em execução até que estão paradas ou o cluster é eliminado.


## <a name="rest-api"></a>API REST

A IU do Storm baseia-se com base na API do REST, para que possa executar semelhantes de gestão e monitorização funcionalidade com a API de REST. Pode utilizar a API REST para criar ferramentas personalizadas para gerir e monitorizar topologias do Storm.

Para obter mais informações, consulte [API de REST de interface do Usuário do Apache Storm](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md). As seguintes informações são específicas para utilizar a API de REST com o Apache Storm no HDInsight.

### <a name="base-uri"></a>URI de base

É o URI de base para a API de REST em clusters do HDInsight **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, em que **clustername** é o nome do Storm no HDInsight cluster.

### <a name="authentication"></a>Autenticação

Tem de utilizar pedidos para a API REST **autenticação básica**, por isso, utilize o nome de administrador de cluster do HDInsight e a palavra-passe.

> [!NOTE]  
> Uma vez que a autenticação básica é enviada com o texto não criptografado, deve **sempre** utilizar HTTPS para proteger as comunicações com o cluster.

### <a name="return-values"></a>Valores de retorno

Informações que são devolvidas a partir da API de REST só podem ser utilizáveis de dentro do cluster ou máquinas virtuais na mesma rede Virtual do Azure que o cluster. Por exemplo, o nome domínio completamente qualificado (FQDN) para devolvidos [Apache ZooKeeper](https://zookeeper.apache.org/) servidores não são estar acessível a partir da Internet.

## <a name="next-steps"></a>Próximos Passos

Agora que aprendeu como implementar e monitorizar topologias através do Dashboard do Storm, saiba como:

* [Desenvolver topologias c# com as ferramentas do HDInsight para Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md)

* [Desenvolver topologias baseadas em Java utilizando o Apache Maven](apache-storm-develop-java-topology.md)

Para obter uma lista das topologias de exemplo mais, consulte [topologias de exemplo para Apache Storm no HDInsight](apache-storm-example-topology.md).

[hdinsight-dashboard]: ./media/apache-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/apache-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/apache-storm-deploy-monitor-topology/storm-ui-summary.png
