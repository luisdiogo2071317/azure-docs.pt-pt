---
ms.topic: include
ms.openlocfilehash: c30dbe25252a18e1edaed5ec81d86cc1dd976250
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39514051"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Efetue o pré-pagamento de máquinas virtuais com instâncias de VM reservadas do Azure

Efetue o pré-pagamento de máquinas virtuais e poupe dinheiro com as instâncias de Máquina Virtual reservada do Azure (VM). Para obter mais informações, consulte [oferta do Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Pode comprar instâncias reservadas do Azure [portal do Azure](https://portal.azure.com). Para comprar uma instância reservada:
-   Deve estar numa função de proprietário de, pelo menos, uma empresa ou uma subscrição pay as you go.
-   Para subscrições Enterprise, compras de instância reservada tem de estar ativadas no [portal EA](https://ea.azure.com).
-   Para o programa de fornecedor de soluções Cloud (CSP), apenas o admin de agentes ou agentes de vendas podem comprar as instâncias reservadas.

## <a name="determine-the-right-vm-size-before-purchase"></a>Determinar o tamanho VM correto antes da compra
Os campos de subcategoria do medidor e produto nos dados de utilização não distinguir entre tamanhos de VM que utilizam o armazenamento premium de tamanhos de VM que não utilizam o armazenamento premium, com estes campos para determinar a VM tamanho para a compra de reserva pode levar a incorreto reserva de compra e não fornecer-lhe descontos de reserva. Utilize um dos métodos abaixo para determinar o tamanho VM certo para compra de reserva.
- Consulte o AdditionalInfo > ServiceType campo no seu ficheiro de utilização ou a dados da API de utilização. Este campo irá fornecer o tamanho VM correto quando implementar as VMs que podem utilizar o armazenamento premium.
- Também pode obter informações precisas de tamanho da VM com o Powershell, do Azure Resource Manager ou a partir de VM de detalhes no portal do Azure.

## <a name="buy-a-reserved-virtual-machine-instance"></a>Comprar uma instância de Máquina Virtual reservada
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione **adicionar** para comprar uma nova instância reservada.
4. Preencha os campos obrigatórios. Instâncias de VM em execução que correspondam aos atributos que selecionar se qualificar para obter o desconto de instância reservada. O número real de suas instâncias de VM que obter o desconto depende do escopo e quantidade selecionado.

    | Campo      | Descrição|
    |:------------|:--------------|
    |Nome        |O nome desta instância reservada.| 
    |Subscrição|A subscrição utilizada para pagar pela instância reservada. Os custos iniciais da Instância Reservada são cobrados de acordo com o método de pagamento indicado na subscrição. O tipo de subscrição tem de ser um Contrato Enterprise (número da oferta: MS-AZR-0017P) ou o modelo Pay As You Go (número da oferta: MS-AZR-0003P). Para uma subscrição Enterprise, os custos são deduzidos do saldo de fidelização monetária da inscrição ou cobrados como utilização excedida. Para a subscrição Pay As You Go, os custos são debitados no cartão de crédito ou cobrados de acordo com o método de pagamento indicado na subscrição.|    
    |Âmbito       |Âmbito da instância reservada pode abranger uma subscrição ou várias subscrições (âmbito partilhado). Se selecionar: <ul><li>Subscrição individual - desconto a instância reservada é aplicada às VMs nesta subscrição. </li><li>O desconto de instância reservada partilhado - é aplicado às VMs em execução no caso de subscrições no seu contexto de faturação. Para os clientes empresariais, o escopo compartilhado é a inscrição e inclui todas as subscrições (exceto as subscrições de desenvolvimento/teste) na inscrição. Para clientes pay as you go, o âmbito partilhado é todas as subscrições pay as you go a criada pelo administrador de conta.</li></ul>|
    |Localização    |A região do Azure que é abrangida pela instância reservada.|    
    |Tamanho da VM     |O tamanho das instâncias de VM.|
    |Otimizar para     |Flexibilidade de tamanho de instância VM aplica o desconto de reserva às outras VMs no mesmo [grupo de tamanho VM](https://aka.ms/RIVMGroups). Prioridade de capacidade reserva-se a capacidade do datacenter para as suas implementações. Isso oferece a confiança adicional na sua habilidade de iniciar as instâncias VM quando precisar delas. Prioridade de capacidade apenas está disponível quando o âmbito da reserva é subscrição única. |
    |Termo        |Um ano ou três anos.|
    |Quantidade    |O número de instâncias que está a ser comprado dentro da instância reservada. A quantidade é o número de instâncias VM que podem obter o desconto de faturação em execução. Por exemplo, se estiver a executar 10 Standard_D2 VMs nos EUA leste, em seguida, tem de especificar quantidade como 10 para maximizar o benefício de todas as máquinas em execução. |
5. Pode ver o custo da instância reservada Quando seleciona **calcular o custo**.

    ![Captura de ecrã antes de submeter a compra de instância reservada](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Selecione **Comprar**.
7. Selecione **ver esta reserva** para ver o estado da sua compra.

    ![Captura de tela depois de submeter a compra de instância reservada](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Passos Seguintes 
O desconto de instância reservada é aplicado automaticamente ao número de máquinas virtuais que correspondem à âmbito da instância reservada e atributos em execução. Pode atualizar o âmbito da instância reservada através de [portal do Azure](https://portal.azure.com), PowerShell, CLI ou através da API. 

Para saber como gerir uma instância reservada, veja [gerir instâncias reservadas no Azure](../articles/billing/billing-manage-reserved-vm-instance.md).

Para saber mais sobre as instâncias reservadas do Azure, veja os artigos seguintes:

- [O que são instâncias de VM reservadas do Azure?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Gerir as instâncias reservadas no Azure](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Compreender a aplicação de descontos sobre as instâncias reservadas](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instâncias reservadas na subscrição Pay As You Go](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de instâncias reservadas na inscrição Enterprise](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Custos de software Windows não incluídos nas instâncias reservadas](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Instâncias reservadas no programa Fornecedor de Soluções Cloud (CSP) do Centro de Parceiros](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para a sua questão resolvidos rapidamente.
