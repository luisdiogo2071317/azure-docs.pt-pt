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
ms.openlocfilehash: 2aec8ba969fb639f2d785a429441f6ed4bbf7dfb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987684"
---
# <a name="configure-your-hosted-ui-experience"></a>Configurar a sua experiência de interface do Usuário alojada

Depois de configurar a sua instância de pesquisa personalizada, pode chamar a API de pesquisa personalizada para obter os resultados da pesquisa e exibi-las na sua aplicação. Em alternativa, se a aplicação for uma aplicação web, pode usar a interface do Usuário alojado que fornece pesquisa personalizada.   

## <a name="configure-custom-hosted-ui"></a>Configurar a interface do Usuário de hospedado personalizado

Para configurar uma interface de Usuário para a sua aplicação web alojada, siga estes passos:

1. Entrar para a pesquisa personalizada [portal](https://customsearch.ai).  
  
2. Clique numa instância de pesquisa personalizada. Para criar uma instância, veja [criar a primeira instância de pesquisa personalizada do Bing](quick-start.md).  

3. Clique nas **alojada a interface do Usuário** separador.  
  
4. Selecione um esquema.
  
  - Barra de pesquisa e os resultados (predefinição) &mdash; este esquema é sua página de pesquisa tradicional com a caixa de pesquisa e os resultados da pesquisa.
  - Os resultados apenas &mdash; este esquema apresenta apenas os resultados da pesquisa. Este esquema não exibe uma caixa de pesquisa. Tem de fornecer a consulta de pesquisa, adicionando o parâmetro de consulta (& q =\<cadeia de consulta >) para o URL do pedido no fragmento do JavaScript ou ligação de ponto final HTML.
  - A ativação pós-falha POP &mdash; esse layout fornece uma caixa de pesquisa e exibe os resultados da pesquisa numa sobreposição deslizante.
      
5. Selecione um tema de cores. Os temas possíveis são: 
  
  - Clássica
  - Escuro
  - Skyline azul

  Clique em cada um dos temas para ver o tema funciona melhor com a sua aplicação web. Se precisar de ajustar o tema de cores para melhor integração com a sua aplicação web, clique em **personalizar tema**. Nem todas as configurações de cor aplicam-se a todos os temas de layout. Para alterar uma cor, introduza o valor de RGB HEXADECIMAL a cor (por exemplo, #366eb8) na caixa de texto correspondente. Em alternativa, clique no botão de cor e, em seguida, clique no tom que funcione para si. 
  
  Depois de alterar uma cor, veja como a alteração afetará o exemplo de pré-visualização do lado direito. Pode sempre clicar **repor para predefinição** para voltar às cores predefinidas para o tema que selecionou.

  > [!NOTE]
  > Quando alterar o tema de cor, considere acessibilidade ao escolher cores.

5. Sob **configurações adicionais**, forneça os valores conforme adequado para a sua aplicação. Estas definições são opcionais. Para ver o efeito de aplicar ou removê-los, consulte o painel de visualização no lado direito. Opções de configuração disponíveis são:  
  
  - Configurações de pesquisa Web:
    - Os resultados ativada da Web &mdash; determina se a pesquisa na web está ativada (verá um separador de Web na parte superior da página).
    - Ativar sugestão automática &mdash; determina se a sugestão automática personalizada está ativada (consulte [preços](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) custo adicional).
    - Resultados por página da Web &mdash; número de resultados de pesquisa web a apresentar ao mesmo tempo (o máximo é de 50 resultados por página).
    - Legenda da imagem deve &mdash; determina se as imagens são apresentadas com os resultados da pesquisa.
  
    As seguintes configurações são apresentadas se clicar **Show configurações avançadas**.  
  
    - Realçar palavras &mdash; determina se os resultados são apresentados com os termos de pesquisa em negrito. 
    - Destino de ligação &mdash; determina se a página Web abre num novo separador do browser (em branco) ou o mesmo separador do browser (auto) quando o usuário clica num resultado de pesquisa. 

  - Configurações de pesquisa de imagem:
    - Os resultados ativada de imagem &mdash; determina se a pesquisa de imagens está ativada (verá um separador de imagens na parte superior da página).   
    - Resultados por página de imagem &mdash; número de resultados de pesquisa de imagem a apresentar ao mesmo tempo (o máximo é 150 resultados por página).  
  
    A seguinte configuração é apresentada se clicar **Show configurações avançadas**.  
  
    - Ativar filtros &mdash; adiciona filtros que o utilizador pode utilizar para filtrar as imagens que o Bing devolve. Por exemplo, o utilizador pode filtrar os resultados para apenas os GIF animados.

  - Configurações de pesquisa de vídeos:
    - Resultados de vídeo ativados &mdash; determina se a pesquisa de vídeos está ativada (verá um separador de vídeos na parte superior da página).  
    - Resultados de vídeo por página &mdash; número de resultados de pesquisa de vídeos para apresentar ao mesmo tempo (o máximo é 150 resultados por página).
  
    A seguinte configuração é apresentada se clicar **Show configurações avançadas**.  
  
    - Ativar filtros &mdash; adiciona filtros que o utilizador pode utilizar para filtrar os vídeos que o Bing devolve. Por exemplo, o utilizador pode filtrar os resultados para vídeos com uma resolução específico ou vídeos detetados nas últimas 24 horas.

  - Diversas configurações:
    - Título da página &mdash; texto apresentado na área de título da página de resultados de pesquisa (não para a ativação pós-falha pop layout).
    - Tema de barra de ferramentas &mdash; determina a cor de fundo da área do título da página de resultados de pesquisa.  
  
    As seguintes configurações são apresentadas se clicar **Show configurações avançadas**.  
  
    - Marcador de posição de texto de caixa de pesquisa &mdash; texto apresentado na antes de caixa de pesquisa de entrada.
    - Url de ligação do título &mdash; destino para a ligação do título.
    - Url de logótipo &mdash; imagem exibida ao lado do título. 
    - Url de Favicon &mdash; ícone apresentado na barra de título do navegador.  

    As seguintes configurações aplicam-se apenas se consumir a interface do Usuário alojadas através do ponto final HTML (eles não se aplicam se utilizar o fragmento do JavaScript).
    
    - Título da página
    - Tema de barra de ferramentas
    - URL de ligação do título
    - URL de logótipo
    - URL de Faviicon  
  
6. Introduza a chave de subscrição de pesquisa ou escolha um na lista pendente. A lista pendente é preenchida com chaves de subscrições da sua conta do Azure. Ver [conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).  

7. Se ativou a sugestão automática, introduza a chave de subscrição de sugestão automática ou escolha um na lista pendente. A lista pendente é preenchida com chaves de subscrições da sua conta do Azure. Sugestão automática personalizada requer um escalão de subscrição específica, consulte a [preços](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Como efetuar alterações à configuração de interface do Usuário hospedada personalizada, o painel à direita fornece uma referência visual para que as alterações feitas. Os resultados da pesquisa exibidos não são os resultados reais da sua instância.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Consumir a interface do Usuário personalizada

Para consumir a interface do Usuário alojado, optar por: 

- Inclua o script na página da web  
  
  ```html
  <html>
      <body>
          <script type="text/javascript" 
              id="bcs_js_snippet"
              src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
          </script>
      </body>    
  </html>
  ```

- Em alternativa, utilize o seguinte URL num browser.   
  
  `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`  
  
  > [!NOTE]
  > Adicione os seguintes parâmetros de consulta para o URL conforme necessário. Para obter informações sobre estes parâmetros, consulte [a API de pesquisa personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) referência.
  >
  > - p
  > - mkt
  > - pesquisa segura
  > - setlang

  > [!IMPORTANT]
  > A página não é possível apresentar a sua declaração de privacidade ou outros avisos e os termos. Pode variar de adequação para a sua utilização.  

Para obter mais informações, incluindo o seu ID de configuração personalizada, aceda a **pontos de extremidade** sob a **produção** separador.

## <a name="next-steps"></a>Passos Seguintes

- [Utilizar marcadores decoration para realçar o texto](./hit-highlighting.md)
- [Páginas Web de página](./page-webpages.md)