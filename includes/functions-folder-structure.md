---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 09/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 2808264b4641bda49a53677ebe216a3b53b7d0d9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293643"
---
O código para todas as funções num aplicativo de função específica está localizado numa pasta de projeto de raiz que contém um ficheiro de configuração de anfitrião e as subpastas de um ou mais. Cada subpasta contém o código para uma função distinta, tal como a seguinte representação:

```
FunctionApp
 | - host.json
 | - Myfirstfunction
 | | - function.json
 | | - ...  
 | - mysecondfunction
 | | - function.json
 | | - ...  
 | - SharedCode
 | - bin
```

Na versão 2.x do runtime das funções, todas as funções na aplicação de função têm de partilhar a mesma função de trabalho do idioma.  

O [Host. JSON](../articles/azure-functions/functions-host-json.md) arquivo, que contém algumas configurações específicas do tempo de execução, está na pasta raiz da aplicação de função. A `bin` pasta contém pacotes e outros ficheiros de biblioteca necessários para a aplicação de funções. Consulte os requisitos de idioma específico para um projeto de aplicação de função:

* [C# biblioteca de classes (arquivo. csproj)](../articles/azure-functions/functions-dotnet-class-library.md#functions-class-library-project)
* [Script do c# (.csx)](../articles/azure-functions/functions-reference-csharp.md#folder-structure)
* [Skriptu F #](../articles/azure-functions/functions-reference-fsharp.md#folder-structure)
* [Java](../articles/azure-functions/functions-reference-java.md#folder-structure)
* [JavaScript](../articles/azure-functions/functions-reference-node.md#folder-structure)



