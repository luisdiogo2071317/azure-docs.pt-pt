---
title: "Como parametrizar os ficheiros de configuração no Service Fabric do Azure | Microsoft Docs"
description: "Mostra como parametrizar os ficheiros de configuração no Service Fabric"
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: 1e7d59ecb231440711b8ed3dc0b27a2b105890c4
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="how-to-parameterize-configuration-files-in-service-fabric"></a>Como parametrizar os ficheiros de configuração no Service Fabric

Este artigo mostra como parametrizar um ficheiro de configuração no Service Fabric.

## <a name="procedure-for-parameterizing-configuration-files"></a>Procedimento para parameterizing ficheiros de configuração

Neste exemplo, substituir um valor de configuração utilizando parâmetros na sua implementação de aplicação.

1. Abra o ficheiro Config\Settings.xml.
1. Defina um parâmetro de configuração, adicionando o seguinte XML:

    ```xml
      <Section Name="MyConfigSection">
        <Parameter Name="CacheSize" Value="25" />
      </Section>
    ```

1. Guarde e feche o ficheiro.
1. Abra o ficheiro `ApplicationManifest.xml`.
1. Adicionar um `ConfigOverride` elemento, o pacote de configuração, a secção e o parâmetro de referência.

      ```xml
        <ConfigOverrides>
          <ConfigOverride Name="Config">
              <Settings>
                <Section Name="MyConfigSection">
                    <Parameter Name="CacheSize" Value="[Stateless1_CacheSize]" />
                </Section>
              </Settings>
          </ConfigOverride>
        </ConfigOverrides>
      ```

1. Ainda no ficheiro ApplicationManifest.xml, em seguida, especifique o parâmetro no `Parameters` elemento

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" />
      </Parameters>
    ```

1. E definir um`DefaultValue`

    ```xml
      <Parameters>
        <Parameter Name="Stateless1_CacheSize" DefaultValue="80" />
      </Parameters>
    ```

> [!NOTE]
> O cenário onde adiciona um ConfigOverride, o Service Fabric escolhe sempre os parâmetros da aplicação ou o valor predefinido especificado no manifesto da aplicação.
>
>

Quando publicar a aplicação a partir do Visual Studio com o Cloud.xml um perfil de publicação, o serviço está configurado para utilizar a porta 80. Se implementar a aplicação sem especificar o parâmetro MyWebAPI_PortNumber, o serviço utiliza a porta 8080.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre alguns dos conceitos principais que são abordados neste artigo, consulte o [gerir aplicações em vários artigos ambientes](service-fabric-manage-multiple-environment-app-configuration.md).

Para obter informações sobre outras capacidades de gestão de aplicações que estão disponíveis no Visual Studio, consulte [gerir as aplicações de Service Fabric no Visual Studio](service-fabric-manage-application-in-visual-studio.md).