---
title: "Instalação publicado aplicação - Dataiku DDS - Azure HDInsight | Microsoft Docs"
description: "Instalar e utilizar a aplicação de Hadoop do Dataiku DDS por terceiros."
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
ms.openlocfilehash: 835f433e0bf79e8bc4d9b30963196f65bc53cd0e
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="install-published-application---dataiku-dds"></a>Instalar a aplicação publicada - Dataiku DDS

Este artigo descreve como instalar e executar o [Dataiku DDS](https://www.dataiku.com/) publicado a aplicação de Hadoop no Azure HDInsight. Para obter uma descrição geral da plataforma de aplicação do HDInsight e uma lista de disponíveis independentes de Software (ISV) aplicações publicadas, consulte [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-dataiku-dss"></a>Sobre Dataiku DSS

O Dataiku [Studio de ciência de dados (DSS)](https://www.dataiku.com/dss/features/connectivity/), é uma plataforma de ciência de dados de colaboração que permite cientistas de dados compilar e disponibilizar soluções analíticas. Oferta DSS como um HDInsight aplicação permite-lhe utilizar a ciência de dados para criar soluções de macrodados e executá-los ao nível da empresa e a escala.

Pode utilizar DSS para implementar uma solução completa de analítica, começando com a ingestão de dados, preparação e de processamento. Também pode incluir uma solução DSS formação e aplicar modelos de machine learning, visualização e, em seguida, operacionalizar.

Pode instalar DSS no HDInsight com clusters do Spark ou do Hadoop. Pode instalar DSS em clusters em execução existentes ou ao criar novos clusters. DSS também suporta a utilização de armazenamento de Blobs do Azure como um conector para a leitura de dados.

Pode utilizar DSS para compilar projetos, que, em seguida, podem gerar tarefas de MapReduce ou Spark. Estas tarefas são executadas como normais trabalhos de MapReduce ou Spark no HDInsight, pelo que pode dimensionar o cluster a pedido.

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação no novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Cluster tier(s): Standard, Premium
* Os tipos de parâmetros de cluster: Hadoop, Spark
* Versões de cluster: 3.4, 3.5

## <a name="install-the-dataiku-dss-published-application"></a>Instalar o DSS Dataiku publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações de ISV disponíveis, leia o artigo [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-dataiku-dss"></a>Iniciar Dataiku DSS

1. Após a instalação, pode iniciar DSS do seu cluster no portal do Azure acedendo ao **definições** painel, em seguida, clicar em **aplicações** sob o **geral** categoria. O painel aplicações instaladas apresenta uma lista de aplicações instaladas.

    ![Aplicações instaladas Dataiku DSS](./media/hdinsight-apps-install-dataiku/app.png)

2. Quando seleciona DSS no HDInsight, verá uma ligação para a página web e o caminho do ponto final SSH. Selecione a hiperligação de página Web.

3. No primeiro lançamento, são apresentados com um formulário para criar uma nova conta de Dataiku gratuitamente ou iniciar sessão a uma conta existente. Também tem a opção para iniciar uma avaliação gratuita de 2 semanas do [Enterprise Edition](https://www.dataiku.com/dss/editions/). A partir deste ponto, tem a opção de continuar com a introduzir uma chave de licenciamento para o Enterprise Edition ou utilizar a edição de Comunidade.

4. Depois de concluir a opção de licenciamento selecionado, são apresentados com um formulário de início de sessão. Introduza as credenciais predefinidas apresentadas antes do formulário de início de sessão.

Os passos seguintes fornecem uma demonstração simple.

1. [Transferir as exemplo as ordens CSV](https://doc.dataiku.com/tutorials/data/101/haiku_shirt_sales.csv).

2. A partir do DSS dashboard, selecione o  **+**  (novo projeto) ligação no menu da esquerda para criar um novo projeto.

    ![Nova ligação do projeto](./media/hdinsight-apps-install-dataiku/new-project.png)

3. No formulário do novo projeto, escreva um **nome**. O **projeto chave** é preenchida automaticamente com um valor sugerido. Neste caso, introduza "Ordens". Clique em **criar**.

    ![Novo formulário de projeto](./media/hdinsight-apps-install-dataiku/new-project-form.png)

4. Selecione **+ importar o primeiro conjunto de dados** na página do seu projeto novo.

    ![Carregamento de Ficheiros](./media/hdinsight-apps-install-dataiku/import-dataset.png)

5. Selecione **carregar os ficheiros** sob o **ficheiros** lista do conjunto de dados. É-lhe apresentada a caixa de diálogo de carregamento. Clique em Adicionar um ficheiro, selecione o `haiku_shirt_sales.csv` ficheiro é transferido e validar.

6. O ficheiro é carregado para DSS. Verifique se DSS detetou o formato CSV corretamente ao clicar no botão de pré-visualização.

    ![Carregamento de Ficheiros](./media/hdinsight-apps-install-dataiku/preview.png)

7. A importação é quase perfeita. O ficheiro CSV está a utilizar um separador de separador. Pode ver que os dados estão no formato tabular, com colunas chamado funcionalidades e as linhas que representam as observações. Um erro é que parecer o ficheiro continha uma linha em branco entre o cabeçalho e os dados. Para corrigir este erro, introduza `1` no **Ignorar linhas seguintes** campo.

    ![Guardar](./media/hdinsight-apps-install-dataiku/skip-lines.png)

8. Dê um nome de novo conjunto de dados. Introduza **haiku_shirt_sales** no campo por cima do ecrã, em seguida, selecione o **criar** botão.

9. Pode ver uma vista em tabela dos seus dados onde pode começar a explorá-lo. Para cada coluna, deverá ver que Dataiku ciência Studio detetou um tipo de dados, _azul_ , em caso deste caso, o texto, o número ou a data (não analisada). Um medidor indica o rácio da coluna para o qual os valores aparentam não corresponde ao tipo de (em vermelho) ou estão em falta (em branco). Este conjunto de dados de exemplo, o departamento tem valores vazios e dados inválidos.

    ![Vista em tabela](./media/hdinsight-apps-install-dataiku/viewing-dataset.png)

## <a name="data-manipulation"></a>Manipulação de dados

Dados do mundo real são quase sempre messy e raramente é neatly empacotado. A limpar dados normalmente, requer uma cadeia de scripts e lógica de negócio associado. Dataiku DSS fornece um dedicado **laboratório** ferramenta para efetuar esta tarefa mais intuitivo.

1. Clique em **laboratório** no canto superior direito.

    ![Botão de laboratório](./media/hdinsight-apps-install-dataiku/lab-button.png)

2. Abre a janela de laboratório. O laboratório é onde iteratively trabalhar no seu conjunto de dados para obter mais para a mesma. Este tutorial demonstra o aspeto de análise Visual. Selecione o **novo** no botão abaixo Visual Analysis Services. Lhe for pedido que especifique um nome para a análise. Deixe o nome predefinido por agora, em seguida, clique em **criar**.

    ![Criar laboratório](./media/hdinsight-apps-install-dataiku/create-lab.png)

3. Selecione o **estatísticas de colunas rápida** botão no canto superior direito da página.

    ![Estatísticas de colunas rápida](./media/hdinsight-apps-install-dataiku/quick-column-stats.png)

4. Pode ver as estatísticas de tipos de dados e valores apresentados nos gráficos de linha cronológica baseada no **vista rápida colunas** painel.

    ![Vista rápida de colunas](./media/hdinsight-apps-install-dataiku/columns-quick-view.png)

Agora pode explorar DSS utilizando os dados de exemplo. Pode limpar e trabalhar com os dados e criar novas visualizações.

Para tutoriais aprofundadas, leia o artigo [saiba DSS de Dataiku](https://www.dataiku.com/learn/).

## <a name="next-steps"></a>Passos Seguintes

* [Documentação de Dataiku DSS](https://doc.dataiku.com/dss/latest/).
* [Instalar aplicações HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada no HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux através da ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilize nós de limite vazio no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder a clusters do HDInsight e para testar e alojamento de aplicações do HDInsight.
