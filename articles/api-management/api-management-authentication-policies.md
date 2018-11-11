---
title: As políticas de autenticação de gestão de API do Azure | Documentos da Microsoft
description: Saiba mais sobre as políticas de autenticação disponíveis para utilização na gestão de API do Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 4c4c03fffa5786bf3a50f4d2c03511f0a2de0f48
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250956"
---
# <a name="api-management-authentication-policies"></a>Políticas de autenticação de gestão de API
Este tópico fornece uma referência para as seguintes políticas de gestão de API. Para informações sobre como adicionar e configurar as políticas, consulte [políticas de gestão de API](https://go.microsoft.com/fwlink/?LinkID=398186).  

##  <a name="AuthenticationPolicies"></a> Políticas de autenticação  
  
-   [Autenticar com o Basic](api-management-authentication-policies.md#Basic) -autenticar com um serviço de back-end usando a autenticação básica.  
  
-   [Autenticar com o certificado de cliente](api-management-authentication-policies.md#ClientCertificate) -autenticar com um serviço de back-end com certificados de cliente.  
  
##  <a name="Basic"></a> Autenticar com o básico  
 Utilize o `authentication-basic` política para autenticar com um serviço de back-end usando a autenticação básica. Esta política efetivamente define o cabeçalho de autorização de HTTP para o valor correspondente para as credenciais fornecidas na política.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a>Exemplo  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Necessário|  
|----------|-----------------|--------------|  
|autenticação básica|Elemento raiz.|Sim|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Necessário|Predefinição|  
|----------|-----------------|--------------|-------------|  
|o nome de utilizador|Especifica o nome de utilizador da credencial básica.|Sim|N/A|  
|palavra-passe|Especifica a palavra-passe da credencial básica.|Sim|N/A|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
  
-   **Âmbitos de política:** API  
  
##  <a name="ClientCertificate"></a> Autenticar com o certificado de cliente  
 Utilize o `authentication-certificate` política para autenticar com um serviço de back-end com o certificado de cliente. O certificado tem de ser [instalados, a gestão de API](https://go.microsoft.com/fwlink/?LinkID=511599) primeiro e é identificado por sua impressão digital.  
  
### <a name="policy-statement"></a>Declaração de política  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a>Exemplo  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a>Elementos  
  
|Nome|Descrição|Necessário|  
|----------|-----------------|--------------|  
|certificado de autenticação|Elemento raiz.|Sim|  
  
### <a name="attributes"></a>Atributos  
  
|Nome|Descrição|Necessário|Predefinição|  
|----------|-----------------|--------------|-------------|  
|Thumbprint|O thumbprint do certificado de cliente.|Sim|N/A|  
  
### <a name="usage"></a>Utilização  
 Esta política pode ser utilizada na política de seguinte [secções](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) e [âmbitos](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).  
  
-   **As secções de política:** entrada  
  
-   **Âmbitos de política:** API  
  

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações para trabalhar com políticas, consulte:

+ [Políticas de gestão de API](api-management-howto-policies.md)
+ [Transforme as APIs](transform-api.md)
+ [Referência de política](api-management-policy-reference.md) para uma lista completa das declarações de política e suas configurações
+ [Exemplos de política](policy-samples.md)   
