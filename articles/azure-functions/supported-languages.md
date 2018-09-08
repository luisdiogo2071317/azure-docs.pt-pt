---
title: Idiomas suportados nas funções do Azure
description: Saiba que idiomas são suportados (GA) e que são experimental ou em pré-visualização.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 08/02/2018
ms.author: glenga
ms.openlocfilehash: b735f93b2d7ad093ef752fd5f26be729a1157b37
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44090685"
---
# <a name="supported-languages-in-azure-functions"></a>Idiomas suportados nas funções do Azure

Este artigo explica que os níveis de suporte oferecido para idiomas que pode usar com as funções do Azure.

## <a name="levels-of-support"></a>Níveis de suporte

Existem três níveis de suporte:

* **Disponibilidade geral (GA)** - totalmente suportado e aprovada para utilização em produção.
* **Pré-visualização** - ainda não suportado, mas é esperado para alcançar o estado de disponibilidade geral no futuro.
* **Experimental** – não suportado e pode ser abandonada no futuro; nenhuma garantia de pré-visualização eventual ou estado de disponibilidade geral.

## <a name="languages-in-runtime-1x-and-2x"></a>Linguagens no runtime 1.x e 2.x

[Duas versões do runtime das funções do Azure](functions-versions.md) estão disponíveis. O runtime 1.x é GA. É o tempo de execução único que esteja aprovado para aplicações de produção. O runtime 2.x está atualmente em pré-visualização, pelo que os idiomas oferece suporte a estão em pré-visualização. A tabela seguinte mostra que idiomas são suportados em cada versão de runtime.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

### <a name="experimental-languages"></a>Linguagens experimentais

As linguagens experimentais na versão 1.x não dimensiona bem e não oferecem suporte a todas as ligações. Por exemplo, Python é lento porque o runtime das funções é executado *python.exe* com cada invocação de função. E embora Python suporte Enlaces de HTTP, não pode acessar o objeto de solicitação.

Experimental suporte para o PowerShell está limitado a versão 5.1, uma vez que é o que é instalado por predefinição nas VMs em que aplicações de função a executar. Se quiser executar scripts do PowerShell, considere [automatização do Azure](https://azure.microsoft.com/services/automation/).

Se pretender utilizar um dos idiomas que só estão disponíveis no 1.x, mantenha-se no 1.x runtime. Mas não utilize linguagens experimentais para tudo o que contar, porque não existe nenhum suporte oficial para eles. Pode pedir ajuda ao [criar problemas do GitHub](https://github.com/Azure/azure-webjobs-sdk-script/issues), mas não devem ser abertos incidentes de suporte para problemas com linguagens experimentais. 

A versão 2.x do runtime não suporta linguagens experimentais. Suporte para novos idiomas é adicionado apenas quando o idioma pode ser suportado em produção. 

### <a name="language-extensibility"></a>Extensibilidade de idioma

O runtime 2.x é projetado para oferecer [extensibilidade da linguagem](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Language-Extensibility). Entre os idiomas primeiro seja baseada nessa extensibilidade o modelo é Java, que está em pré-visualização na 2.x.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre como utilizar uma das linguagens de disponibilidade geral ou de pré-visualização nas funções do Azure, consulte os seguintes recursos:

> [!div class="nextstepaction"]
> [C#](functions-reference-csharp.md)

> [!div class="nextstepaction"]
> [F#](functions-reference-fsharp.md)

> [!div class="nextstepaction"]
> [JavaScript](functions-reference-node.md)

> [!div class="nextstepaction"]
> [Java](functions-reference-java.md)
