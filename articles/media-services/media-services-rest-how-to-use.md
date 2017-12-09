---
title: "Descrição geral da REST API de operações de Media Services | Microsoft Docs"
description: "Descrição geral da API de REST de serviços de suporte de dados"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/05/2017
ms.author: juliako;johndeu
ms.openlocfilehash: 066959058576af830103aa98a12f0c36acfdbb14
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="media-services-operations-rest-api-overview"></a>Descrição geral da REST API de operações de Media Services
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

O **REST de operações de serviços de suporte de dados** API é utilizada para criar tarefas, recursos, canais em direto e outros recursos numa conta de Media Services. Para obter mais informações, consulte [referência da API de REST de operações de serviços de suporte de dados](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Os Media Services fornecem uma API de REST que aceita JSON ou atom + pub formato XML. API de REST dos serviços de suporte de dados requer que os cabeçalhos de HTTP específicos que cada cliente terá de enviar ao estabelecer ligação com os Media Services, bem como um conjunto de cabeçalhos opcionais. As secções seguintes descrevem os cabeçalhos e verbos HTTP pode utilizar quando criar pedidos e receber respostas de Media Services.

Autenticação para a API de REST do suporte de dados Serivces é feita através da autenticação do Azure Active Directory que é descrita no artigo [autenticação de utilização do Azure AD para aceder à API de serviços de suporte de dados do Azure com REST](media-services-rest-connect-with-aad.md)

## <a name="considerations"></a>Considerações

Aplicam as seguintes considerações ao utilizar REST.

* Ao consultar entidades, não há um limite de 1000 entidades devolvido de uma só vez porque pública REST v2 limita os resultados da consulta para resultados de 1000. Tem de utilizar **ignorar** e **demorar** (.NET) / **superior** (REST), conforme descrito em [neste exemplo .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) e [neste exemplo de REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* Quando utilizando JSON e especificando a utilizar o **METADATA** palavra-chave no pedido (por exemplo, para um objeto ligado referência) tem de definir o **aceitar** cabeçalho para [formato JSON verboso](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)(consulte o exemplo seguinte). OData não compreende a **METADATA** propriedade no pedido, a menos que pode defini-la verboso.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.17
        Authorization: Bearer <token> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Cabeçalhos de pedido HTTP padrão suportados pelos Media Services
Para cada chamada que efetuar nos Media Services, existe um conjunto de cabeçalhos necessários, que tem de incluir no seu pedido e também um conjunto de cabeçalhos opcionais pode querer incluir. A tabela abaixo lista os cabeçalhos necessários:

| Cabeçalho | Tipo | Valor |
| --- | --- | --- |
| Autorização |Portador |Portador é o mecanismo de autorização aceites apenas. O valor também tem de incluir o token de acesso fornecido pelo Azure Active Directory. |
| x-ms-version |Decimal |2.17 (ou versão mais recente)|
| DataServiceVersion |Decimal |3.0 |
| MaxDataServiceVersion |Decimal |3.0 |

> [!NOTE]
> Uma vez que os Media Services utiliza OData para expor das respetivas APIs REST, os cabeçalhos DataServiceVersion e MaxDataServiceVersion devem ser incluídos em todos os pedidos; No entanto, se não estiverem, em seguida, atualmente dos Media Services assume o valor de DataServiceVersion em utilização 3.0.
> 
> 

Segue-se um conjunto de cabeçalhos opcionais:

| Cabeçalho | Tipo | Valor |
| --- | --- | --- |
| Data |RFC 1123 data |Timestamp do pedido |
| Aceitar |Tipo de conteúdo |O tipo de conteúdo solicitado para a resposta, como o seguinte:<p> -application/json; odata = verboso<p> -application/atom + xml<p> As respostas podem ter um tipo de conteúdo diferente, tais como obtenção de um blob, em que uma resposta com êxito contém a sequência de BLOBs, como o payload. |
| Codificação aceitar |Gzip, deflate |GZIP e DEFLATE codificação, quando aplicável. Nota: Para obter recursos grande, os Media Services podem ignorar este cabeçalho e devolver dados noncompressed. |
| Idioma aceitar |"pt", "es" e assim sucessivamente. |Especifica a linguagem preferencial a resposta. |
| Conjunto de carateres aceitar |Tipo de conjunto de carateres como "UTF-8" |Predefinição é UTF-8. |
| X-HTTP-Method |Método de HTTP |Permite que os clientes ou firewalls que não suporta métodos HTTP PUT ou eliminação para utilizar estes métodos, em túnel através de uma chamada GET. |
| Content-Type |Tipo de conteúdo |Pedidos de tipo de conteúdo do corpo do pedido PUT ou POST. |
| id do pedido do cliente |Cadeia |Um valor definido pelo autor da chamada que identifica o pedido especificado. Se for especificado, este valor será incluído na mensagem de resposta como uma forma para mapear o pedido. <p><p>**Importante**<p>Os valores devem ser limitados a 2096b (2k). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Cabeçalhos de resposta HTTP padrão suportados pelos Media Services
Segue-se um conjunto de cabeçalhos que podem ser devolvidos para si, consoante o recurso que foram pedir e a ação que pretende efetuar.

| Cabeçalho | Tipo | Valor |
| --- | --- | --- |
| id do pedido |Cadeia |Um identificador exclusivo para a operação atual, o serviço gerado. |
| id do pedido do cliente |Cadeia |Um identificador especificado pelo chamador no pedido original, se estiver presente. |
| Data |RFC 1123 data |A data/hora que o pedido foi processado. |
| Content-Type |varia |O tipo de conteúdo do corpo da resposta. |
| Codificação de conteúdo |varia |Gzip ou deflate, conforme adequado. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Verbos HTTP padrão suportados pelos Media Services
Segue-se uma lista completa dos verbos HTTP que podem ser utilizados quando efetuar HTTP pedidos:

| Verbo | Descrição |
| --- | --- |
| INTRODUÇÃO |Devolve o valor atual de um objeto. |
| POST |Cria um objeto com base nos dados fornecidos ou submete um comando. |
| COLOCAR |Substitui um objeto ou cria um objeto com o nome (quando aplicável). |
| ELIMINAR |Elimina um objeto. |
| INTERCALAÇÃO |Atualiza um objeto existente com alterações de propriedades com nome. |
| CABEÇALHO |Devolve os metadados de um objeto para uma resposta GET. |

## <a name="discover-and-browse-the-media-services-entity-model"></a>Detetar e procurar o modelo de entidade de Media Services
Para tornar as entidades de Media Services mais detetável, a operação de $metadata pode ser utilizada. Permite-lhe obter todos os tipos de entidade válido, propriedades de entidade, associações, funções, ações e assim sucessivamente. Ao adicionar a operação de $metadata ao fim do seu ponto final de API de REST dos serviços de suporte de dados, pode aceder a este serviço de deteção.

 /API/$ metadados.

Deve acrescentar "? api-version=2.x" ao fim do URI se pretende ver os metadados num browser ou não inclua o cabeçalho x-ms-version no seu pedido.

## <a name="authenticate-with-media-services-rest-using-azure-active-directory"></a>Autenticar com REST de serviços de suporte de dados com o Azure Active Directory

Autenticação na REST API é feita Directory(AAD) Active Directory do Azure.
Para obter mais informações sobre como obter os detalhes de autenticação necessário para a sua conta de Media Services do Portal do Azure, consulte [aceder à API de serviços de suporte de dados do Azure com a autenticação do Azure AD](media-services-use-aad-auth-to-access-ams-api.md).

Para obter detalhes sobre como escrever código que liga à API REST através da autenticação do Azure AD, consulte o artigo [autenticação de utilização do Azure AD para aceder à API de serviços de suporte de dados do Azure com o resto](media-services-rest-connect-with-aad.md).

## <a name="next-steps"></a>Passos seguintes
Para saber como utilizar a autenticação do Azure AD com a API de REST dos serviços de suporte de dados, consulte [autenticação de utilização do Azure AD para aceder à API de serviços de suporte de dados do Azure com o resto](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

