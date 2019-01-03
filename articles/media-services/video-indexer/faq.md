---
title: Perguntas mais frequentes sobre o Video Indexer - Azure
titlesuffix: Azure Media Services
description: Obtenha respostas às perguntas mais frequentes sobre o Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 12/19/2018
ms.author: juliako
ms.openlocfilehash: 972858b4bae995b6e9073cf7ee451a317e9f3909
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53731576"
---
# <a name="frequently-asked-questions"></a>Perguntas Mais Frequentes

Este artigo responde às perguntas mais frequentes sobre o Video Indexer.

## <a name="general-questions"></a>Perguntas Gerais

### <a name="what-is-video-indexer"></a>O que é o Video Indexer?

Indexador de vídeos é um serviço de extração de metadados para ficheiros de multimédia de áudio e vídeo dos serviços de multimédia do Azure. Ele usa um conjunto avançado de algoritmos de machine learning para facilitar a classificar, analisar e obter informações sobre o áudio e vídeo usando conteúdo [modelos predefinidos](video-indexer-overview.md). Pode utilizar estas informações de várias maneiras, como a melhorar a detectabilidade do conteúdo e a acessibilidade, criando novas oportunidades de monetização, ou criar novas experiências que tiram partido de informações.

Indexador de vídeo fornece uma interface baseada na web para testes, configuração e personalização e uma API baseada em REST para desenvolvedores integrar o sistema de produção.

### <a name="what-can-i-do-with-video-indexer"></a>O que posso fazer com o indexador de vídeo?

O Video Indexer pode efetuar os seguintes tipos de operações nos ficheiros de multimédia:

* Identifique e extrair a conversão de voz e identificar oradores.
* Identificar e extrair no ecrã texto num vídeo.
* Identificar e etiquetar objetos num arquivo de vídeo.
* Identifica marcas, como a Microsoft a partir de faixas de áudio e no ecrã texto num vídeo.
* Detetar e reconhecer rostos de uma base de dados de celebridades e uma base de dados definidas pelo utilizador de faces.
* Extrair palavras-chave de conteúdo de áudio e vídeo com base no texto falado e visual.
* Extraia os tópicos abordados, mas não necessariamente explicitamente mencionado no conteúdo de áudio e vídeo com base no texto falado e visual.
* Crie legendas ou textos da faixa de áudio.

Para obter mais informações, veja [Descrição geral](video-indexer-overview.md).

### <a name="how-do-i-get-started-with-video-indexer"></a>Como posso começar a utilizar com o indexador de vídeo?

Indexador de vídeos é gratuito para experimentar. A avaliação gratuita fornece 600 minutos na interface baseada na web e 2.400 minutos através da API.

Vídeos de índice e flies áudio em escala, pode ligar o indexador de vídeo para uma subscrição paga do Microsoft Azure. Pode encontrar mais informações sobre os preços sobre o [preços](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) página.

Pode encontrar mais informações sobre como começar [começar](video-indexer-get-started.md).

### <a name="do-i-need-coding-skills-to-use-video-indexer"></a>É necessário habilidades para utilizar o Video Indexer?

Pode integrar as APIs do indexador de vídeo no seu pipeline, ou pode utilizar o Portal do indexador de vídeo e não precisa escrever qualquer código em todos os. 

### <a name="do-i-need-machine-learning-skills-to-use-video-indexer"></a>Preciso de habilidades de aprendizado de máquina para utilizar o Video Indexer?

Não, pode extraia informações dos seus conteúdos de vídeo e áudio completamente sem qualquer competências ou dados de conhecimento em algoritmos de aprendizagem automática. 

### <a name="what-media-formats-does-video-indexer-support"></a>Formatos de suporte de dados de que é o suporte de indexador de vídeo?

O Video Indexer suporta formatos de mídia mais comuns. Consulte a lista de formatos padrão do codificador de multimédia do Azure para obter mais detalhes.

### <a name="how-to-do-i-upload-a-media-into-video-indexer"></a>Como fazer carrego um suporte de dados para o Video Indexer?

No portal de baseada na web de indexador de vídeo, pode carregar um ficheiro de suporte de dados usando a caixa de diálogo de carregamento de ficheiro ou ao apontar para um URL ou diretamente Especifica o ficheiro. Por exemplo, `http://contoso.cloudapp.net/videos/example.mp4`. Uma ligação para uma página que contém um player de vídeo como `http://contoso.cloudapp.net/videos` não funcionará. A API do Video Indexer requer que especifique o ficheiro de entrada através de um URL ou uma matriz de bytes. Tenha em atenção que carrega através de um URL são limitados a 10 GB, mas não tem um limite de tempo de duração. Para obter mais informações, consulte este [guia de procedimentos](upload-index-videos.md).

