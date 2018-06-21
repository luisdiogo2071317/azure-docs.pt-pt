---
title: Proteger serviços de back-end utilizando o cliente de autenticação de certificado - API Management do Azure | Microsoft Docs
description: Saiba como proteger serviços de back-end utilizando a autenticação de certificado de cliente na API Management do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: apimpm
ms.openlocfilehash: 844a7ea1c2dd8f7dbb4984fc148575529ac154db
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36292863"
---
# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Como proteger serviços de back-end utilizando o cliente de autenticação de certificado na API Management do Azure

API Management permite proteger o acesso ao serviço de back-end de uma API utilizando certificados de cliente. Este guia mostra como gerir certificados numa instância de serviço de API Management do Azure no portal do Azure. Também explica como configurar uma API para utilizar um certificado para aceder a um serviço de back-end.

Para obter informações sobre como gerir certificados utilizando a API de REST de gestão de API, consulte <a href="https://docs.microsoft.com/en-us/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-certificate-entity">entidade de certificado de API do REST de gestão do Azure API</a>.

## <a name="prerequisites"> </a>Pré-requisitos

Este guia mostra como configurar a sua instância do serviço de API Management para utilizar a autenticação de certificado de cliente para aceder ao serviço de back-end de uma API. Antes de seguir os passos neste artigo, deverá ter o serviço de back-end configurado para autenticação de certificados de cliente ([para configurar a autenticação de certificados de Web sites do Azure, consulte a este artigo] [ to configure certificate authentication in Azure WebSites refer to this article]). Precisa de acesso para o certificado e a palavra-passe para carregá-lo para o serviço de API Management.

## <a name="step1"> </a>Carregar um certificado de cliente

![Adicione certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert.png)

Siga os passos abaixo para carregar um novo certificado de cliente. Se ainda não criou uma instância de serviço de API Management, consulte o tutorial [criar uma instância de serviço de API Management][Create an API Management service instance].

1. Navegue para a instância de serviço de API Management do Azure no portal do Azure.
2. Selecione **certificados de cliente** no menu.
3. Clique em de **+ adicionar** botão.  
    ![Adicione certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-add.png)  
4. Navegar para o certificado, forneça o ID e a palavra-passe.  
5. Clique em **Criar**.

> [!NOTE]
> O certificado tem de constar **. pfx** formato. Certificados autoassinados são permitidos.

Depois do certificado é carregado, mostra o **certificados de cliente**.  Se tiver muitos certificados, anote o thumbprint do certificado pretendido para [configurar uma API a utilizar um certificado de cliente para autenticação de gateway][Configure an API to use a client certificate for gateway authentication].

> [!NOTE]
> Para desativar a validação da cadeia de certificados quando utilizar, por exemplo, um certificado autoassinado, siga os passos descritos neste FAQ [item](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Eliminar um certificado de cliente

Para eliminar um certificado, clique no menu de contexto **...**  e selecione **eliminar** junto do certificado.

![Eliminar certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-delete.png)

Se o certificado está a ser utilizado por uma API, um ecrã de aviso é apresentado. Para eliminar o certificado, primeiro tem de remover o certificado a partir de qualquer APIs que estão configuradas para utilizá-lo.

![Eliminar a falha de certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-delete-failure.png)

## <a name="step2"> </a>Configurar uma API a utilizar um certificado de cliente para autenticação de gateway

1. Clique em **APIs** do **API Management** menu à esquerda e navegue para a API.  
    ![Ativar certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-enable.png)

2. No **Design** separador, clique num ícone de lápis do **back-end** secção. 
3. Alterar o **credenciais Gateway** para **certificados de cliente** e selecione o seu certificado na lista pendente.  
    ![Ativar certificados de cliente](media/api-management-howto-mutual-certificates/apim-client-cert-enable-select.png)

4. Clique em **Guardar**. 

> [!WARNING]
> Esta alteração tem efeita imediato e chamadas para operações dessa API vai utilizar o certificado para autenticação no servidor back-end.


> [!TIP]
> Quando é especificado um certificado para autenticação de gateway para o serviço de back-end de uma API, este torna-se parte da política para essa API e pode ser visualizado no editor de políticas.

## <a name="self-signed-certificates"></a>Certificados autoassinados

Se estiver a utilizar certificados autoassinados, terá de desativar a validação da cadeia de certificado por ordem para a gestão de API comunicar com o sistema de back-end. Caso contrário, irá devolver um código de 500 Erro. Para configurar, pode utilizar o [ `New-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/new-azurermapimanagementbackend) (para o novo back-end) ou [ `Set-AzureRmApiManagementBackend` ](https://docs.microsoft.com/powershell/module/azurerm.apimanagement/set-azurermapimanagementbackend) (para o back-end existente) cmdlets do PowerShell e defina o `-SkipCertificateChainValidation` parâmetro `True`.

```
$context = New-AzureRmApiManagementContext -resourcegroup 'ContosoResourceGroup' -servicename 'ContosoAPIMService'
New-AzureRmApiManagementBackend -Context  $context -Url 'https://contoso.com/myapi' -Protocol http -SkipCertificateChainValidation $true
```

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[Create an API Management service instance]: get-started-create-service-instance.md

[Azure API Management REST API Certificate entity]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[to configure certificate authentication in Azure WebSites refer to this article]: ../app-service/app-service-web-configure-tls-mutual-auth.md

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[Configure an API to use a client certificate for gateway authentication]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps
