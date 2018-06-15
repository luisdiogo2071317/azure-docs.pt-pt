---
title: Depurar a aplicação de recursos de infraestrutura de serviço do Azure no Eclipse | Microsoft Docs
description: Melhorar a fiabilidade e desempenho dos seus serviços, desenvolver e a depurá-los no Eclipse, num cluster de desenvolvimento local.
services: service-fabric
documentationcenter: .net
author: suhuruli
manager: timlt
editor: ''
ms.assetid: cb888532-bcdb-4e47-95e4-bfbb1f644da4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 0e9e816fa84816b1b5d12f066dc65aee7b4930f7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34205458"
---
# <a name="debug-your-java-service-fabric-application-using-eclipse"></a>Depurar a aplicação de Java Service Fabric com o Eclipse
> [!div class="op_single_selector"]
> * [Visual Studio/CSharp](service-fabric-debugging-your-application.md) 
> * [Eclipse/Java](service-fabric-debugging-your-application-java.md)
> 

1. Iniciar um cluster de desenvolvimento local, seguindo os passos no [como configurar o ambiente de desenvolvimento do Service Fabric](service-fabric-get-started-linux.md).

2. Atualize entryPoint.sh do serviço que pretende depurar, para que este inicia o processo de java com parâmetros de depuração remota. Este ficheiro pode ser encontrado na seguinte localização: ``ApplicationName\ServiceNamePkg\Code\entrypoint.sh``. A porta 8001 está definida para depuração neste exemplo.

    ```sh
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=y -Djava.library.path=$LD_LIBRARY_PATH -jar myapp.jar
    ```
3. Atualize o manifesto da aplicação, definindo a contagem de instâncias ou a contagem de réplica para o serviço que está a ser debugged como 1. Esta definição evita conflitos na porta utilizada para depuração. Por exemplo, para serviços sem estado, defina ``InstanceCount="1"`` e para serviços com estado, defina os tamanhos de destino e de réplica mínimos como 1 da seguinte forma: `` TargetReplicaSetSize="1" MinReplicaSetSize="1"``.

4. Implemente a aplicação.

5. No Eclipse IDE, selecione **executar -> configurações de depuração -> aplicação de Java remota e as propriedades de ligação de entrada** e defina as propriedades da seguinte forma:

   ```
   Host: ipaddress
   Port: 8001
   ```
6.  Configurar pontos de interrupção na pontos pretendidos e depurar a aplicação.

Se a aplicação esteja a falhar, pode também pretender ativar coredumps. Executar ``ulimit -c`` numa shell e se devolve 0, em seguida, coredumps não estão ativadas. Para ativar coredumps ilimitado, execute o seguinte comando: ``ulimit -c unlimited``. Também pode verificar o estado utilizando o comando ``ulimit -a``.  Se pretendesse atualizar o caminho de geração de coredump, executar ``echo '/tmp/core_%e.%p' | sudo tee /proc/sys/kernel/core_pattern``. 

### <a name="next-steps"></a>Passos Seguintes

* [Recolher registos de diagnóstico do Linux do Azure a utilizar](service-fabric-diagnostics-how-to-setup-lad.md).
* [Monitorizar e diagnosticar os serviços localmente](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md).
