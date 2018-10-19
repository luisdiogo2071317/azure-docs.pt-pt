---
title: O serviço do Azure a lista de verificação de preparação de produção do Fabric | Documentos da Microsoft
description: Prepare-se a aplicação de Service Fabric e a produção de cluster por práticas recomendadas a seguir.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/10/2018
ms.author: subramar
ms.openlocfilehash: 7557e2b993a5059df8aea63c7394539acc28c110
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49403529"
---
# <a name="production-readiness-checklist"></a>Lista de verificação da preparação de produção

É a aplicação e o cluster estão prontos para dar o tráfego de produção? Executar e testar a sua aplicação e o cluster não necessariamente significa que ele está pronto para entrar em produção. Mantenha a sua aplicação e o cluster a funcionar sem problemas através da lista de verificação seguinte. Recomendamos vivamente que todos estes itens a ser verificado. Obviamente, pode optar por utilizar soluções alternativas para um determinado item de linha (por exemplo, suas próprias estruturas de diagnóstico).


## <a name="pre-requisites-for-production"></a>Pré-requisitos para produção

1. Para clusters com mais de 20 núcleos ou 10 nós, crie um tipo de nó primário dedicado para serviços do sistema. Adicione [restrições de posicionamento](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) para reservar o tipo de nó primário para serviços do sistema. 
2. Utilize um D2v2 ou SKU superior para o tipo de nó primário. Recomenda-se de escolher um SKU com pelo menos 50 GB de capacidade de disco rígido.
2. Clusters de produção têm de ser [seguro](service-fabric-cluster-security.md). Para obter um exemplo de como configurar um cluster seguro, consulte [modelo de cluster](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/7-VM-Windows-3-NodeTypes-Secure-NSG). Utilizar nomes comuns para certificados e evitar o uso de certificados assinados personalizados.
4. Adicione [restrições de recursos em contentores e serviços](service-fabric-resource-governance.md), para que eles não consomem mais de 75% dos recursos de nó. 
5. Compreender e definir o [nível de durabilidade](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). Para executar cargas de trabalho com monitorização de estado de tipos de nós, recomenda-se ao nível de durabilidade Silver ou superior. O tipo de nó primário deve ter um nível de durabilidade definido para Silver ou superior.
6. Compreender e escolha o [nível de fiabilidade](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) do tipo de nó. Confiabilidade Silver ou superior é recomendada.
7. As cargas de trabalho para identificar de teste de carga e dimensionamento [requisitos de capacidade](service-fabric-cluster-capacity.md) para o seu cluster. 
8. Seus serviços e aplicações são monitorizadas e registos de aplicações estão a ser gerados e armazenados, com o alerta. Por exemplo, veja [adicionar registos à sua aplicação do Service Fabric](service-fabric-how-to-diagnostics-log.md) e [monitorizar contentores com o Log Analytics](service-fabric-diagnostics-oms-containers.md).
9. O cluster é monitorizado com alertas (por exemplo, com [do Log Analytics](service-fabric-diagnostics-event-analysis-oms.md)). 
10. A infraestrutura de conjunto de dimensionamento de máquina virtual subjacente é monitorizada com alertas (por exemplo, com [do Log Analytics](service-fabric-diagnostics-oms-agent.md).
11. O cluster tem [certificados primário e secundário](service-fabric-cluster-security-update-certs-azure.md) sempre (para que não fica bloqueado).
12. Manter clusters separados para desenvolvimento, teste e produção. 
13. [As atualizações de aplicações](service-fabric-application-upgrade.md) e [atualizações do cluster](service-fabric-tutorial-upgrade-cluster.md) são testados no desenvolvimento e teste clusters pela primeira vez. 
14. Desativar as atualizações automáticas em clusters de produção e ativá-la para desenvolvimento e teste clusters (reversão conforme necessário). 
15. Estabelecer um objetivo de ponto de recuperação (RPO) para o seu serviço e configurar uma [processo de recuperação após desastre](service-fabric-disaster-recovery.md) e testá-lo.
16. Planear [dimensionamento](service-fabric-cluster-scaling.md) seu cluster manualmente ou através de programação.
17. Planear [aplicação de patches](service-fabric-patch-orchestration-application.md) os nós do cluster. 
18. Estabelece um pipeline CI/CD para que as alterações mais recentes estão a ser testadas continuamente. Por exemplo, utilizando [do Azure DevOps](service-fabric-tutorial-deploy-app-with-cicd-vsts.md) ou [Jenkins](service-fabric-cicd-your-linux-applications-with-jenkins.md)
19. Testar o seu desenvolvimento e teste clusters em condições de carga com o [serviço de análise de falhas](service-fabric-testability-overview.md) e induza controlado [chaos](service-fabric-controlled-chaos.md). 
20. Planear [dimensionamento](service-fabric-concepts-scalability.md) seus aplicativos. 


Se estiver a utilizar o modelo de programação do Service Fabric Reliable Services ou Reliable Actors, tem de ser assinalou os seguintes itens:
21. Atualizar aplicativos durante o desenvolvimento local para verificar que o seu código de serviço está a respeitar o token de cancelamento no `RunAsync` método e fecho de serviços de escuta de comunicação personalizado.
22. Evitar [armadilhas comuns](service-fabric-work-with-reliable-collections.md) ao utilizar a Reliable Collections.
23. Monitorize o desempenho de memória de .NET CLR contadores ao executar testes de carga e verificar a existência de taxas altas de coleta de lixo ou crescimento de fuga de heap.
24. Manter a cópia de segurança offline das [Reliable Services e Reliable Actors](service-fabric-reliable-services-backup-restore.md) e testar o processo de restauro. 


## <a name="optional-best-practices"></a>Práticas recomendadas opcionais

Embora as listas anteriores estão pré-requisitos para entrar em produção, também devem ser considerados os seguintes itens:
25. Conectar o [modelo de estado de funcionamento do Service Fabric](service-fabric-health-introduction.md) para expandir a avaliação de integridade incorporadas e relatórios.
26. Implementar um watchdog personalizado que está a monitorizar a aplicação e os relatórios [carregar](service-fabric-cluster-resource-manager-metrics.md) para [balanceamento de recurso](service-fabric-cluster-resource-manager-balancing.md). 


## <a name="next-steps"></a>Passos Seguintes
* [Implementar um cluster do Windows do Service Fabric](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
* [Implementar um cluster do Service Fabric do Linux](service-fabric-tutorial-create-vnet-and-linux-cluster.md)
* Saiba mais sobre o [ciclo de vida das aplicações](service-fabric-application-lifecycle.md) do Service Fabric.
