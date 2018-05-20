---
title: Gerir instâncias reservadas Azure - faturação do Azure | Microsoft Docs
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
ms.openlocfilehash: fc473906be9c572e6d6549c85f9faa8fe7566b86
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="manage-reserved-instances"></a>Gerir instâncias reservadas

Depois de comprar uma instância de VM reservados do Azure, pode pretender aplicar a instância reservado para uma subscrição diferente daquela especificado durante a compra. Em alternativa, se as máquinas virtuais correspondentes estiver a executar em várias subscrições, poderá pretender alterar o âmbito de reservado instância partilhada. Para maximizar o desconto instância reservado, certifique-se de que corresponde ao número de instâncias comprou os atributos e o número de máquinas virtuais que terá de executar. Para saber mais sobre instâncias reservados do Azure, consulte [poupar dinheiro pelo previamente pagar para máquinas virtuais do Azure](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reserved-instance"></a>Alterar o âmbito de uma instância reservado
 Os descontos de instância reservado se aplica a máquinas virtuais que corresponde à sua instância reservado e são executados dentro do âmbito de instância reservado. O âmbito de uma instância reservado pode ser única subscrição ou todas as subscrições do contexto de faturação. Se definir o âmbito para subscrição único, a instância reservado é correspondida a máquinas virtuais em execução na subscrição selecionada. Se definir o âmbito para correspondências partilhadas, do Azure a instância reservado para máquinas virtuais que são executados em todas as subscrições dentro do contexto de faturação. O contexto de faturação é dependente da subscrição utilizada para comprar a instância reservado. Para obter mais informações, consulte [pré-pagamento para VMs com instâncias reservado](https://go.microsoft.com/fwlink/?linkid=861721).

Para atualizar o âmbito de uma instância reservado: 
1. Inicie sessão no [Portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione a instância reservada.
4. Selecione **definições** > **configuração**.
5. Altere o âmbito. Se mudar de partilhada único âmbito, só pode selecionar subscrições em que seja proprietário. Apenas as subscrições dentro do contexto de faturação mesmo que a instância reservado, pode ser selecionada. O contexto de faturação é determinado pela subscrição que selecionou quando a instância reservado foi comprou. O âmbito apenas se aplica a subscrições de MS-AZR - 0003P oferta pay as you go e as subscrições de oferta MS-AZR - 0017P empresariais. Para contratos enterprise, subscrições de programador/teste não são elegíveis para obter o desconto instância reservado.

## <a name="split-a-single-reserved-instance-into-two-reserved-instances"></a>Dividir uma única instância reservado em duas instâncias reservado
 Depois de comprar mais do que uma instância, poderá ser útil de atribuir instâncias dentro de uma instância reservado a subscrições diferentes. Por predefinição, todas as instâncias (quantidade especificada durante a compra) tem um âmbito - uma única subscrição ou partilham. Por exemplo, adquirido 10 VMs D2 padrão e especificar o âmbito para ser subscrição A. Pode agora pretende alterar o âmbito de sete instâncias reservado para a subscrição A e o restante 3 à subscrição B. dividir uma instância reservado permite-lhe distribuir as instâncias para granulares definir o âmbito de gestão. Pode simplificar a alocação para subscrições escolhendo âmbito partilhado. Mas para efeitos de gestão ou budgeting de custo, pode atribuir quantidades às subscrições específicas.

 Pode dividir uma instância reservado para duas instâncias reservado apesar do PowerShell, CLI, ou através da API.

### <a name="split-a-reserved-instance-by-using-powershell"></a>Dividir uma instância reservado com o PowerShell
1. Obter o ID de ordem de instância reservado executando o seguinte comando:

    ```powershell
    # Get the Reserved Instance orders you have access to
    Get-AzureRmReservationOrder
    ```
2. Obter os detalhes de uma instância reservado:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```
3. Dividir a instância reservado em dois e distribuir as instâncias:

    ```powershell
    # Split the Reserved Instance. The sum of the Reserved Instances, the quantity, must equal the total number of instances in the Reserved Instance that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
1. Pode atualizar o âmbito, executando o seguinte comando:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="add-or-change-users-who-can-manage-a-reserved-instance"></a>Adicionar ou alterar os utilizadores que podem gerir uma instância reservado
Pode delegar a gestão de uma instância reservado adicionando as pessoas a funções na instância reservado. Por predefinição, a pessoa que comprou a instância reservado e o administrador de conta possui a função de proprietário na instância reservado. 

Pode gerir o acesso a instâncias reservado independentemente de subscrições que obtenha o desconto instância reservado. Quando alguém conceder permissões para gerir uma instância reservado, que não conceder-lhes direitos para gerir a subscrição. E se alguém conceder permissões para gerir uma subscrição no âmbito da instância reservado, que não conceder-lhes direitos para gerir a instância reservado.
 
Delegar a gestão de acesso para uma instância reservado: 
1.  Inicie sessão no [Portal do Azure](https://portal.azure.com).
2.  Selecione **todos os serviços** > **reserva** à lista de instâncias reservado que tem acesso.
3.  Selecione a instância reservado que pretende delegar o acesso a outros utilizadores.
4.  Selecione **controlo de acesso (IAM)** no menu.
5.  Selecione **adicionar** > **função** > **proprietário** (ou uma função diferente se pretende conceder acesso limitado). 
6. Escreva o endereço de e-mail do utilizador que pretende adicionar como proprietário. 
7. Selecione o utilizador e, em seguida, selecione **guardar**.

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre instâncias reservados do Azure, consulte os artigos seguintes:

- [Poupar dinheiro em máquinas virtuais com instâncias de reservados do Azure](billing-save-compute-costs-reservations.md)
- [Prepay para máquinas virtuais com instâncias reservadas](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Compreender a forma como é aplicado o desconto instância reservado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instância reservado para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de instância reservado para a inscrição Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Custos de software do Windows não incluídos com instâncias reservado](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.
