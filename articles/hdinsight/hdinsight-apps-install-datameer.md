---
title: Instalar a aplicação publicada - Datameer - Azure HDInsight
description: Instalar e utilizar a aplicação de Apache Hadoop de terceiros Datameer.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 97d99aa59c490cf2dcdd4a69f32411a051942d36
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037809"
---
# <a name="install-published-application---datameer"></a>Instalar a aplicação publicada - Datameer

Este artigo descreve como instalar e executar o [Datameer](https://www.datameer.com/) publicado a aplicação do Apache Hadoop no HDInsight do Azure. Para uma visão geral da plataforma de aplicações do HDInsight e uma lista de disponíveis independentes fabricante de Software (ISV) a aplicações publicadas, consulte [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-datameer"></a>Sobre o Datameer

Datameer é um aplicativo nativo para o Hadoop plataforma, estendendo existente capacidades do Azure HDInsight e fornecer integração rápida, preparação e análises de dados estruturados e não estruturados. Datameer pode aceder a mais de 70 origens e formatos: estruturados, semiestruturados e não estruturados. Diretamente pode carregar dados, ou utilizar as suas ligações de dados exclusivo para extrair os dados a pedido. A funcionalidade de autoatendimento do Datameer e interface de folha de cálculo familiar reduz a complexidade da tecnologia de grandes volumes de dados e acelera o tempo para obter conhecimentos aprofundados. A interface de folha de cálculo fornece um mecanismo simples para inserir fórmulas declarativas que, em seguida, são traduzidas para otimizadas para tarefas do Hadoop. Com o Datameer e sua business intelligence (BI) e a habilidades de Excel, pode utilizar o Hadoop na cloud rapidamente. Para obter mais informações, consulte a [Datameer documentação](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação num novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Escalão do cluster: Standard
* Tipo de cluster: Hadoop
* Versão do cluster: 3.4

## <a name="install-the-datameer-published-application"></a>Instalar o Datameer publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações disponíveis do ISV, leia [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-datameer"></a>Inicie o Datameer

1. Após a instalação, pode iniciar Datameer do seu cluster no portal do Azure ao aceder a **definições** painel, em seguida, clicar em **aplicativos** sob o **geral** categoria . O painel aplicações instaladas apresenta uma lista de aplicações instaladas.

    ![Aplicação de Datameer instalado](./media/hdinsight-apps-install-datameer/datameer-app.png)

2. Quando seleciona o Datameer, verá um link para a página da web e o caminho do ponto final SSH. Selecione a ligação de página Web.

3. Na primeira inicialização, existem duas opções de licença: qualquer um de uma avaliação gratuita de 14 dias, ou ativar uma licença já existente.

    ![Opções da licença](./media/hdinsight-apps-install-datameer/license.png)

4. Depois de concluir a sua opção de licenças selecionado, será apresentada com um formulário de início de sessão. Introduza as credenciais de predefinição exibidas antes o formulário de início de sessão. Após iniciar sessão, aceite o contrato de software para continuar.

    ![Iniciar sessão](./media/hdinsight-apps-install-datameer/login.png)

Os passos seguintes mostram uma demonstração de "Hello World".

1. [Transferir o CSV de exemplo](https://datameer.box.com/s/wzzw27za3agic4yjj8zrn6vfrph0ppnf).

2. Clique nas **+** iniciar sessão no dashboard Datameer e clique em **carregamento de ficheiros**.

    ![Carregamento de Ficheiros](./media/hdinsight-apps-install-datameer/upload.png)

3. Na caixa de diálogo de carregamento, procure e selecione o **Hello World.csv** ficheiro que transferiu. Certifique-se de que o **tipo de ficheiro** está definido como CSV / TSV. Selecione **Seguinte**. Continua clicando **seguinte** até que chegue ao final do assistente.

    ![Carregamento de Ficheiros](./media/hdinsight-apps-install-datameer/upload-browse.png)

4. Nomeie o arquivo **Olá, mundo** por baixo de uma nova pasta. Renomeie a nova pasta como "Demonstração". Selecione **Guardar**.

    ![Guardar](./media/hdinsight-apps-install-datameer/save.png)

5. Clique nas **+** iniciar sessão mais uma vez e selecione **livro** para criar uma nova pasta de trabalho para os dados.

    ![Adicionar pasta de trabalho](./media/hdinsight-apps-install-datameer/add-workbook.png)

6. Expanda a **dados** pasta, **FileUploads**, o **demonstração** pasta que criou ao guardar o ficheiro de "Hello World". Selecione **Olá, mundo** formam a lista de ficheiros, em seguida, clique em **adicionar dados**.

    ![Guardar](./media/hdinsight-apps-install-datameer/select-file.png)

7. Ver os dados carregados numa interface de folha de cálculo. Para selecionar um subconjunto dos dados, selecione o **filtro** botão na barra de ferramentas.

    ![Botão Filtro](./media/hdinsight-apps-install-datameer/filter-button.png)

8. Na caixa de diálogo Aplicar filtro, selecione o **Cidade** coluna **é igual a** operador e o tipo **Chicago** na caixa de texto de filtro. Verifique os **criar filtro na nova folha de cálculo** caixa de verificação, em seguida, selecione **criar filtro**.

    ![Aplicar filtro](./media/hdinsight-apps-install-datameer/apply-filter.png)

9. Guarde o livro clicando **arquivo**, em seguida, **guardar**. Forneça um nome, como "Hello World livro".

10. É apresentada com opções sobre como e quando executar a pasta de trabalho. Por enquanto, deixe todas as opções com os valores padrão, em seguida, verifique os **save imediatamente depois do processo de cálculo início**e selecione **guardar**.

    ![Guarde o livro](./media/hdinsight-apps-install-datameer/save-workbook.png)

11. Datameer fornece ferramentas de visualização poderosa. Para exibir os dados, crie um gráfico informativo. Selecione o **+** iniciar sessão na parte superior do dashboard, em seguida, selecione **Infografia**.

    ![Adicionar o Infográfico](./media/hdinsight-apps-install-datameer/infographic-button.png)

12. Arraste um widget de gráfico de barras da lista de widgets à esquerda, conforme mostrado no passo 1 no diagrama seguinte. Em seguida, navegue através da pasta de dados sob o navegador de dados no lado direito, expanda o seu livro, em seguida, a folha de cálculo que adicionou com o filtro (etapa 2). Arrastar o **Name** coluna ao longo da parte superior do gráfico de barras e soltar para o **etiqueta** destino para definir a coluna de nome do livro como campo de etiqueta do gráfico (etapa 3).

    ![Infografia](./media/hdinsight-apps-install-datameer/infographic.png)

13. Para definir o tempo como eixo de Y do gráfico, arraste o **idade** coluna para o gráfico **dados** campo.

    ![Infografia](./media/hdinsight-apps-install-datameer/infographic-age.png)

Parabéns! Acabou de criar uma visualização dos seus dados sem escrever nenhum código. Pode, agora, explore variações e visualizações adicionais.

## <a name="next-steps"></a>Passos Seguintes

* [Documentação de Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).
* [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada para o HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux com ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilizar nós de extremidade vazios no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder aos clusters do HDInsight e de teste e alojamento de aplicações do HDInsight.
