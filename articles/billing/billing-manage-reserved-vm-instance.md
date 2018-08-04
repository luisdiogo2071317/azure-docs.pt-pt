---
title: Gerir instâncias de VM reservadas do Azure | Documentos da Microsoft
description: Saiba como pode alterar o âmbito da subscrição e gerir o acesso para instâncias de VM reservadas do Azure.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashesvi
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2018
ms.author: yashesvi
ms.openlocfilehash: 589afc736faaa210fdcd5cf6c79d10af4af3c0e9
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502530"
---
# <a name="manage-reserved-instances-in-azure"></a>Gerir as instâncias reservadas no Azure

Depois de comprar uma instância de VM reservada do Azure, poderá ser útil aplicar a instância reservada para uma subscrição diferente que o especificado durante a compra. Em alternativa, se as máquinas virtuais correspondentes estiverem a executar em várias subscrições, convém alterar o âmbito de instância reservada para compartilhado. Para maximizar o desconto de instância reservada, certifique-se de que o número de instâncias comprou corresponde aos atributos e o número de máquinas virtuais que tem em execução. Para saber mais sobre as instâncias reservadas do Azure, veja [economizar dinheiro ao pagar previamente para máquinas virtuais do Azure](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reserved-instance"></a>Alterar o âmbito de uma instância reservada
 O desconto de instância reservada aplica-se às máquinas virtuais que corresponde à sua instância reservada e executados dentro do escopo de instância reservada. O âmbito de uma instância reservada pode ser subscrição única ou todas as subscrições no seu contexto de faturação. Se definir o âmbito para cada subscrição individual, a instância reservada é correspondida a execução de máquinas virtuais na subscrição selecionada. Se definir o âmbito para correspondências de partilhado, o Azure a instância reservada para máquinas virtuais que executem em todas as subscrições dentro do contexto de faturação. O contexto de faturação é dependente da subscrição utilizada para comprar a instância reservada. Para obter mais informações, consulte [pré-pagamento para as VMs com as instâncias reservadas](https://go.microsoft.com/fwlink/?linkid=861721).

Para atualizar o âmbito de uma instância reservada: 
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione a instância reservada.
4. Selecione **Definições** > **Configuração**.
5. Altere o âmbito. Se alterar do partilhadas com âmbito único, só pode selecionar subscrições em que é o proprietário. Apenas as subscrições no mesmo contexto de faturação como a instância reservada, pode ser selecionada. O contexto de faturação é determinado pela subscrição que selecionou quando a instância reservada foi adquirida. O âmbito aplica-se apenas a subscrições de MS-AZR - 0003p oferta pay as you go e as subscrições de oferta MS-AZR - 0017P empresariais. Para contratos enterprise, subscrições de desenvolvimento/teste não são elegíveis para obter o desconto de instância reservada.

## <a name="add-or-change-users-who-can-manage-a-reserved-instance"></a>Adicionar ou alterar os utilizadores que podem gerir uma instância reservada
Pode delegar a gestão de uma instância reservada, adicionando as pessoas a funções na instância reservada. Por predefinição, a pessoa que comprou a instância reservada e o administrador de conta têm a função de proprietário na instância reservada. 

Pode gerir o acesso às instâncias reservadas independentemente das subscrições do que obtém o desconto de instância reservada. Quando conceder a alguém permissões para gerir uma instância reservada, que não lhes direitos para gerir a subscrição. E se conceder a alguém permissões para gerir uma subscrição no âmbito da instância reservada, que não lhes direitos para gerir a instância reservada.
 
Para delegar a gestão de acesso para uma instância reservada: 
1.  Inicie sessão no [portal do Azure](https://portal.azure.com).
2.  Selecione **todos os serviços** > **reserva** para listar as instâncias reservadas tem acesso.
3.  Selecione a instância reservada que pretende delegar acesso a outros utilizadores.
4.  Selecione **controlo de acesso (IAM)** no menu.
5.  Selecione **Add** > **função** > **proprietário** (ou uma função diferente se pretende conceder acesso limitado). 
6. Escreva o endereço de e-mail do utilizador que pretende adicionar como proprietário. 
7. Selecione o utilizador e, em seguida, selecione **guardar**.

## <a name="optimize-reserved-vm-instance-for-vm-size-flexibility-or-capacity-priority"></a>Otimizar a instância de VM reservada para prioridade de flexibilidade ou capacidade de tamanho VM
 Flexibilidade de instância VM aplica o desconto de reserva às outras VMs no mesmo [grupo de tamanho VM](https://aka.ms/RIVMGroups). Por predefinição, quando o âmbito da reserva é partilhado, a flexibilidade de tamanho de instância está ativada e a capacidade do datacenter não está priorizada para implementações de VM. Para as reservas em que o âmbito é único, pode otimizar a reserva de prioridade de capacidade em vez de flexibilidade de tamanho de instância VM. Prioridade de capacidade reserva-se a capacidade do Centro de dados para as implementações, oferece uma confiança adicional na sua habilidade de iniciar as instâncias VM quando precisar delas.

Para atualizar o âmbito de uma instância reservada: 
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione a instância reservada.
4. Selecione **Definições** > **Configuração**.
5. Altere a otimizar para definição.

## <a name="split-a-single-reserved-instance-into-two-reserved-instances"></a>Dividir uma única instância reservada em duas instâncias reservadas
 Depois de comprar mais de uma instância, pode querer atribuir instâncias dentro de uma instância reservada a subscrições diferentes. Por predefinição, todas as instâncias (quantidade especificada durante a compra) tem um âmbito - qualquer subscrição individual ou partilhado. Por exemplo, comprado 10 VMs D2 padrão e especificar o âmbito para a subscrição A. Agora pretende alterar o âmbito das instâncias reservadas sete, a subscrição A e o 3 restantes à subscrição B. a divisão de uma instância reservada permite que distribua as instâncias para gestão do âmbito granular. Pode simplificar a alocação para subscrições escolhendo o âmbito partilhado. Mas para fins de gestão ou o orçamento de custo, pode alocar quantidades para subscrições específicas.

 Pode dividir uma instância reservada em duas instâncias reservadas entanto PowerShell, CLI, ou por meio da API.

### <a name="split-a-reserved-instance-by-using-powershell"></a>Dividir uma instância reservada com o PowerShell
1. Obter o ID de encomenda de instância reservada, execute o seguinte comando:

    ```powershell
    # Get the reserved instance orders you have access to
    Get-AzureRmReservationOrder
    ```

2. Obter os detalhes de uma instância reservada:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Dividir a instância reservada em duas e distribuir as instâncias:

    ```powershell
    # Split the reserved instance. The sum of the reserved instances, the quantity, must equal the total number of instances in the reserved instance that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```

1. Pode atualizar o âmbito ao executar o seguinte comando:

    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre as instâncias reservadas do Azure, veja os artigos seguintes:

- [O que são instâncias de VM reservadas do Azure?](billing-save-compute-costs-reservations.md)
- [Efetue o pré-pagamento de máquinas virtuais com instâncias de VM reservadas do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Compreender a aplicação de descontos sobre as instâncias reservadas](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instâncias reservadas na subscrição Pay As You Go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de instâncias reservadas na inscrição Enterprise](billing-understand-reserved-instance-usage-ea.md)
- [Custos de software Windows não incluídos nas instâncias reservadas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
