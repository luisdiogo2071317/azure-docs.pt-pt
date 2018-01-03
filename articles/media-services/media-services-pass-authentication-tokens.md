---
title: "Transmita os tokens de autenticação para Media Services do Azure | Microsoft Docs"
description: "Saiba como enviar os tokens de autenticação do cliente para o serviço de entrega de chave de Media Services do Azure"
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
ms.openlocfilehash: 7d143242231444b8557a303d1b504d5311693f1a
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="learn-how-clients-pass-tokens-to-the-azure-media-services-key-delivery-service"></a>Saiba como os clientes passaram tokens para o serviço de entrega de chave de Media Services do Azure
Os clientes, muitas vezes, pergunte como um leitor pode passar tokens para o serviço de entrega de chave de Media Services do Azure para fins de verificação para que o leitor pode obter a chave. Os Media Services suportam o token de web simples (SWT) e formatos JSON Web tokens (JWT). Autenticação de token é aplicada a qualquer tipo de chave, independentemente se utilizar encriptação comum ou de encriptação de envelope Advanced Encryption Standard (AES) no sistema.

 Consoante a plataforma de destino e player, pode passar o token com o seu leitor das seguintes formas:

- Através do cabeçalho de autorização de HTTP.
    > [!NOTE]
    > O prefixo "Portador" é esperado pelas especificações de OAuth 2.0. Um leitor de exemplo com a configuração de token está alojado num leitor de multimédia do Azure [página demonstração](http://ampdemo.azureedge.net/). Para definir a origem de vídeo, escolha **AES (JWT Token)** ou **AES (SWT Token)**. O token é transmitido através do cabeçalho de autorização.

- Através da adição de um URL de consulta parâmetro com o "token = tokenvalue."  
    > [!NOTE]
    > O prefixo "Portador" não é esperado. Porque o token é enviado através de um URL, terá de proteger a cadeia do token. Eis um exemplo código c# que mostra como fazê-lo:

    ```csharp
    string armoredAuthToken = System.Web.HttpUtility.UrlEncode(authToken);
    string uriWithTokenParameter = string.Format("{0}&token={1}", keyDeliveryServiceUri.AbsoluteUri, armoredAuthToken);
    Uri keyDeliveryUrlWithTokenParameter = new Uri(uriWithTokenParameter);
    ```

- Através do campo de CustomData.
Esta opção é utilizada para apenas o aquisição de licença PlayReady, através do campo de CustomData do desafio de aquisição de licença PlayReady. Neste caso, o token tem de ser existente no documento XML conforme descrito aqui:

    ```xml
    <?xml version="1.0"?>
    <CustomData xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyCustomData/v1"> 
        <Token></Token> 
    </CustomData>
    ```
    Coloque o seu token de autenticação no elemento de Token.

- Através de uma lista de reprodução de HTTP Live Streaming (HLS) alternativo. Se pretender configurar a autenticação de token para AES + HLS reprodução no iOS/Safari, não é uma forma pode enviar diretamente no token. Para obter mais informações sobre como alternativa a lista de reprodução para ativar este cenário, consulte este [blogue](http://azure.microsoft.com/blog/2015/03/06/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

## <a name="next-steps"></a>Passos Seguintes

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]