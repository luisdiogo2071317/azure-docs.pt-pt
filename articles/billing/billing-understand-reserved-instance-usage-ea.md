---
title: Compreender a utilização de instância reservados do Azure para Enterprise | Microsoft Docs
description: Saiba como ler a sua utilização para compreender a forma como a instância de VM reservados do Azure para a inscrição Enterprise é aplicada.
services: billing
documentationcenter: ''
author: manish-shukla01
manager: manshuk
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: manshuk
ms.openlocfilehash: d6e8b2544f919abeb7fde0e37fc12bd29f0171ef
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064135"
---
# <a name="understand-azure-reserved-instance-usage-for-your-enterprise-enrollment"></a>Compreender a utilização de instância reservados do Azure para a inscrição Enterprise
Compreender a utilização de uma instância reservada utilizando o **ReservationId** de [página reservas](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) e o ficheiro de utilização do [EA portal](https://ea.azure.com). Também pode ver a utilização de instância reservado na secção de resumo de utilização do [EA portal](https://ea.azure.com).

>[!NOTE]
>Se comprou a instância reservada num contexto de faturação pay as you go, consulte o artigo [compreender reservados a utilização de instância para a sua subscrição pay as you go.](billing-understand-reserved-instance-usage.md)

Para a secção seguinte, partem do princípio de que está a executar uma VM do Windows Standard_D1_v2 o região EUA do leste e a sua instância reservado informações parece que a tabela seguinte:

| Campo | Valor |
|---| --- |
|ReservationId |8f82d880-d33e-4e0d-bcb5-6bcb5de0c719|
|Quantidade |1|
|SKU | Standard_D1|
|Região | eualeste |

## <a name="reserved-instance-application"></a>Aplicação de instância reservado

A parte de hardware da VM é abrangida porque a VM implementada corresponda os atributos de instância reservado. Para ver o software do Windows não está abrangido pela instância reservada, aceda a custos de software reservado instâncias de VM do Azure, aceda a [os custos de software do Windows de instâncias de VM de reserva do Azure.](billing-reserved-instance-windows-software-costs.md)


### <a name="reserved-instance-usage-in-csv"></a>Utilização de instância reservado no csv
Pode transferir o csv de utilização EA partir do portal EA. No ficheiro csv transferido, filtrar as informações adicionais e escreva no seu **ReservationID**. A seguinte captura de ecrã mostra os campos relacionados com a instância reservada:

![Csv de Enterprise Agreement (EA) para a instância reservada do Azure](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-csv.png)

1. **ReservationId** das informações adicionais de campo representa a instância reservada que foi utilizada para aplicar benefício à VM.
2. ConsumptionMeter é MeterId para a VM.
3. Esta é a medição de reserva de custo de $0, desde que o custo de execução da VM já é paga pela instância reservada. 
4. Standard_D1 é uma vCPU VM e a VM está implementado sem benefício híbrida do Azure. Por conseguinte, este medidor inclui o custo adicional de software do Windows. Consulte [os custos de software do Windows de instâncias de VM de reserva do Azure.](billing-reserved-instance-windows-software-costs.md) Para localizar o medidor correspondente a série de D 1 núcleo VM. Se for utilizado o benefício de híbrida do Azure, este custo extra não será aplicado.

### <a name="reserved-instance-usage-in-usage-summary-page-in-ea-portal"></a>Utilização de instância reservado na página de resumo de utilização no portal EA

Utilização de instância reservado também aparece na secção de resumo de utilização do portal EA: ![resumo de utilização de Enterprise Agreement (EA)](./media/billing-understand-reserved-instance-usage-ea/billing-ea-reserved-instance-usagesummary.png)

1. Não são cobradas para o componente de hardware da VM, tal como está abrangido por instância reservada. 
2. São-lhe cobrados de software do Windows que benefício híbrida do Azure não está a ser utilizado. 

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre instâncias reservadas do Azure, consulte os artigos seguintes:

- [Quais são reservados instâncias de VM do Azure?](billing-save-compute-costs-reservations.md)
- [Prepay para máquinas virtuais com instâncias VM reservados do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gerir instâncias reservadas no Azure](billing-manage-reserved-vm-instance.md)
- [Compreender a forma como é aplicado o desconto instância reservado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instância reservado para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Custos de software do Windows não incluídos com instâncias reservadas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.