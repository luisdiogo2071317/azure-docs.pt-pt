---
title: 'Pesquisa do Bing personalizada: Criar uma página web de pesquisa personalizada | Microsoft Docs'
description: Descreve como configurar uma instância de pesquisa personalizada e integrá-lo para uma página web
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/16/2017
ms.author: v-brapel
ms.openlocfilehash: c1431ec852cab943e00d3933ef4f0500a4fdb151
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352897"
---
# <a name="build-a-custom-search-web-page"></a>Criar uma página web de pesquisa personalizada
Pesquisa do Bing personalizada permite-lhe criar experiências de pesquisa personalizáveis para tópicos que mais lhe interessam. Por exemplo, se tiver um site de martial arts que fornece uma experiência de pesquisa, pode especificar os domínios, subsites e páginas Web que pesquisa do Bing. Os seus utilizadores verão os resultados da pesquisa adaptados para o conteúdo que mais importantes para si em vez de paginação percorrer os resultados de pesquisa gerais que podem conter conteúdo irrelevantes. 

Este tutorial demonstra como configurar uma instância de pesquisa personalizada e integrá-lo para uma nova página web.

As tarefas abrangidas são:

> [!div class="checklist"]
> - Criar uma instância de pesquisa personalizada
> - Adicionar entradas de Active Directory
> - Adicionar entradas bloqueadas
> - Adicionar entradas afixadas
> - Integrar pesquisa personalizada uma página web

