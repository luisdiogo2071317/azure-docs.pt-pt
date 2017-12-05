---
title: "As políticas de autenticação de API Management do Azure | Microsoft Docs"
description: "Saiba mais sobre as políticas de autenticação disponíveis para utilização na API Management do Azure."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 4d13d9dbea9da9db5bfe9a9af85fdbf9eab1ae84
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2017
---
# <a name="api-management-authentication-policies"></a>Políticas de autenticação de API Management
Este tópico fornece uma referência para as seguintes políticas de gestão de API. Para obter informações sobre adicionar e configurar as políticas, consulte [políticas na API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a>Políticas de autenticação  
  
-   [Autenticar com Basic](api-management-authentication-policies.md#Basic) -autenticar com um serviço de back-end utilizando a autenticação básica.  
  
-   [Autenticar com o certificado de cliente](api-management-authentication-policies.md#ClientCertificate) -autenticar com um serviço de back-end utilizando certificados de cliente.  
  
##  <a name="Basic"></a>Autenticar com básico  
 Utilize o `authentication-basic` política para autenticar com um serviço de back-end utilizando a autenticação básica. Esta política define eficazmente o cabeçalho de autorização de HTTP para o valor correspondente às credenciais fornecidas na política.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Exemplo  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Requerido|  
|----------|-----------------|--------------|  
|autenticação básica|Elemento raiz.|Sim|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Requerido|Predefinição|  
|----------|-----------------|--------------|-------------|  
|nome do utilizador|Especifica o nome de utilizador da credencial básico.|Sim|N/D|  
|palavra-passe|Especifica a palavra-passe da credencial básico.|Sim|N/D|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
  
-   **Âmbitos de política:** API  
  
##  <a name="ClientCertificate"></a>Autenticar com o certificado de cliente  
 Utilize o `authentication-certificate` política para autenticar com um serviço de back-end com o certificado de cliente. O certificado tem de ser [instalada na API Management](http://go.microsoft.com/fwlink/?LinkID=511599) primeiro e é identificado pelo respetivo thumbprint.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Exemplo  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Requerido|  
|----------|-----------------|--------------|  
|certificado de autenticação|Elemento raiz.|Sim|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Requerido|Predefinição|  
|----------|-----------------|--------------|-------------|  
|impressão digital|O thumbprint do certificado de cliente.|Sim|N/D|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
  
-   **Âmbitos de política:** API  
  

## <a name="next-steps"></a>Passos seguintes
Para obter mais informações para trabalhar com as políticas, consulte:

+ [Políticas de gestão de API](api-management-howto-policies.md)
+ [APIs de transformação](transform-api.md)
+ [Referência de política](api-management-policy-reference.md) para uma lista completa das declarações de política e as respetivas definições
+ [Exemplos de política](policy-samples.md)   
