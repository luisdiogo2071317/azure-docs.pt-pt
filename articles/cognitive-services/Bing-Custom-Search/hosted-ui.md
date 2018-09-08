---
title: 'De pesquisa personalizada do Bing: Pesquisa do Site | Documentos da Microsoft'
description: Descreve como configurar a interface do Usuário alojado
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 7f2b97479ffcdb7ec8b3a1a635562d1fe68c3269
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158429"
---
# <a name="configure-your-hosted-ui-experience"></a>Configurar a sua experiência de interface do Usuário alojada
Depois de configurar a sua instância de pesquisa personalizada, pode chamar a API de pesquisa personalizada para obter os resultados da pesquisa e exibi-las na sua aplicação. Em alternativa, se a aplicação for uma aplicação web, pode usar a interface do Usuário alojado que fornece pesquisa personalizada.   

## <a name="configure-custom-hosted-ui"></a>Configurar a interface do Usuário de hospedado personalizado
Utilize as seguintes instruções para configurar uma interface do Usuário alojado a incluir na sua aplicação web.
1.  Entrar para a pesquisa personalizada [portal](https://customsearch.ai).
2.  Clique numa instância de pesquisa personalizada. Para criar uma instância, veja [criar a primeira instância de pesquisa personalizada do Bing](quick-start.md).
3.  Clique nas **alojada a interface do Usuário** separador.
4.  Selecione um esquema.
    - Barra de pesquisa e os resultados (predefinição) &mdash; exibir uma caixa de pesquisa e os resultados da pesquisa
    - Os resultados apenas &mdash; não exibir uma caixa de pesquisa, mostrar apenas os resultados
    - A ativação pós-falha POP &mdash; não exibir uma caixa de pesquisa, mostrar apenas os resultados numa sobreposição deslizante
    
   > [!IMPORTANT]
   > Certifique-se de que inclua o parâmetro de consulta de customConfig ao selecionar o **resulta só** layout, consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).

5.  Sob **configurações adicionais**, forneça os valores conforme adequado para a sua aplicação. Estas definições são opcionais. Para ver o efeito de aplicar ou removê-los, consulte o painel de visualização no lado direito.  Opções de configuração disponíveis são:
    - Configurações de pesquisa Web:
        - Os resultados ativada da Web &mdash; determina se os resultados de pesquisa na web são devolvidos
        - Ativar sugestão automática &mdash; determina se a sugestão automática personalizada está ativada
        - Resultados por página da Web &mdash; número de resultados de pesquisa web a apresentar ao mesmo tempo
        - Legenda da imagem deve &mdash; determina se as imagens são apresentadas com os resultados da pesquisa
        - Realçar palavras &mdash; determina se os resultados são apresentados com os termos de pesquisa em negrito
    - Configurações de pesquisa de imagem:
        - Os resultados ativada de imagem &mdash; determina se os resultados da pesquisa de imagem são devolvidos
    - Diversas configurações:
        - Título da página &mdash; texto apresentado na área de título da página
        - Tema de barra de ferramentas &mdash; determina a cor de fundo da área de título da página
        - Marcador de posição de texto de caixa de pesquisa &mdash; texto apresentado na antes de caixa de pesquisa de entrada
        - Url de ligação do título &mdash; destino para a ligação de título
        - Url de logótipo &mdash; imagem exibida ao lado do título 
        - Url de Favicon &mdash; ícone apresentado na barra de título do browser

   > [!IMPORTANT]
   > Pelo menos um de pesquisa de imagens ou de pesquisa na Web que tem de estar ativado.

6.  Introduza a chave de subscrição de pesquisa ou escolha um na lista pendente. Na lista pendente é preenchida com chaves de subscrições do Azure da sua conta. Ver [conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
7.  Se ativou a sugestão automática, introduza a chave de subscrição de sugestão automática ou escolha um na lista pendente. Na lista pendente é preenchida com chaves de subscrições do Azure da sua conta. Sugestão automática personalizada requer um escalão de subscrição specifiec, consulte a [páginas de preços](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Como efetuar alterações à configuração de interface do Usuário hospedada personalizada, o painel à direita fornece uma referência visual para que as alterações feitas. Os resultados da pesquisa exibidos não são os resultados reais da sua instância

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Consumir a interface do Usuário personalizada
Para consumir a interface do Usuário alojado, optar por: 

- Inclua o script na página da web
    ``` html
    <html>
        <body>
            <script type="text/javascript"
                id="bcs_js_snippet"            
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">            
            </script>
        </body>    
    </html>
    ```

- Utilize o URL fornecido `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`

  > [!IMPORTANT]
  > A página não é possível apresentar a sua declaração de privacidade ou outros avisos e os termos. Pode variar de adequação para a sua utilização.  

Para obter mais informações, incluindo o seu ID de configuração personalizada, aceda a **pontos de extremidade** sob a **produção** separador.

## <a name="next-steps"></a>Passos Seguintes
- [Utilizar marcadores decoration para realçar o texto](./hit-highlighting.md)
- [Páginas Web de página](./page-webpages.md)