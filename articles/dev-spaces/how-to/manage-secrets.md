---
title: Como gerir segredos ao trabalhar com um espaço do programador do Azure | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: article
ms.technology: azds-kubernetes
description: Desenvolvimento rápido Kubernetes com contentores e micro-serviços no Azure
keywords: Docker, Kubernetes, Azure, AKS, serviço de contentor do Azure, contentores
manager: douge
ms.openlocfilehash: b77d862f578ddc374dbb58117b4ea58eb973e5fe
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Como gerir segredos ao trabalhar com um espaço do programador do Azure

Os serviços poderão ser necessários determinadas palavras-passe, as cadeias de ligação e outros segredos, tal como para bases de dados ou outros serviços do Azure segurados. Ao definir os valores destes segredos nos ficheiros de configuração, é possível disponibilizá-los no seu código como variáveis de ambiente.  Estas devem ser processadas com cuidado para evitar a comprometer a segurança dos segredos.

Os espaços de programador do Azure fornece duas opções para armazenar segredos de recomendado: values.dev.yaml, e inline diretamente no azds.yaml. Não é recomendado para armazenar segredos no values.yaml.
 
## <a name="method-1-valuesdevyaml"></a>Método 1: values.dev.yaml
1. Abra o VS Code com o projeto que está ativado para espaços de programador do Azure.
2. Adicionar um ficheiro chamado _values.dev.yaml_ na mesma pasta que existente _values.yaml_ e definir a chave secreta e valores, como no exemplo seguinte:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. Atualização _azds.yaml_ dizer espaços de programador do Azure para utilizar a nova _values.dev.yaml_ ficheiro. Para tal, adicione esta configuração na secção configurations.develop.container:

    ```yaml
           container:
             values:
             - "charts/webfrontend/values.dev.yaml"
    ```
 
4. Modificar o código do serviço de consultar estes segredos como variáveis de ambiente, como no exemplo seguinte:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Atualize os serviços em execução no seu cluster com estas alterações. Na linha de comandos, execute o comando:

    ```
    azds up
    ```
 
6. (Opcional) Na linha de comandos, verifique se estes segredos foram criados:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Certifique-se de que adiciona _values.dev.yaml_ para o _. gitignore_ ficheiros para evitar a consolidar segredos no controlo de origem.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Método 2: Inline diretamente no azds.yaml
1.  No _azds.yaml_, defina os segredos no yaml secção configurações/desenvolver/instalar. Apesar de poder introduzir segredo valores diretamente existe, não é recomendada porque _azds.yaml_ está marcada para o controlo de origem. Em vez disso, adicione os marcadores de posição utilizando a sintaxe "$PLACEHOLDER".

    ```yaml
    configurations:
      develop:
        ...
        install:
          set:
            secrets:
              redis:
                port: "$REDIS_PORT_DEV"
                host: "$REDIS_HOST_DEV"
                key: "$REDIS_KEY_DEV"
    ```
     
2.  Criar um _.env_ ficheiro na mesma pasta que _azds.yaml_. Introduza os segredos utilizando a chave padrão = notação de valor. Não consolidar o _.env_ ficheiro ao controlo de origem. (Omitir do controlo de origem nos sistemas de controlo de versão com base no git, adicione-o ao _. gitignore_ ficheiro.) O seguinte exemplo mostra um _.env_ ficheiro:

    ```
    REDIS_PORT_DEV=3333
    REDIS_HOST_DEV=myredishost
    REDIS_KEY_DEV=myrediskey
    ```
2.  Modificar o código de origem do serviço para fazer referência a estes segredos no código, como no exemplo seguinte:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Atualize os serviços em execução no seu cluster com estas alterações. Na linha de comandos, execute o comando:

    ```
    azds up
    ```

4.  (opcional) Segredos de vista de kubectl:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Passos Seguintes

Através destes métodos, pode agora ligar de forma segura para uma base de dados, uma cache de Redis ou acesso segura, serviços do Azure.
 