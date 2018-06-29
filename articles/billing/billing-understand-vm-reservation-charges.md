---
title: Compreender a instâncias reservadas Azure desconto | Microsoft Docs
description: Saiba como desconto reservado instância de VM do Azure é aplicado a máquinas virtuais em execução.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: yashar
ms.openlocfilehash: d1229a49a5bb3bf3198c91a748ed37b7a626c506
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063915"
---
# <a name="understand-how-the-reserved-instance-discount-is-applied"></a>Compreender a forma como é aplicado o desconto instância reservado
Depois de comprar uma instância de VM reservados do Azure, o desconto instância reservado é aplicado automaticamente a máquinas virtuais correspondentes os atributos e a quantidade da instância reservada. Uma instância reservada abrange os custos de infraestrutura das suas máquinas virtuais. A tabela seguinte ilustra os custos para a máquina virtual depois de comprar uma instância reservada. Em todos os casos, são-lhe cobrados para armazenamento e redes nas taxas normais.

| Tipo de máquina virtual  | Encargos com instância reservado |    
|-----------------------|--------------------------------------------|
|VMs do Linux sem software adicional | A instância reservada abrange os custos de infraestrutura VM.|
|VMs com Linux com custos de software (por exemplo, Red Hat) | A instância reservada abrange os custos de infraestrutura. São-lhe cobrados para o software adicional.|
|VMs do Windows sem software adicional |A instância reservada abrange os custos de infraestrutura. São-lhe cobrados de software do Windows.|
|VMs do Windows com o software adicional (por exemplo, o SQL server) | A instância reservada abrange os custos de infraestrutura. São-lhe cobrados para software do Windows e para software adicional.|
|VMs do Windows com [benefício híbridos do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing) | A instância reservada abrange os custos de infraestrutura. Os custos de software do Windows estão abrangidos pelo benefício de híbrida do Azure. Qualquer software adicional é cobrada separadamente.| 

## <a name="application-of-reserved-instance-discount-to-non-windows-vms"></a>Aplicação de desconto de instância reservado para não - VMs do Windows
 O desconto instância reservados do Azure é aplicado a executar instâncias VM numa base horária. As instâncias reservadas que adquiriu são correspondidas para utilização emitida pelas VMs em execução para aplicar o desconto instância reservado. Para VMs que poderão não ser executadas na hora completa, a instância reservada será preenchida de outras VMs que não a utilizar uma instância reservada, incluindo VMs em execução em simultâneo. No final da hora, a aplicação de instância reservados para as VMs na hora está bloqueada. Em caso de uma VM não é executado para uma hora ou VMs em simultâneo dentro da hora não preencher a hora da instância reservada, a instância reservada é subutilizada para essa hora. O gráfico seguinte ilustra a aplicação de uma instância reservada para utilização VM sujeito a faturação. A ilustração é com base na compra de uma instância reservado e duas instâncias VM correspondentes.

![Captura de ecrã de uma instância de reservado aplicados e duas instâncias VM correspondentes](media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1.  Qualquer utilização que é superior a linha de instância reservado obtém debitada às taxas de pay as you go regulares. Não está encargos para qualquer utilização abaixo da linha de instâncias reservado, desde que tenha sido já paga como parte da compra de instância reservado.
2.  Na hora 1, instância 1 executa 0.75 horas e instância 2 executa 0.5 horas. Total de utilizações para hora 1 é 1.25 horas. São-lhe cobrados a taxas pay as you go para as horas 0,25 restantes.
3.  Para hora 2 e 3 de hora, ambas as instâncias foi executada para 1 hora. Uma instância é abrangida pela instância reservada e o outro é debitado às taxas de pay as you go.
4.  Para as 4 horas, instância 1 executa 0.5 horas e instância 2 executa para 1 hora. Instância 1 totalmente está abrangida pela instância reservada e são explicadas 0.5 horas de instância de 2. Está a cobrada a taxa de pay as you go para as horas 0.5 restantes.

Compreender e ver a aplicação das suas instâncias reservadas Azure relatórios de utilização de faturação, consulte [utilização compreender reservado instância](https://go.microsoft.com/fwlink/?linkid=862757).

## <a name="application-of-reserved-instance-discount-to-windows-vms"></a>Aplicação de desconto de instância reservado para VMs do Windows
Quando estiver a executar instâncias de VM do Windows, é aplicada a instância reservada para cobrir os custos de infraestrutura. A aplicação da instância reservada para os custos de infraestrutura VM para VMs do Windows é o mesmo para não - VMs do Windows. São-lhe cobrados em separado para software do Windows numa base por vCPU. Consulte [os custos de software do Windows com instâncias reservados](https://go.microsoft.com/fwlink/?linkid=862756). Pode abranger o Windows licenciamento os custos com [Azure híbrida benefício para o Windows Server] (https://docs.microsoft.com/azure/virtual-machines/windows/hybrid-use-benefit-licensing).

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre instâncias reservadas, consulte os artigos seguintes:

- [Quais são reservados instâncias de VM do Azure?](billing-save-compute-costs-reservations.md)
- [Prepay para máquinas virtuais com instâncias VM reservados do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gerir instâncias reservadas no Azure](billing-manage-reserved-vm-instance.md)
- [Compreender a forma como é aplicado o desconto instância reservado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instância reservado para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de instância reservado para a inscrição Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Compreender a utilização de instância reservado para subscrições de CSP](https://docs.microsoft.com/partner-center/azure-reservations)
- [Custos de software do Windows não incluídos com instâncias reservadas](billing-reserved-instance-windows-software-costs.md)


## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.