## <a name="prerequisites"></a>Pré-requisitos
- Para seguir o tutorial, precisa de uma chave de subscrição para a API de pesquisa do Bing personalizada.  Para obter uma chave, consulte [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
- Se ainda não tiver o Visual Studio 2017 instalado, pode transferir e utilizar a [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) **gratuita**.

## <a name="create-a-custom-search-instance"></a>Criar uma instância de pesquisa personalizada
Para criar uma instância de pesquisa do Bing personalizada:

1.  Abra um browser da internet.
2.  Navegue para a pesquisa personalizada [portal](https://customsearch.ai).
3.  Inicie sessão no portal utilizando uma conta Microsoft (MSA). Se não tiver uma MSA, clique em **criar uma conta Microsoft**. Se for a primeira vez utilizando o portal, irá perguntar-permissões aceder aos seus dados. Clique em **Sim**.
4.  Depois de iniciarem sessão, clique em **nova pesquisa personalizada**. No **criar uma nova instância de pesquisa personalizada** janela, introduza um nome que faça sentido e descreve o tipo de conteúdo devolve a pesquisa. Pode alterar o nome em qualquer altura.
 
    ![Captura de ecrã de criar uma nova caixa de instância de pesquisa personalizada](../media/newCustomSrch.png)

5.  Clique em OK, especifique um URL e se pretende incluir subpages da página base:

    ![Captura de ecrã da página de definição do URL](../media/newCustomSrch1-a.png)

## <a name="add-active-entries"></a>Adicionar entradas de Active Directory
Para incluir resultados de URLs ou sites específicos, adicione-os para o **Active Directory** separador.

1.  No **definição Editor**, clique em de **Active Directory** separador e introduza o URL de um ou mais sites que pretende incluir na sua pesquisa.

    ![Captura de ecrã do separador de Active Directory do Editor de definição](../media/customSrchEditor.png)

2.  Para confirmar que a sua instância devolve resultados, introduza uma consulta no painel de pré-visualização no lado direito. Bing devolve resultados apenas para os sites públicas que tem indexada.

## <a name="add-blocked-entries"></a>Adicionar entradas bloqueadas
Para excluir resultados da sites específicos ou URLs, adicioná-los para o **bloqueado** separador.

1. No **definição Editor**, clique em de **bloqueado** separador e introduza o URL de um ou mais sites que pretende excluir da sua pesquisa.

    ![Captura de ecrã do Editor de definição bloqueado separador](../media/blockedCustomSrch.png)


2. Para confirmar que a instância não devolve resultados dos sites bloqueados, introduza uma consulta no painel de pré-visualização no lado direito. 

## <a name="add-pinned-entries"></a>Adicionar entradas afixadas
Para afixar uma página Web específica na parte superior dos resultados da pesquisa, adicione o termo de consulta e uma página Web para o **Pinned** separador.  O **Pinned** separador contém uma lista de pares de termo página Web e a consulta que especificam a página Web que é apresentado como o resultado superior para uma consulta específica. O termo de consulta do utilizador têm de corresponder exatamente o termo de consulta afixado para a página Web ser afixado na parte superior.

1. No **definição Editor**, clique em de **Pinned** separador e introduza o termo de consulta e uma página Web de página Web que deve ser devolvido como o resultado superior.

    ![Captura de ecrã do Editor de definição afixado separador](../media/pinnedCustomSrch.png)

2. Para confirmar que a instância devolve a página Web especificada como o resultado superior, introduza o termo de consulta que afixou no painel de pré-visualização no lado direito.

## <a name="configure-hosted-ui"></a>Configurar IU alojada
Pesquisa personalizada fornece uma IU alojada compor a resposta JSON da sua instância de pesquisa personalizada.  Para definir a sua experiência de IU:

1. Clique em de **alojado IU** separador.
2. Selecione um esquema.

    ![Captura de ecrã da IU alojado selecione o passo de esquema](./media/custom-search-hosted-ui-select-layout.png)

3. Selecione um tema de cor.

    ![Captura de ecrã da IU alojado selecione o passo de esquema](./media/custom-search-hosted-ui-select-color-theme.png)

4. Especificar opções de configuração adicionais.

    ![Captura de ecrã do passo de configurações adicionais IU alojado](./media/custom-search-hosted-ui-additional-configurations.png)

5. Colar o **chave de subscrição**. Consulte [experimentar serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).

    ![Captura de ecrã do passo de configurações adicionais IU alojado](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE[publish or revert](../includes/publish-revert.md)]

<a name="consuminghostedui"></a>
## <a name="consuming-hosted-ui"></a>Consumo de IU alojada
Existem duas formas de consumir a IU alojada.  

- Opção 1: Integre o fragmento JavaScript fornecido na sua aplicação.
- Opção 2: Utilize o ponto final de HTML fornecido.

O resto deste tutorial ilustra **opção 1: fragmento do Javascript**.  

## <a name="set-up-your-visual-studio-solution"></a>Configurar a sua solução Visual Studio
1. Abra o **Visual Studio** no seu computador.
2. No menu **Ficheiro**, selecione **Novo**, e, em seguida, escolha **Projeto**.
3. No **novo projeto** janela, selecione **Visual c# / Web / aplicação Web do ASP.NET Core**, nomeie o projeto e, em seguida, clique em **OK**.
   
    ![Captura de ecrã da janela do novo projeto](./media/custom-search-new-project.png)

4. No **nova aplicação de Web de ASP.NET Core** janela, selecione **aplicação Web** e clique em **OK**.
    
    ![Captura de ecrã da janela do novo projeto](./media/custom-search-new-webapp.png)

## <a name="edit-indexcshtml"></a>Editar Index. cshtml
1. No **Explorador de soluções**, expanda **páginas** e faça duplo clique **Index. cshtml** para abrir o ficheiro.

    ![Captura de ecrã do Explorador de soluções com páginas expandidas e Index. cshtml selecionado](./media/custom-search-visual-studio-webapp-solution-explorer-index.png)

2. No Index. cshtml, elimine tudo a partir da linha 7 e abaixo.
    
    ``` razor
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }    
    ```

3. Adicione um elemento de quebra de linha e um div para atuar como um contentor.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch"></div>
    ```

4. Do **alojado IU** separador, vá para a secção intitulada **consumir a IU**. Copie o fragmento do JavaScript.

    ![Captura de ecrã da IU alojado botão Guardar](./media/custom-search-hosted-ui-consuming-ui.png)

5. Cole o elemento de script para o contentor que adicionou.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch">
        <script type="text/javascript"
                id="bcs_js_snippet"
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">
        </script>
    </div>
    ```

6. No **Explorador de soluções**, clique em **wwwroot** e clique em **vista no Browser**.

    ![Captura de ecrã do Explorador de soluções, selecionando a vista no Browser a partir do menu de contexto wwwroot](./media/custom-search-webapp-view-in-browser.png)

A nova página web de pesquisa personalizada deve ter um aspeto semelhante a isto:

![Captura de ecrã da página web de pesquisa personalizada](./media/custom-search-webapp-browse-index.png)

Realizar uma pesquisa compõe resultados como esta.

![Captura de ecrã dos resultados da pesquisa personalizada](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, fez o seguinte:

> [!div class="checklist"]
> - Criou uma instância de pesquisa personalizada
> - Foram adicionadas entradas de Active Directory
> - Foram adicionadas entradas bloqueadas
> - Foram adicionadas entradas afixadas
> - Pesquisa integrada personalizada para uma página web

Agora pode avançar para chamar pontos finais de pesquisa do Bing personalizada através de programação.

> [!div class="nextstepaction"]
> [Ponto final de pesquisa do Bing personalizada de chamada (c#)](../call-endpoint-csharp.md)