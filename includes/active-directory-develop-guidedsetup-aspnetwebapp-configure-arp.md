---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: b18b6e2fd5bb2071f57033dbe74ef87da028720b
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843226"
---
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>Configurar a aplicação Web ASP.NET com informações de registo da aplicação

Neste passo, irá configurar seu projeto para utilizar SSL e, em seguida, utilize o URL do SSL para configurar informações de registo do seu aplicativo. Depois disso, adicionar a aplicação "informações de registo à sua solução através de *Web. config*.

1.  No Solution Explorer, selecione o projeto e observe o `Properties` janela (se não vir uma janela de propriedades, pressione F4)
2.  Alteração `SSL Enabled` para `True`
3.  Copie o valor de `SSL URL` acima e colá-lo no `Redirect URL` campo na parte superior desta página, em seguida, clique em *atualização*:<br/><br/>![Propriedades do projeto](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
4.  Adicione o seguinte na `web.config` localizado na pasta do raiz, na secção de ficheiro `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
