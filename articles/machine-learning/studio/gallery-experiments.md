---
title: Experimentações de Galeria de IA do Azure | Documentos da Microsoft
description: Detetar e partilhar as experimentações na Galeria de IA do Azure.
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.custom: (previous ms.author hshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: f4248922-c961-4d3a-9e1b-aec743210166
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.openlocfilehash: 4f1a06d8daa2a811871c8469dddb75ce36ceb04e
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/16/2018
ms.locfileid: "51821339"
---
# <a name="discover-experiments-in-azure-ai-gallery"></a>Descubra as experimentações na Galeria de IA do Azure

[Galeria de IA do Azure](http://gallery.cortanaintelligence.com) tem uma grande variedade de [experimentações](https://gallery.cortanaintelligence.com/experiments) que tenha sido desenvolvido em [Azure Machine Learning Studio](https://studio.azureml.net). Intervalo de experimentações de experimentações de prova de conceito rápidos que demonstram uma máquina específica de aprendizagem técnica, para as soluções inteiramente desenvolvidas para problemas do complexos de machine learning.

> [!NOTE]
> Uma ***experimentar*** é uma tela no Machine Learning Studio, que pode utilizar para construir um modelo de Análise Preditiva. Criar o modelo ao ligar os dados com vários módulos analíticos. Pode experimentar idéias diferentes, realizar execuções de versão de avaliação e, eventualmente, implementar o seu modelo como um serviço web no Azure. Para obter um exemplo de como criar uma experiência básica, consulte [tutorial do Machine learning: crie a sua primeira experiência no Azure Machine Learning Studio](create-experiment.md). Para obter instruções mais abrangente de como criar uma solução de Análise Preditiva, veja [instruções: desenvolver uma solução de Análise Preditiva para a avaliação de risco de crédito no Azure Machine Learning](walkthrough-develop-predictive-solution.md).
>
>

## <a name="discover"></a>Descobrir
Para procurar experimentações [na galeria](http://gallery.cortanaintelligence.com), na parte superior da home page da galeria, selecione **experimentações**.

O **[experimentações](https://gallery.cortanaintelligence.com/experiments)** página apresenta uma lista de experimentações recentemente adicionadas e populares. Para ver todas as experimentações, selecione o **ver todas as** botão. Para procurar uma experimentação específica, selecione **ver todas as**e os critérios de filtro, em seguida, selecione. Também pode introduzir termos da procura na **pesquisa** caixa na parte superior da página da galeria.

Pode obter mais informações sobre uma experimentação na página de detalhes de experimentação. Para abrir uma página de detalhes de experimentação, selecione a experimentação. Numa experimentação detalhes da página, além da **comentários** seção, pode comentar, fornecer comentários ou fazer perguntas sobre a experimentação. Pode até compartilhar a experimentação com amigos ou colegas no Twitter ou LinkedIn. Também pode correio uma ligação para a página de detalhes de experimentação, convidar outros utilizadores para ver a página.

![Compartilhar este item com amigos](./media/gallery-how-to-use-contribute-publish/share-links.png)

![Adicione os seus próprios comentários](./media/gallery-how-to-use-contribute-publish/comments.png)

## <a name="download"></a>Transferência
Pode transferir uma cópia qualquer experiência na Galeria à sua área de trabalho do Machine Learning Studio. Em seguida, pode modificar a cópia para criar suas próprias soluções.

Galeria de IA do Azure oferece duas formas de importar uma cópia de uma experiência:

* **Na galeria do**. Se encontrar uma experimentação que desejar na galeria, pode baixar uma cópia e, em seguida, abra-o na sua área de trabalho do Machine Learning Studio.
* **De dentro do Machine Learning Studio**. No Machine Learning Studio, pode utilizar qualquer experiência na galeria como um modelo para criar uma nova experimentação.

### <a name="from-the-gallery"></a>Partir da Galeria

1. Na galeria, abra a página de detalhes de experimentação.
2. Selecione **abrir no Studio**.

    ![Experimentação aberta da Galeria](./media/gallery-experiments/open-experiment-from-gallery.png)

Quando seleciona **abrir no Studio**, a experimentação é aberto na sua área de trabalho do Machine Learning Studio. (Se ainda não tem sessão iniciada Machine Learning Studio, deve primeiro início de sessão utilizando a sua conta Microsoft.)

### <a name="from-within-machine-learning-studio"></a>De dentro do Machine Learning Studio

1. No Machine Learning Studio, selecione **NEW**.
2. Selecione **experimentação**. Pode escolher de entre uma lista de experimentações de galeria ou encontrar uma experimentação específica com o **pesquisa** caixa.
3. Apontar o mouse para uma experimentação e, em seguida, selecione **abrir no Studio**. (Para ver informações sobre a experimentação, selecione **vista na galeria**. Isto leva-o para a página de detalhes de experimentação na galeria.)

    ![Galeria de aberto de experimentação do Machine Learning Studio](./media/gallery-experiments/open-experiment-from-studio.png)

Pode personalizar, iterar e implementar uma experimentação transferida como qualquer outra experimentação que criar no Machine Learning Studio.

![Experimentação abrir no Studio](./media/gallery-experiments/experiment-open-in-studio.png)

## <a name="contribute"></a>Contribuir
Quando iniciar sessão na galeria, se tornar um membro da Comunidade da galeria. Como um membro da Comunidade, pode contribuir suas próprias experiências, para que outros utilizadores podem se beneficiar com as soluções que descobertos.

### <a name="publish-your-experiment-to-the-gallery"></a>Publicar a sua experimentação para a Galeria

1. Inicie sessão Machine Learning Studio, com a sua conta Microsoft.
2. Criar a sua experiência e, em seguida, executá-lo.
3. Quando estiver pronto para publicar a sua experimentação na galeria, na lista de ações abaixo a tela da experimentação, selecione **publicar na galeria**.

    ![Selecione "Publicar Galeria"](./media/gallery-experiments/publish-experiment-to-gallery.png)
4. Sobre o **experimentação Descrição** página, introduza um título e etiquetas. Torne o título e marcas descritivas. Realce as técnicas que utilizou ou que está a resolver o problema do mundo real. Um exemplo de um título descritivo experimentação é "classificação binária: análise de sentimentos do Twitter."

    ![Introduza o título e as etiquetas para publicação](./media/gallery-experiments/experiment-description.png)
5. Na **resumo** , introduza um resumo da sua experimentação. Descreva resumidamente o problema que resolve a experimentação e, como o abordado.
6. Na **descrição detalhada** caixa, descreve as etapas realizadas em cada parte da sua experimentação. Alguns tópicos úteis para incluir são:
   * Captura de ecrã de gráfico de experimentação
   * Explicação e origens de dados
   * Processamento de dados
   * Com engenharia
   * Descrição do modelo
   * Os resultados e de avaliação do desempenho de modelos

   Pode utilizar o markdown para formatar sua descrição. Para ver como as suas entradas na página de descrição de experimentação serão exibida quando a experimentação é publicada, selecione **pré-visualização**.

   ![Selecione "Pré-visualização" para ver a aparência seu texto](./media/gallery-experiments/preview-markdown-text.png)

   > [!TIP]
   > As caixas de texto fornecidas para a edição de markdown e pré-visualização são pequenos. Recomendamos que escrever a documentação de experiência num editor de markdown, copiá-lo e, em seguida, cole a documentação concluída na caixa de texto na galeria. Depois de publicar a sua experimentação, pode efetuar as correções necessárias ao utilizar as ferramentas baseadas na web padrão que utilizar markdown para editar e pré-visualizar.

7. Sobre o **seleção de imagem** página, selecione uma imagem em miniatura para a experimentação. É apresentada na imagem em miniatura na parte superior da página de detalhes de experimentação e no mosaico de experimentação. Outros usuários verão a imagem em miniatura quando navegam na galeria. Pode carregar uma imagem do computador ou selecione uma imagem das ações a partir da galeria.
    </br>
    ![Carregar ou selecionar uma imagem da Galeria](./media/gallery-experiments/select-gallery-image.png)
8. Na **definições** página, em **visibilidade**, escolha se pretende publicar os seus conteúdos publicamente (**pública**) ou para enviá-lo acessível apenas a pessoas que têm uma ligação para a página ( **Não listados**).

    ![Escolha se pretende publicar publicamente ou como não listados](./media/gallery-experiments/choose-public-or-unlisted.png)

    <!-- -->

   > [!TIP]
   > Se pretender certificar-se de que a documentação do parece correta antes do lançamento publicamente, pode publicar a experimentação como primeiro **Unlisted**. Mais tarde, pode alterar a definição de visibilidade para **público** na página de detalhes de experimentação.
   >
   >
9. Para publicar a experimentação para a galeria, selecione o **OK** marca de verificação.

    ![Selecione a marca de verificação OK para publicar a experimentação](./media/gallery-experiments/ok-checkmark.png)

Para obter dicas sobre como publicar uma experimentação de Galeria de alta qualidade, consulte [dicas para documentar e publicar a sua experimentação](#tips-for-documenting-and-publishing-your-experiment).

Isso é tudo – está tudo concluído.

Agora pode ver a sua experimentação na galeria e partilhar a ligação com outras pessoas. Se publicou sua experimentação ao utilizar o **público** visibilidade definição, a experimentação aparece nos resultados de pesquisa e procura na galeria. Pode editar a documentação de experimentação na página de detalhes de experimentação sempre que tem sessão iniciada na galeria.

Para ver a lista de suas contribuições, selecione a sua imagem no canto superior direito de qualquer página da galeria. Em seguida, selecione o nome para abrir a página da sua conta.

![Selecione o nome da sua conta](./media/gallery-experiments/click-account-name.png)

### <a name="update-your-experiment"></a>Atualizar a sua experimentação
Se for necessário, pode efetuar alterações ao fluxo de trabalho (módulos, parâmetros e assim por diante) numa experimentação que publicado na galeria. No Machine Learning Studio, efetue as alterações que gostaria de fazer a experimentação e, em seguida, publicar novamente. Sua experimentação publicada será atualizada com as suas alterações.

Pode alterar qualquer uma das seguintes informações para a experimentação diretamente na galeria:

* Nome de experimentação
* Resumo ou descrição
* Etiquetas
* Imagem
* Definição de visibilidade (**pública** ou **Unlisted**)

Também pode eliminar a experimentação da galeria.

Pode efetuar estas alterações ou eliminar a experimentação, da página de detalhes de experimentação ou a partir de sua página de perfil na galeria.


#### <a name="from-the-experiment-details-page"></a>Na página de detalhes de experimentação
Na página de detalhes de experimentação, para alterar os detalhes para a experimentação, selecione **editar**.

![Selecione Editar para editar a sua experimentação](./media/gallery-experiments/edit-button.png)

A página de detalhes entra em modo de edição. Para fazer alterações, selecione **editar** junto do nome de experimentação, resumo ou etiquetas. Quando tiver terminado de fazer alterações, selecione **feito**.

![Selecione "Editar" para editar os detalhes e selecione "Concluído" quando terminar](./media/gallery-experiments/edit-details-page.png)

Para alterar as definições de visibilidade para a experimentação (**pública** ou **Unlisted**), ou para eliminar a experimentação da galeria, selecione o **definições** ícone.

![Selecione "Definições" para alterar a visibilidade ou para eliminar a experimentação](./media/gallery-experiments/settings-button.png)

#### <a name="from-your-profile-page"></a>Da sua página de perfil
Na página de perfil, selecione a seta para baixo para a experimentação e, em seguida, selecione **editar**. Isto leva-o para a página de detalhes para a experimentação, no modo de edição. Quando terminar de efetuar alterações, selecione **feito**.

Para eliminar a experimentação da galeria, selecione **eliminar**.

![Selecione "Editar" ou "Eliminar"](./media/gallery-experiments/edit-delete-buttons.png)

### <a name="tips-for-documenting-and-publishing-your-experiment"></a>Dicas para documentar e publicar a sua experimentação
* Pode assumir que o leitor tenha experiência de ciência de dados anterior, mas pode ser útil utilizar uma linguagem simple. Explica coisas detalhadamente, sempre que possível.
* Cortana Intelligence Suite é relativamente novo. Nem todos os leitores estão familiarizados com como usá-lo. Fornece informações e suficientes explicações passo a passo para ajudar os leitores, navegue até sua experimentação.
* Os elementos visuais podem ser úteis para os leitores de interpretar e utilizar a documentação de experimentação corretamente. Os elementos visuais incluem gráficos de experimentação e capturas de ecrã dos dados. Para obter mais informações sobre como incluir imagens na sua documentação de experimentação, consulte a [coleção de diretrizes de publicação e exemplos](https://gallery.cortanaintelligence.com/Collection/Publishing-Guidelines-and-Examples-1).
* Se incluir um conjunto de dados na sua experimentação (ou seja, está não importar o conjunto de dados por meio do módulo de importação de dados), o conjunto de dados faz parte da sua experimentação e é publicado na galeria. Certifique-se de que o conjunto de dados que publicar tem termos que permitem a partilha e a transferência por qualquer pessoa de licenciamento. Contribuições da Galeria são abordadas do Azure [termos de utilização](https://azure.microsoft.com/support/legal/website-terms-of-use/).

## <a name="frequently-asked-questions"></a>Perguntas mais frequentes
**Quais são os requisitos para submeter ou editar uma imagem para a minha experiência?**

Imagens que submeter com sua experimentação são utilizadas para criar um mosaico de experimentação para sua contribuição. Recomendamos que imagens de ser inferior a 500 KB, com um aspect ratio de 3:2 e uma resolução de 960 &#215; 640.

**O que acontece ao conjunto de dados que usei na experimentação? O conjunto de dados também é publicado na Galeria?**

Se o seu conjunto de dados faz parte da sua experimentação e não está a ser importado por meio do módulo de importar dados, o conjunto de dados é publicado na galeria como parte da sua experimentação. Certifique-se de que o conjunto de dados que publicar com sua experimentação tem os termos de licenciamento adequados. Os termos de licenciamento devem permitir que qualquer pessoa partilhar e transferir os dados.

**Tenho uma experimentação que utiliza um módulo de importar dados para extrair os dados a partir do Azure HDInsight ou o SQL Server. Utiliza as minhas credenciais para recuperar os dados. Pode publicar esse tipo de experimentação? Como pode posso ter a garantia de que as minhas credenciais não ser partilhados?**

Atualmente, não é possível publicar uma experimentação que utiliza as credenciais na galeria.

**Como posso participar múltiplas etiquetas?**

Depois de introduzir uma etiqueta, introduza outra etiqueta, prima a tecla Tab.

**[Vá para a Galeria](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