### <a name="how-long-does-it-take-visual-indexer-to-extract-insights-from-media"></a>O tempo que demora Visual indexador para extrair informações de suporte de dados?

A quantidade de tempo que demora a indexar um ficheiro de vídeo ou áudio, as duas usando a API do indexador de vídeo e a interface baseada na web do Video Indexer, depende de vários parâmetros, como o comprimento do ficheiro e qualidade, o número de informações encontradas no arquivo, o número de computação une disponíveis, e se o ponto final de transmissão em fluxo está ativado ou não. Para tipos de conteúdo mais, supondo que 10 S3 RUs estiverem ativadas, podemos prever que indexação irá demorar de 1/3 para ½ de duração do vídeo.  No entanto, recomendamos que execute alguns arquivos de teste com seu próprio conteúdo e tirar uma média para ter uma idéia melhor. 

### <a name="in-which-azure-regions-is-video-indexer-available"></a>Em que regiões do Azure é o indexador de vídeo disponível?

Pode ver que regiões do Azure o indexador de vídeo está disponível na [regiões](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all) página.

### <a name="what-is-the-sla-for-video-indexer"></a>O que é o SLA para o Video Indexer?

A avaliação gratuita para o indexador de vídeo não tem SLA. SLA abrange o dos serviços de multimédia do Azure Video Indexer.

