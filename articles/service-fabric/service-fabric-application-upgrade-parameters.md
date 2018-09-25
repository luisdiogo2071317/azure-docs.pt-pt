---
title: 'Atualização da aplicação: parâmetros de atualização | Documentos da Microsoft'
description: Descreve os parâmetros relacionados à atualização de uma aplicação do Service Fabric, incluindo verificações de estado de funcionamento para executar e políticas para desfazer automaticamente a atualização.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 9/17/2018
ms.author: subramar
ms.openlocfilehash: f3f381fddee9c1830202854f02556f73b5aeed23
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47055582"
---
# <a name="application-upgrade-parameters"></a>Parâmetros da atualização da aplicação
Este artigo descreve os vários parâmetros que se aplicam durante a atualização de uma aplicação do Azure Service Fabric. Parâmetros de atualização de aplicativo controlam os tempos limite e verificações de estado de funcionamento que são aplicadas durante a atualização, e eles especificam as políticas que devem ser aplicadas quando ocorre uma falha de uma atualização.

Parâmetros da aplicação se aplicam a atualizações com o PowerShell ou o Visual Studio. Os parâmetros obrigatórios e opcionais aplicáveis ao PowerShell e/ou o Visual Studio são descritos nas tabelas de parâmetros necessários e os parâmetros opcionais da seguinte forma.

As atualizações de aplicações são iniciadas por meio de um dos três modos de atualização selecionáveis pelo utilizador. Cada modo tem seu próprio conjunto de parâmetros da aplicação:
- Monitorizada
- Automática não monitorizada
- Manual não monitorizado

As atualizações de aplicações do Service Fabric com o uso do PowerShell a [Start-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade) comando. O modo de atualização está selecionado ao transmitir um a **monitorizados**, **UnmonitoredAuto**, ou **UnmonitoredManual** parâmetro [ Start-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade).

Parâmetros de atualização Visual Studio Service Fabric application são definidos por meio da caixa de diálogo de configurações de atualização do Visual Studio. O modo de atualização do Visual Studio é selecionado com o **modo de atualização** na caixa pendente para qualquer um **monitorizados**, **UnmonitoredAuto**, ou **UnmonitoredManual** . Para obter mais informações, consulte [configurar a atualização de uma aplicação do Service Fabric no Visual Studio](service-fabric-visualstudio-configure-upgrade.md).

## <a name="required-parameters"></a>Parâmetros necessários
(PS = PowerShell, VS = o Visual Studio)

| Parâmetro | Aplica-se A | Descrição |
| --- | --- | --- |
ApplicationName |PS| Nome da aplicação que está a ser atualizado. Exemplos: fabric: / VisualObjects, fabric: / ClusterMonitor. |
ApplicationTypeVersion|PS|Tipo de versão do aplicativo que os destinos de atualização. |
FailureAction |PS, VS|Valores permitidos são **inválido**, **reversão**, e **Manual**. A ação tomada pelo Service Fabric quando a atualização falhar. O aplicativo pode ser revertido para a versão de pré-atualização (reversão) ou a atualização poderá ser interrompida no domínio de atualização atual. No último caso, o modo de atualização também é alterado para **Manual**.|
Monitorizada |PS|Indica que o modo de atualização está a ser monitorizado. Quando o cmdlet termina uma atualização para um domínio de atualização, se o estado de funcionamento do cluster e o domínio de atualização cumprir as políticas de estado de funcionamento que definir, o Service Fabric atualiza o domínio de atualização seguinte. Se o domínio de atualização ou o cluster não cumprir as políticas de estado de funcionamento, a atualização falhar e o Service Fabric reverte a atualização para o domínio de atualização ou reverte para o modo manual pela política especificada. Este é o modo recomendado para as atualizações de aplicações num ambiente de produção. |
UpgradeMode | VS | Valores permitidos são **monitorizados** (predefinição), **UnmonitoredAuto**, ou **UnmonitoredManual**. Veja os parâmetros de PowerShell para cada modo neste artigo para obter detalhes. |
UnmonitoredAuto | PS | Indica que o modo de atualização é automático não monitorizado. Após um domínio de atualização de atualização do Service Fabric, o Service Fabric atualiza o próximo domínio de atualização, independentemente do Estado de funcionamento da aplicação. Este modo não é recomendado para produção e só é útil durante o desenvolvimento de um aplicativo. |
UnmonitoredManual | PS | Indica que o modo de atualização é manual não monitorizado. Após um domínio de atualização de atualização do Service Fabric, ele aguarda a atualizar o domínio de atualização seguinte ao utilizar o *retomar ServiceFabricApplicationUpgrade* cmdlet. |

## <a name="optional-parameters"></a>Parâmetros opcionais

Os parâmetros de avaliação do Estado de funcionamento são opcionais. Se os critérios de avaliação do Estado de funcionamento não forem especificados quando uma atualização é iniciado, o Service Fabric utiliza as políticas de estado de funcionamento da aplicação especificadas em Applicationmanifest da instância da aplicação.

Utilize a barra de deslocamento horizontal na parte inferior da tabela para ver o campo de descrição completa.

(PS = PowerShell, VS = o Visual Studio)

| Parâmetro | Aplica-se A | Descrição |
| --- | --- | --- |
| ApplicationParameter |PS, VS| Especifica as substituições para os parâmetros de aplicação.<br>Parâmetros do PowerShell applcation são especificados como pares de nome/valor de tabela de hash. Por exemplo, @{"VotingData_MinReplicaSetSize" = "3"; "VotingData_PartitionCount" = "1"}.<br>Parâmetros de aplicação do Visual Studio podem ser especificados na caixa de diálogo Publicar aplicação do Service Fabric no **o ficheiro de parâmetros de aplicação** campo.
| Confirmar |PS| Valores permitidos são **True** e **falso**. Pedidos de confirmação antes de executar o cmdlet. |
| ConsiderWarningAsError |PS, VS |Valores permitidos são **True** e **falso**. O valor predefinido é **Falso**. Processar os eventos de estado de funcionamento de aviso para a aplicação como erros ao avaliar o estado de funcionamento da aplicação durante a atualização. Por predefinição, o Service Fabric não avalia a eventos de estado de funcionamento de aviso para ser falhas (erros), para que a atualização possa prosseguir, mesmo se existem eventos de aviso. |
| DefaultServiceTypeHealthPolicy | PS, VS |Especifica a política de estado de funcionamento para o tipo de serviço predefinido a utilizar para a atualização monitorizada no formato MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition, MaxPercentUnhealthyServices. Por exemplo, 5,10,15 indica os seguintes valores: MaxPercentUnhealthyPartitionsPerService = 5, MaxPercentUnhealthyReplicasPerPartition = 10, MaxPercentUnhealthyServices = 15. |
| Force | PS, VS | Valores permitidos são **True** e **falso**. Indica que o processo de atualização ignora a mensagem de aviso e força a atualização, mesmo quando o número de versão não mudou. Isso é útil para a realização de testes locais, mas não é recomendado para uso num ambiente de produção, é necessário remover a implementação existente que faz com que a perda de dados de tempo de inatividade e potenciais. |
| ForceRestart |PS, VS |Se atualizar uma configuração ou o pacote de dados sem atualizar o código do serviço, o serviço for reiniciado apenas se a propriedade ForceRestart estiver definida como **True**. Quando a atualização estiver concluída, o Service Fabric notifica o serviço que está disponível um novo pacote de configuração ou o pacote de dados. O serviço é responsável por aplicar as alterações. Se necessário, pode reiniciar o serviço em si. |
| HealthCheckRetryTimeoutSec |PS, VS |A duração (em segundos) que o Service Fabric continua a executar a avaliação de estado de funcionamento antes da atualização ser declarada como falhado. A predefinição é de 600 segundos. Esta duração é iniciado após *HealthCheckWaitDurationSec* for atingido. Dentro desta *HealthCheckRetryTimeout*, Service Fabric, pode executar várias verificações de estado de funcionamento do Estado de funcionamento da aplicação. O valor predefinido é de 10 minutos e deve ser personalizado de forma adequada para a sua aplicação. |
| HealthCheckStableDurationSec |PS, VS |A duração (em segundos) para verificar se o aplicativo está estável antes de mover para o domínio de atualização seguinte ou a conclusão da atualização. Esta duração de espera é usada para impedir alterações não detetadas do Estado de funcionamento correto, após ter sido efetuada a verificação de estado de funcionamento. O valor predefinido é de 120 segundos e deve ser personalizado de forma adequada para a sua aplicação. |
| HealthCheckWaitDurationSec |PS, VS | O tempo de espera (em segundos) após a atualização foi concluída no domínio de atualização antes de Service Fabric avalia o estado de funcionamento da aplicação. Esta duração também pode ser considerada como o tempo de que um aplicativo deve estar em execução antes de ele pode ser considerado em bom estado. Se a verificação de estado de funcionamento passar, continua o processo de atualização para o domínio de atualização seguinte.  Se a verificação de estado de funcionamento falhar, o Service Fabric aguarda [UpgradeHealthCheckInterval](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-fabric-settings#clustermanager) antes de repetir o estado de funcionamento, verifique novamente até o *HealthCheckRetryTimeoutSec* for atingido. O valor predefinido e recomendado é de 0 segundos. |
| MaxPercentUnhealthyDeployedApplications|PS, VS |Valor predefinido e recomendado é 0. Especifique o número máximo de aplicações implementadas (consulte a [secção do Estado de funcionamento](service-fabric-health-introduction.md)) que pode ser mau estado de funcionamento antes do aplicativo é considerado em mau estado de funcionamento e falha da atualização. Este parâmetro define o estado de funcionamento do aplicativo no nó e ajuda a detetar problemas durante a atualização. Normalmente, as réplicas do aplicativo obtém com balanceamento de carga para o outro nó, que permite que as aplicações apareçam em bom estado, permitindo assim que a atualização continuar. Ao especificar um rigoroso *MaxPercentUnhealthyDeployedApplications* estado de funcionamento, Service Fabric pode detetar um problema com o pacote de aplicação rapidamente e ajudar a produzir uma falha de atualização rápida. |
| MaxPercentUnhealthyServices |PS, VS |Um parâmetro para *DefaultServiceTypeHealthPolicy* e *ServiceTypeHealthPolicyMap*. Valor predefinido e recomendado é 0. Especifique o número máximo de serviços na instância da aplicação que pode ser mau estado de funcionamento antes do aplicativo é considerado em mau estado de funcionamento e falha da atualização. |
| MaxPercentUnhealthyPartitionsPerService|PS, VS |Valor predefinido e recomendado é 0. Especifique o número máximo de partições num serviço que pode ser mau estado de funcionamento antes do serviço é considerado em mau estado de funcionamento. |
| MaxPercentUnhealthyReplicasPerPartition|PS, VS |Valor predefinido e recomendado é 0. Especifique o número máximo de réplicas de partição que pode ser mau estado de funcionamento antes da partição é considerada em mau estado de funcionamento. |
| ServiceTypeHealthPolicyMap | PS, VS | Representa a política de estado de funcionamento utilizada para avaliar o estado de funcionamento dos serviços que pertençam a um tipo de serviço. Leva uma entrada de tabela de hash no seguinte formato: @ {"ServiceTypeName": "MaxPercentUnhealthyServices MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition,"} por exemplo: @{"ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5"} |
| TimeoutSec | PS, VS | Especifica o período de tempo limite em segundos para a operação. |
| UpgradeDomainTimeoutSec |PS, VS |Tempo máximo (em segundos) para a atualização de um único domínio de atualização. Se for atingido este limite de tempo, a atualização deixa e prossegue com base na definição *FailureAction*. O valor predefinido é nunca (infinito) e deve ser personalizada de forma adequada para a sua aplicação. |
| UpgradeReplicaSetCheckTimeoutSec |PS, VS |**Serviço sem estado**– dentro de um único domínio de atualização, o Service Fabric tenta garantir que as instâncias adicionais do serviço estão disponíveis. Se a contagem de instâncias de destino é mais do que um, o Service Fabric aguarda mais de uma instância disponível, até um valor máximo de tempo limite. Este limite de tempo é especificado, utilizando o *UpgradeReplicaSetCheckTimeoutSec* propriedade. Se o tempo limite expirar, o Service Fabric prosseguirá com a atualização, independentemente do número de instâncias de serviço. Se a contagem de instâncias de destino é um, o Service Fabric não aguarda e imediatamente prosseguir com a atualização.<br><br>**Serviço com estado**– dentro de um único domínio de atualização, o Service Fabric tenta garantir que o conjunto de réplicas tem um quórum. Service Fabric aguarda para um quórum de estar disponível, até um valor de limite de tempo máximo (especificado pelos *UpgradeReplicaSetCheckTimeoutSec* propriedade). Se o tempo limite expirar, o Service Fabric prosseguirá com a atualização, independentemente do quórum. Esta definição é como nunca conjunto (infinito), quando avançar e segundos 1200 quando a reversão. |
| UpgradeTimeoutSec |PS, VS |Um limite de tempo (em segundos) que aplica-se para a atualização de toda. Se for atingido este limite de tempo, a atualização será interrompido e *FailureAction* é acionado. O valor predefinido é nunca (infinito) e deve ser personalizada de forma adequada para a sua aplicação. |
| WhatIf | PS | Valores permitidos são **True** e **falso**. Mostra o que aconteceria se a execução do cmdlet. O cmdlet não é executado. |

O *MaxPercentUnhealthyServices*, *MaxPercentUnhealthyPartitionsPerService*, e *MaxPercentUnhealthyReplicasPerPartition* critérios podem ser especificados por tipo de serviço para uma instância de aplicação. Definir estes parâmetros por serviço permite que um aplicativo conter tipos diferentes de serviços com as políticas de avaliação diferentes. Por exemplo, um tipo de serviço do gateway sem monitoração de estado pode ter uma *MaxPercentUnhealthyPartitionsPerService* que é diferente de um tipo de serviço do motor com monitoração de estado para uma instância do aplicativo em particular.

## <a name="next-steps"></a>Passos Seguintes
[Atualizar a sua aplicação com o Visual Studio](service-fabric-application-upgrade-tutorial.md) explica-lhe uma atualização da aplicação com o Visual Studio.

[Atualizar a sua aplicação utilizar o Powershell](service-fabric-application-upgrade-tutorial-powershell.md) explica-lhe uma atualização da aplicação com o PowerShell.

[Atualizar a sua aplicação utilizar a CLI do Service Fabric no Linux](service-fabric-application-lifecycle-sfctl.md#upgrade-application) explica-lhe uma atualização da aplicação com a CLI do Service Fabric.

[Atualizar a sua aplicação com o plug-in do Service Fabric Eclipse](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Tornar as atualizações de aplicações compatíveis, aprendendo a usar [serialização de dados](service-fabric-application-upgrade-data-serialization.md).

Saiba como utilizar funcionalidades avançadas ao atualizar a sua aplicação por consultar [tópicos avançados](service-fabric-application-upgrade-advanced.md).

Corrigir problemas comuns em atualizações da aplicação ao referir-se os passos no [resolução de problemas de atualizações de aplicações](service-fabric-application-upgrade-troubleshooting.md).
