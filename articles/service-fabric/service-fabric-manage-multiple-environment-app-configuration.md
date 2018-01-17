---
title: "Gerir aplicações para vários ambientes no Service Fabric do Azure | Microsoft Docs"
description: "Aplicações de Service Fabric do Azure podem ser executadas em clusters desse intervalo de tamanho de uma máquina para milhares de máquinas. Em alguns casos, irá querer configurar a sua aplicação para esses ambientes variadas. Este artigo abrange como definir parâmetros da aplicação diferente por ambiente."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: f406eac9-7271-4c37-a0d3-0a2957b60537
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/09/2017
ms.author: mikhegn
ms.openlocfilehash: 959fdb4aceee48863f3914d1b91f2bec6e256d6b
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2018
---
# <a name="manage-applications-for-multiple-environments"></a>Gerir aplicações para vários ambientes

Clusters de Service Fabric do Azure permitem-lhe criar clusters utilizando qualquer parte de um para muitos milhares máquinas. Na maioria dos casos, encontrará sozinho ter de implementar a sua aplicação através de várias configurações de cluster: o cluster de desenvolvimento local, um cluster de desenvolvimento partilhado e o cluster de produção. Todas estas clusters são consideradas ambientes diferentes, que o código tem de executar. Os binários da aplicação podem ser executada sem modificação entre este largo espetro, mas, muitas vezes, pretende configurar a aplicação de forma diferente.

Considere dois exemplos simples:
  - o serviço de escuta numa porta definida, mas tem essa porta ser diferente em todos os ambientes
  - tem de fornecer credenciais de enlace diferente para uma base de dados em todos os ambientes

## <a name="specifying-configuration"></a>Especificar a configuração

A configuração que fornecer pode ser dividida em duas categorias:

- Configuração aplica-se a forma como os serviços são executados
  - Por exemplo, o número de porta para um ponto final ou o número de instâncias de um serviço
  - Esta configuração é especificada no ficheiro de manifesto do serviço ou aplicação
- Configuração que se aplica ao código da aplicação
  - Por exemplo, as informações de enlace para uma base de dados
  - Esta configuração pode ser fornecida através de ficheiros de configuração ou variáveis de ambiente

> [!NOTE]
> Nem todos os atributos na aplicação e serviço de manifestam do ficheiro de parâmetros de suporte.
> Nesses casos, tem de confiar na substituindo cadeias como parte do seu fluxo de trabalho de implementação. No Visual Studio Team Services, pode utilizar uma extensão como substituir Tokens: https://marketplace.visualstudio.com/items?itemName=qetza.replacetokens ou no Jenkins conseguiu executar uma tarefa de script de substituir os valores.
>

## <a name="specifying-parameters-during-application-creation"></a>Especificar os parâmetros durante a criação da aplicação

Quando criar um instâncias com nome de aplicação no Service Fabric, terá a opção para passar os parâmetros. Da mesma forma, depende de como criar a instância da aplicação.

  - No PowerShell, o [ `New-ServiceFabricApplication` ](https://docs.microsoft.com/en-us/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet aceita os parâmetros da aplicação como uma tabela hash.
  - Utilizar sfctl, o [ `sfctl application create` ](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-create) comando aceita parâmetros como uma cadeia JSON. O script de install.sh utiliza sfctl.
  - Visual Studio fornece um conjunto de ficheiros de parâmetro na pasta de parâmetros no projeto de aplicação. Estes ficheiros de parâmetro são utilizados durante a publicação do Visual Studio, utilizando o serviço de equipa do Visual Studio Team Foundation Server. No Visual Studio, os ficheiros de parâmetro são que está a ser transmitidos para o script de FabricApplication.ps1 implementar.

## <a name="next-steps"></a>Passos Seguintes
Os artigos seguintes mostram como utilizar alguns dos conceitos descritos aqui:

- [Como especificar variáveis de ambiente para serviços no Service Fabric](service-fabric-how-to-specify-port-number-using-parameters.md)
- [Como especificar o número de porta de um serviço utilizando parâmetros no Service Fabric](service-fabric-how-to-specify-environment-variables.md)
- [Como parametrizar os ficheiros de configuração](service-fabric-how-to-parameterize-configuration-files.md)

- [Referência de variável de ambiente](service-fabric-environment-variables-reference.md)
