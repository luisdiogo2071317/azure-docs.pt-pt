---
title: Não é possível ligar às máquinas de virtuais do Azure, porque a porta RDP não está ativada no NSG | Documentos da Microsoft
description: Saiba como resolver um problema em que o RDP falha devido à configuração de NSG no portal do Azure | Documentos da Microsoft
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 89af30533e10df0968b60039d7ea15886e89bc25
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/20/2018
ms.locfileid: "52267099"
---
#  <a name="cannot-rdp-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>Não é possível RDP para uma VM porque a porta RDP não está ativada no NSG

Este artigo mostra como resolver um problema em que não é possível ligar ao Azure Windows (VMs máquinas virtuais), porque a porta não está ativada no grupo de segurança de rede.


> [!NOTE] 
> O Azure tem dois modelos de implementação para criar e trabalhar com recursos: [Resource Manager e Clássico](../../azure-resource-manager/resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação do Resource Manager, que recomendamos que utilize para novas implementações em vez do modelo de implementação clássica. 

## <a name="symptom"></a>Sintoma

Não pode fazer um protocolo RDP (Remote Desktop) ligação a uma VM no Azure devido à porta RDP não está aberta no grupo de segurança de rede.

## <a name="solution"></a>Solução 

Quando cria uma nova VM, todo o tráfego da Internet é bloqueado por predefinição. 

Para ativar a porta RDP no grupo de segurança de rede, siga estes passos:
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Na **máquinas virtuais**, a VM que tem o problema. 
3. Na **configurações**, selecione **Networking**. 
4. Na **regras de porta de entrada**, verifique se a porta para RDP está definida corretamente. Segue-se uma amostra da configuração. 

    **Prioridade**: 300 </br>
    **Porta**: 3389 </br>
    **Nome**: Port_3389 </br>
    **Porta**: 3389 </br>
    **Protocolo**: TCP </br>
    **Origem**: qualquer </br>
    **Destinos**: qualquer </br>
    **Ação**: permitir </br>

Em especificar o endereço IP de origem, esta definição permite o tráfego apenas a partir de um IP ou intervalo de IPs específicos para ligar à VM. Certifique-se de que o computador que estiver a utilizar para inicializar a sessão do RDP está no intervalo.

Para obter mais informações sobre o grupo de segurança de rede, consulte [grupo de segurança de rede](../../virtual-network/security-overview.md).

> [!NOTE]
> A porta RDP 3389 está exposta à Internet. Isto só é recomendado para teste. Para ambientes de produção, recomendamos que utilize uma VPN ou ligação privada.

## <a name="next-steps"></a>Passos Seguintes

Se a porta RDP já está ativada no grupo de segurança de rede, consulte [resolver problemas relacionados com um erro geral do RDP na VM do Azure](./troubleshoot-rdp-general-error.md).



