---
title: Os custos de software do Windows de instâncias de reserva do Azure | Microsoft Docs
description: Saiba quais medidores de software do Windows não estejam incluídas nos custos de instância de VM do Azure reservado.
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
ms.openlocfilehash: 6602424e2b2e4a4c2c3c51b5cb4bb1024a39d2e0
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063914"
---
# <a name="windows-software-costs-not-included-with-azure-reserved-instances"></a>Custos de software do Windows não incluídos com instâncias de reservados do Azure

Se não tiver uma vantagem de utilização de híbrida do Azure em máquinas virtuais instância reservado, em seguida, são-lhe cobrados para medidores de software do Windows listadas na secção seguinte.

## <a name="windows-software-meters-not-included-in-reserved-instance-cost"></a>Medidores de software do Windows não incluídos no custo de instância reservado

| Id do Medidor | MeterName no ficheiro de utilização | Utilizado por VM |
| ------- | ------------------------| --- |
| e7e152ac-f29c-4cce-ad6e-026192c01ef2 | Reserva Windows Svr Impulsar (1 núcleo) | Série B |
| cac255a2-9f0f-4c62-8bd6-f0fa449c5f76 | Reserva Windows Svr Impulsar (2 núcleos) | Série B |
| 09756b58-3fb5-4390-976d-9ddd14f9ed18 | Reserva Windows Svr Impulsar (4 núcleos) | Série B |
| e828cb37-5920-4dc7-b30f-664e4dbcb6c7 | Reserva Windows Svr Impulsar (8 núcleos) | Série B |
| f65a06cf-c9c3-47a2-8104-f17a8542215a | Reserva Windows Svr (1 núcleo) | Tudo, exceto série B |
| b99d40ae-41fe-4d1d-842b-56d72f3d15ee | Reserva Windows Svr (2 núcleos) | Tudo, exceto série B |
| 1cb88381-0905-4843-9ba2-7914066aabe5 | Reserva Windows Svr (4 núcleos) | Tudo, exceto série B |
| 07d9e10d-3e3e-4672-ac30-87f58ec4b00a | Reserva Windows Svr (6 núcleos) | Tudo, exceto série B |
| 603f58d1-1e96-460b-a933-ce3775ac7e2e | Reserva Windows Svr (8 núcleos) | Tudo, exceto série B |
| 36aaadda-da86-484a-b465-c8b5ab292d71 | Reserva Windows Svr (12 núcleos) | Tudo, exceto série B |
| 02968a6b-1654-4495-ada6-13f378ba7172 | Reserva Windows Svr (16 núcleos) | Tudo, exceto série B |
| 175434d8-75f9-474b-9906-5d151b6bed84 | Reserva Windows Svr (20 núcleos) | Tudo, exceto série B |
| 77eb6dd0-88f5-4a16-ab39-05d1742efb25 | Reserva Windows Svr (24 núcleos) | Tudo, exceto série B |
| 0d5bdf46-b719-4b1f-a780-b9bdfffd0591 | Reserva Windows Svr (32 núcleos) | Tudo, exceto série B |
| f1214b5c-cc16-445f-be6c-a3bb75f8395a | Reserva Windows Svr (40 núcleos) | Tudo, exceto série B |
| 637b7c77-65ad-4486-9cc7-dc7b3e9a8731 | Reserva Windows Svr (64 núcleos) | Tudo, exceto série B |
| da612742-e7cc-4ca3-9334-0fb7234059cd | Reserva Windows Svr (72 núcleos) | Tudo, exceto série B |
| a485cb8c-069b-4cf3-9a8e-ddd84b323da2 | Reserva Windows Svr (128 núcleos) | Tudo, exceto série B |
| 904c5c71-1eb7-43a6-961c-d305a9681624 | Reserva Windows Svr (256 núcleos) | Tudo, exceto série B |
| 6fdab81b-4284-4df9-8939-c237cc7462fe | Reserva Windows Svr (96 núcleos) | Tudo, exceto série B |

Pode obter o custo de cada uma destes medidores através do Azure RateCard API. Para obter informações sobre como obter as taxas de uma medição do azure, consulte [obter informações de preços e metadados para os recursos utilizados numa subscrição do Azure](https://msdn.microsoft.com/library/azure/mt219004).

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre instâncias reservadas do Azure, consulte os artigos seguintes:

- [Quais são reservados instâncias de VM do Azure?](billing-save-compute-costs-reservations.md)
- [Prepay para máquinas virtuais com instâncias VM reservados do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Gerir instâncias reservadas no Azure](billing-manage-reserved-vm-instance.md)
- [Compreender a forma como é aplicado o desconto instância reservado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instância reservado para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de instância reservado para a inscrição Enterprise](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.



