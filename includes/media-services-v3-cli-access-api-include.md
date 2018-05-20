---
title: incluir ficheiro
description: incluir ficheiro
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 04/13/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: acb9bdf294dd66005df203f957c155540b658698
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
## <a name="access-the-media-services-api"></a>Aceder à API dos Serviços de Multimédia

Para ligar às APIs dos Serviços de Multimédia do Azure, utilize a autenticação do principal de serviço do Azure AD. O comando seguinte cria uma aplicação do Azure AD e anexa um principal de serviço à conta. Irá utilizar os valores devolvidos para configurar a aplicação .NET, conforme apresentado no passo seguinte.

Antes de executar o script, pode substituir o `amsaccount` e o `amsResourceGroup` pelos nomes que escolheu ao criar estes recursos. `amsaccount` é o nome da conta dos Serviços de Multimédia do Azure à qual o principal de serviço será anexado. <br/>O comando que se segue utiliza a opção `xml` que devolve um ficheiro xml que pode colar no app.config. Se omitir a opção `xml`, a resposta estará em `json`.

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```

Este comando irá produzir uma resposta semelhante à seguinte:

```xml
<add key="Region" value="West US 2" />
<add key="ResourceGroup" value="amsResourceGroup" />
<add key="AadEndpoint" value="https://login.microsoftonline.com" />
<add key="AccountName" value="amsaccount" />
<add key="SubscriptionId" value="111111111-0000-2222-3333-55555555555" />
<add key="ArmAadAudience" value="https://management.core.windows.net/" />
<add key="AadTenantId" value="2222222222-0000-2222-3333-6666666666666" />
<add key="AadSecret" value="33333333-0000-2222-3333-55555555555" />
<add key="AadClientId" value="44444444-0000-2222-3333-55555555555" />
<add key="ArmEndpoint" value="https://management.azure.com/" />
```

### <a name="configure-the-sample-app"></a>Configurar a aplicação de exemplo

Para executar a aplicação e aceder às APIs dos Serviços de Multimédia, tem de especificar os valores de acesso corretos no app.config. 

1. Abra o Visual Studio.
2. Navegue até à solução que clonou.
3. No Explorador de Soluções, desdobre o projeto *EncodeAndStreamFiles*.
4. Defina este projeto como o projeto de arranque.
5. Abra o app.config.
6. Substitua os valores de appSettings pelos valores que obteve no passo anterior.

 ```xml
 <add key="Region" value="value" />
 <add key="ResourceGroup" value="value" />
 <add key="AadEndpoint" value="value" />
 <add key="AccountName" value="value" />
 <add key="SubscriptionId" value="value" />
 <add key="ArmAadAudience" value="value" />
 <add key="AadTenantId" value="value" />
 <add key="AadSecret" value="value" />
 <add key="AadClientId" value="value" />
 <add key="ArmEndpoint" value="value" />
 ```    
 
7. Prima Ctrl+Shift+B para compilar a solução.
