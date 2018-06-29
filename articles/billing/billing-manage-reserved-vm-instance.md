---
title: Gerir instâncias VM reservados do Azure | Microsoft Docs
description: Saiba como pode alterar o âmbito da subscrição e gerir o acesso para instâncias de VM do Azure reservado.
services: billing
documentationcenter: ''
author: vikramdesai01
manager: vikramdesai01
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: vikdesai
ms.openlocfilehash: ddb9d46dc2689b0dbcd8734e276916f7cd9d2728
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064130"
---
# <a name="manage-reserved-instances-in-azure"></a>Gerir instâncias reservadas no Azure

Depois de comprar uma instância de VM reservados do Azure, pode pretender aplicar a instância reservada para uma subscrição diferente daquela especificado durante a compra. Em alternativa, se as máquinas virtuais correspondentes estiver a executar em várias subscrições, poderá pretender alterar o âmbito de reservado instância partilhada. Para maximizar o desconto instância reservado, certifique-se de que corresponde ao número de instâncias comprou os atributos e o número de máquinas virtuais que terá de executar. Para saber mais sobre instâncias reservadas do Azure, consulte o artigo [poupar dinheiro pelo previamente pagar para máquinas virtuais do Azure](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reserved-instance"></a>Alterar o âmbito de uma instância reservado
 Os descontos de instância reservado se aplica a máquinas virtuais que corresponde à sua instância reservada e são executados dentro do âmbito de instância reservado. O âmbito de uma instância reservado pode ser única subscrição ou todas as subscrições do contexto de faturação. Se definir o âmbito para subscrição único, a instância reservada é correspondida a máquinas virtuais em execução na subscrição selecionada. Se definir o âmbito para correspondências partilhadas, do Azure a instância reservada para máquinas virtuais que são executados em todas as subscrições dentro do contexto de faturação. O contexto de faturação é dependente da subscrição utilizada para comprar a instância reservada. Para obter mais informações, consulte [pré-pagamento para VMs com instâncias reservadas](https://go.microsoft.com/fwlink/?linkid=861721).

Para atualizar o âmbito de uma instância reservado: 
1. Inicie sessão no [Portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione a instância reservada.
4. Selecione **definições** > **configuração**.
5. Altere o âmbito. Se mudar de partilhada único âmbito, só pode selecionar subscrições em que seja proprietário. Apenas as subscrições dentro do contexto de faturação mesmo que a instância reservado, pode ser selecionada. O contexto de faturação é determinado pela subscrição que selecionou quando a instância reservada foi comprou. O âmbito apenas se aplica a subscrições de MS-AZR - 0003P oferta pay as you go e as subscrições de oferta MS-AZR - 0017P empresariais. Para contratos enterprise, subscrições de programador/teste não são elegíveis para obter o desconto instância reservado.

## <a name="split-a-single-reserved-instance-into-two-reserved-instances"></a>Dividir uma única instância reservada para duas instâncias reservadas
 Depois de comprar mais do que uma instância, poderá ser útil de atribuir instâncias dentro de uma instância reservada a subscrições diferentes. Por predefinição, todas as instâncias (quantidade especificada durante a compra) tem um âmbito - uma única subscrição ou partilham. Por exemplo, adquirido 10 VMs D2 padrão e especificar o âmbito para ser subscrição A. Pode agora pretende alterar o âmbito de sete instâncias reservados para a subscrição A e o restante 3 à subscrição B. dividir uma instância reservada permite-lhe distribuir as instâncias para a gestão do âmbito granular. Pode simplificar a alocação para subscrições escolhendo âmbito partilhado. Mas para efeitos de gestão ou budgeting de custo, pode atribuir quantidades às subscrições específicas.

 Pode dividir uma instância reservada para duas instâncias reservadas apesar do PowerShell, CLI, ou através da API.

### <a name="split-a-reserved-instance-by-using-powershell"></a>Dividir uma instância reservada com o PowerShell
1. Obter o ID de ordem de instância reservado executando o seguinte comando:

    ```powershell
    # Get the reserved instance orders you have access to
    Get-AzureRmReservationOrder
    ```
2. Obter os detalhes de uma instância reservado:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```
3. Dividir a instância reservada para dois e distribuir as instâncias:

    ```powershell
    # Split the reserved instance. The sum of the reserved instances, the quantity, must equal the total number of instances in the reserved instance that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
1. Pode atualizar o âmbito, executando o seguinte comando:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="add-or-change-users-who-can-manage-a-reserved-instance"></a>Adicionar ou alterar os utilizadores que podem gerir uma instância reservada
Pode delegar a gestão de uma instância reservada adicionando as pessoas a funções na instância reservada. Por predefinição, a pessoa que comprou a instância reservada e o administrador de conta possui a função de proprietário na instância reservada. 

Pode gerir o acesso a instâncias reservadas independentemente de subscrições que obtenha o desconto instância reservado. Quando alguém conceder permissões para gerir uma instância reservada, que não conceder-lhes direitos para gerir a subscrição. E se alguém conceder permissões para gerir uma subscrição no âmbito da instância reservado, que não conceder-lhes direitos para gerir a instância reservada.
 
Delegar a gestão de acesso para uma instância reservada: 
1.  Inicie sessão no [Portal do Azure](https://portal.azure.com).
2.  Selecione **todos os serviços** > **reserva** para listar as instâncias reservadas que tenha acesso.
3.  Selecione a instância reservada que pretende delegar o acesso a outros utilizadores.
4.  Selecione **controlo de acesso (IAM)** no menu.
5.  Selecione **adicionar** > **função** > **proprietário** (ou uma função diferente se pretende conceder acesso limitado). 
6. Escreva o endereço de e-mail do utilizador que pretende adicionar como proprietário. 
7. Selecione o utilizador e, em seguida, selecione **guardar**.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre instâncias reservadas do Azure, consulte os artigos seguintes:

- [Quais são reservados instâncias de VM do Azure?](billing-save-compute-costs-reservations.md)
- [Prepay para máquinas virtuais com instâncias VM reservados do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Compreender a forma como é aplicado o desconto instância reservado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instância reservado para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de instância reservado para a inscrição Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Custos de software do Windows não incluídos com instâncias reservadas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.
