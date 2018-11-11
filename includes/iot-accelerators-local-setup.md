---
title: incluir ficheiro
description: incluir ficheiro
services: iot-accelerators
author: avneet723
ms.service: iot-accelerators
ms.topic: include
ms.date: 10/29/2018
ms.author: avneet723
ms.custom: include file
ms.openlocfilehash: 900d75f826830ea7336044a892506d3bec546e30
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283935"
---
## <a name="download-the-source-code"></a>Baixe o código-fonte

Os repositórios de código de origem monitorização remota incluem os ficheiros de configuração do Docker que tem de executar os microsserviços imagens do Docker.

Para clonar e criar uma versão local do repositório, utilize o seu ambiente de linha de comandos para navegar para uma pasta adequada no seu computador local. Em seguida, execute um dos seguintes conjuntos de comandos para clonar o repositório do .NET ou Java:

Para transferir a versão mais recente das implementações de microsserviços .NET, execute:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-dotnet
git submodule foreach git pull origin master
```

Para transferir a versão mais recente das implementações de microsserviços Java, execute:

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> Estes comandos Baixe o código-fonte para todos os microsserviços, além dos scripts que utiliza para executar os microsserviços localmente. Apesar de não precisar do código-fonte para executar os microsserviços no Docker, o código-fonte é útil se pretender mais tarde modificar o acelerador de solução e testar as suas alterações localmente.

## <a name="deploy-the-azure-services"></a>Implementar os serviços do Azure

Embora este artigo mostra como executar os microsserviços localmente, dependem de serviços do Azure em execução na cloud. Utilize o seguinte script para implementar os serviços do Azure. Os exemplos de script seguintes partem do princípio de que estiver a utilizar o repositório .NET num computador Windows. Se estiver trabalhando em outro ambiente, ajuste adequadamente os caminhos, extensões de ficheiro e os separadores de caminho.

### <a name="create-new-azure-resources"></a>Criar novos recursos do Azure

Se ainda não criou os recursos do Azure necessários, siga estes passos:

1. No seu ambiente de linha de comandos, navegue para o **\services\scripts\local\launch** pasta na sua cópia clonada do repositório.

1. Execute os seguintes comandos para instalar o **pcs** CLI a ferramenta e inicie sessão na sua conta do Azure:

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. Executar o **start.cmd** script. O script pede-lhe as seguintes informações:
    * Um nome de solução.
    * A subscrição do Azure que deve utilizar.
    * A localização do datacenter do Azure para utilizar.

    O script cria o grupo de recursos no Azure com o nome da sua solução. Este grupo de recursos contém os recursos do Azure que utiliza o solution accelerator. Pode eliminar este grupo de recursos assim que não precisa mais os recursos correspondentes.

    O script também adiciona um conjunto de variáveis de ambiente com um prefixo **PCS** no seu computador local. Quando inicia os contentores do Docker localmente, eles leem seus valores de configuração destas variáveis de ambiente.

> [!TIP]
> Quando o script tiver concluído, ele exibe uma lista de variáveis de ambiente. Se salvar esses valores para o **serviços\\scripts\\local\\. env** arquivo, pode usá-los para implementações de acelerador de solução futuras. Tenha em atenção que todas as variáveis de ambiente definidas no seu computador local substituem valores a **serviços\\scripts\\local\\. env** ficheiro ao executar **docker-compose**.

### <a name="use-existing-azure-resources"></a>Utilizar recursos do Azure existentes

Se já tiver criado os recursos do Azure necessários, crie as variáveis de ambiente correspondente no seu computador local. Pode guardar estes valores no **serviços\\scripts\\local\\. env** ficheiro como parte da implementação do último. Tenha em atenção que as variáveis de ambiente definidas no seu computador local substituem valores a **serviços\\scripts\\local\\. env** ficheiro ao executar **docker-compose**.