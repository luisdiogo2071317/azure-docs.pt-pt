---
title: As funções runtime versões descrição geral do Azure
description: As funções do Azure suporta várias versões do tempo de execução. Saber as diferenças entre-los e como escolher aquele que é adequada para si.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2018
ms.author: glenga
ms.openlocfilehash: 9f916aaa8032ff519709d73a1c1f51195f811686
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
ms.locfileid: "28919360"
---
# <a name="azure-functions-runtime-versions-overview"></a>As funções runtime versões descrição geral do Azure

 Existem duas versões principais do tempo de execução das funções do Azure.: 1. x e 2. x. Este artigo explica como escolher a versão principal a utilizar.

> [!IMPORTANT] 
> Tempo de execução 1. x é a única versão aprovada para utilização em produção.

| Runtime | Estado |
|---------|---------|
|1. x|Geralmente disponível (GA)|
|2. x|Pré-visualização|

Para obter informações sobre como configurar uma aplicação de função ou o ambiente de desenvolvimento para uma versão específica, consulte [como destino a versões de tempo de execução das funções do Azure](set-runtime-version.md) e [código e teste das funções do Azure localmente](functions-run-local.md).

## <a name="cross-platform-development"></a>Desenvolvimento de plataforma

Tempo de execução 1. x suporta desenvolvimento e alojamento de apenas no portal ou no Windows. Tempo de execução 2 é executado no .NET Core, o que significa que pode ser executado em todas as plataformas suportadas pelo .NET Core, incluindo macOS e Linux. Isto permite que o desenvolvimento de plataforma e cenários de alojamento que não são possíveis com 1. x.

## <a name="languages"></a>Linguagens

Tempo de execução 2. x utiliza um novo modelo de extensibilidade do idioma. Inicialmente, JavaScript e Java são tirar partido deste novo modelo. Idiomas experimental do Azure funções 1. x ainda não foi atualizados para utilizar o novo modelo, pelo que não são suportados em 2. x. A tabela seguinte indica as linguagens de programação são suportadas em cada versão do tempo de execução.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Para obter mais informações, consulte [idiomas suportados](supported-languages.md).

## <a name="bindings"></a>Enlaces 

Tempo de execução 2. x utiliza um novo [modelo de extensibilidade do enlace](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) que oferece as seguintes vantagens:

* Suporte para extensões de enlace de terceiros.
* Desemparelhamento de tempo de execução e enlaces. Isto permite que as extensões de enlace com a versão e lançada de forma independente. Por exemplo, pode optar por atualizar para uma versão de uma extensão que depende de uma versão mais recente de um SDK subjacente.
* Um mais ligeira ambiente de execução, onde apenas os enlaces utilizados são conhecidos e carregados pelo runtime.

Todos os enlaces de funções do Azure incorporados ADOTARAM este modelo e já não estão incluídos por predefinição, exceto para o acionador de temporizador e o acionador HTTP. As extensões são instaladas automaticamente quando criar funções através de ferramentas, como o Visual Studio ou através do portal.

A tabela seguinte indica os enlaces são suportadas em cada versão do tempo de execução.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="known-issues-in-2x"></a>Problemas conhecidos no 2. x

Para obter mais informações sobre suporte Enlaces e outras funcionais lacunas na 2. x, consulte [2.0 Runtime problemas conhecidos](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [O tempo de execução 2.0 no seu ambiente de desenvolvimento local de destino](functions-run-local.md)

> [!div class="nextstepaction"]
> [Consulte as notas de versão para versões de tempo de execução](https://github.com/Azure/azure-webjobs-sdk-script/releases)