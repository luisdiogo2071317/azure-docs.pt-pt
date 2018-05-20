---
title: Compreender a utilização de instância reservados do Azure para a sua subscrição pay as you go - faturação do Azure | Microsoft Docs
description: Saiba como ler a sua utilização para compreender a forma como é aplicada a instância de VM reservados do Azure para a sua subscrição pay as you go.
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
ms.openlocfilehash: 7bf4aea86d4d430c15d60a8d73365705ace18b5a
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="understand-reserved-instance-usage-for-your-pay-as-you-go-subscription"></a>Compreender a utilização de instância reservado para a sua subscrição pay as you go

Compreender a utilização de uma instância de VM reservados do Azure utilizando o ReservationId de [página reserva](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=Reservations&Microsoft_Azure_Reservations=true#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) e o ficheiro de utilização a partir de [portal de contas do Azure](https://account.azure.com).


>[!NOTE]
>Este artigo não se aplica aos clientes EA. Se um cliente EA, consulte o artigo [utilização compreender instância reservado para inscrição da sua empresa.](billing-understand-reserved-instance-usage-ea.md) Este artigo também assume que a instância reservado é aplicada a uma única subscrição. Se a instância reservado é aplicada a mais do que uma subscrição, o benefício de instância reservado pode abranger vários ficheiros de utilização de csv. 

Para a secção seguinte, partem do princípio de que está a executar uma VM do Windows Standard_DS1_v2 no região EUA do leste e a instância reservado informações aspeto a tabela seguinte:

| Campo | Valor |
|---| :---: |
|ReservationId |8117adfb-1d94-4675-be2b-f3c1bca808b6|
|Quantidade |1|
|SKU | Standard_DS1_v2|
|Região | eualeste |

## <a name="reserved-instance-application"></a>Aplicação de instância reservada

A parte de hardware da VM é abrangida porque a VM implementada corresponda os atributos de instância reservado. Para ver o software do Windows não está abrangido pela instância reservado, aceda à [os custos de software do Windows de instâncias de VM de reserva do Azure.](billing-reserved-instance-windows-software-costs.md)

### <a name="statement-section-of-csv"></a>Secção de instrução de csv
Esta secção do seu csv mostra a utilização total para a instância reservado. Aplique o filtro no campo Subcategoria de medição que contenha "Reserva-" e o parece dados de captura de ecrã seguinte: ![captura de ecrã de detalhes de utilização de instância reservado filtrados e custos](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-statements.png)

A linha de Base de reserva de VM tem o número total de horas que estão abrangidas pela instância reservado. Esta linha é $0.00 porque a instância reservado abrange-lo. Reserva Windows Svr (1 núcleo) linha abrange o custo de software do Windows.

### <a name="daily-usage-section-of-csv"></a>Secção utilização diária de csv
Filtrar as informações adicionais e escreva no seu **ID de reserva**. A seguinte captura de ecrã mostra os campos relacionados com a instância reservado. 

![Captura de ecrã de detalhes de utilização diária e os encargos](./media/billing-understand-reserved-instance-usage/billing-payg-reserved-instance-csv-details.png)

1. **ReservationId** das informações adicionais de campo é a instância reservado que foi utilizado para aplicar benefício à VM.
2. ConsumptionMeter é o Id de medição para a VM.
3. Linha de Base de reserva VM medição subcategoria representa a linha de custo de $0 na secção de instrução. Custo de execução da VM já é paga pela instância reservado.
4. Este é o Id de medição para a instância reservado. Custo deste medidor é $0. Qualquer VM que qualificam para a instância reservado tem este MeterId no csv para a conta para o custo. 
5. Standard_DS1_v2 é uma vCPU VM e a VM está implementado sem benefício híbrida do Azure. Por conseguinte, este medidor inclui o custo adicional de software do Windows. Consulte [os custos de software do Windows de instâncias de VM de reserva do Azure.](billing-reserved-instance-windows-software-costs.md) Para localizar o medidor correspondente a série de D 1 núcleo VM. Se for utilizado o benefício de híbrida do Azure, este custo extra não está aplicado. 

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre instâncias reservadas, consulte os artigos seguintes:

- [Poupar dinheiro em máquinas virtuais com instâncias de reservados do Azure](billing-save-compute-costs-reservations.md)
- [Prepay para máquinas virtuais com instâncias reservadas](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gerir instâncias reservadas](billing-manage-reserved-vm-instance.md)
- [Compreender a forma como é aplicado o desconto instância reservado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instância reservado para a inscrição Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Custos de software do Windows não incluídos com instâncias reservado](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.
