---
title: Desenvolver e executar as funções do Azure localmente | Microsoft Docs
description: Saiba como code e testar as funções do Azure no seu computador local antes de executar em funções do Azure.
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
ms.openlocfilehash: 89f94be4cf624914183480362ae23c62c363622f
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37088637"
---
# <a name="code-and-test-azure-functions-locally"></a>Código e teste das funções do Azure localmente

Enquanto poderá desenvolver e testar as funções do Azure no [portal do Azure], os programadores muitos preferem uma experiência de desenvolvimento local. As funções torna mais fácil de utilizar as ferramentas de desenvolvimento e editor favorito código para criar e testar as funções no seu computador local. As suas funções locais podem ligar a serviços do Azure live e pode depurá-los no seu computador local utilizando o tempo de execução de funções completo.

## <a name="local-development-environments"></a>Ambientes de desenvolvimento local

A forma de desenvolver funções no seu computador local depende o [idioma](supported-languages.md) e preferências de ferramentas. Os ambientes na seguinte tabela suportem de desenvolvimento local:

|Ambiente                              |Linguagens         |Descrição|
|-----------------------------------------|------------|---|
| [Linha de comandos ou de terminal](functions-run-local.md) | [Script do c# (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | [Ferramentas de núcleos de funções do Azure] fornece o tempo de execução de núcleos e modelos para criar funções, que permite um desenvolvimento local. Desenvolvimento de suporte de 2. x de versão no Linux, MacOS e Windows. Todos os ambientes baseiam-se as ferramentas de núcleo para o tempo de execução local do funções.|
|[Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started)| [Script do c# (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md) | O [extensão de funções do Azure para o VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) adiciona o suporte de funções para o VS Code. Requer que as ferramentas de núcleos. Suporta a programação em Linux, MacOS e Windows, ao utilizar a versão 2. x das ferramentas principal. Para obter mais informações, consulte [implementar no Azure utilizando as funções do Azure](https://code.visualstudio.com/tutorials/functions-extension/getting-started).  |
| [Visual Studio 2017](functions-develop-vs.md) | [C# (biblioteca de classes)](functions-dotnet-class-library.md) | As ferramentas de funções do Azure estão incluídas no **programação do Azure** carga de trabalho de [Visual Studio 2017 versão 15.5](https://www.visualstudio.com/vs/) e versões posteriores. Permite-lhe compilar funções na biblioteca de classes e publicar o ficheiro. dll para o Azure. Inclui as ferramentas de núcleos para fins de teste local. Para obter mais informações, consulte [desenvolver as funções do Azure com o Visual Studio](functions-develop-vs.md). |
| [Maven](functions-create-first-java-maven.md) | [Java](functions-reference-java.md) | Integra-se com ferramentas de núcleo para permitir o desenvolvimento das funções de Java. Versão 2 suporta programação em Linux, MacOS e Windows. Para obter mais informações, consulte [criar a sua primeira função com o Java e Maven](functions-create-first-java-maven.md).|

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Cada um destes ambientes de desenvolvimento local permite-lhe criar uma função de projetos de aplicação e utilizar modelos predefinidos de funções para criar novas funções. Cada utiliza as ferramentas de núcleos para que possa testar e depurar as suas funções contra o tempo de execução de funções real na sua própria máquina, tal como faria com qualquer outra aplicação. Também pode publicar projeto de aplicação de função de qualquer um destes ambientes para o Azure.  

## <a name="next-steps"></a>Passos Seguintes

+ Para saber mais sobre o desenvolvimento local de c# as funções compiladas com 2017 do Visual Studio, consulte o artigo [desenvolver as funções do Azure com o Visual Studio](functions-develop-vs.md).
+ Para saber mais sobre o desenvolvimento local de funções utilizando o VS Code num computador Mac, Linux ou Windows, consulte o [documentação VS Code para as funções do Azure](https://code.visualstudio.com/tutorials/functions-extension/getting-started).
+ Para obter mais informações sobre como desenvolver funções a partir da linha de comandos ou terminal, consulte [trabalhar com ferramentas de núcleos de funções do Azure](functions-run-local.md).

<!-- LINKS -->

[Ferramentas de núcleos de funções do Azure]: https://www.npmjs.com/package/azure-functions-core-tools
[Portal do Azure]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
