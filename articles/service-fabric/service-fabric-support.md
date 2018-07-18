---
title: Saiba mais sobre as opções de suporte do Azure Service Fabric | Documentos da Microsoft
description: Pedidos de suporte de versões de cluster do Service Fabric suportadas e links para ficheiros do Azure
services: service-fabric
documentationcenter: .net
author: pkcsf
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/13/2018
ms.author: pkc
ms.openlocfilehash: 596e71be75453874492aac15d91cb6153c2076f5
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39112895"
---
# <a name="azure-service-fabric-support-options"></a>Opções de suporte do Azure Service Fabric

Para fornecer o suporte adequado para os seus clusters do Service Fabric que está a executar as cargas de trabalho do aplicativo, configuramos várias opções para. Consoante o nível de suporte necessário e a gravidade do problema, tem de escolher as opções certas. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Comunicar problemas de produção ou pedido de suporte pago para o Azure

Para problemas de relatórios no seu cluster do Service Fabric implementado no Azure, abra um pedido de suporte [no portal do Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) ou [portal de suporte da Microsoft](http://support.microsoft.com/oas/default.aspx?prid=16146).

Saiba mais sobre:
 
- [Suporte da Microsoft para o Azure](https://azure.microsoft.com/support/plans/?b=16.44).
- [O suporte premier da Microsoft](https://support.microsoft.com/en-us/premier).

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Comunicar problemas de produção ou pedido de suporte pago autónomo que clusters do Service Fabric

Para os relatórios de problemas no seu cluster do Service Fabric implementado no local ou noutras clouds, abra um pedido de suporte profissional no [portal de suporte da Microsoft](http://support.microsoft.com/oas/default.aspx?prid=16146).

Saiba mais sobre:

- [Suporte profissional da Microsoft para on-premises](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [O suporte premier da Microsoft](https://support.microsoft.com/en-us/premier).

## <a name="report-azure-service-fabric-issues"></a>Problemas de relatório do Azure Service Fabric 
Configuramos um repositório do GitHub para os relatórios de problemas do Service Fabric.  Estamos também ativamente monitorando os seguintes fóruns.

### <a name="github-repo"></a>Repositório do GitHub 
Relatórios de problemas de recursos de infraestrutura do serviço do Azure sobre [repositório de git de problemas de recursos de infraestrutura de serviço](https://github.com/Azure/service-fabric-issues). Este repositório destina-se para reportar e acompanhar problemas com o Azure Service Fabric e para fazer solicitações de recursos pequenos. **Não utilize esta opção para comunicar problemas de sites ativos**.

### <a name="stackoverflow-and-msdn-forums"></a>Fóruns de Stack Overflow e do MSDN
O [tag do Service Fabric no Stack Overflow] [ stackoverflow] e o [fórum do Service Fabric no MSDN] [ msdn-forum] são melhor utilizado para fazer perguntas sobre como o plataforma funciona e como fazer determinadas tarefas com o mesmo.

### <a name="azure-feedback-forum"></a>Fórum de comentários do Azure
O [fórum de comentários do Azure para o Service Fabric] [ uservoice-forum] é o melhor local para o envio de ideias para funcionalidades grande tem para o produto como vamos rever os pedidos mais populares fazem parte do nosso médio para planejamento de longo prazo. Incentivamos a rally suporte para suas sugestões na Comunidade.


## <a name="supported-service-fabric-versions"></a>Versões suportadas do Service Fabric.

Certifique-se de que o cluster está sempre em execução uma versão suportada do Service Fabric. Como e quando podemos anunciar o lançamento de uma nova versão do Service Fabric, a versão anterior está marcada para o fim do suporte após um mínimo de 60 dias dessa data. As novas versões sejam anunciadas [no blog da equipe de recursos de infraestrutura do serviço](https://blogs.msdn.microsoft.com/azureservicefabric/).

Consulte os seguintes documentos em detalhes sobre como manter o seu cluster executar uma versão suportada do Service Fabric.

- [Atualizar a versão do Service Fabric num cluster do Azure ](service-fabric-cluster-upgrade.md)
- [Atualizar a versão do Service Fabric num cluster de servidores windows autónomo ](service-fabric-cluster-upgrade-windows-server.md)
 
Seguem-se a lista das versões de Service Fabric que são suportadas e respetivas datas de fim do suporte.

| **Runtime do Service Fabric no cluster** | **Pode atualizar diretamente a partir da versão do cluster** |**SDK compatível / versões de pacote de NuGet** | **Fim do suporte** |
| --- | --- |--- | --- |
| Todas as versões de cluster antes de 5.3.121 | 5.1.158* |Menor ou igual a versão 2.3 |20 de Janeiro de 2017 |
| 5.3.* | 5.1.158.* |Menor ou igual a versão 2.3 |24 de Fevereiro de 2017 |
| 5.4.* | 5.1.158.* |Menor ou igual a versão 2.4 |Pode 10,2017       |
| 5.5.* | 5.4.164.* |Menor ou igual a versão 2.5 |Agosto 10,2017    |
| 5.6.* | 5.4.164.* |Menor ou igual a versão 2.6 |Outubro 13,2017   |
| 5.7.* | 5.4.164.* |Menor ou igual a versão 2.7 |Dezembro 15,2017  |
| 6.0.* | 5.6.205.* |Menor ou igual a versão 2.8 |Março de 30,2018     | 
| 6.1.* | 5.7.221.* |Menor ou igual a versão 3.0 |Julho 15,2018      |
| 6.2. * | 6.0.232.* |Menor ou igual à versão 3.1 |Setembro 15,2018 |
| 6.3. * | 6.1.480.* |Menor ou igual à versão 3.2 |Versão atual e por isso, sem data de fim |

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Versões de pré-visualização de recursos de infraestrutura do serviço, não suportadas para utilização em produção.
Periodicamente, fazemos lançamentos de versões que tenham recursos significativos queremos os comentários, que são lançados como pré-visualizações. Estas versões de pré-visualização só devem ser utilizadas para fins de teste. O cluster de produção deve sempre executar uma versão de Service Fabric estável, e suportada. Uma versão de pré-visualização sempre começa com um número de versão principal e secundária de 255. Por exemplo, se vir uma versão 255.255.5703.949 do Service Fabric, essa versão de lançamento só deve ser usado em clusters de teste e está em pré-visualização. Estas versões de pré-visualização também sejam anunciadas no [blogue da equipa do Service Fabric](https://blogs.msdn.microsoft.com/azureservicefabric) e que serão tiver os detalhes sobre as funcionalidades incluídas.

Não existe nenhuma opção de suporte pagas para estas versões de pré-visualização. Utilize uma das opções listadas na [problemas de relatório do Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) para colocar questões ou comentários.

## <a name="next-steps"></a>Passos Seguintes

- [Atualizar versão do service fabric num cluster do Azure ](service-fabric-cluster-upgrade.md)
- [Atualizar a versão do Service Fabric num cluster de servidores windows autónomo ](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples
