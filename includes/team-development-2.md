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
ms.openlocfilehash: 8580e19f1955becb2cfaee13742054af1a9edb7f
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47410459"
---
### <a name="run-the-service"></a>Executar o serviço

1. Prima F5 (ou escreva `azds up` na Janela do Terminal) para executar o serviço. O serviço será executado automaticamente no espaço `default/scott`, selecionado recentemente. 
1. Pode confirmar se o serviço está a executar no seu próprio espaço ao executar `azds list-up` novamente. Em primeiro lugar, verá que uma instância de `mywebapi` está agora em execução no espaço `default/scott` (a versão em execução em `default` ainda está em execução, mas não é apresentada). Se executar `azds list-uris`, vai reparar que o URL do ponto de acesso de `webfrontend` tem o prefixo com o texto "scott.s.". Este URL é exclusivo ao espaço `default/scott`. O URL especial significa que os pedidos enviados para o "URL scott" tentarão, primeiro, encaminhar para os serviços no espaço `default/scott`, mas, se isso falhar, irão reverter para serviços no espaço `default`.

```
Name                      DevSpace  Type     Updated  Status
------------------------  --------  -------  -------  -------
mywebapi                  scott     Service  3m ago   Running
mywebapi-bb4f4ddd8-sbfcs  scott     Pod      3m ago   Running
webfrontend               default   Service  26m ago  Running
```

```
Uri                                                            Status
-------------------------------------------------------------  ---------
http://localhost:53831 => mywebapi.scott:80                    Tunneled
http://webfrontend.6364744826e042319629.canadaeast.aksapp.io/  Available
```

![](../articles/dev-spaces/media/common/space-routing.png)

Esta funcionalidade incorporada dos Espaços de Programador do Azure permite-lhe testar código num espaço partilhado sem que cada programador tenha de recriar a pilha completa de serviços no respetivo espaço. Este tipo de encaminhamento requer que o código da aplicação reencaminhe os cabeçalhos de propagação, conforme ilustrado no passo anterior deste guia.

### <a name="test-code-in-a-space"></a>Testar código num espaço
Para testar a nova versão de `mywebapi` com `webfrontend`, abra o browser no URL do ponto de acesso público de `webfrontend` e aceda à página About (Acerca de). Deverá ver sua nova mensagem apresentada.

Agora, remova a parte do URL "scott.s." e atualize o browser. Deverá ver o comportamento antigo (com a versão `mywebapi` em execução em `default`).
