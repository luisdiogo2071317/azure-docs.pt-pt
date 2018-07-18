---
title: Criar uma aplicação de função do Azure com Java e IntelliJ | Documentos da Microsoft
description: Guia de procedimentos para criar e publicar um pedido de HTTP simple acionada por aplicações sem servidor com Java e o IntelliJ para as funções do Azure.
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: Azure funções, funções, processamento de eventos, computação, arquitetura sem servidor, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: e926bfb023fe3edfd564aa6389e21f6594bec169
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117505"
---
# <a name="create-your-first-function-with-java-and-intellij-preview"></a>Criar a primeira função com o Java e IntelliJ (pré-visualização)

> [!NOTE] 
> O Java para as Funções do Azure encontra-se atualmente em pré-visualização.

Este artigo mostra-lhe como criar uma [sem servidor](https://azure.microsoft.com/overview/serverless-computing/) projeto de função com o IntelliJ IDEA e o Apache Maven, testar e depurá-lo no seu computador a partir do IDE e implementá-la para as funções do Azure. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Configurar o ambiente de desenvolvimento

Para desenvolver uma aplicação de funções do Azure com Java e IntelliJ, tem de ter o seguinte instalado:

-  [Java Development Kit](https://www.azul.com/downloads/zulu/), versão 8.
-  [Apache Maven](https://maven.apache.org), versão 3.0 ou superior.
-  [IntelliJ IDEA](https://www.jetbrains.com/idea/download), Comunidade ou Ultimate com o Maven, as ferramentas.
-  [CLI do Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> A variável de ambiente do JAVA_HOME tem de ser definida para a localização de instalação do JDK para concluir este guia de introdução.

É altamente recomendado também instala [ferramentas de núcleo de funções do Azure, versão 2](functions-run-local.md#v2), que proporcionam um ambiente de desenvolvimento local para escrever, executar e depurar funções do Azure. 


## <a name="create-a-functions-project"></a>Criar um projeto de funções

1. No IntelliJ IDEA, clique para **criar novo projeto**.  
1. Selecione para criar a partir de **Maven**
1. Selecione a caixa de verificação **criar do mvn** e **adicionar Mvn** para o [azure-funções-mvn](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
    - GroupId: com.microsoft.azure
    - ArtifactId: azure-funções-mvn
    - Versão: Versão mais recente com uma utilização partir [o repositório central](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![criar IntelliJ Maven](media/functions-create-first-java-intellij/functions-create-intellij.png)  
1. Clique em **próxima** e introduza os detalhes do projeto atual e, eventualmente, **concluir**.

O Maven cria os ficheiros de projeto numa nova pasta com um nome de _artifactId_. O código gerado no projeto é um simples [acionada por HTTP](/azure/azure-functions/functions-bindings-http-webhook) função que ecoa o corpo da solicitação HTTP acionadora.

## <a name="run-functions-locally-in-the-ide"></a>Executar funções localmente no IDE

> [!NOTE]
> [Ferramentas de núcleo das funções do Azure, versão 2](functions-run-local.md#v2) tem de estar instalado para executar e depurar funções localmente.

1. Selecione para importar as alterações ou certifique-se de que [automaticamente importação](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html) está ativado.
1. Abra o **projetos do Maven** barra de ferramentas
1. No ciclo de vida, faça duplo clique em **pacote** para empacotar e criar a solução e crie um diretório de destino.
1. Em plug-ins -> clique duplo de funções do azure **azure-funções: run** para iniciar o tempo de execução local do funções do azure.  
  ![Barra de ferramentas do maven para as funções do Azure](media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

Fechar a caixa de diálogo de execução quando tiver terminado a testar a sua função. Anfitrião de apenas uma função pode ser ativa e em execução localmente ao mesmo tempo.

### <a name="debug-the-function-in-intellij"></a>Depurar a função no IntelliJ

Pode depurar funções no IntelliJ anexando para o anfitrião de função, após o arranque.  Executar a função do Azure localmente, utilizando os passos acima e, em seguida, no **execute** menu select **anexar a processo local**.  Deverá ver um processo na porta 5005 disponíveis.  Depois de anexar pode ter pontos de interrupção atingido e depurar na sua aplicação de função.

Quando terminar parar o depurador e o processo em execução. Anfitrião de apenas uma função pode ser ativa e em execução localmente no momento.

## <a name="deploy-the-function-to-azure"></a>Implementar a função no Azure

O processo de implementação para as Funções do Azure utiliza credenciais de conta a partir da CLI do Azure. [Inicie sessão com a CLI do Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) antes de continuar a utilizar a linha de comandos do seu computador.

```azurecli
az login
```

Implementar o seu código numa nova aplicação de funções com o `azure-functions:deploy` destino do Maven, ou selecione as funções do azure: implementar a opção na janela de projetos do Maven.

```
mvn azure-functions:deploy
```

Quando a implementação estiver concluída, verá o URL que pode utilizar para aceder à sua aplicação de funções do Azure:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

## <a name="next-steps"></a>Passos Seguintes

- Reveja o [Guia para programadores das Funções do Java](functions-reference-java.md) para obter mais informações sobre a programação de funções do Java.
- Adicione mais funções com diferentes acionadores ao seu projeto com o destino do Maven `azure-functions:add`.
