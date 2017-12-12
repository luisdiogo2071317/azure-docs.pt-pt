---
title: Configurar o Balanceador de carga para o SQL Server sempre no | Microsoft Docs
description: "Configurar o Balanceador de carga para trabalhar com o SQL Server Always On e aprender a utilizar o PowerShell para criar um balanceador de carga para a implementação do SQL Server"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 5e890f8314c8f191dbfa6c6818d810b91d0e829d
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="configure-a-load-balancer-for-sql-server-always-on"></a>Configurar um balanceador de carga para o SQL Server Always On

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

SQL Server Always On nos grupos de disponibilidade podem agora executar com um balanceador de carga interno. Um grupo de disponibilidade é a solução de flagship do SQL Server para elevada disponibilidade e recuperação após desastre. O serviço de escuta do grupo de disponibilidade permite aplicações liguem de forma totalmente integrada a réplica primária, independentemente do número de réplicas na configuração de cliente.

O nome do serviço de escuta (DNS) está mapeado para um endereço IP com balanceamento de carga. Balanceador de carga do Azure direciona o tráfego de entrada para apenas o servidor primário no conjunto de réplicas.

Pode utilizar o suporte de Balanceador de carga interno para o SQL Server Always On (serviço de escuta) pontos finais. Agora, tem controlo sobre a acessibilidade do serviço de escuta. Pode escolher o endereço IP com balanceamento de carga de uma sub-rede específica na sua rede virtual.

Utilizando um interno o Balanceador de carga no serviço de escuta, o ponto final do SQL Server (por exemplo, servidor = tcp:ListenerName, 1433; base de dados = DatabaseName) é acessível apenas a:

* Os serviços e VMs na mesma rede virtual.
* Os serviços e VMs do ligado redes no local.
* Os serviços e VMs do interligado redes virtuais.

![Balanceador de carga interno SQL Server Always On](./media/load-balancer-configure-sqlao/sqlao1.png)

## <a name="add-an-internal-load-balancer-to-the-service"></a>Adicionar um balanceador de carga interno para o serviço

1. No exemplo seguinte, configure uma rede virtual que contém uma sub-rede denominada 'Subnet-1':

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Adicione pontos finais com balanceamento de carga para um balanceador de carga interno em cada VM.

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    No exemplo anterior, tem duas VMs chamado "sqlsvc1" e "sqlsvc2" com o serviço em nuvem "Sqlsvc". Depois de criar o Balanceador de carga interno com o `DirectServerReturn` mudar, adicionar pontos finais com balanceamento de carga para o Balanceador de carga interno. Os pontos finais com balanceamento de carga permitir que o SQL Server para configurar os serviços de escuta para os grupos de disponibilidade.

Para obter mais informações sobre o SQL Server Always On, consulte [configurar um balanceador de carga interno para um grupo de disponibilidade Always On no Azure](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Consultar também
* [Começar a configurar um balanceador de carga público](load-balancer-get-started-internet-arm-ps.md)
* [Começar a configurar um balanceador de carga interno](load-balancer-get-started-ilb-arm-ps.md)
* [Configurar um modo de distribuição de balanceador de carga](load-balancer-distribution-mode.md)
* [Configurar definições de tempo limite TCP inativo para o balanceador de carga](load-balancer-tcp-idle-timeout.md)
