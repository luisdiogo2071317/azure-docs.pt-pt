---
title: FAQ do Azure IoT Suite | Microsoft Docs
description: Perguntas mais frequentes sobre o IoT Suite
services: iot-suite
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: c79c90c4f6c28153d4d299015a06a6bc37145081
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Perguntas mais frequentes sobre o IoT Suite

Consulte também o [ligado específicos de fábrica FAQ](iot-suite-faq-cf.md) e o [FAQ de específicos de monitorização remota](iot-suite-faq-rm-v2.md) .

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Onde posso encontrar o código de origem para as soluções pré-configuradas?

O código de origem é armazenado no repositórios do GitHub seguintes:

* [(.NET) de solução pré-configurada de monitorização remota](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [(Java) de solução pré-configurada de monitorização remota](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Solução pré-configurada de manutenção preditiva](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Solução de fábrica ligado pré-configurada](https://github.com/Azure/azure-iot-connected-factory)

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-preconfigured-solutions"></a>Nova arquitetura do micro-serviços está disponível para todas as soluções pré-configuradas três?

Atualmente, apenas a solução de monitorização remota utiliza a arquitetura de micro-serviços como abrange o cenário mais amplas.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>As vantagens que o novo tenham origem open micro-serviços arquitetura baseada em fornecem na nova atualização?

Ao longo dos últimos anos dois, arquitetura na cloud, evoluiu e deu significativamente tem lugar. Micro-serviços tem emerged como um excelente padrão para alcançar a escala e a flexibilidade, sem sacrificar a velocidade de desenvolvimento. Neste padrão de arquitetura é utilizado em vários serviços do Microsoft internamente com resultados de escalabilidade e fiabilidade excelente. Estamos a finalizar estes learning na prática, para que os nossos clientes tirar partido dos mesmos.

### <a name="is-the-new-preconfigured-solution-available-in-the-same-geographic-region-as-the-existing-solution"></a>A nova solução pré-configurada está disponível na mesma região geográfica que a solução existente?

Sim, nova monitorização remota está disponível as mesmas regiões geográficas.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Qual a diferença entre a eliminação de um grupo de recursos no Portal do Azure e clicar Eliminar numa solução pré-configurada em azureiotsuite.com?

* Se eliminar a solução pré-configurada em [azureiotsuite.com](https://www.azureiotsuite.com/), estará a eliminar todos os recursos que aprovisionou aquando da criação da solução pré-configurada. Se adicionou recursos adicionais ao grupo de recursos, estes recursos também serão eliminados.
* Se eliminar o grupo de recursos no [portal do Azure](https://portal.azure.com), apenas estará a eliminar os recursos nesse grupo de recursos. Também terá de eliminar a aplicação do Azure Active Directory associada à solução pré-configurada.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-suite"></a>Pode continuar a tirar partido do meu investimentos existentes no Azure IoT Suite?

Sim. Qualquer solução de que existe atualmente continua a trabalhar na sua subscrição do Azure e o código de origem permanece disponível no GitHub.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Quantas instâncias do Hub IoT é possível aprovisionar numa subscrição?

Por predefinição pode aprovisionar [10 hubs IoT por subscrição](../azure-subscription-service-limits.md#iot-hub-limits). Pode criar um [pedido de suporte do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para aumentar este limite. Como resultado, uma vez que cada solução pré-configurada aprovisiona um IoT Hub novo, apenas pode aprovisionar até 10 soluções pré-configuradas numa determinada subscrição.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Quantas instâncias de base de dados do Azure Cosmos possível aprovisionar numa subscrição?

Cinquenta. Pode criar um [pedido de suporte do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para aumentar este limite, mas por predefinição, apenas pode aprovisionar 50 instâncias do Cosmos DB por subscrição.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Quantas APIs para o Free Bing Maps é possível aprovisionar numa subscrição?

Duas. Pode criar apenas dois Bing Maps de Transações Internas de Nível 1 para planos Enterprise numa subscrição do Azure. Por predefinição, a solução de monitorização remota é aprovisionada com o plano de Transações Internas de Nível 1. Como resultado, apenas pode aprovisionar até duas soluções de monitorização remota numa subscrição sem qualquer modificação.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Posso criar uma solução pré-configurada se tiver o Microsoft Azure para DreamSpark?

> [!NOTE]
> Microsoft Azure para DreamSpark é agora conhecido como Microsoft Imagine para os estudantes que estejam.

Atualmente, não é possível criar uma solução pré-configurada com uma [Microsoft Azure para DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) conta. No entanto, pode criar um [conta de avaliação gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos que lhe permite criar uma solução pré-configurada.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Posso criar uma solução pré-configurada se tiver subscrição do fornecedor de solução em nuvem (CSP)?

Atualmente, não é possível criar uma solução pré-configurada com uma subscrição do fornecedor de solução em nuvem (CSP). No entanto, pode criar um [conta de avaliação gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos que lhe permite criar uma solução pré-configurada.

### <a name="how-do-i-delete-an-aad-tenant"></a>Como posso eliminar um inquilino do AAD?

Consulte a mensagem de blogue de Eric Golpe [instruções de eliminação de um inquilino do Azure AD](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Passos Seguintes

Também pode explorar algumas das outras funcionalidades e capacidades das soluções pré-configuradas do IoT Suite:

* [Explorar as funcionalidades da solução pré-configurada de monitorização remota](iot-suite-remote-monitoring-explore.md)
* [Descrição geral de solução pré-configurada de manutenção preditiva](iot-suite-predictive-overview.md)
* [Descrição geral da solução de fábrica ligado pré-configurada](iot-suite-connected-factory-overview.md)
* [Segurança de IoT a partir do zero](securing-iot-ground-up.md)