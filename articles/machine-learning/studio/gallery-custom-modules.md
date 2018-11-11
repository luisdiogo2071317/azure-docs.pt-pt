---
title: Módulos personalizados de Galeria de IA do Azure | Documentos da Microsoft
description: Descubra os módulos de aprendizagem automática personalizada na Galeria de IA do Azure.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 16037a84-dad0-4a8c-9874-a1d3bd551cf0
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.openlocfilehash: fe58b335a029e558ce29871483589908f17c03cd
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/09/2018
ms.locfileid: "51345696"
---
# <a name="discover-custom-machine-learning-modules-in-azure-ai-gallery"></a>Descubra os módulos de aprendizagem automática personalizada na Galeria de IA do Azure

[Galeria de IA do Azure](http://gallery.cortanaintelligence.com) oferece várias [módulos personalizados](https://gallery.cortanaintelligence.com/customModules) que expandem os recursos do Azure Machine Learning Studio. Pode importar os módulos para usar em suas experimentações, portanto pode desenvolver soluções de Análise Preditiva ainda mais avançadas.

Atualmente, a Galeria oferece módulos no *análises de séries de tempo*, *regras de associação*, *algoritmos de clustering* (além do k-means), e  *visualizações*e outros módulos de utilitário do trabalho pesado.


## <a name="discover"></a>Descobrir
Para procurar módulos personalizados [na galeria](http://gallery.cortanaintelligence.com), em **mais**, selecione **módulos personalizados**.

![Selecione módulos personalizados na home page da Galeria](./media/gallery-custom-modules/select-custom-modules-in-gallery.png)

O **[módulos personalizados](https://gallery.cortanaintelligence.com/customModules)** página apresenta uma lista de módulos recentemente adicionados e populares. Para ver todos os módulos personalizados, selecione o **ver todas as** botão. Para pesquisar por um módulo personalizado específico, selecione **ver todas as**e os critérios de filtro, em seguida, selecione. Também pode introduzir termos da procura na **pesquisa** caixa na parte superior da página da galeria.

![Selecione "ver todos os" para procurar todos os módulos personalizados](./media/gallery-custom-modules/click-see-all-for-all-custom-modules.png)

### <a name="understand"></a>Compreender

Para compreender como funciona um módulo personalizado publicado, selecione o módulo personalizado para abrir a página de detalhes do módulo. A página de detalhes oferece uma experiência de aprendizado informativo e consistente. Por exemplo, a página de detalhes realça a finalidade do módulo e lista esperadas de entradas, saídas e parâmetros. A página de detalhes também tem uma ligação para o código de origem subjacente, o que pode examinar e personalizar.

### <a name="comment-and-share"></a>Comentário e partilha
Num página de detalhes, além de módulo personalizado a **comentários** seção, pode comentar, fornecer comentários ou fazer perguntas sobre o módulo. Pode até compartilhar o módulo com amigos ou colegas no Twitter ou LinkedIn. Também pode enviar um e-mail uma ligação para a página de detalhes do módulo, convidar outros utilizadores para ver a página.

![Compartilhar este item com amigos](./media/gallery-how-to-use-contribute-publish/share-links.png)

![Adicione os seus próprios comentários](./media/gallery-how-to-use-contribute-publish/comments.png)

## <a name="import"></a>Importar
Pode importar qualquer módulo personalizado a partir da Galeria para suas próprias experiências.

Galeria de IA do Azure oferece duas formas de importar uma cópia do módulo:

* **Na galeria do**. Quando importa um módulo personalizado a partir da galeria, também obtém uma experimentação de exemplo que lhe dá um exemplo de como utilizar o módulo.
* **De dentro do Machine Learning Studio**. Pode importar qualquer módulo personalizado enquanto está a trabalhar no Machine Learning Studio (neste caso, não obtém a experimentação de exemplo).

### <a name="from-the-gallery"></a>Partir da Galeria

1. Na galeria, abra a página de detalhes do módulo. 
2. Selecione **abrir no Studio**.
   
    ![Módulo personalizado aberto da Galeria](./media/gallery-custom-modules/open-custom-module-from-gallery.png)
   
Cada módulo personalizado inclui uma experimentação de exemplo que demonstra como utilizar o módulo. Quando seleciona **abrir no Studio**, a experimentação de exemplo é aberto na sua área de trabalho do Machine Learning Studio. (Se ainda não tem sessão iniciada Studio, deve primeiro início de sessão utilizando a sua conta Microsoft.)

Para além da experimentação de exemplo, o módulo personalizado é copiado para a área de trabalho. Também é colocada na sua paleta do módulo, com todos os seus internas ou personalizadas Machine Learning Studio módulos. Agora pode usá-lo em suas próprias experiências, como qualquer outro módulo na sua área de trabalho.

### <a name="from-within-machine-learning-studio"></a>De dentro do Machine Learning Studio

1. No Machine Learning Studio, selecione **NEW**.
2. Selecione **módulo**. Pode escolher de entre uma lista de módulos da galeria ou localizar um módulo específico utilizando o **pesquisa** caixa.
3. Apontar o mouse para um módulo e, em seguida, selecione **Import Module**. (Para obter informações sobre o módulo, selecione **vista na galeria**. Isto leva-o para a página de detalhes do módulo na galeria.)
   
    ![Importar o módulo personalizado para o Machine Learning Studio](./media/gallery-custom-modules/add-custom-module-in-studio.png)

O módulo personalizado é copiado para a área de trabalho e colocado na sua paleta do módulo, com os módulos de Machine Learning Studio internas ou personalizadas. Agora pode usá-lo em suas próprias experiências, como qualquer outro módulo na sua área de trabalho.

## <a name="use"></a>Utilizar

Independentemente do método escolhido para importar um módulo personalizado, ao importar o módulo, o módulo é colocado na sua paleta do módulo no Machine Learning Studio. Da paleta do módulo, pode usar o módulo personalizado em qualquer experiência na área de trabalho, tal como qualquer outro módulo.

Para utilizar um módulo importado:

1. Criar uma experimentação ou abrir uma experimentação existente.
2. Para expandir a lista de módulos personalizados na sua área de trabalho, na paleta do módulo, selecione **personalizado**. É a paleta do módulo para a esquerda da tela de experimentação.
   
    ![Lista de módulo personalizado na paleta do Studio](./media/gallery-custom-modules/custom-module-in-studio-palette.png)
3. Selecione o módulo que importou e arraste-o à sua experimentação.


**[Vá para a Galeria](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

