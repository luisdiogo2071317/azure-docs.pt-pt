---
title: Implementar um executável existente para o Azure Service Fabric | Microsoft Docs
description: Saiba mais sobre empacotamento uma aplicação existente como convidado executável, pelo que pode ser implementada para um cluster do Service Fabric.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: ''
ms.assetid: d799c1c6-75eb-4b8a-9f94-bf4f3dadf4c3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 03/15/2018
ms.author: mfussell
ms.openlocfilehash: cdaf3dae12c2c9da1f6bcbebbff560b98e62bade
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="deploy-an-existing-executable-to-service-fabric"></a>Implementar um executável existente para o Service Fabric
Pode executar qualquer tipo de código, tal como o Node.js, Java ou C++ no Service Fabric do Azure como um serviço. Service Fabric refere-se para estes tipos de serviços como convidado executáveis.

Executáveis de convidado são tratadas pelo serviço de recursos de infraestrutura, como serviços sem monitorização de estado. Como resultado, são colocadas em nós num cluster, com base na disponibilidade e outras métricas. Este artigo descreve como o pacote e implementar um executável de convidado para um cluster do Service Fabric, utilizando o Visual Studio ou um utilitário da linha de comandos.

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Benefícios da execução de um convidado executável no Service Fabric
Existem várias vantagens à execução de um convidado executável num cluster de Service Fabric:

* Elevada disponibilidade. Aplicações executadas no Service Fabric que são efetuadas altamente disponíveis. Service Fabric assegura que as instâncias de uma aplicação estão em execução.
* Monitorização de estado de funcionamento. Monitorização de estado de funcionamento do Service Fabric Deteta se uma aplicação está em execução e fornece informações de diagnóstico, se ocorrer uma falha.   
* Gestão de ciclo de vida de aplicações. Para além de fornecer atualizações sem período de indisponibilidade, o Service Fabric fornece Reversão automática para a versão anterior se houver um evento de estado de funcionamento incorreto comunicado durante uma atualização.    
* Densidade. Pode executar várias aplicações num cluster, que elimina a necessidade de cada aplicação para ser executada no seu próprio hardware.
* Capacidade de Deteção: Utilizar REST pode chamar o serviço de nomenclatura de recursos de infraestrutura de serviço para localizar outros serviços em cluster. 

## <a name="samples"></a>Amostras
* [Exemplo de empacotamento e implementação de um executável de convidado](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Exemplo de dois convidado executáveis (c# e nodejs) comunicar através do serviço de nomes através de REST](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Descrição geral da aplicação e ficheiros de manifesto do serviço
Como parte da implementação de um executável de convidado, é útil para entender o modelo de empacotamento e implementação do Service Fabric, conforme descrito em [modelo de aplicação](service-fabric-application-model.md). O modelo de empacotamento de Service Fabric baseia-se em dois ficheiros XML: os manifestos da aplicação e serviços. A definição de esquema para os ficheiros ApplicationManifest.xml e ServiceManifest.xml é instalada com o SDK de Service Fabric para *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

* **O manifesto da aplicação** o manifesto da aplicação utilizado para descrever a aplicação. Lista os serviços que compõem-lo e outros parâmetros que são utilizados para definir como um ou mais serviços devem ser implementados, como o número de instâncias.

  No Service Fabric, uma aplicação é uma unidade de implementação e a atualização. É possível atualizar uma aplicação como uma única unidade em que é geridos os potenciais falhas e reverte potenciais. Service Fabric garante que o processo de atualização é concluída com êxito, ou, se a atualização falhar, deixe a aplicação num Estado desconhecido ou instável.
* **O manifesto do serviço** o manifesto do serviço descreve os componentes de um serviço. Inclui dados, tais como o nome e tipo de serviço e o código e a configuração. O manifesto do serviço também inclui alguns parâmetros adicionais que podem ser utilizados para configurar o serviço uma vez que é implementado.

## <a name="application-package-file-structure"></a>Estrutura de ficheiros do pacote de aplicação
Para implementar uma aplicação de Service Fabric, a aplicação deve seguir a estrutura de diretórios predefinidos. Segue-se um exemplo dessa estrutura.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

O ApplicationPackageRoot contém o ficheiro de ApplicationManifest.xml que define a aplicação. Um subdiretório para cada serviço incluído na aplicação é utilizado para conter todos os artefactos requer que o serviço. Estes subdiretórios são o ServiceManifest.xml e, normalmente, o seguinte:

* *Código*. Este diretório contém o código do serviço.
* *Configuração*. Este diretório contém um ficheiro de Settings.xml (e outros ficheiros, se necessário) que o serviço pode aceder no tempo de execução para obter as definições de configuração específicos.
* *Dados*. Este é um diretório adicional para armazenar os dados locais adicionais que poderá ter o serviço. Dados devem ser utilizados para armazenar apenas os dados de efémeras. Service Fabric não copie ou replicar as alterações para o diretório de dados se o serviço tem de ser relocalizada (por exemplo, durante a ativação pós-falha).

> [!NOTE]
> Não tem de criar o `config` e `data` diretórios se não precisar deles.
>
>

## <a name="next-steps"></a>Passos Seguintes
Consulte os artigos seguintes para informações relacionadas e tarefas.
* [Implementar um executável convidado](service-fabric-deploy-existing-app.md)
* [Implementar vários executáveis convidados](service-fabric-deploy-multiple-apps.md)
* [Criar a primeira aplicação executável de convidados utilizando o Visual Studio](quickstart-guest-app.md)
* [Exemplo de empacotamento e implementação de um executável de convidado](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started), incluindo uma hiperligação para a pré-lançamento da ferramenta de empacotamento
* [Exemplo de dois convidado executáveis (c# e nodejs) comunicar através do serviço de nomes através de REST](https://github.com/Azure-Samples/service-fabric-containers)

