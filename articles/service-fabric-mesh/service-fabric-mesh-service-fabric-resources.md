---
title: Introdução ao modelo de recursos de recursos de infraestrutura do serviço do Azure | Documentos da Microsoft
description: Saiba mais sobre o modelo de recursos de recursos de infraestrutura de serviço, uma abordagem simplificada para definir a malha de recursos de infraestrutura do serviço de aplicações.
services: service-fabric-mesh
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/12/2018
ms.author: vturecek
ms.custom: mvc, devcenter
ms.openlocfilehash: 6a96995a46cd2ad0a1afe4f4afe49d62409f2b68
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076236"
---
# <a name="introduction-to-service-fabric-resource-model"></a>Introdução ao modelo de recursos do Service Fabric

Modelo de recursos do Service Fabric descreve uma abordagem simples para definir os recursos que compõem uma aplicação de malha do Service Fabric. Os seguintes tipos de recursos atualmente são suportados neste modelo:

- Aplicações
- Serviços
- Volumes
- Redes

Cada recurso é descrito de forma declarativa num arquivo de recurso, o que é um YAML simple ou um documento JSON que descreve a aplicação de malha e é aprovisionado pela plataforma do Service Fabric.

## <a name="resource-overview"></a>Descrição geral do recurso

### <a name="applications-and-services"></a>Aplicações e serviços

Um recurso de aplicação é a unidade de implementação, o controle de versão e o tempo de vida de um aplicativo de malha. Ele é composto por um ou mais, dos recursos de serviço que representam um microsserviço. Cada recurso de serviço, por sua vez, é composto por um ou mais pacotes de código que descrevem tudo necessários para executar a imagem de contentor associada ao pacote de código, incluindo o seguinte:

- Nome do contentor, versão e do Registro
- Recursos de CPU e memória necessários para cada contentor
- Pontos finais de rede
- Volumes para montar no contentor, fazendo referência um recurso de volume separado.

Todos os pacotes de código definidos como parte de um recurso de serviço são implementados e ativados em conjunto como um grupo. O recurso de serviço também descreve o número de instâncias de serviço para ser executado e também faz referência a outros recursos (por exemplo, o recurso de rede), que depende.

Se um aplicativo de malha é composto por mais de um serviço, não é garantido que seja executado em conjunto no mesmo nó. Além disso, durante uma atualização do aplicativo, falha de atualização de um único serviço resultará em todos os serviços que está a ser revertidos para a versão anterior.



```yaml
    services:
      - name: helloWorldService
        properties:
          description: Hello world service.
          osType: linux
          codePackages:
            - name: helloworld
              image: myapp:1.0-alpine
              resources:
                requests:
                  cpu: 2
                  memoryInGB: 2
              endpoints:
                - name: helloWorldEndpoint
                  port: 8080
    replicaCount: 3
    networkRefs:
      - name: mynetwork
```

Conforme mencionamos anteriormente, o ciclo de vida de cada instância da aplicação pode ser gerido de forma independente. Por exemplo, uma instância da aplicação pode ser atualizada independentemente de outras instâncias do aplicativo. Normalmente, mantenha o número de serviços num aplicativo bem pequeno, como os serviços mais que coloca num aplicativo, o mais difícil torna-se gerir cada serviço independente.

### <a name="networks"></a>Redes

Recurso de rede é um recurso implementável individualmente, independente de um recurso de aplicação ou serviço que pode fazer referência a ele como sua dependência. É utilizado para criar uma rede privada para as suas aplicações. Vários serviços de aplicativos diferentes podem fazer parte da mesma rede.

> [!NOTE]
> A pré-visualização atual só suporta um mapeamento um-para-um entre os aplicativos e de redes

### <a name="volumes"></a>Volumes

Volumes são diretórios que ser montados dentro as instâncias de contentor que pode utilizar para persistir o estado. O recurso de Volume é uma forma declarativa para descrever como um diretório está montado e o armazenamento de backup para o mesmo.

## <a name="programming-models"></a>Modelos de programação
Recurso de serviço requer apenas uma imagem de contentor para executar, o que é referenciada nos pacotes de código associado ao recurso. Pode executar qualquer código, escrito em qualquer linguagem, através de qualquer arquitetura dentro do contêiner sem a necessidade de saber ou utilizar o Service Fabric em malha APIs específicas. 

O código da aplicação permanece portátil fora de malha do Service Fabric e suas implementações de aplicações permanecem consistentes, independentemente da linguagem ou estrutura usada para implementar os seus serviços. Se seu aplicativo for ASP.NET Core, Go ou apenas um conjunto de processos e scripts, o modelo de implementação de recursos de malha de recursos de infraestrutura de serviço permanece igual. 

## <a name="deployment"></a>Implementação

Ao implementar a malha do Service Fabric, os recursos são implementados como modelos Azure Resource Manager para o Azure através de HTTP ou a CLI do Azure. 


## <a name="next-steps"></a>Passos Seguintes 
Para saber mais sobre a malha de recursos de infraestrutura do serviço, leia a visão geral:
- [Descrição geral de malha do Service Fabric](service-fabric-mesh-overview.md)
