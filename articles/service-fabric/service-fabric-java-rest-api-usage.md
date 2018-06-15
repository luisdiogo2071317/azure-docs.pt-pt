---
title: APIs de cliente de Java de recursos de infraestrutura de serviço do Azure | Microsoft Docs
description: Gerar e utilizar as APIs do cliente de Java de recursos de infraestrutura de serviço com a especificação de REST API do cliente de Service Fabric
services: service-fabric
documentationcenter: java
author: rapatchi
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/27/2017
ms.author: rapatchi
ms.openlocfilehash: bc5302b9af023b04fb6b1654ac13e9fc02e0a465
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206271"
---
# <a name="azure-service-fabric-java-client-apis"></a>APIs de cliente de Java de recursos de infraestrutura de serviço do Azure

Cliente de Service Fabric APIs permite implementar e gerir micro-serviços com base em aplicações e contentores de recursos de infraestrutura do serviço de cluster no Azure, no local, na máquina de desenvolvimento local ou na outra nuvem. Este artigo descreve como gerar e utilizar o Service Fabric Java APIs cliente sobre o cliente de Service Fabric REST APIs

## <a name="generate-the-client-code-using-autorest"></a>Gerar o código de cliente utilizando o AutoRest

[AutoRest](https://github.com/Azure/autorest) é uma ferramenta que gera bibliotecas de cliente para aceder aos serviços RESTful web. A entrada para AutoRest é uma especificação que descrevem a API de REST utilizando o formato de especificação de OpenAPI. [Cliente de Service Fabric REST APIs](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/servicefabric/data-plane) siga esta especificação.

Siga os passos mencionados abaixo para gerar código de cliente de Java de recursos de infraestrutura de serviço utilizando a ferramenta de AutoRest.

1. Instalar nodejs e NPM no seu computador

    Se estiver a utilizar, em seguida, Linux:
    ```bash
    sudo apt-get install npm
    sudo apt install nodejs
    ```
    Se estiver a utilizar, em seguida, Mac OS X:
    ```bash
    brew install node
    ```

2. Instale AutoRest utilizando NPM.
    ```bash
    npm install -g autorest
    ```

3. Bifurcação e clone [azure-rest-api-especificações](https://github.com/Azure/azure-rest-api-specs) repositório no seu computador local e vá para a localização clonada no terminal da sua máquina.


4. Vá para a localização mencionada abaixo no seu repositório clonado.
    ```bash
    cd specification\servicefabric\data-plane\Microsoft.ServiceFabric\stable\6.0
    ```

    > [!NOTE]
    > Se a sua versão do cluster não for 6.0. *, em seguida, vá para o diretório adequado na pasta estável.
    >   

5. Execute o seguinte comando autorest para gerar o código de cliente de java.
    
    ```bash
    autorest --input-file= servicefabric.json --java --output-folder=[output-folder-name] --namespace=[namespace-of-generated-client]
    ```
   Abaixo está um exemplo que demonstra a utilização de autorest.
   
    ```bash
    autorest --input-file=servicefabric.json --java --output-folder=java-rest-api-code --namespace=servicefabricrest
    ```
   
   O seguinte comando demora ``servicefabric.json`` especificação do ficheiro como entrada e gera o código de cliente de java no ``java-rest-api-     code`` pasta e encloses o código no ``servicefabricrest`` espaço de nomes. Após este passo poderia localizar duas pastas ``models``, ``implemenation`` e dois ficheiros ``ServiceFabricClientAPIs.java`` e ``package-info.java`` gerados pelo ``java-rest-api-code`` pasta.


## <a name="include-and-use-the-generated-client-in-your-project"></a>Incluir e utilizar o cliente gerado no seu projeto

1. Adicione o código gerado corretamente no seu projeto. Recomendamos que crie uma biblioteca com o código gerado e incluir esta biblioteca no seu projeto.
2. Se estiver a criar uma biblioteca de incluir a seguinte dependência no projeto da biblioteca. Se estiver a seguir uma abordagem diferente, em seguida, inclua a dependência adequadamente.

    ```
        GroupId:  com.microsoft.rest
        Artifactid: client-runtime
        Version: 1.2.1
    ```
    Por exemplo, se estiver a utilizar Maven sistema de compilação incluem o seguinte na sua ``pom.xml`` ficheiro:

    ```xml
        <dependency>
          <groupId>com.microsoft.rest</groupId>
          <artifactId>client-runtime</artifactId>
          <version>1.2.1</version>
        </dependency>
    ```

3. Crie um RestClient utilizando o seguinte código:

    ```java
        RestClient simpleClient = new RestClient.Builder()
            .withBaseUrl("http://<cluster-ip or name:port>")
            .withResponseBuilderFactory(new ServiceResponseBuilder.Factory())
            .withSerializerAdapter(new JacksonAdapter())
            .build();
        ServiceFabricClientAPIs client = new ServiceFabricClientAPIsImpl(simpleClient);
    ```
4. Utilize o objeto de cliente e efetuar as chamadas adequadas conforme necessário. Seguem-se alguns exemplos que demonstram a utilização de objeto de cliente. Iremos partem do princípio de que o pacote de aplicação é criado e carregado para o arquivo de imagens antes de utilizar a abaixo da API.
    * Aprovisionar uma aplicação
    
        ```java
            ApplicationTypeImageStorePath imageStorePath = new ApplicationTypeImageStorePath();
            imageStorePath.withApplicationTypeBuildPath("<application-path-in-image-store>");
            client.provisionApplicationType(imageStorePath);
        ```
    * Criar uma aplicação

        ```java
            ApplicationDescription applicationDescription = new ApplicationDescription();
            applicationDescription.withName("<application-uri>");
            applicationDescription.withTypeName("<application-type>");
            applicationDescription.withTypeVersion("<application-version>");
            client.createApplication(applicationDescription);
        ```

## <a name="understanding-the-generated-code"></a>Noções sobre o código gerado
Para cada API encontrará quatro sobrecargas de implementação. Se existirem parâmetros opcionais poderia localizar quatro variações mais, incluindo os parâmetros opcionais. Por exemplo, considere a API ``removeReplica``.
 1. **removeReplica void público (cadeia nodeName, UUID partitionId, cadeia replicaId, forceRemove booleano, tempo limite de tempo)**
    * Esta é a variante síncrona da chamada removeReplica API
 2. **ServiceFuture pública<Void> removeReplicaAsync (cadeia nodeName, UUID partitionId, cadeia replicaId, forceRemove booleano, tempo limite de tempo, final ServiceCallback<Void> serviceCallback)**
    * Este variante de chamada à API pode ser utilizado se pretender utilizar futura programação assíncrona com base e utilizar as chamadas de retorno
 3. **Observable pública<Void> removeReplicaAsync (cadeia nodeName, UUID partitionId, replicaId de cadeia)**
    * Este variante de chamada à API pode ser utilizado se quiser utilizar reativa programação assíncrona
 4. **Observable pública < ServiceResponse<Void>> removeReplicaWithServiceResponseAsync (cadeia nodeName, UUID partitionId, replicaId de cadeia)**
    * Este variante de chamada à API pode ser utilizado se pretender utilizar programação assíncrona reativa e lidar com a resposta de rest não processados

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [Service Fabric REST APIs](https://docs.microsoft.com/rest/api/servicefabric/)

