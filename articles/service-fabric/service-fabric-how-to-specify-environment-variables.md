---
title: Como especificar variáveis de ambiente para serviços no Azure Service Fabric | Microsoft Docs
description: Mostra como utilizar variáveis de ambiente para aplicações do Service Fabric
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
ms.openlocfilehash: 4325b3acd3cbc73ee5976021bebe96c267b2a6dd
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206033"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Como especificar variáveis de ambiente para serviços no Service Fabric

Este artigo mostra como especificar variáveis de ambiente para um serviço ou o contentor no Service Fabric.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Procedimento para especificar as variáveis de ambiente de serviços

Neste exemplo, definir uma variável de ambiente para um contentor. O artigo pressupõe que já tem um manifesto de aplicação e serviço.

1. Abra o ficheiro ServiceManifest.xml.
1. No `CodePackage` elemento, adicione um novo `EnvironmentVariables` elemento e um `EnvironmentVariable` elemento para cada variável de ambiente.

    ```xml
      <CodePackage Name="MyCode" Version="CodeVersion1">
      ...
        <EnvironmentVariables>
          <EnvironmentVariable Name="MyEnvVariable" Value="DeafultValue"/>
          <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentVariables>
      </CodePackage>
    ```

    As variáveis de ambiente podem ser substituídas no manifesto da aplicação.

1. Para substituir as variáveis de ambiente no manifesto da aplicação, utilize o `EnvironmentOverrides` elemento.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre alguns dos conceitos principais que são abordados neste artigo, consulte o [gerir aplicações em vários artigos ambientes](service-fabric-manage-multiple-environment-app-configuration.md).

Para obter informações sobre outras capacidades de gestão de aplicações que estão disponíveis no Visual Studio, consulte [gerir as aplicações de Service Fabric no Visual Studio](service-fabric-manage-application-in-visual-studio.md).