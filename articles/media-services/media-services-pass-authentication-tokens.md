---
title: "Transmita o token de autenticação para Media Services do Azure | Microsoft Docs"
description: "Saiba como enviar os tokens de autenticação do cliente ao serviço de entrega de chave de Media Services do Azure"
services: media-services
keywords: "autenticação de token de proteção de conteúdos, DRM,"
documentationcenter: 
author: dbgeorge
manager: jasonsue
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: dwgeo
ms.openlocfilehash: 0e56726266898e5738dd797a8a019987d457170e
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2017
---
# <a name="how-clients-pass-tokens-to-azure-media-services-key-delivery-service"></a>Como os clientes passaram tokens para o serviço de entrega de chave de Media Services do Azure
Estamos constantemente obter perguntas em torno como um leitor poderia transmitir token para os nossos serviços de entrega de chave, o qual irão obter verificados e o leitor obtém a chave. Suportamos simples Web tokens (SWT) e JSON Web tokens (JWT) estes dois formatos de token. Autenticação de token foi possível aplicar a qualquer tipo de chave – independentemente de já estão a fazer encriptação comum ou AES encriptação envelope no sistema.

Estas são as formas seguintes poderia transmitir o token com o seu leitor, depende o leitor e a plataforma de destino que:
- Através do cabeçalho de autorização de HTTP.
> [!NOTE]
> Tenha em atenção que o prefixo "Portador" é esperado pelas especificações de OAuth 2.0. Há um leitor de exemplo com a configuração de Token alojado no Azure Media Player [página demonstração](http://ampdemo.azureedge.net/). Escolha AES (JWT Token) ou AES (SWT Token) para definir a origem de vídeo. Token é transmitido através do cabeçalho de autorização.

- Através de adicionar um parâmetro de consulta de Url com o "token = tokenvalue".  
> [!NOTE]
> Tenha em atenção que o prefixo "Portador" é esperada. Uma vez que o token é enviado através de um URL, terá de proteger a cadeia do token. Eis um código de exemplo do c# sobre como fazê-lo:

```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
```

- Através do campo de CustomData.
Para PlayReady só, a aquisição de licença através do campo de CustomData do desafio de aquisição de licença PlayReady. Neste caso, o token tem de ser existente no documento xml descrito abaixo.

```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
```
Coloque o seu token de autenticação no <Token> elemento.

- Através de uma lista de reprodução HLS alternativa. Se tiver de configurar a autenticação de Token para AES + HLS reprodução no iOS/Safari, não é uma forma que poderia enviar diretamente no token. Consulte este [blogue](http://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/) sobre como alternativa a lista de reprodução para ativar este cenário.

## <a name="next-steps"></a>Passos seguintes
Rever os percursos de aprendizagem dos Serviços de Multimédia

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]