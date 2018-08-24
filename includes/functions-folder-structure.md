---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 08/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3cbe634d862682a5f6b06c2cfc77a4d3b03954f9
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811599"
---
O código para todas as funções num aplicativo de função específica está localizado numa pasta de raiz (`wwwroot`) que contém um ficheiro de configuração de anfitrião e as subpastas de um ou mais. Cada subpasta contém o código para uma função separada, como no exemplo seguinte:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
 | - bin
 | | - mycompiledcsharp.dll
```

O ficheiro de Host. JSON contém algumas configurações específicas do tempo de execução e que estão localizados na pasta raiz da aplicação de função. Para obter informações sobre as definições que estão disponíveis, consulte a [referência de Host. JSON](../articles/azure-functions/functions-host-json.md).

Cada função tem uma pasta que contém um ou mais arquivos de código, a configuração de Function e outras dependências. Para um c# projeto de biblioteca de classe, o ficheiro de biblioteca (. dll) da classe compilados for implementado para o `bin` subpasta.

