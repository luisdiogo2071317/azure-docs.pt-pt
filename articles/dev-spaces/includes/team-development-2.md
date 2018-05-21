---
title: incluir ficheiro
description: incluir ficheiro
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: 01b830f8a5e6e5f957b36bc2d6ef035754c06157
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
### <a name="run-the-service"></a>Executar o serviço

1. Prima F5 (ou tipo `azds up` na janela de Terminal) para executar o serviço. O serviço será automaticamente executado no seu espaço de recentemente selecionado `scott`. 
1. Pode confirmar que o serviço está a ser executado no seu próprio espaço executando `azds resource list` novamente. Em primeiro lugar, verá uma instância de `mywebapi` está agora em execução `scott` espaço (a versão em execução no `default` está ainda em execução, mas não estiver listado). Lugar, o acesso ao ponto de URL para `webfrontend` é o prefixo com o texto *scott.s.*. Este URL é exclusivo para o `scott` espaço e significa que os pedidos enviados para o URL"blogue" irão tentar primeiro rotas a serviços no `scott` espaço e irá reverter para serviços no `default` espaço.

```
Name         Space     Chart              Ports   Updated     Access Points
-----------  --------  -----------------  ------  ----------  -------------
mywebapi     scott     mywebapi-0.1.0     80/TCP  15s ago     http://localhost:61466
webfrontend  default  webfrontend-0.1.0  80/TCP  5h ago      http://scott.s.webfrontend-contosodev.1234abcdef.eastus.aksapp.io
```

![](../media/common/space-routing.png)

Esta capacidade incorporada de ativa do Azure Dev espaços testar código ponto-a-ponto num ambiente partilhado sem necessidade de cada programador recriar a pilha completa de serviços no respetivo espaço. Este tipo de encaminhamento requer o código da aplicação para cabeçalhos de propagação direta, conforme ilustrado no passo anterior deste guia.

## <a name="test-code-in-a-space"></a>Código de teste num espaço
Para testar a nova versão do `mywebapi` em conjunto com `webfrontend`, abra o browser para o URL de ponto de acesso público para webfrontend e aceda à página acerca de. Deverá ver a nova mensagem apresentada.

Agora, remova o "scott.s". parte do URL e atualize o browser. Deverá ver o comportamento antigo (diversidade de `mywebapi` versão em execução no `default`)