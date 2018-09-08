---
title: 'A pesquisa personalizada do Bing: Criar uma página da web de pesquisa personalizada | Documentos da Microsoft'
description: Descreve como configurar uma instância de pesquisa personalizada e integrá-la numa página da web
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/16/2017
ms.author: v-brapel
ms.openlocfilehash: 1f9b689ac6127bc2f7d1e810356ae9a23b8e0996
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162398"
---
# <a name="build-a-custom-search-web-page"></a>Criar uma página Web de Pesquisa Personalizada
Pesquisa personalizada do Bing permite-lhe criar experiências de pesquisa personalizadas para tópicos que mais lhe interessa. Por exemplo, se for proprietário de um site de artes marciais que fornece uma experiência de pesquisa, pode especificar os domínios, subsites e páginas da Web que pesquisa do Bing. Os utilizadores veem resultados de pesquisa personalizados para o conteúdo que ele se preocupa em vez de paginação por meio de resultados da pesquisa gerais que podem conter conteúdo irrelevante. 

Este tutorial demonstra como configurar uma instância de pesquisa personalizada e integrá-lo numa nova página web.

As tarefas abrangidas são:

> [!div class="checklist"]
> - Criar uma instância de pesquisa personalizada
> - Adicionar entradas do Active Directory
> - Adicionar entradas bloqueadas
> - Adicionar entradas afixadas
> - Integre a pesquisa personalizada uma página da web

