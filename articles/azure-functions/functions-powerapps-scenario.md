---
title: "Chamar uma função a partir do PowerApps | Microsoft Docs"
description: "Criar um conetor personalizado e chamar uma função utilizando esse conector."
services: functions
keywords: "aplicações em nuvem, na nuvem processos de negócio de serviços, PowerApps, aplicação empresarial"
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: mblythe
ms.custom: 
ms.openlocfilehash: 28c2fc8246851807e1f65911d6a5d56322c5ea16
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2017
---
# <a name="call-a-function-from-powerapps"></a>Chamar uma função a partir do PowerApps
O [PowerApps](https://powerapps.microsoft.com) plataforma foi concebida para os especialistas em negócio criar aplicações sem o código de aplicação tradicionais. Programadores profissionais podem utilizar as funções do Azure para aumentar as capacidades do PowerApps, enquanto efetua a blindagem PowerApps construtores de aplicação dos detalhes técnicos.

Criar uma aplicação neste tópico com base num cenário de manutenção para turbines vento. Este tópico mostra como chamar a função que definiu no [criar uma definição de OpenAPI para uma função](functions-openapi-definition.md). A função determina se uma reparação de emergência num turbine vento económica.

![Aplicação concluída no PowerApps](media/functions-powerapps-scenario/finished-app.png)

Para obter informações sobre a chamar a mesma função a partir de Microsoft Flow, consulte [chamar uma função do Microsoft Flow](functions-flow-scenario.md).

Neste tópico, saiba como:

> [!div class="checklist"]
> * Prepare os dados de exemplo no Excel.
> * Exporte uma definição de API.
> * Adicione uma ligação para a API.
> * Criar uma aplicação e adicione as origens de dados.
> * Adicione controlos para ver os dados na aplicação.
> * Adicione controlos para chamar a função e apresentar dados.
> * Execute a aplicação para determinar se a reparação é económica.

## <a name="prerequisites"></a>Pré-requisitos

+ Um Active Directory [PowerApps conta](https://powerapps.microsoft.com/tutorials/signup-for-powerapps.md) com o início de sessão mesmo as credenciais de início como a sua conta do Azure. 
+ Excel e o [ficheiro de exemplo do Excel](https://procsi.blob.core.windows.net/docs/turbine-data.xlsx) que irá utilizar como origem de dados para a sua aplicação.
+ Concluir o tutorial [criar uma definição de OpenAPI para uma função](functions-openapi-definition.md).

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>Adicionar uma ligação para a API
A API personalizada (também conhecido como um conetor personalizado) está disponível no PowerApps, mas tem de estabelecer uma ligação para a API para poder utilizá-la numa aplicação.

1. No [web.powerapps.com](https://web.powerapps.com), clique em **ligações**.

    ![Ligações de PowerApps](media/functions-powerapps-scenario/powerapps-connections.png)

1. Clique em **nova ligação**, desloque para baixo até o **Turbine reparação** conector e clique no mesmo.

    ![Nova ligação](media/functions-powerapps-scenario/new-connection.png)

1. Introduza a chave de API e clique em **criar**.

    ![Criar a ligação](media/functions-powerapps-scenario/create-connection.png)

> [!NOTE]
> Se partilhar a aplicação com outras pessoas, cada pessoa que funciona na ou utiliza a aplicação também tem de introduzir a chave de API para ligar à API. Este comportamento pode ser alterado no futuro e, neste tópico para refletir que iremos atualizar.

## <a name="create-an-app-and-add-data-sources"></a>Criar uma aplicação e adicione as origens de dados
Agora está pronto para criar a aplicação no PowerApps e adicionar os dados do Excel e a API personalizada como origens de dados para a aplicação.

1. No [web.powerapps.com](https://web.powerapps.com), escolha **início de em branco** > ![ícone da aplicação de telefone](media/functions-powerapps-scenario/icon-phone-app.png) (telemóvel) > **tornar esta aplicação**.

    ![Inicie a partir em branco - a aplicação de telefone](media/functions-powerapps-scenario/create-phone-app.png)

    Abre a aplicação no PowerApps Studio para o web. A imagem seguinte mostra as diferentes partes da PowerApps Studio.

    ![PowerApps Studio](media/functions-powerapps-scenario/powerapps-studio.png)

    **(A) barra de navegação à esquerda**, onde ver uma vista hierárquica dos controlos em cada ecrã

    **(B) painel meio**, que mostra o ecrã se estiver a funcionar no

    **Painel à direita (C)**, onde definir opções, como o esquema e origens de dados

    **(D) propriedade** na lista pendente, onde poderá selecionar as propriedades que se aplicam as fórmulas para

    **(E) barra fórmulas**, onde são adicionados fórmulas (tal como no Excel) que definem o comportamento da aplicação
    
    **(F) friso**, onde o utilizador adicionar controlos e personalizar os elementos do design

1. Adicione o ficheiro do Excel como uma origem de dados.

    Os dados que irá importar tem o seguinte aspeto:

    ![Para importar dados do Excel](media/functions-powerapps-scenario/excel-table.png)

    1. Na tela aplicação, escolha **ligar aos dados**.

    1. No **dados** painel, clique em **adicionar dados estáticos para a sua aplicação**.

        ![Adicionar origem de dados](media/functions-powerapps-scenario/add-static-data.png)

        Normalmente, teria de ler e escrever dados a partir de uma origem externa, mas estiver a adicionar os dados do Excel como dados estáticos porque este é um exemplo.

    1. Navegue para o ficheiro do Excel guardou, selecione o **Turbines** tabela e clique em **Connect**.

        ![Adicionar origem de dados](media/functions-powerapps-scenario/choose-table.png)


1. Adicione a API personalizada como uma origem de dados.

    1. No **dados** painel, clique em **Adicionar origem de dados**.

    1. Clique em **Turbine reparação**.

        ![Conector de reparação Turbine](media/functions-powerapps-scenario/turbine-connector.png)

## <a name="add-controls-to-view-data-in-the-app"></a>Adicionar controlos para ver os dados na aplicação
Agora que as origens de dados estão disponíveis na aplicação, adicionar um ecrã para a sua aplicação, para que possa visualizar os dados de turbine.

1. No **home page** separador, clique em **novo ecrã** > **ecrã lista**.

    ![Ecrã de lista](media/functions-powerapps-scenario/list-screen.png)

    PowerApps adiciona um ecrã que contém um *galeria* para apresentar os itens e outros controlos que permitem a procurar, ordenar e filtrar.

1. Alterar a barra de título para `Turbine Repair`e redimensione a galeria, pelo que não existe espaço para controlos mais sob a mesma.

    ![Alterar o título e redimensione da Galeria](media/functions-powerapps-scenario/gallery-title.png)

1. Com a Galeria selecionada no painel direito, em **propriedades**, clique em **CustomGallerySample**.

    ![Origem de dados de alteração](media/functions-powerapps-scenario/change-data-source.png)

1. No **dados** painel, selecione **Turbines** da lista.

    ![Selecione a origem de dados](media/functions-powerapps-scenario/select-data-source.png)

    O conjunto de dados não contém uma imagem, por isso, em seguida, alterar o esquema para caber melhor os dados. 

1. Ainda no **dados** painel, altere **esquema** para **título, subtítulo do e corpo**.

    ![Esquema de Galeria de alteração](media/functions-powerapps-scenario/change-layout.png)

1. Como o último passo do **dados** painel, altere os campos que são apresentados na galeria.

    ![Altere os campos de galeria](media/functions-powerapps-scenario/change-fields.png)
    
    + **Body1** = LastServiceDate
    + **Subtitle2** = ServiceRequired
    + **Title2** = título 

1. Com a Galeria selecionada, defina o **TemplateFill** propriedade para a seguinte fórmula: `If(ThisItem.IsSelected, Orange, White)`.

    ![Fórmula de preenchimento do modelo](media/functions-powerapps-scenario/formula-fill.png)

    Agora é mais fácil ver qual o item da galeria é selecionado.

    ![Item selecionado](media/functions-powerapps-scenario/selected-item.png)

1. Não precisa do ecrã original na aplicação. No painel esquerdo, coloque o cursor sobre **Screen1**, clique em **...** , e **eliminar**.

    ![Eliminar ecrã](media/functions-powerapps-scenario/delete-screen.png)

1. Clique em **ficheiro**e atribua o nome da aplicação. Clique em **guardar** no menu da esquerda, em seguida, clique em **guardar** no canto inferior direito.

Há muitos outros formatação que normalmente iria fazer numa aplicação de produção, mas irá mover para o papel importante para este cenário - chamar a função.

## <a name="add-controls-to-call-the-function-and-display-data"></a>Adicionar controlos para chamar a função e visualizar dados
Tiver uma aplicação que apresenta dados de resumo para cada turbine, agora está na altura de adicionar controla que chamar a função que criou e apresentar os dados que são devolvidos. Aceder a função com base na forma que o nome na definição do OpenAPI; Neste caso, tem `TurbineRepair.CalculateCosts()`.

1. No Friso, no **inserir** separador, clique em **botão**. Em seguida, no mesmo separador, clique em **etiqueta**

    ![Botão para inserir e a etiqueta](media/functions-powerapps-scenario/insert-controls.png)

1. Arraste o botão e a etiqueta abaixo na galeria e redimensionar a etiqueta. 

1. Selecione o texto do botão e altere-o para `Calculate costs`. A aplicação deverá ser semelhante a imagem seguinte.

    ![Aplicação com o botão](media/functions-powerapps-scenario/move-button-label.png)

1. Selecione o botão e introduza a seguinte fórmula para o botão **OnSelect** propriedade.

    ```
    If (BrowseGallery1.Selected.ServiceRequired="Yes", ClearCollect(DetermineRepair, TurbineRepair.CalculateCosts({hours: BrowseGallery1.Selected.EstimatedEffort, capacity: BrowseGallery1.Selected.MaxOutput})))
    ```
    Esta fórmula executa quando o botão é clicado e faz o seguinte se o item da Galeria selecionada tiver uma **ServiceRequired** valor `Yes`:

    + Limpa o *coleção* `DetermineRepair` para remover dados de chamadas anteriores. Uma coleção é uma variável de tabela.

    + Atribui à coleção de dados devolvidos ao chamar a função `TurbineRepair.CalculateCosts()`. 
    
        Os valores transmitidos para a função provenientes de **EstimatedEffort** e **MaxOutput** campos para o item selecionado na galeria. Estes campos não são apresentados na galeria, mas estão ainda disponível para utilização em fórmulas.

1. Selecione a etiqueta e introduza a seguinte fórmula para a etiqueta **texto** propriedade.

    ```
    "Repair decision: " & First(DetermineRepair).message & " | Cost: " & First(DetermineRepair).costToFix & " | Revenue: " & First(DetermineRepair).revenueOpportunity
    ```
    Utiliza esta fórmula o `First()` função para aceder a linha primeiro (e apenas) da `DetermineRepair` coleção. Em seguida, apresenta os três valores que a função devolve: `message`, `costToFix`, e `revenueOpportunity`. Estes valores são em branco antes da aplicação é executada pela primeira vez.

    A aplicação concluída deverá ser semelhante a imagem seguinte.

    ![Aplicação concluída antes de executar](media/functions-powerapps-scenario/finished-app-before-run.png)


## <a name="run-the-app"></a>Executar a aplicação
Tiver uma aplicação concluída! Agora está na altura de executá-lo e ver que a função chama-se em ação.

1. No canto superior direito do PowerApps Studio, clique no botão executar: ![Botão de execução da aplicação](media/functions-powerapps-scenario/f5-arrow-sm.png).

1. Selecione um turbine com um valor de `Yes` para **ServiceRequired**, em seguida, clique o **calcular os custos** botão. Deverá ver um resultado como a imagem seguinte.

    ![Aplicação concluída no PowerApps](media/functions-powerapps-scenario/finished-app.png)

1. Tente outros turbines para ver o que é devolvido pela função de cada vez.

## <a name="next-steps"></a>Passos Seguintes
Neste tópico, aprendeu como:

> [!div class="checklist"]
> * Prepare os dados de exemplo no Excel.
> * Exporte uma definição de API.
> * Adicione uma ligação para a API.
> * Criar uma aplicação e adicione as origens de dados.
> * Adicione controlos para ver os dados na aplicação.
> * Adicionar controlos para chamar a função e visualizar dados
> * Execute a aplicação para determinar se a reparação é económica.

Para saber mais sobre PowerApps, consulte o artigo [introdução às PowerApps](https://powerapps.microsoft.com/tutorials/getting-started/).

Para mais informações sobre outros cenários interessantes que utilizam as funções do Azure, consulte o artigo [chamar uma função do Microsoft Flow](functions-flow-scenario.md) e [criar uma função que se integra com Azure Logic Apps](functions-twitter-email.md).