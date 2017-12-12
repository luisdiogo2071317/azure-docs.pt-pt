---
title: Configurar portas de disponibilidade elevada para o Balanceador de carga do Azure | Microsoft Docs
description: "Saiba como utilizar as portas de disponibilidade elevada para o tráfego interno em todas as portas de balanceamento de carga"
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/02/2017
ms.author: kumud
ms.openlocfilehash: 36bc3d7a35f41384706cbc7101457d00848639b2
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="configure-high-availability-ports-for-an-internal-load-balancer"></a>Configurar portas de disponibilidade elevada para um balanceador de carga interno

Este artigo fornece um exemplo de implementação de elevada disponibilidade portas num Balanceador de carga interno. Para obter mais informações sobre as configurações específicas para aparelhos virtuais (NVAs) de rede, consulte os sites correspondentes do fornecedor.

>[!NOTE]
> A funcionalidade de elevada disponibilidade portas está atualmente em pré-visualização. Durante a pré-visualização, a funcionalidade não pode ter o mesmo nível de disponibilidade e fiabilidade como versão de funcionalidades que estão em geral disponibilidade. Para obter mais informações, consulte [Termos de Utilização Suplementares do Microsoft Azure para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

A ilustração mostra a seguinte configuração de exemplo de implementação descrita neste artigo:

- Os NVAs são implementadas no conjunto back-end de um balanceador de carga interno subjacente à configuração de elevada disponibilidade portas. 
- A rota definida pelo utilizador (UDR) aplicado as rotas de sub-rede DMZ todo o tráfego para os NVAs efetuando o salto seguinte como interna IP virtuais do Balanceador de carga. 
- O Balanceador de carga interno distribui o tráfego para um os NVAs Active Directory, de acordo com o algoritmo de Balanceador de carga.
- A NVA processa o tráfego e reencaminha-lo para o destino original da sub-rede de back-end.
- O caminho de retorno pode demorar a rota mesma se um UDR correspondente estiver configurada na sub-rede de back-end. 

![Exemplo de implementação de elevada disponibilidade portas](./media/load-balancer-configure-ha-ports/haports.png)


## <a name="preview-sign-up"></a>Pré-visualização inscrição

Participar na pré-visualização da funcionalidade de elevada disponibilidade portas no balanceador de carga do Azure Standard, registe a sua subscrição para obter acesso utilizando 2.0 do Azure CLI ou PowerShell. Registar a sua subscrição para [padrão pré-visualização de Balanceador de carga](https://aka.ms/lbpreview#preview-sign-up).

>[!NOTE]
>Registo de pré-visualização de Balanceador de carga Standard pode demorar até uma hora.

## <a name="configure-high-availability-ports"></a>Configurar portas de disponibilidade elevada

Para configurar as portas de disponibilidade elevada, configure um balanceador de carga interno com os NVAs no conjunto de back-end. Configure uma configuração de pesquisa do Estado de funcionamento de Balanceador de carga correspondente para detetar o estado de funcionamento NVA e a regra de Balanceador de carga com alta disponibilidade portas. A configuração de associadas ao balanceador de carga geral é abordada [começar](load-balancer-get-started-ilb-arm-portal.md). Este artigo realça a configuração de elevada disponibilidade portas.

A configuração essencialmente envolve a definição de porta de front-end e o valor da porta de back-end para **0**. Defina o valor de protocolo **todos os**. Este artigo descreve como configurar portas de disponibilidade elevada, utilizando o portal do Azure, PowerShell e do Azure CLI 2.0.

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-the-azure-portal"></a>Configurar uma regra de Balanceador de carga de alta disponibilidade portas com o portal do Azure

Para configurar as portas de disponibilidade elevada, utilizando o portal do Azure, selecione o **HA portas** caixa de verificação. Quando selecionada, a configuração de porta e protocolo relacionada é preenchida automaticamente. 

![Configuração de elevada disponibilidade portas através do portal do Azure](./media/load-balancer-configure-ha-ports/haports-portal.png)


### <a name="configure-a-high-availability-ports-load-balancing-rule-via-the-resource-manager-template"></a>Configurar uma regra de balanceamento de carga para alta disponibilidade portas através do modelo do Resource Manager

Pode configurar as portas de disponibilidade elevada, utilizando a versão da API de 2017-08-01 para Microsoft.Network/loadBalancers no recurso de Balanceador de carga. O fragmento JSON seguinte ilustra as alterações na configuração de Balanceador de carga para alta disponibilidade portas através da API REST:

```json
    {
        "apiVersion": "2017-08-01",
        "type": "Microsoft.Network/loadBalancers",
        ...
        "sku":
        {
            "name": "Standard"
        },
        ...
        "properties": {
            "frontendIpConfigurations": [...],
            "backendAddressPools": [...],
            "probes": [...],
            "loadBalancingRules": [
             {
                "properties": {
                    ...
                    "protocol": "All",
                    "frontendPort": 0,
                    "backendPort": 0
                }
             }
            ],
       ...
       }
    }
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-powershell"></a>Configurar uma regra de Balanceador de carga de alta disponibilidade portas com o PowerShell

Utilize o seguinte comando para criar a regra de Balanceador de carga de alta disponibilidade portas enquanto cria o Balanceador de carga interno com o PowerShell:

```powershell
lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HAPortsRule" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "All" -FrontendPort 0 -BackendPort 0
```

### <a name="configure-a-high-availability-ports-load-balancer-rule-with-azure-cli-20"></a>Configurar uma regra de Balanceador de carga de alta disponibilidade portas com o Azure CLI 2.0

No passo 4 da [criar um conjunto de Balanceador de carga interno](load-balancer-get-started-ilb-arm-cli.md), utilize o seguinte comando para criar a regra de Balanceador de carga de alta disponibilidade portas:

```azurecli
azure network lb rule create --resource-group contoso-rg --lb-name contoso-ilb --name haportsrule --protocol all --frontend-port 0 --backend-port 0 --frontend-ip-name feilb --backend-address-pool-name beilb
```

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre [alta disponibilidade portas](load-balancer-ha-ports-overview.md).