## <a name="prerequisites"></a>Pré-requisitos
- Para acompanhar o tutorial, precisa de uma chave de subscrição para a API de pesquisa personalizada do Bing.  Para obter uma chave, consulte [experimentar os serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
- Se ainda não tiver o Visual Studio 2017 instalado, pode transferir e utilizar a [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) **gratuita**.

## <a name="create-a-custom-search-instance"></a>Criar uma instância de pesquisa personalizada
Para criar uma instância de pesquisa personalizada do Bing:

1.  Abra um browser da internet.
2.  Navegue para a pesquisa personalizada [portal](https://customsearch.ai).
3.  Inicie sessão no portal com uma conta Microsoft (MSA). Se não tiver uma MSA, clique em **criar uma conta Microsoft**. Se for a primeira vez com o portal, é-lhe perguntado para permissões para aceder aos seus dados. Clique em **Sim**.
4.  Depois de iniciar sessão, clique em **nova pesquisa personalizada**. Na **criar uma nova instância de pesquisa personalizada** janela, introduza um nome que é significativo e descreve o tipo de conteúdo a pesquisa retorna. Pode alterar o nome em qualquer altura.
 
    ![Captura de ecrã de criar uma nova caixa de instância de pesquisa personalizada](../media/newCustomSrch.png)

5.  Clique em OK, especifique um URL e se pretende incluir subpáginas da página base:

    ![Captura de ecrã da página de definição de URL](../media/newCustomSrch1-a.png)

## <a name="add-active-entries"></a>Adicionar entradas do Active Directory
Para incluir os resultados de sites específicos ou URLs, adicione-os para o **Active Directory** separador.

1.  Na **Editor de definição**, clique no **Active Directory** separador e introduza o URL de um ou mais sites que pretende incluir na sua pesquisa.

    ![Captura de ecrã do separador ativo do Editor de definição](../media/customSrchEditor.png)

2.  Para confirmar que a sua instância retorna resultados, introduza uma consulta no painel de pré-visualização no lado direito. O Bing devolve resultados apenas para sites públicos, que tem indexados.

## <a name="add-blocked-entries"></a>Adicionar entradas bloqueadas
Para excluir resultados de sites específicos ou URLs, adicione-os para o **bloqueado** separador.

1. Na **Editor de definição**, clique nas **bloqueado** separador e introduza o URL de um ou mais sites que pretende excluir da sua pesquisa.

    ![Captura de ecrã do Editor de definição bloqueado separador](../media/blockedCustomSrch.png)


2. Para confirmar que a instância não retorna os resultados dos sites bloqueados, introduza uma consulta no painel de pré-visualização no lado direito. 

## <a name="add-pinned-entries"></a>Adicionar entradas afixadas
Para afixar uma página Web específica na parte superior dos resultados da pesquisa, adicionar o termo de consulta e de página Web para o **Pinned** separador.  O **Pinned** separador contém uma lista de pares de termo página da Web e de consulta que especificam a página Web que é apresentado como o melhor resultado para uma consulta específica. Termo de consulta do utilizador tem de corresponder exatamente ao termo de consulta afixado para a página Web ser afixado à parte superior.

1. Na **Editor de definição**, clique nas **Pinned** separador e introduza o termo de consulta e de página Web de página Web que deve ser devolvido como o melhor resultado.

    ![Captura de ecrã do Editor de definição afixado separador](../media/pinnedCustomSrch.png)

2. Para confirmar que a sua instância retorna a página de Web especificada como o melhor resultado, introduza o termo de consulta que tiver afixado no painel de pré-visualização no lado direito.

## <a name="configure-hosted-ui"></a>Configurar a interface do Usuário alojado
Pesquisa personalizada fornece uma interface do Usuário alojado para processar a resposta JSON da sua instância de pesquisa personalizada.  Para definir a sua experiência de interface do Usuário:

1. Clique nas **alojada a interface do Usuário** separador.
2. Selecione um esquema.

    ![Captura de ecrã da interface de Usuário alojado selecione o passo de layout](./media/custom-search-hosted-ui-select-layout.png)

3. Selecione um tema de cores.

    ![Captura de ecrã da interface de Usuário alojado selecione o passo de layout](./media/custom-search-hosted-ui-select-color-theme.png)

4. Especifica opções de configuração adicionais.

    ![Captura de ecrã do passo de configurações adicionais da interface do Usuário alojado](./media/custom-search-hosted-ui-additional-configurations.png)

5. Colar seu **chave de subscrição**. Ver [experimente os serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).

    ![Captura de ecrã do passo de configurações adicionais da interface do Usuário alojado](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE [publish or revert](../includes/publish-revert.md)]

<a name="consuminghostedui"></a>
## <a name="consuming-hosted-ui"></a>Consumo da interface do Usuário alojado
Existem duas formas de consumir a interface do Usuário alojado.  

- Opção 1: Integre o fragmento do JavaScript fornecido na sua aplicação.
- Opção 2: Utilize o ponto de final do HTML fornecida.

O resto deste tutorial ilustra **opção 1: fragmento do Javascript**.  

## <a name="set-up-your-visual-studio-solution"></a>Configurar a sua solução Visual Studio
1. Abra o **Visual Studio** no seu computador.
2. No menu **Ficheiro**, selecione **Novo**, e, em seguida, escolha **Projeto**.
3. Na **novo projeto** janela, selecione **Visual c# / Web / aplicação Web ASP.NET Core**, nomeie o projeto e, em seguida, clique em **OK**.
   
    ![Captura de ecrã da janela novo projeto](./media/custom-search-new-project.png)

4. Na **novo aplicativo da Web ASP.NET Core** janela, selecione **aplicação Web** e clique em **OK**.
    
    ![Captura de ecrã da janela novo projeto](./media/custom-search-new-webapp.png)

## <a name="edit-indexcshtml"></a>Editar Index. cshtml
1. Na **Explorador de soluções**, expanda **páginas** e faça duplo clique em **Index. cshtml** para abrir o ficheiro.

    ![Captura de ecrã do Explorador de soluções com páginas expandidas e Index. cshtml selecionado](./media/custom-search-visual-studio-webapp-solution-explorer-index.png)

2. Em index. cshtml, elimine tudo a partir da linha 7 e abaixo.
    
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

4. Do **alojada a interface do Usuário** separador, desloque para baixo para a seção intitulada **consumindo a interface do Usuário**. Copie o fragmento do JavaScript.

    ![Botão de guardar a captura de ecrã da interface de Usuário alojado](./media/custom-search-hosted-ui-consuming-ui.png)

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

6. Na **Explorador de soluções**, clique com o botão direito do rato em **wwwroot** e clique em **exibir no navegador**.

    ![Captura de ecrã do Explorador de soluções, selecionar a vista no Browser do menu de contexto wwwroot](./media/custom-search-webapp-view-in-browser.png)

Sua nova página de web de pesquisa personalizada deve ser semelhante ao seguinte:

![Captura de ecrã da página da web de pesquisa personalizada](./media/custom-search-webapp-browse-index.png)

Realizar uma pesquisa processa resultados como este.

![Captura de ecrã dos resultados da pesquisa personalizada](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, fez o seguinte:

> [!div class="checklist"]
> - Criou uma instância de pesquisa personalizada
> - Entradas de Active Directory foi adicionadas
> - Adicionar entradas bloqueadas
> - Adicionar entradas afixadas
> - Pesquisa personalizada integrada numa página da web

Agora, pode avançar para chamar pontos finais de pesquisa personalizada do Bing através de programação.

> [!div class="nextstepaction"]
> [Chamar o ponto final de pesquisa personalizada do Bing (c#)](../call-endpoint-csharp.md)