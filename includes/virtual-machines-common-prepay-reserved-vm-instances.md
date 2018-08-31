---
author: yashesvi
ms.author: yashar
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 08-07-2018
ms.openlocfilehash: 6be00937a6645fbdd266ac0b1aa1ce95ecc6c3d9
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301872"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Efetue o pré-pagamento de máquinas virtuais com instâncias de VM reservadas do Azure

Efetue o pré-pagamento de máquinas virtuais e poupe dinheiro com as instâncias de Máquina Virtual reservada do Azure (VM). Para obter mais informações, consulte [oferta do Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Pode comprar uma instância de VM reservada [portal do Azure](https://portal.azure.com). Para comprar uma instância:

- Deve estar numa função de proprietário de, pelo menos, uma empresa ou uma subscrição pay as you go.
- Para subscrições Enterprise, compras de reserva tem de estar ativadas no [portal EA](https://ea.azure.com).
- Para o programa de fornecedor de soluções Cloud (CSP), apenas o admin de agentes ou agentes de vendas podem comprar as reservas.

## <a name="determine-the-right-vm-size-before-purchase"></a>Determinar o tamanho VM correto antes da compra

Os campos de subcategoria do medidor e produto nos dados de utilização não distinguir entre tamanhos de VM que utilizam o armazenamento premium de tamanhos de VM que não utilizam o armazenamento premium, com estes campos para determinar a VM tamanho para a compra de reserva pode levar a incorreto reserva de compra e não fornecer-lhe descontos de reserva. Utilize um dos métodos abaixo para determinar o tamanho VM certo para compra de reserva.

- Consulte o campo de AdditionalInfo no seu ficheiro de utilização ou a API de utilização para determinar o tamanho VM correto para uma compra de reserva. Não utilize os valores dos campos de subcategoria do medidor ou um produto, uma vez que estes campos não diferenciam entre versões de uma VM S e não-S.
- Também pode obter informações precisas de tamanho da VM com o Powershell, do Azure Resource Manager, ou a partir de VM de detalhes no portal do Azure.

Instâncias de VM reservadas estão disponíveis para a maioria dos tamanhos de VM com algumas exceções:

- As VMs clássicas e Cloud services não receber o desconto de reserva.
- VMs de núcleo suprimido não obtém descontos de reserva.
- Seguinte série de VM não receber os descontos de reserva: série A, série Av2 ou série G.
- As VMs em pré-visualização: qualquer série de VM ou o tamanho que está em pré-visualização não estão disponíveis para compra de reserva.
- Nuvens: Reservas não estão disponíveis para compra nas regiões do Azure US Government, Alemanha e China.
- Quota insuficiente: uma reserva de que tem um âmbito a uma subscrição individual tem de ter a quota de vCPU disponível na subscrição para a nova RI. Por exemplo, se a subscrição de destino tem um limite de quota de 10 vCPUs para a série D, em seguida, é possível comprar uma reserva para 11 Standard_D1 instâncias. A verificação de quota para as reservas inclui VMs já implementadas na subscrição. Por exemplo, se a subscrição tem uma quota de 10 vCPUs para a série D e tem duas instâncias de standard_D1 implementadas, em seguida, pode comprar uma reserva para 10 instâncias standard_D1 nesta subscrição. 
- Restrições de capacidade: limites em raras circunstâncias, a compra de reservas de novo para o subconjunto de tamanhos VM, devido à baixa capacidade numa região do Azure.

## <a name="buy-a-reserved-vm-instance"></a>Comprar uma instância VM reservadas

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione **adicionar** para comprar uma reserva nova.
4. Preencha os campos obrigatórios. Instâncias de VM em execução que correspondam aos atributos que selecionar se qualificar para obter o desconto de reserva. O número real de suas instâncias de VM que obter o desconto depende do escopo e quantidade selecionado.

    | Campo      | Descrição|
    |:------------|:--------------|
    |Nome        |O nome desta reserva.| 
    |Subscrição|A subscrição utilizada para pagar a reserva. O método de pagamento da subscrição é cobrado os custos iniciais para a reserva. O tipo de subscrição tem de ser um Contrato Enterprise (número da oferta: MS-AZR-0017P) ou o modelo Pay As You Go (número da oferta: MS-AZR-0003P). Para uma subscrição Enterprise, os custos são deduzidos do saldo de fidelização monetária da inscrição ou cobrados como utilização excedida. Para a subscrição Pay As You Go, os custos são debitados no cartão de crédito ou cobrados de acordo com o método de pagamento indicado na subscrição.|    
    |Âmbito       |Âmbito da reserva pode abranger uma subscrição ou várias subscrições (âmbito partilhado). Se selecionar: <ul><li>Subscrição individual - o desconto de reserva é aplicada a VMs nesta subscrição. </li><li>Partilhado - o desconto de reserva é aplicado a VMs em execução no caso de subscrições no seu contexto de faturação. Para os clientes empresariais, o escopo compartilhado é a inscrição e inclui todas as subscrições (exceto as subscrições de desenvolvimento/teste) na inscrição. Para clientes pay as you go, o âmbito partilhado é todas as subscrições pay as you go a criada pelo administrador de conta.</li></ul>|
    |Localização    |A região do Azure que é abrangida pela reserva.|    
    |Tamanho da VM     |O tamanho das instâncias de VM.|
    |Otimizar para     |Flexibilidade de tamanho de instância VM aplica o desconto de reserva às outras VMs no mesmo [grupo de tamanho VM](https://aka.ms/RIVMGroups). Prioridade de capacidade reserva-se a capacidade do datacenter para as suas implementações. Isso oferece a confiança adicional na sua habilidade de iniciar as instâncias VM quando precisar delas. Prioridade de capacidade apenas está disponível quando o âmbito da reserva é subscrição única. |
    |Termo        |Um ano ou três anos.|
    |Quantidade    |O número de instâncias que está a ser comprado dentro a reserva. A quantidade é o número de instâncias VM que podem obter o desconto de faturação em execução. Por exemplo, se estiver a executar 10 Standard_D2 VMs nos EUA leste, em seguida, tem de especificar quantidade como 10 para maximizar o benefício de todas as máquinas em execução. |
5. Pode ver o custo da reserva quando seleciona **calcular o custo**.

    ![Captura de ecrã antes de submeter a compra de reserva](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Selecione **Comprar**.
7. Selecione **ver esta reserva** para ver o estado da sua compra.

    ![Captura de tela depois de submeter a compra de reserva](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Passos Seguintes

O desconto de reserva é aplicado automaticamente ao número de máquinas virtuais que correspondem à âmbito de reserva e atributos em execução. Pode atualizar o âmbito da reserva através de [portal do Azure](https://portal.azure.com), PowerShell, CLI, ou por meio da API.

Para saber como gerir uma reserva, consulte [Gerir reservas de Azure](../articles/billing/billing-manage-reserved-vm-instance.md).

Para saber mais sobre as reservas do Azure, veja os artigos seguintes:

- [Quais são as reservas do Azure?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Gerir reservas no Azure](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Compreender como o desconto de reserva é aplicado](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de reserva para a sua subscrição pay as you go](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de reserva para inscrição da sua empresa](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Custos de software do Windows não incluídos com reservas](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Reservas do Azure no programa de fornecedor de soluções (CSP) do parceiro Center na nuvem](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
