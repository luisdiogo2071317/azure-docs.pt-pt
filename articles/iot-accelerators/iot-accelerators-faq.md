---
title: Aceleradores de solução IoT do Azure FAQ | Microsoft Docs
description: Perguntas mais frequentes sobre Aceleradores de solução IoT
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: decae2fee0d040d0857950bec507df173e2820b9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627164"
---
# <a name="frequently-asked-questions-for-iot-solution-accelerators"></a>Perguntas mais frequentes sobre Aceleradores de solução IoT

Consulte também o [ligado específicos de fábrica FAQ](iot-accelerators-faq-cf.md) e o [FAQ de específicos de monitorização remota](iot-accelerators-faq-rm-v2.md) .

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerators"></a>Onde posso encontrar o código de origem para os Aceleradores de solução?

O código de origem é armazenado no repositórios do GitHub seguintes:

* [Remoto acelerador de solução de monitorização (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Remoto acelerador de solução de monitorização (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Acelerador de solução de manutenção preditiva](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Ligado acelerador de solução de fábrica](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-sdks-can-i-use-to-develop-device-clients-for-the-solution-accelerators"></a>Os SDKs podem utilizar para clientes de dispositivos para os Aceleradores de soluções de desenvolver?

Pode encontrar ligações para SDKs do dispositivo IoT idioma diferente (C, .NET, Java, Node.js, Python) no [SDKs do IoT do Microsoft Azure](https://github.com/Azure/azure-iot-sdks) repositórios do GitHub.

Se estiver a utilizar o dispositivo DevKit, pode encontrar recursos e exemplos de [IoT DevKit SDK](https://github.com/Microsoft/devkit-sdk) repositório do GitHub.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-solution-accelerators"></a>Nova arquitetura do micro-serviços está disponível para todos os Aceleradores de três solução?

Atualmente, apenas a solução de monitorização remota utiliza a arquitetura de micro-serviços como abrange o cenário mais amplas.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>As vantagens que o novo tenham origem open micro-serviços arquitetura baseada em fornecem na nova atualização?

Ao longo dos últimos anos dois, arquitetura na cloud, evoluiu e deu significativamente tem lugar. Micro-serviços tem emerged como um excelente padrão para alcançar a escala e a flexibilidade, sem sacrificar a velocidade de desenvolvimento. Neste padrão de arquitetura é utilizado em vários serviços do Microsoft internamente com resultados de escalabilidade e fiabilidade excelente. Estamos a finalizar estes learning na prática, para que os nossos clientes tirar partido dos mesmos.

### <a name="is-the-new-solution-accelerator-available-in-the-same-geographic-region-as-the-existing-solution"></a>É o acelerador solução nova disponíveis na mesma região geográfica que a solução existente?

Sim, nova monitorização remota está disponível as mesmas regiões geográficas.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-solution-accelerator-in-azureiotsuitecom"></a>O que é a diferença entre a eliminação de um grupo de recursos no portal do Azure e clicar eliminar num acelerador de solução em azureiotsuite.com?

* Se eliminar o acelerador de solução em [azureiotsuite.com](https://www.azureiotsolutions.com/), estará a eliminar todos os recursos que aprovisionou quando criou o acelerador de solução. Se adicionou recursos adicionais ao grupo de recursos, estes recursos também serão eliminados.
* Se eliminar o grupo de recursos no [portal do Azure](https://portal.azure.com), apenas estará a eliminar os recursos nesse grupo de recursos. Também terá de eliminar a aplicação do Azure Active Directory associada com o acelerador de solução.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-solution-accelerators"></a>Pode continuar a tirar partido do meu investimentos existentes no Aceleradores de solução IoT do Azure?

Sim. Qualquer solução de que existe atualmente continua a trabalhar na sua subscrição do Azure e o código de origem permanece disponível no GitHub.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Quantas instâncias do Hub IoT é possível aprovisionar numa subscrição?

Por predefinição pode aprovisionar [10 hubs IoT por subscrição](../azure-subscription-service-limits.md#iot-hub-limits). Pode criar um [pedido de suporte do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para aumentar este limite. Como resultado, dado que Aprovisiona de acelerador cada solução um novo IoT Hub, apenas pode aprovisionar até 10 Aceleradores de solução numa determinada subscrição.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Quantas instâncias de base de dados do Azure Cosmos possível aprovisionar numa subscrição?

Cinquenta. Pode criar um [pedido de suporte do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para aumentar este limite, mas por predefinição, apenas pode aprovisionar 50 instâncias do Cosmos DB por subscrição.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Quantas APIs para o Free Bing Maps é possível aprovisionar numa subscrição?

Duas. Pode criar apenas dois Bing Maps de Transações Internas de Nível 1 para planos Enterprise numa subscrição do Azure. A solução de monitorização remota é aprovisionada por predefinição com o plano de transações nível 1 interno. Como resultado, apenas pode aprovisionar até duas monitorização remota soluções numa subscrição sem qualquer modificação.

### <a name="can-i-create-a-solution-accelerator-if-i-have-microsoft-azure-for-dreamspark"></a>Posso criar um acelerador solução se tiver o Microsoft Azure para DreamSpark?

> [!NOTE]
> Microsoft Azure para DreamSpark é agora conhecido como Microsoft Imagine para os estudantes que estejam.

Atualmente, não é possível criar um acelerador solução com um [Microsoft Azure para DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) conta. No entanto, pode criar um [conta de avaliação gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos que lhe permite criar um acelerador de solução.

### <a name="can-i-create-a-solution-accelerator-if-i-have-cloud-solution-provider-csp-subscription"></a>Posso criar um acelerador solução se tiver subscrição do fornecedor de solução em nuvem (CSP)?

Atualmente, não é possível criar um acelerador solução com uma subscrição do fornecedor de solução em nuvem (CSP). No entanto, pode criar um [conta de avaliação gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos que lhe permite criar um acelerador de solução.

### <a name="how-do-i-delete-an-aad-tenant"></a>Como posso eliminar um inquilino do AAD?

Consulte a mensagem de blogue de Eric Golpe [instruções de eliminação de um inquilino do Azure AD](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Passos Seguintes

Também pode explorar algumas das outras funcionalidades e capacidades dos aceleradores de soluções do IoT:

* [Explore as capacidades do acelerador de solução de monitorização remota](iot-accelerators-remote-monitoring-explore.md)
* [Descrição geral do acelerador de soluções de Manutenção Preditiva](iot-accelerators-predictive-overview.md)
* [Descrição geral da ligados da acelerador solução do Factory](iot-accelerators-connected-factory-overview.md)
* [Segurança de IoT a partir do zero](securing-iot-ground-up.md)
