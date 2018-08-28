---
title: Instalar a aplicação publicada - Dataiku DDS - Azure HDInsight
description: Instalar e utilizar a aplicação do Hadoop de terceiros Dataiku DDS.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 64a6f393498ca90675712747afc8f9befc4b932f
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43105174"
---
# <a name="install-published-application---dataiku-dds"></a>Instalar a aplicação publicada - Dataiku DDS

Este artigo descreve como instalar e executar o [Dataiku DDS](https://www.dataiku.com/) publicado a aplicação do Hadoop no HDInsight do Azure. Para uma visão geral da plataforma de aplicações do HDInsight e uma lista de disponíveis independentes fabricante de Software (ISV) a aplicações publicadas, consulte [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-dataiku-dss"></a>Sobre o Dataiku DSS

A Dataiku [Studio de ciência de dados (DSS)](https://www.dataiku.com/dss/features/connectivity/), é uma plataforma de ciência de dados colaborativa que permite que os cientistas de dados e criem e forneçam soluções analíticas. Oferta DSS como um HDInsight aplicação permite-lhe utilizar a ciência de dados para criar soluções de macrodados e executá-los em nível empresarial e escala.

Pode usar DSS para implementar uma solução completa de análise, começando com a ingestão de dados, preparação e o processamento. Também pode incluir uma solução DSS treinamento e aplicar modelos de aprendizagem automática, visualização e, em seguida, operacionalização.

Pode instalar DSS no HDInsight com clusters do Hadoop ou do Spark. Pode instalar o DSS em clusters em execução existentes, ou durante a criação de novos clusters. DSS também oferece suporte a utilizar o armazenamento de Blobs do Azure como um conector para a leitura de dados.

Pode usar DSS para criar projetos, que, em seguida, podem gerar tarefas de MapReduce ou do Spark. Estas tarefas são executadas como normais trabalhos de MapReduce ou do Spark no HDInsight, pelo que pode dimensionar o cluster a pedido.

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação num novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Escalões de cluster:{0}<br>tipos de cluster: Standard, Premium
* Tipo ou tipos de cluster: Hadoop, Spark
* Versões de cluster: 3.4, 3.5

## <a name="install-the-dataiku-dss-published-application"></a>Instalar o Dataiku DSS publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações disponíveis do ISV, leia [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-dataiku-dss"></a>Inicie o Dataiku DSS

1. Após a instalação, pode iniciar DSS do seu cluster no portal do Azure ao aceder a **definições** painel, em seguida, clicar em **aplicativos** sob o **geral** categoria. O painel aplicações instaladas apresenta uma lista de aplicações instaladas.

    ![Aplicações instaladas Dataiku DSS](./media/hdinsight-apps-install-dataiku/app.png)

2. Quando seleciona DSS no HDInsight, pode ver uma ligação para a página da web e o caminho do ponto final SSH. Selecione a ligação de página Web.

3. Na primeira execução, é-lhe apresentado um formulário para criar uma nova conta de Dataiku gratuitamente ou para iniciar sessão a uma conta existente. Também tem a opção para iniciar uma avaliação gratuita de 2 semanas de [Enterprise Edition](https://www.dataiku.com/dss/editions/). A partir deste ponto, tem a opção de continuar com a introduzir uma chave de licença do Enterprise Edition ou através da edição de Comunidade.

4. Depois de concluir a sua opção de licença selecionada, é-lhe apresentado um formulário de início de sessão. Introduza as credenciais de predefinição exibidas antes o formulário de início de sessão.

Os passos seguintes fornecem uma demonstração simple.

1. [Transferir os pedidos de exemplo CSV](https://doc.dataiku.com/tutorials/data/101/haiku_shirt_sales.csv).

2. A partir do DSS dashboard, selecione o **+** (novo projeto) ligação no menu da esquerda para criar um novo projeto.

    ![Nova ligação do projeto](./media/hdinsight-apps-install-dataiku/new-project.png)

3. No formulário de projeto novo, escreva um **nome**. O **chave de projeto** é preenchida automaticamente com um valor sugerido. Neste caso, introduza "Orders". Clique em **criar**.

    ![Novo formulário do projeto](./media/hdinsight-apps-install-dataiku/new-project-form.png)

4. Selecione **+ importar seu primeiro conjunto de dados** na sua nova página do projeto.

    ![Carregamento de Ficheiros](./media/hdinsight-apps-install-dataiku/import-dataset.png)

5. Selecione **os ficheiros são carregados** sob a **ficheiros** lista do conjunto de dados. É apresentada a caixa de diálogo de carregamento. Clique em Adicionar um ficheiro, selecione o `haiku_shirt_sales.csv` do ficheiro é transferido e validar.

6. O ficheiro é carregado para DSS. Verifique se DSS detetado o formato CSV corretamente ao clicar no botão de pré-visualização.

    ![Carregamento de Ficheiros](./media/hdinsight-apps-install-dataiku/preview.png)

7. A importação é quase perfeita. O ficheiro CSV está a utilizar um separador de separador. Pode ver que os dados estão em formato de tabela com colunas denominadas funcionalidades e linhas que representam as observações. Um erro é que aparentemente o ficheiro continha uma linha em branco entre o cabeçalho e os dados. Para corrigir este erro, introduza `1` no **Ignorar linhas seguintes** campo.

    ![Guardar](./media/hdinsight-apps-install-dataiku/skip-lines.png)

8. Dê um nome ao novo conjunto de dados. Introduza **haiku_shirt_sales** no campo na parte superior do ecrã, em seguida, selecione a **criar** botão.

9. Verá uma exibição tabular dos seus dados onde pode começar a explorá-lo. Para cada coluna, deverá ver que o estúdio de ciência de Dataiku detetou um tipo de dados, no _azul_ - neste caso, o texto, o número ou a data (na neanalyzovanou). Um medidor indica a proporção da coluna para o qual os valores não pareçam corresponde ao tipo de (a vermelho) ou estão em falta (em branco). Este conjunto de dados de exemplo, o departamento tem valores vazios e dados inválidos.

    ![Vista tabular](./media/hdinsight-apps-install-dataiku/viewing-dataset.png)

## <a name="data-manipulation"></a>Manipulação de dados

Dados do mundo real quase sempre são confusos e raramente é ele empacotado organizadamente. Limpeza de dados, normalmente, requer uma cadeia de scripts e lógica de negócios associados. Dataiku DSS fornece um dedicado **laboratório** ferramenta para facilitar essa tarefa mais fácil de utilizar.

1. Clique em **laboratório** no canto superior direito.

    ![Botão de laboratório](./media/hdinsight-apps-install-dataiku/lab-button.png)

2. É aberta a janela de laboratório. O laboratório é onde iterativamente trabalhar no seu conjunto de dados para avançar para o mesmo. Este tutorial demonstra o aspecto de análise Visual. Selecione o **New** botão abaixo análise Visual. Deve especificar um nome para a sua análise. Deixe o nome predefinido por agora, em seguida, clique em **criar**.

    ![Criar laboratório](./media/hdinsight-apps-install-dataiku/create-lab.png)

3. Selecione o **estatísticas de colunas rápida** botão no canto superior direito da página.

    ![Estatísticas de colunas rápida](./media/hdinsight-apps-install-dataiku/quick-column-stats.png)

4. Pode ver as estatísticas de tipos de dados e os valores apresentados em gráficos com base na linha cronológica sob o **vista rápida de colunas** painel.

    ![Vista rápida de colunas](./media/hdinsight-apps-install-dataiku/columns-quick-view.png)

Agora, pode explorar DSS usando os dados de exemplo. Pode limpar e trabalhar com os dados e criar novas visualizações.

Para tutoriais detalhados, leia [Dataiku DSS Saiba](https://www.dataiku.com/learn/).

## <a name="next-steps"></a>Passos Seguintes

* [Documentação de Dataiku DSS](https://doc.dataiku.com/dss/latest/).
* [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada para o HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux com ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilizar nós de extremidade vazios no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder aos clusters do HDInsight e de teste e alojamento de aplicações do HDInsight.
