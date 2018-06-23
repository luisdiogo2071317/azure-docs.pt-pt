---
title: 'Pesquisa do Bing personalizada: Pesquisa de Site | Microsoft Docs'
description: Descreve como configurar IU alojada
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 593ea4d23f8ddcec8efc4be632afa2aab1a5210f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352879"
---
# <a name="configure-your-hosted-ui-experience"></a>Configurar a sua experiência de IU alojada
Depois de configurar a instância de pesquisa personalizada, pode chamar a API de pesquisa personalizada para obter os resultados da pesquisa e apresentá-los na sua aplicação. Em alternativa, se a aplicação é uma aplicação web, pode utilizar a IU alojada que fornece pesquisa personalizada.   

## <a name="configure-custom-hosted-ui"></a>Configurar IU alojada personalizada
Utilize as seguintes instruções para configurar uma IU alojada a incluir na sua aplicação web.
1.  Inicie sessão na pesquisa personalizada [portal](https://customsearch.ai).
2.  Clique uma instância de pesquisa personalizada. Para criar uma instância, consulte [criar a primeira instância de pesquisa do Bing personalizada](quick-start.md).
3.  Clique em de **alojado IU** separador.
4.  Selecione um esquema.
    - Barra de pesquisa e resultados (predefinição) &mdash; apresentar uma caixa de pesquisa e resultados da pesquisa
    - Resulta apenas &mdash; não apresentar uma caixa de pesquisa, mostrar apenas os resultados
    - Ativação pós-falha POP &mdash; não apresentar uma caixa de pesquisa, mostrar apenas os resultados numa sobreposição deslizante
    
   > [!IMPORTANT]
   > Não se esqueça de incluir o parâmetro de consulta customConfig quando selecionar o **resulta apenas** esquema, consulte [parâmetros de consulta](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).

5.  Em **configurações adicionais**, indique os valores conforme adequado para a sua aplicação. Estas definições são opcionais. Para ver o efeito de aplicar ou removê-los, consulte o painel de pré-visualização no lado direito.  Opções de configuração disponíveis são:
    - Configurações de pesquisa da Web:
        - Web resultados ativados &mdash; determina se os resultados da pesquisa web são devolvidos
        - Ativar de sugestão automática &mdash; determina se a sugestão automática do personalizado está ativada
        - Web resultados por página &mdash; número de resultados de pesquisa de web a apresentar num momento
        - Legenda de imagem &mdash; determina se as imagens são apresentadas com os resultados da pesquisa
        - Realce palavras &mdash; determina se os resultados são apresentados com os termos de pesquisa em negrito
    - Configurações de pesquisa de imagem:
        - Imagem resultados ativados &mdash; determina se são devolvidos os resultados da pesquisa de imagem
    - Diversas configurações:
        - Título de página &mdash; texto apresentado na área de título de página
        - Tema de barra de ferramentas &mdash; determina a cor de fundo da área do título de página
        - Marcador de posição de texto de caixa de pesquisa &mdash; texto apresentado no antes da caixa de pesquisa de entrada
        - Url de ligação do título &mdash; destino para a ligação de título
        - Url de logótipo &mdash; apresentada junto ao título de imagem 
        - Url de Favicon &mdash; ícone apresentado na barra de título do browser

   > [!IMPORTANT]
   > Pelo menos, a pesquisa de imagem ou de pesquisa na Web que tem de estar ativado.

6.  Introduza a chave de subscrição de pesquisa ou escolha um na lista pendente. Na lista pendente é preenchida com as chaves de subscrições do Azure da sua conta. Consulte [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
7.  Se estiver ativada autosuggest, introduza a chave de subscrição autosuggest ou escolha um na lista pendente. Na lista pendente é preenchida com as chaves de subscrições do Azure da sua conta. Autosuggest personalizado requer um escalão de subscrição specifiec, consulte o [preços páginas](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Como efetuar alterações à configuração de IU alojada personalizada, o painel à direita fornece uma referência visual para que as alterações efetuadas. Os resultados da pesquisa apresentado não são resultados reais na sua instância

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Consumir IU personalizada
Para consumir IU alojada, está: 

- Incluir o script na sua página web
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

- Utilizar o URL fornecido `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`

  > [!IMPORTANT]
  > A página não é possível apresentar a sua declaração de privacidade ou outros avisos e os termos de licenciamento. Pode variar adequabilidade para utilização.  

Para obter informações adicionais, incluindo o seu ID de configuração personalizada, aceda a **pontos finais** sob o **produção** separador.

## <a name="next-steps"></a>Passos Seguintes
- [Utilizar marcadores decoration para realçar texto](./hit-highlighting.md)
- [Páginas de página Web](./page-webpages.md)