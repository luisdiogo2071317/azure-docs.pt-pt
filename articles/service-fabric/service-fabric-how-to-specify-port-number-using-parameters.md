---
title: Como especificar o número de porta de um serviço utilizando parâmetros no Service Fabric do Azure | Microsoft Docs
description: Mostra como utilizar os parâmetros para especificar a porta para uma aplicação no Service Fabric
documentationcenter: .net
author: mikkelhegn
manager: markfuss
editor: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: 06cfb375c6c18082a0d0316cfcb742a7779fc8a8
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-specify-the-port-number-of-a-service-using-parameters-in-service-fabric"></a>Como especificar o número de porta de um serviço utilizando parâmetros no Service Fabric

Este artigo mostra como especificar o número de porta de um serviço utilizando parâmetros no Service Fabric com o Visual Studio.

## <a name="procedure-for-specifying-the-port-number-of-a-service-using-parameters"></a>Procedimento para especificar o número de porta de um serviço utilizando parâmetros

Neste exemplo, definir o número de porta para o asp.net core API web utilizando um parâmetro.

1. Abra o Visual Studio e crie uma nova aplicação de Service Fabric.
1. Escolha o modelo sem monitorização de estado ASP.NET Core.
1. Escolha a Web API.
1. Abra o ficheiro ServiceManifest.xml.
1. Tome nota do nome do ponto final especificado para o seu serviço. A predefinição é `ServiceEndpoint`.
1. Abra o ficheiro ApplicationManifest.xml
1. No `ServiceManifestImport` elemento, adicione um novo `RessourceOverrides` elemento com uma referência para o ponto final no seu ficheiro ServiceManifest.xml.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. No `Endpoint` elemento, agora pode substituir qualquer atributo utilizando um parâmetro. Neste exemplo, especificou `Port` e defina-o como um nome de parâmetro utilizando parênteses Retos - por exemplo, `[MyWebAPI_PortNumber]`

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="Web1Pkg" ServiceManifestVersion="1.0.0" />
        <ResourceOverrides>
          <Endpoints>
            <Endpoint Name="ServiceEndpoint" Port="[MyWebAPI_PortNumber]"/>
          </Endpoints>
        </ResourceOverrides>
        <ConfigOverrides />
      </ServiceManifestImport>
    ```

1. Ainda no ficheiro ApplicationManifest.xml, em seguida, especifique o parâmetro no `Parameters` elemento

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" />
      </Parameters>
    ```

1. E definir um `DefaultValue`

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" DefaultValue="8080" />
      </Parameters>
    ```

1. Abra a pasta de ApplicationParameters e `Cloud.xml` ficheiro
1. Para especificar uma porta diferente para ser utilizado quando publicar a um cluster remoto, adicione o parâmetro com o número de porta para este ficheiro.

    ```xml
      <Parameters>
        <Parameter Name="MyWebAPI_PortNumber" DefaultValue="80" />
      </Parameters>
    ```

Quando publicar a aplicação a partir do Visual Studio com o Cloud.xml um perfil de publicação, o serviço está configurado para utilizar a porta 80. Se implementar a aplicação sem especificar o parâmetro MyWebAPI_PortNumber, o serviço utiliza a porta 8080.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre alguns dos conceitos principais que são abordados neste artigo, consulte o [gerir aplicações em vários artigos ambientes](service-fabric-manage-multiple-environment-app-configuration.md).

Para obter informações sobre outras capacidades de gestão de aplicações que estão disponíveis no Visual Studio, consulte [gerir as aplicações de Service Fabric no Visual Studio](service-fabric-manage-application-in-visual-studio.md).