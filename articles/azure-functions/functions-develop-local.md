---
title: Desenvolver e executar as funções do Azure localmente | Documentos da Microsoft
description: Aprenda a codificar e testar as funções do Azure no seu computador local antes de executá-las nas funções do Azure.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/27/2018
ms.author: glenga
ms.openlocfilehash: 4425c0594f4a3520f780b723d2ffbd41fc4b62c7
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "42055930"
---
# <a name="code-and-test-azure-functions-locally"></a>Codificar e testar as funções do Azure localmente

Enquanto pode desenvolver e testar as funções do Azure no [portal do Azure], muitos desenvolvedores preferem uma experiência de desenvolvimento local. As funções torna mais fácil usar suas ferramentas de desenvolvimento e editor de código preferido para criar e testar as funções no seu computador local. As suas funções locais podem ligar-se ao vivo de serviços do Azure e pode depurá-los no seu computador local com o tempo de execução de funções completo.

## <a name="local-development-environments"></a>Ambientes de desenvolvimento local

A forma de desenvolver as funções no seu computador local depende da sua [linguagem](supported-languages.md) e preferências de ferramentas. Os ambientes na seguinte tabela ofereça suporte ao desenvolvimento local:

|Ambiente                              |Linguagens         |Descrição|
|-----------------------------------------|------------|---|
| [Linha de comandos ou terminal](functions-run-local.md) | [C# (biblioteca de classes)](functions-dotnet-class-library.md), [script c# (. csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | [Ferramentas de núcleo das funções do Azure] fornece o tempo de execução de núcleo e modelos para a criação de funções, que permite o desenvolvimento local. Versão 2.x o desenvolvimento de suporta no Linux, MacOS e Windows. Todos os ambientes contam com ferramentas de núcleo para o runtime das funções local. |
|[Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started)| [Script c# (. csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | O [extensão de funções do Azure para VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) adiciona as funções de suporte para o VS Code. Requer as ferramentas de núcleo. Oferece suporte ao desenvolvimento no Linux, MacOS e Windows, ao utilizar a versão 2.x das ferramentas de núcleo. Para obter mais informações, consulte [implementar no Azure com as funções do Azure](https://code.visualstudio.com/tutorials/functions-extension/getting-started).  |
| [Visual Studio 2017](functions-develop-vs.md) | [C# (biblioteca de classes)](functions-dotnet-class-library.md) | As ferramentas de funções do Azure estão incluídas na **desenvolvimento do Azure** carga de trabalho de [Visual Studio 2017 versão 15.5](https://www.visualstudio.com/vs/) e versões posteriores. Permite-lhe compilar as funções numa biblioteca de classes e publicar o ficheiro. dll para o Azure. Inclui as ferramentas essenciais para a realização de testes locais. Para obter mais informações, consulte [desenvolver as funções do Azure com o Visual Studio](functions-develop-vs.md). |
| [Maven](functions-create-first-java-maven.md) | [Java](functions-reference-java.md) | Integra-se com as ferramentas de núcleo para permitir um desenvolvimento das funções do Java. Versão 2.x o desenvolvimento de suporta no Linux, MacOS e Windows. Para obter mais informações, consulte [criar a primeira função com o Java e Maven](functions-create-first-java-maven.md).|

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Cada um desses ambientes de desenvolvimento local permite-lhe criar função os projetos de aplicativos e usar modelos predefinidos de funções para criar novas funções. Cada usa as ferramentas essenciais para que possa testar e depurar as suas funções contra o runtime das funções real no seu computador, tal como faria com qualquer outra aplicação. Também pode publicar projeto de aplicação de função de qualquer um desses ambientes para o Azure.  

## <a name="next-steps"></a>Passos Seguintes

+ Para saber mais sobre o desenvolvimento local do c# as funções compiladas com o Visual Studio 2017, veja [desenvolver as funções do Azure com o Visual Studio](functions-develop-vs.md).
+ Para saber mais sobre o desenvolvimento local das funções de utilizar o VS Code num computador Mac, Linux ou Windows, consulte a [documentação das funções do Azure do VS Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started).
+ Para saber mais sobre o desenvolvimento das funções a partir da linha de comandos ou terminal, veja [trabalhar com as ferramentas de núcleo de funções do Azure](functions-run-local.md).

<!-- LINKS -->

[Ferramentas de núcleo das funções do Azure]: https://www.npmjs.com/package/azure-functions-core-tools
[Portal do Azure]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
