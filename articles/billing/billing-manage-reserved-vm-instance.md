---
title: Gerir reservas do Azure | Documentos da Microsoft
description: Saiba como pode alterar o âmbito da subscrição e gerir o acesso para as reservas do Azure.
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
ms.date: 08/08/2018
ms.author: yashesvi
ms.openlocfilehash: d47c85d4197f45db50f1974b6faea270e6761237
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628577"
---
# <a name="manage-reservations-for-resources-in-azure"></a>Gerir reservas para recursos no Azure

Depois de comprar uma reserva do Azure, poderá ser útil aplicar a reserva para uma subscrição diferente que o especificado durante a compra. Em alternativa, se suas máquinas virtuais correspondentes, bases de dados SQL ou outros recursos estão a executar em várias subscrições, convém alterar o âmbito de reserva para compartilhado. Para maximizar o desconto de reserva, certifique-se de que o número de instâncias comprou corresponde aos atributos e o número de recursos que tem em execução. Para obter mais informações, consulte [reservas Azure](https://go.microsoft.com/fwlink/?linkid=862121).

## <a name="change-the-scope-for-a-reservation"></a>Alterar o âmbito de uma reserva

 O desconto de reserva se aplica a máquinas virtuais, bases de dados SQL ou outros recursos que corresponde à sua reserva e executados dentro do âmbito de reserva. O âmbito de uma reserva pode ser subscrição única ou todas as subscrições no seu contexto de faturação. Se definir o âmbito para cada subscrição individual, a reserva é correspondida à execução de recursos na subscrição selecionada. Se definir o escopo compartilhado, o Azure corresponde a reserva de recursos que são executados em todas as subscrições dentro do contexto de faturação. O contexto de faturação é dependente da subscrição utilizada para comprar a reserva.

Para atualizar o âmbito de uma reserva:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione a reserva.
4. Selecione **Definições** > **Configuração**.
5. Altere o âmbito. Se alterar do partilhadas com âmbito único, só pode selecionar subscrições em que é o proprietário. Apenas as subscrições no mesmo contexto de faturação que a reserva, pode ser selecionada. O contexto de faturação é determinado pela subscrição que selecionou quando a reserva foi adquirida. O âmbito aplica-se apenas a subscrições de MS-AZR - 0003p oferta pay as you go e as subscrições de oferta MS-AZR - 0017P empresariais. Para contratos enterprise, subscrições de desenvolvimento/teste não são elegíveis para obter o desconto de reserva.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Adicionar ou alterar os utilizadores que podem gerir uma reserva

Pode delegar a gestão de uma reserva ao adicionar as pessoas a funções de reserva. Por predefinição, a pessoa que comprasse a reserva e o administrador de conta têm a função de proprietário a reserva.

Pode gerir o acesso a reservas de forma independente das subscrições do que obtém o desconto de reserva. Quando concede permissões para gerir uma reserva de alguém, que não lhes direitos para gerir a subscrição. E se conceder a alguém permissões para gerir uma subscrição no âmbito da reserva, que não lhes direitos para gerir a reserva.

Para delegar a gestão de acesso para uma reserva:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reserva** para reservas de lista que tem acesso a.
3. Selecione a reserva que pretende delegar acesso a outros utilizadores.
4. Selecione **controlo de acesso (IAM)** no menu.
5. Selecione **Add** > **função** > **proprietário** (ou uma função diferente se pretende conceder acesso limitado).
6. Escreva o endereço de e-mail do utilizador que pretende adicionar como proprietário. 
7. Selecione o utilizador e, em seguida, selecione **Guardar**.

## <a name="optimize-reserved-vm-instance-for-vm-size-flexibility-or-capacity-priority"></a>Otimizar a instância de VM reservada para prioridade de flexibilidade ou capacidade de tamanho VM

 Flexibilidade de instância VM aplica o desconto de reserva às outras VMs no mesmo [grupo de tamanho VM](https://aka.ms/RIVMGroups). Por predefinição, quando o âmbito da reserva é partilhado, a flexibilidade de tamanho de instância está ativada e a capacidade do datacenter não está priorizada para implementações de VM. Para as reservas em que o âmbito é único, pode otimizar a reserva de prioridade de capacidade em vez de flexibilidade de tamanho de instância VM. Prioridade de capacidade reserva-se a capacidade do Centro de dados para as implementações, oferece uma confiança adicional na sua habilidade de iniciar as instâncias VM quando precisar delas.

Para atualizar o âmbito de uma reserva:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione a reserva.
4. Selecione **Definições** > **Configuração**.
5. Altere a otimizar para definição.

## <a name="split-a-single-reservation-into-two-reservations"></a>Dividir uma reserva única em duas reservas

 Depois de comprar mais de uma instância, pode querer atribuir instâncias dentro de uma reserva a subscrições diferentes. Por predefinição, todas as instâncias (quantidade especificada durante a compra) tem um âmbito - qualquer subscrição individual ou partilhado. Por exemplo, comprado 10 VMs D2 padrão e especificar o âmbito para a subscrição A. Agora pretende alterar o âmbito de reservas de sete a subscrição A e o 3 restantes à subscrição B. a divisão de uma reserva permite que distribua as instâncias para gestão do âmbito granular. Pode simplificar a alocação para subscrições escolhendo o âmbito partilhado. Mas para fins de gestão ou o orçamento de custo, pode alocar quantidades para subscrições específicas.

 Pode dividir uma reserva para duas reservas entanto PowerShell, CLI, ou por meio da API.

### <a name="split-a-reservation-by-using-powershell"></a>Dividir uma reserva com o PowerShell

1. Obtenha o ID de encomenda de reserva ao executar o seguinte comando:

    ```powershell
    # Get the reservation orders you have access to
    Get-AzureRmReservationOrder
    ```

2. Obter os detalhes de uma reserva:

    ```powershell
    Get-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a
    ```

3. Dividir a reserva em duas e distribuir as instâncias:

    ```powershell
    # Split the reservation. The sum of the reservations, the quantity, must equal the total number of instances in the reservation that you're splitting.
    Split-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId b8be062a-fb0a-46c1-808a-5a844714965a -Quantity 3,2
    ```
4. Pode atualizar o âmbito ao executar o seguinte comando:
    ```powershell
    Update-AzureRmReservation -ReservationOrderId a08160d4-ce6b-4295-bf52-b90a5d4c96a0 -ReservationId 5257501b-d3e8-449d-a1ab-4879b1863aca -AppliedScopeType Single -AppliedScope /subscriptions/15bb3be0-76d5-491c-8078-61fe3468d414
    ```

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as reservas do Azure, veja os artigos seguintes:

- [Quais são as reservas do Azure?](billing-save-compute-costs-reservations.md)
- [Efetue o pré-pagamento de máquinas virtuais com instâncias de VM reservadas do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Efetue o pré-pagamento do recursos de computação de base de dados SQL com capacidade de base de dados do SQL Azure reservados](../sql-database/sql-database-reserved-capacity.md)
- [Compreender como o desconto de reserva é aplicado](billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de reserva para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de reserva para inscrição da sua empresa](billing-understand-reserved-instance-usage-ea.md)
- [Custos de software do Windows não incluídos com reservas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