Pode encontrar as informações sobre o [SLA](https://azure.microsoft.com/support/legal/sla/media-services/v1_0/) página.

## <a name="billing-questions"></a>Questões sobre Faturação

### <a name="how-much-does-video-indexer-cost"></a>Quanto custa o Video Indexer?

O Video Indexer utiliza um simple pagamento, o que utilizar com base na duração da entrada de conteúdo de modelo de preços. Encargos adicionais poderão aplicar-se para a codificação, transmissão em fluxo, armazenamento, a utilização de rede e suporte de dados de unidades reservadas. Para o preço atual, consulte a [preços](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) página.

### <a name="does-video-indexer-offer-a-free-trial"></a>O indexador de vídeo oferece uma avaliação gratuita?

Sim, o indexador de vídeo oferece uma avaliação gratuita, que está alojada na região do Azure do Leste E.u.a. e fornece o serviço completo e a funcionalidade de API. Existe uma quota de 10 horas que vale a pena de vídeos para utilizadores do Web site e 40 horas para os utilizadores de API. 

## <a name="security-questions"></a>Perguntas de Segurança

### <a name="are-audio-and-video-files-indexed-by-video-indexer-stored"></a>Arquivos de áudio e vídeo são indexados pelo indexador de vídeos armazenados?

Sim, a menos que elimine o ficheiro do indexador de vídeos usando o Web site do indexador de vídeos ou a API, serão armazenados os ficheiros de áudio e vídeos. A avaliação gratuita, o vídeo e áudio, arquivos que índice será armazenado na região do Azure E.U.A. Leste. Caso contrário, os ficheiros de áudio e vídeos serão armazenados na conta de armazenamento da sua subscrição do Azure.

### <a name="can-i-delete-my-files-that-are-stored-in-video-indexer-portal"></a>Posso excluir minha ficheiros que estão armazenados no Portal do indexador de vídeo?

Sim, pode sempre eliminar os ficheiros de vídeos e áudio, bem como os seus conhecimentos do Video Indexer. Depois de eliminar os ficheiros, ele já não existe permanentemente do Video Indexer e onde quer que o indexador de vídeo armazenou o ficheiro (região do Azure dos EUA Leste para contas de avaliação e a conta de armazenamento da sua subscrição do Azure, caso contrário).

### <a name="who-has-access-to-my-audio-and-video-files-and-that-have-been-indexed-andor-stored-by-video-indexer"></a>Quem tem acesso a meus arquivos de áudio e vídeos e que tem sido indexado e/ou armazenados pelo indexador de vídeos?

Sim, pode sempre eliminar os ficheiros de vídeos e áudio, bem como os seus conhecimentos do Video Indexer. Depois de eliminar os ficheiros, ele já não existe permanentemente do Video Indexer e onde quer que o indexador de vídeo armazenou o ficheiro (região do Azure dos EUA Leste para contas de avaliação e a conta de armazenamento da sua subscrição do Azure, caso contrário).

### <a name="can-i-control-user-access-to-my-video-indexer-account"></a>Pode controlar o acesso de utilizador para a minha conta do indexador de vídeo?

Sim, selecionar o que é convidado para a conta.

### <a name="who-has-access-to-the-insights-that-were-extracted-from-my-audio-and-video-files-by-video-indexer"></a>Quem tem acesso às informações que foram extraídos de meus arquivos de áudio e vídeos pelo indexador de vídeos?

Seus vídeos que tenham públicos como sua definição de privacidade podem ser acedidos por qualquer pessoa que a ligação para o vídeo e suas informações. Seus vídeos que tenham privados como sua definição de privacidade só podem ser acedidos por utilizadores que foram convidados para a conta do vídeo.

### <a name="do-i-still-own-my-content-that-have-been-indexed-and-stored-by-video-indexer"></a>Ainda tenho meu conteúdo que foram indexados e armazenados pelo indexador de vídeo?

Sim, pelo [termos de serviço Online do Azure](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), que possui seu conteúdo.

### <a name="is-the-content-indexed-by-video-indexer-kept-within-the-azure-region-where-i-am-using-video-indexer"></a>O conteúdo é indexado pelo indexador de vídeo mantidas ao nível da região do Azure, onde estou usando o indexador de vídeo?

Sim, o conteúdo e as suas informações serão mantidas na região do Azure, a menos que tem uma configuração manual na sua subscrição do Azure que utiliza várias regiões do Azure. 

### <a name="what-is-the-privacy-policy-for-video-indexer"></a>O que é a política de privacidade para o indexador de vídeo?

Indexador de vídeos é abrangido pela [declaração de privacidade do Microsoft](https://privacy.microsoft.com/privacystatement).

## <a name="api-questions"></a>Perguntas de API

### <a name="what-are-the-differences-in-capability-on-the-video-indexer-website-versus-the-video-indexer-api"></a>Quais são as diferenças na capacidade no site do indexador de vídeo em comparação com a API do Video Indexer?

Com o [Video Indexer](https://www.videoindexer.com) Web site, pode pesquisar na sua biblioteca de vídeo, analisar insights, personalize modelos, configura a sua conta e editar os seus vídeos com facilidade. Com a robusta [API REST do Video Indexer](https://api-portal.videoindexer.ai/), pode integrar com qualquer infraestrutura ou incorporar widgets diretamente no seu próprio site ou aplicação.

### <a name="what-apis-does-video-indexer-offer"></a>As APIs oferecem o Video Indexer?

Pode encontrar informações sobre como utilizar as APIs do indexador de vídeo no [Portal de programador do indexador de vídeo](https://api-portal.videoindexer.ai/)

### <a name="how-do-i-get-started-with-video-indexers-api"></a>Como posso começar a utilizar com a API do Video Indexer?

Siga [Tutorial: introdução à API do Video Indexer](video-indexer-use-apis.md).

### <a name="what-is-an-access-token"></a>O que é um token de acesso?

A API do Video Indexer contém uma API de autorização e uma API de operações. A API de autorizações contém chamadas que lhe token de acesso. Cada chamada efetuada para a API de Operações deve estar associada a um token de acesso que corresponda ao âmbito de autorização da chamada.

### <a name="what-is-the-difference-between-account-access-token-user-access-token-and-video-access-token"></a>O que é a diferença entre o token de acesso de conta, o token de acesso de utilizador e o token de acesso de vídeo?

Representam o âmbito de autorização de uma chamada.

* Nível de utilizador - a tokens de acesso de nível de utilizador permitem-lhe efetuar operações no nível do usuário. Permitem, por exemplo, obter contas associadas.
* Nível de conta – a tokens de acesso de nível de conta permitem-lhe efetuar operações no nível da conta ou ao nível do vídeo. Permitem, por exemplo, carregar vídeos, listar todos os vídeos, obter informações de vídeo, etc.
* Nível de vídeo – a tokens de acesso de nível de vídeo permitem-lhe efetuar operações num vídeo específico. Permitem, por exemplo, obter informações de vídeo, transferir legendas, obter widgets, etc.

Pode controlar se estes tokens são só de leitura ou permitir a edição, especificando pokud vlastnost AllowEdit má = verdadeiro/falso.

### <a name="why-do-i-need-access-tokens-when-using-the-video-indexer-apis"></a>Por que razão necessito tokens de acesso ao utilizar as APIs do indexador de vídeo?

Tokens de acesso são necessárias para utilizar as APIs do indexador de vídeo por motivos de segurança. Isto garante que todas as chamadas são provenientes de ou os utilizadores que têm permissões de acesso à sua conta. 

### <a name="how-often-do-i-need-to-get-a-new-access-token-when-do-access-tokens-expire"></a>A frequência com que é necessário obter um novo token de acesso? Quando os tokens de acesso expira?

Tokens de acesso expirarem a cada hora, por isso terá de gerar um novo token de acesso a cada hora. 

## <a name="next-steps"></a>Passos Seguintes

[Descrição geral](video-indexer-overview.md)
