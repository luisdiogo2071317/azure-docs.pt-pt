---
title: "Instalação publicado aplicação - Datameer - Azure HDInsight | Microsoft Docs"
description: "Instalar e utilizar a aplicação de Hadoop do Datameer por terceiros."
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
ms.openlocfilehash: 8a898b4f82cf2d7e05e8c3895e5eddd8cf02b173
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="install-published-application---datameer"></a>Instalar a aplicação publicada - Datameer

Este artigo descreve como instalar e executar o [Datameer](https://www.datameer.com/) publicado a aplicação de Hadoop no Azure HDInsight. Para obter uma descrição geral da plataforma de aplicação do HDInsight e uma lista de disponíveis independentes de Software (ISV) aplicações publicadas, consulte [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md). Para obter instruções sobre como instalar a sua própria aplicação, veja [Instalar aplicações do HDInsight personalizadas](hdinsight-apps-install-custom-applications.md).

## <a name="about-datameer"></a>Sobre Datameer

Datameer é uma aplicação nativa para o Hadoop plataforma, expandir existente do Azure HDInsight capacidades e pelo fornecimento de integração rápida, preparação e de análise de dados estruturados e não estruturados. Datameer pode aceder a mais de 70 origens e formatos: structured, estruturados, semi-estruturados e não estruturados. Diretamente pode carregar dados, ou utilizar as respetivas ligações de dados exclusiva para retirar dados a pedido. A funcionalidade de self-service do Datameer e interface de folha de cálculo familiar reduz a complexidade da tecnologia de macrodados e acelera o tempo para obter conhecimentos aprofundados. A interface de folha de cálculo fornece um mecanismo simple para introduzir declarativas fórmulas que, em seguida, são convertidas para otimizada tarefas do Hadoop. Com o Datameer e o business intelligence (BI) e a competências de Excel, pode utilizar Hadoop na nuvem rapidamente. Para obter mais informações, consulte o [Datameer documentação](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).

## <a name="prerequisites"></a>Pré-requisitos

Para instalar esta aplicação no novo cluster de HDInsight ou um cluster existente, tem de ter a seguinte configuração:

* Camada do cluster: Standard
* Tipo de cluster: Hadoop
* Versão do cluster: 3.4

## <a name="install-the-datameer-published-application"></a>Instalar o Datameer publicado a aplicação

Para obter instruções passo a passo sobre como instalar esta e outras aplicações de ISV disponíveis, leia o artigo [instalar aplicações do Hadoop de terceiros](hdinsight-apps-install-applications.md).

## <a name="launch-datameer"></a>Inicie o Datameer

1. Após a instalação, pode iniciar Datameer do seu cluster no portal do Azure acedendo ao **definições** painel, em seguida, clicar em **aplicações** sob o **geral** categoria . O painel aplicações instaladas apresenta uma lista de aplicações instaladas.

    ![Aplicação de Datameer instalado](./media/hdinsight-apps-install-datameer/datameer-app.png)

2. Quando seleciona Datameer, verá uma ligação para a página web e o caminho do ponto final SSH. Selecione a hiperligação de página Web.

3. No primeiro lançamento, existem duas opções de licença: a uma avaliação gratuita de 14 dias, ou ativar uma licença existente.

    ![Opções da licença](./media/hdinsight-apps-install-datameer/license.png)

4. Depois de concluir a opção de licenciamento selecionado, será apresentada com um formulário de início de sessão. Introduza as credenciais predefinidas apresentadas antes do formulário de início de sessão. Após iniciar sessão, aceite o contrato de software para continuar.

    ![Iniciar sessão](./media/hdinsight-apps-install-datameer/login.png)

Os passos seguintes mostram uma demonstração de "Olá mundo".

1. [Transferir o exemplo de CSV](https://datameer.box.com/s/wzzw27za3agic4yjj8zrn6vfrph0ppnf).

2. Clique em de  **+**  assinar por cima do Datameer dashboard e clique em **carregar ficheiro**.

    ![Carregamento de Ficheiros](./media/hdinsight-apps-install-datameer/upload.png)

3. Na caixa de diálogo de carregamento, procurar e selecionar o **Hello World.csv** ficheiro que transferiu. Certifique-se de que o **tipo de ficheiro** está definido como CSV / TSV. Selecione **Seguinte**. Manter clicar **seguinte** até chegar ao fim do assistente.

    ![Carregamento de Ficheiros](./media/hdinsight-apps-install-datameer/upload-browse.png)

4. Nome de ficheiro **Olá, mundo** por baixo de uma nova pasta. Mudar o nome da nova pasta como "Demonstração". Selecione **Guardar**.

    ![Guardar](./media/hdinsight-apps-install-datameer/save.png)

5. Clique em de  **+**  iniciar sessão mais uma vez e selecionar **livro** para criar um novo livro para que os dados.

    ![Adicionar o livro](./media/hdinsight-apps-install-datameer/add-workbook.png)

6. Expanda o **dados** pasta, **FileUploads**, em seguida, a **demonstração** pasta que criou ao guardar o ficheiro de "Olá mundo". Selecione **Olá, mundo** formam a lista de ficheiros, em seguida, clique em **adicionar dados**.

    ![Guardar](./media/hdinsight-apps-install-datameer/select-file.png)

7. Ver os dados carregados numa interface de folha de cálculo. Para selecionar um subconjunto dos dados, selecione o **filtro** botão na toolbar.

    ![Botão Filtrar](./media/hdinsight-apps-install-datameer/filter-button.png)

8. Na caixa de diálogo Aplicar filtro, selecione o **Cidade** coluna **é igual a** operador e tipo **Chicago** na caixa de texto de filtro. Verifique o **criar filtro na nova folha** caixa de verificação, em seguida, selecione **criar filtro**.

    ![Aplicar o filtro](./media/hdinsight-apps-install-datameer/apply-filter.png)

9. Guarde o livro clicando **ficheiro**, em seguida, **guardar**. Forneça um nome, como "Olá mundo livro".

10. São apresentados com opções para saber como e quando executar o livro. Agora, deixe todas as opções dos respetivos valores predefinidos, em seguida, verifique o **guardar imediatamente após de processo do cálculo de início**e selecione **guardar**.

    ![Guardar o livro](./media/hdinsight-apps-install-datameer/save-workbook.png)

11. Datameer fornece ferramentas de visualização de elevado desempenho. Para apresentar os dados, crie um gráfico informativo. Selecione o  **+**  assinar por cima do dashboard, em seguida, selecione **gráfico informativo**.

    ![Adicionar gráfico informativo](./media/hdinsight-apps-install-datameer/infographic-button.png)

12. Arraste um widget de gráfico de barras da lista de widgets à esquerda, conforme mostrado no passo 1 no diagrama seguinte. Em seguida, navegar pela pasta de dados sob o browser de dados à direita, expanda o seu livro, em seguida, a folha de cálculo que adicionou com o filtro (passo 2). Arraste o **nome** coluna ao longo da parte superior do gráfico de barras e largar para o **etiqueta** destino para definir a coluna de nome do livro como campo de etiqueta do gráfico (passo 3).

    ![Infografia](./media/hdinsight-apps-install-datameer/infographic.png)

13. Para definir o tempo como eixo Y do gráfico, arraste o **idade** coluna no gráfico de **dados** campo.

    ![Infografia](./media/hdinsight-apps-install-datameer/infographic-age.png)

Parabéns! Criou uma visualização de dados sem escrever qualquer código. Agora pode explorar variações e visualizações adicionais.

## <a name="next-steps"></a>Passos Seguintes

* [Documentação de Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft).
* [Instalar aplicações HDInsight personalizadas](hdinsight-apps-install-custom-applications.md): Saiba como implementar uma aplicação HDInsight não publicada no HDInsight.
* [Publicar aplicações do HDInsight](hdinsight-apps-publish-applications.md): Saiba como publicar as suas aplicações do HDInsight personalizadas no Azure Marketplace.
* [MSDN: Instalar uma aplicação do HDInsight](https://msdn.microsoft.com/library/mt706515.aspx): Saiba como definir aplicações do HDInsight.
* [Personalizar clusters do HDInsight baseado em Linux através da ação de Script](hdinsight-hadoop-customize-cluster-linux.md): Saiba como utilizar a ação de Script para instalar outras aplicações.
* [Utilize nós de limite vazio no HDInsight](hdinsight-apps-use-edge-node.md): Saiba como utilizar um nó de extremidade vazio para aceder a clusters do HDInsight e para testar e alojamento de aplicações do HDInsight.
