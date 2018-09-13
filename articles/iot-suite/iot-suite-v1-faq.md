---
title: FAQ do Suite IoT do Azure | Documentos da Microsoft
description: Perguntas mais frequentes sobre o IoT Suite
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 7e7c4affee64a945900c02b6375ba4df5d085183
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35757809"
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Perguntas mais frequentes sobre o IoT Suite

Além disso, veja as específicas de fábrica ligada [FAQ](../iot-accelerators/iot-accelerators-faq-cf.md).

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Onde posso encontrar o código-fonte para as soluções pré-configuradas?

O código-fonte é armazenado nos repositórios do GitHub seguintes:
* [Solução pré-configurada de monitorização remota][lnk-remote-monitoring-github]
* [Solução pré-configurada de manutenção preditiva][lnk-predictive-maintenance-github]
* [Solução pré-configurada de fábrica ligada](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-to-the-latest-version-of-the-remote-monitoring-preconfigured-solution-that-uses-the-iot-hub-device-management-features"></a>Como posso atualizar para a versão mais recente da solução pré-configurada de monitorização remota que utiliza as funcionalidades de gestão de dispositivos do IoT Hub?

* Se implementar uma solução pré-configurada do https://www.azureiotsuite.com/ site, implementa sempre uma nova instância da versão mais recente da solução.
* Se implementar uma solução pré-configurada com a linha de comandos, pode atualizar uma implementação existente com o novo código. Ver [implementação na nuvem] [ lnk-cloud-deployment] no GitHub [repositório][lnk-remote-monitoring-github].

### <a name="how-can-i-add-support-for-a-new-device-method-to-the-remote-monitoring-preconfigured-solution"></a>Como posso adicionar suporte para um novo método de dispositivo para a solução pré-configurada de monitorização remota?

Consulte a secção [adicionar suporte para um novo método para o simulador] [ lnk-add-method] no [personalizar uma solução pré-configurada] [ lnk-customize] artigo.

### <a name="the-simulated-device-is-ignoring-my-desired-property-changes-why"></a>O dispositivo está a ignorar minhas alterações de propriedade pretendida, por que?
A solução pré-configurada monitorização remota, o dispositivo simulado código utiliza apenas a **Desired.Config.TemperatureMeanValue** e **Desired.Config.TelemetryInterval** propriedades para pretendidas Atualize as propriedades comunicadas. Todos os outros pedidos de alteração de propriedades pretendidas são ignorados.

### <a name="my-device-does-not-appear-in-the-list-of-devices-in-the-solution-dashboard-why"></a>O meu dispositivo não aparece na lista de dispositivos no dashboard da solução, por que?

A lista de dispositivos no dashboard da solução utiliza uma consulta para devolver a lista de dispositivos. Atualmente, uma consulta não pode retornar mais de 10 mil dispositivos. Tente fazer com que os critérios de pesquisa para a sua consulta mais restrita.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Qual a diferença entre a eliminação de um grupo de recursos no Portal do Azure e clicar Eliminar numa solução pré-configurada em azureiotsuite.com?

* Se eliminar a solução pré-configurada [azureiotsuite.com][lnk-azureiotsuite], eliminar todos os recursos que foram aprovisionados quando criou a solução pré-configurada. Se adicionou recursos adicionais para o grupo de recursos, esses recursos também são eliminados. 
* Se eliminar o grupo de recursos no [portal do Azure][lnk-azure-portal], só é eliminar os recursos nesse grupo de recursos. Também precisa de eliminar a aplicação do Azure Active Directory associada com a solução para o [portal do Azure][lnk-azure-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Quantas instâncias do Hub IoT é possível aprovisionar numa subscrição?

Por predefinição, pode aprovisionar [10 hubs IoT por subscrição][link-azuresublimits]. Pode criar uma [pedido de suporte do Azure] [ link-azuresupportticket] para aumentar este limite. Como resultado, uma vez que cada solução pré-configurada aprovisiona um novo IoT Hub, apenas pode aprovisionar até 10 soluções pré-configuradas numa determinada subscrição. 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Quantas instâncias do Azure Cosmos DB pode aprovisionar numa subscrição?

Cinquenta. Pode criar uma [pedido de suporte do Azure] [ link-azuresupportticket] para aumentar este limite, mas por predefinição, apenas pode aprovisionar 50 instâncias do Cosmos DB por subscrição. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Quantas APIs para o Free Bing Maps é possível aprovisionar numa subscrição?

Duas. Pode criar apenas dois Bing Maps de Transações Internas de Nível 1 para planos Enterprise numa subscrição do Azure. Por predefinição, a solução de monitorização remota é aprovisionada com o plano de Transações Internas de Nível 1. Como resultado, apenas pode aprovisionar até duas soluções de monitorização remota numa subscrição sem qualquer modificação.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Tenho uma implementação de solução de monitorização remota com um mapa estático, como adicionar um mapa interativo ao Bing?

1. Obter a API do Bing Maps para Enterprise QueryKey a partir [portal do Azure][lnk-azure-portal]: 
   
   1. Navegue para o grupo de recursos em que a sua API dos mapas Bing para empresas é no [portal do Azure][lnk-azure-portal].
   2. Clique em **todas as definições**, em seguida, **gestão de chaves**. 
   3. Pode ver duas chaves: **MasterKey** e **QueryKey**. Copie o valor para **QueryKey**.
      
      > [!NOTE]
      > Não tem uma API para o Bing Maps para conta Enterprise? Criar um da [portal do Azure] [ lnk-azure-portal] às clicando em + novo, procurar a API do Bing Maps para Enterprise e siga as instruções para criar.
      > 
      > 
2. Obter o código mais recente a partir da [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].
3. Executar local ou na cloud a implementação, seguindo as orientações de implementação da linha de comandos na pasta /docs/ do repositório. 
4. Depois de ter executado a implementação local ou na nuvem, procure na sua pasta raiz o ficheiro *.user.config criado aquando da implementação. Abra este ficheiro num editor de texto. 
5. Altere a seguinte linha para incluir o valor que copiou do seu **QueryKey**: 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Posso criar uma solução pré-configurada se tiver o Microsoft Azure para DreamSpark?

> [!NOTE]
> Microsoft Azure para DreamSpark é agora conhecido como Microsoft Imagine para estudantes.

Atualmente, não é possível criar uma solução pré-configurada com uma [Microsoft Azure para DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) conta. No entanto, pode criar uma [conta de avaliação gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos que lhe permitem criar uma solução pré-configurada.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>É possível criar uma solução pré-configurada se eu tiver a subscrição do fornecedor de soluções Cloud (CSP)?

Atualmente, não é possível criar uma solução pré-configurada com uma subscrição do fornecedor de soluções Cloud (CSP). No entanto, pode criar uma [conta de avaliação gratuita do Azure] [ lnk-30daytrial] em apenas alguns minutos que lhe permitem criar uma solução pré-configurada.

### <a name="how-do-i-delete-an-azure-ad-tenant"></a>Como posso eliminar um inquilino do Azure AD?

Consulte a postagem de blog de Eric Golpe [instruções de eliminação de um inquilino do Azure AD][lnk-delete-aad-tennant].

### <a name="next-steps"></a>Passos Seguintes

Também pode explorar algumas das outras funcionalidades e capacidades das soluções pré-configuradas do IoT Suite:

* [Descrição de geral da solução pré-configurada de manutenção preditiva][lnk-predictive-overview]
* [Descrição geral da solução de fábrica ligada pré-configurada](../iot-accelerators/iot-accelerators-connected-factory-overview.md)
* [Segurança de IoT desde o início][lnk-security-groundup]

[lnk-predictive-overview]:../iot-accelerators/iot-accelerators-predictive-overview.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
