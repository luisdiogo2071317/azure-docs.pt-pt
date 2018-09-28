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
ms.author: cwatson
ms.openlocfilehash: 0b19bb0d77bb600258596ce369713464641a7d2f
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423243"
---
# <a name="manage-reservations-for-azure-resources"></a>Gerir reservas para recursos do Azure

Depois de comprar uma reserva de Azure, terá de aplicar a reserva para uma subscrição diferente, altere a quem pode gerir a reserva ou alterar o âmbito da reserva. Também pode dividir uma reserva para duas reservas para aplicar algumas instâncias comprou a outra subscrição.

Se comprasse Azure Reserved Virtual Machine Instances, pode alterar a definição de otimização para a reserva. O desconto de reserva pode aplicar às VMs da série do mesmo ou pode reservar capacidade do Centro de dados para um tamanho VM específico.

## <a name="change-the-scope-for-a-reservation"></a>Alterar o âmbito de uma reserva

 O desconto de reserva se aplica a máquinas virtuais, bases de dados SQL, Azure Cosmos DB ou outros recursos que corresponde à sua reserva e executados dentro do âmbito de reserva. O âmbito de uma reserva pode ser subscrição única ou todas as subscrições no seu contexto de faturação. Se definir o âmbito para cada subscrição individual, a reserva é correspondida à execução de recursos na subscrição selecionada. Se definir o escopo compartilhado, o Azure corresponde a reserva de recursos que são executados em todas as subscrições dentro do contexto de faturação. O contexto de faturação é dependente da subscrição utilizada para comprar a reserva.

Para atualizar o âmbito de uma reserva:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione a reserva.
4. Selecione **Definições** > **Configuração**.
5. Altere o âmbito. 

Se alterar do partilhadas com âmbito único, só pode selecionar subscrições em que é o proprietário. Apenas as subscrições no mesmo contexto de faturação que a reserva, pode ser selecionada.

O âmbito só se aplica a oferta de pay as you go MS-AZR - 0003p, a oferta Enterprise MS-AZR - 0017P ou tipos de subscrição do CSP. Para contratos enterprise, subscrições de desenvolvimento/teste não são elegíveis para obter o desconto de reserva.

## <a name="add-or-change-users-who-can-manage-a-reservation"></a>Adicionar ou alterar os utilizadores que podem gerir uma reserva

Pode delegar a gestão de uma reserva ao adicionar as pessoas a funções de reserva. Por predefinição, a pessoa que comprasse a reserva e o administrador de conta têm a função de proprietário a reserva.

Pode gerir o acesso a reservas de forma independente das subscrições do que obtém o desconto de reserva. Quando concede permissões para gerir uma reserva de alguém, que não lhes direitos para gerir a subscrição. E se conceder a alguém permissões para gerir uma subscrição no âmbito da reserva, que não lhes direitos para gerir a reserva.

Para delegar a gestão de acesso para uma reserva:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reserva** para reservas de lista que tem acesso a.
3. Selecione a reserva que pretende delegar acesso a outros utilizadores.
4. Selecione **controlo de acesso (IAM)**.
5. Selecione **adicione** > **função** > **proprietário**. Em alternativa, se pretende conceder acesso limitado, selecione uma função diferente.
6. Escreva o endereço de e-mail do utilizador que pretende adicionar como proprietário.
7. Selecione o utilizador e, em seguida, selecione **Guardar**.

## <a name="split-a-single-reservation-into-two-reservations"></a>Dividir uma reserva única em duas reservas

 Depois de comprar mais do que uma instância de recurso dentro de uma reserva, pode querer atribuir instâncias dentro desse reserva a subscrições diferentes. Por predefinição, todas as instâncias têm um âmbito - qualquer subscrição individual ou partilhado. Por exemplo, comprou 10 instâncias de reserva e especificar o âmbito para a subscrição A. Agora pretende alterar o âmbito de reservas de 7 a subscrição A e o 3 restantes à subscrição B. a divisão de uma reserva permite que distribua as instâncias para gestão do âmbito granular. Pode simplificar a alocação para subscrições escolhendo o âmbito partilhado. Mas para fins de gestão ou o orçamento de custo, pode alocar quantidades para subscrições específicas.

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

## <a name="cancellations-and-exchanges"></a>Cancelamentos e trocas

Dependendo do tipo de reserva, poderá cancelar ou trocar uma reserva. Para obter mais informações, consulte o cancelamento e trocas secções nos seguintes tópicos:

- [Efetue o pré-pagamento de máquinas virtuais com instâncias de VM reservadas do Azure](..//virtual-machines/windows/prepay-reserved-vm-instances.md#cancellations-and-exchanges)
- [Efetue o pré-pagamento do planos de software SUSE das reservas do Azure](../virtual-machines/linux/prepay-suse-software-charges.md#cancellation-and-exchanges-not-allowed)
- [Efetue o pré-pagamento do recursos de computação de base de dados SQL com capacidade de base de dados do SQL Azure reservados](../sql-database/sql-database-reserved-capacity.md#cancellations-and-exchanges)

## <a name="change-optimize-setting-for-reserved-vm-instances"></a>Alteração de otimizar a definição para instâncias de VM reservadas

 Quando comprar uma instância de VM reservada, pode escolher flexibilidade de tamanho de instância ou prioridade de capacidade. Flexibilidade de tamanho de instância aplica o desconto de reserva às outras VMs no mesmo [grupo de tamanho VM](https://aka.ms/RIVMGroups). Prioridade de capacidade dá prioridade à capacidade do Centro de dados para as suas implementações. Essa opção oferece uma confiança adicional na sua habilidade de iniciar as instâncias VM quando precisar delas.

Por predefinição, quando o âmbito da reserva é partilhado, a flexibilidade de tamanho de instância é na. A capacidade de centro de dados não está priorizada para implementações de VM.

Para as reservas em que o âmbito é único, pode otimizar a reserva de prioridade de capacidade em vez de flexibilidade de tamanho de instância VM.

Para atualizar a definição de otimização para a reserva:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione a reserva.
4. Selecione **Definições** > **Configuração**.
5. Alteração da **otimizar** definição.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre as reservas do Azure, veja os artigos seguintes:

- [Quais são as reservas do Azure?](billing-save-compute-costs-reservations.md)
- [Efetue o pré-pagamento de máquinas virtuais com instâncias de VM reservadas do Azure](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Efetue o pré-pagamento do recursos de computação de base de dados SQL com capacidade de base de dados do SQL Azure reservados](../sql-database/sql-database-reserved-capacity.md)
- [Efetue o pré-pagamento do recursos do Azure Cosmos DB com capacidade de reservada do Azure Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md)
- [Efetue o pré-pagamento do planos de software SUSE das reservas do Azure](../virtual-machines/linux/prepay-suse-software-charges.md)
- [Compreender a forma como o desconto de reserva de VM é aplicado](billing-understand-vm-reservation-charges.md)
- [Compreender a forma como o desconto de plano de software SUSE Linux Enterprise é aplicado](../billing/billing-understand-suse-reservation-charges.md)
- [Compreender como é aplicado a outro desconto de reserva](billing-understand-reservation-charges.md)
- [Compreender a utilização de reserva para a sua subscrição pay as you go](billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de reserva para inscrição da sua empresa](billing-understand-reserved-instance-usage-ea.md)
- [Custos de software do Windows não incluídos com reservas](billing-reserved-instance-windows-software-costs.md)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
