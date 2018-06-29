---
ms.topic: include
ms.openlocfilehash: 509d2bc3f58c57a3e2e15eed2ea2c70ed17856f3
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37063702"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Prepay para máquinas virtuais com instâncias VM reservados do Azure

Prepay para máquinas virtuais e a poupar dinheiro com instâncias de Máquina Virtual reservada do Azure (VM). Para obter mais informações, consulte [oferta de instâncias de VM do Azure reservado](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Pode comprar o Azure instâncias reservadas [portal do Azure](https://portal.azure.com). Para comprar uma instância reservado:
-   Tem de ser uma função de proprietário para, pelo menos, uma empresa ou de subscrição pay as you go.
-   Para as subscrições empresariais, instância reservado compras tem de estar ativadas no [EA portal](https://ea.azure.com).
-   Para o programa fornecedor de solução em nuvem (CSP), só o admin de agentes ou agentes de vendas podem comprar as instâncias reservadas.

[!IMPORTANT]
Tem de utilizar um dos métodos descritos abaixo para identificar o tamanho da VM corretamente para uma compra de reserva.

## <a name="determine-the-right-vm-size-before-purchase"></a>Determinar o tamanho VM adequado antes de compra
1. Consulte o campo de AdditionalInfo no seu ficheiro de utilização ou a API de utilização para determinar o tamanho VM correto para uma compra de reserva. Não utilize os valores de campos de produto ou subcategoria de medição, uma vez que estes campos não diferenciam entre versões S e não-S de uma VM.
2. Também pode obter informações de tamanho VM exatas relacionadas com o Powershell, do Azure Resource Manager ou a partir de uma VM em detalhe no portal do Azure.

## <a name="buy-a-reserved-virtual-machine-instance"></a>Comprar uma instância de Máquina Virtual reservada
1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione **adicionar** para comprar uma nova instância reservado.
4. Preencha os campos obrigatórios. Instâncias de VM em execução que corresponda os atributos que selecionar elegíveis para obter o desconto instância reservado. O número real das instâncias de VM que obtenha o desconto depende no âmbito e na quantidade selecionado.

    | Campo      | Descrição|
    |:------------|:--------------|
    |Nome        |O nome desta instância reservado.| 
    |Subscrição|A subscrição utilizada pagar para a instância reservado. O método de pagamento de subscrição é-lhe cobrado os custos de compromisso para a instância reservado. O tipo de subscrição tem de ser um enterprise agreement (oferecem número: MS-AZR - 0017P) ou pay as you go (oferecem número: MS-AZR - 0003P). Para uma subscrição do enterprise, os encargos são deducted do saldo de compromisso monetário a inscrição ou cobrados como excedido. Para a subscrição pay as you go, os encargos são cobrados para o método de pagamento de cartão de crédito ou fatura na subscrição.|    
    |Âmbito       |Âmbito da instância reservado pode abranger uma subscrição ou várias subscrições (âmbito partilhada). Se selecionar: <ul><li>Única subscrição - desconto a instância reservado é aplicada às VMs nesta subscrição. </li><li>O desconto reservado instância partilhada - é aplicado a VMs em execução em quaisquer subscrições dentro do contexto de faturação. Para os clientes empresariais, o âmbito partilhado é a inscrição e inclui todas as subscrições (exceto as subscrições de programador/teste) dentro de inscrição. Para clientes de pay as you go, o âmbito partilhado é todas as subscrições de pay as you go criadas pelo administrador de conta.</li></ul>|
    |Localização    |A região do Azure que está abrangida pela instância reservado.|    
    |Tamanho da VM     |O tamanho das instâncias da VM.|
    |Termo        |Um ano ou três anos.|
    |Quantidade    |O número de instâncias que está a ser adquirido dentro de instância reservado. A quantidade é o número de instâncias VM que podem obter o desconto faturação em execução. Por exemplo, se estiver a executar o 10 Standard_D2 VMs nos E.U.A. leste, em seguida, tem de especificar quantidade como 10 para maximizar o benefício de todas as máquinas em execução. |
5. Pode ver o custo da instância reservado ao selecionar **calcular o custo**.

    ![Captura de ecrã antes de submeter a compra de instância reservado](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Selecione **Compra**.
7. Selecione **ver esta reserva** para ver o estado da compra.

    ![Captura de ecrã depois de submeter a compra de instância reservado](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="next-steps"></a>Passos Seguintes 
O desconto instância reservado é aplicado automaticamente para o número de máquinas virtuais que corresponder ao âmbito de instância reservado e atributos em execução. Pode atualizar o âmbito da instância reservado através de [portal do Azure](https://portal.azure.com), PowerShell, CLI ou através da API. 

Para saber como gerir uma instância reservada, consulte [gerir instâncias reservadas no Azure](../articles/billing/billing-manage-reserved-vm-instance.md).

Para saber mais sobre instâncias reservadas do Azure, consulte os artigos seguintes:

- [Quais são reservados instâncias de VM do Azure?](../articles/billing/billing-save-compute-costs-reservations.md)
- [Gerir instâncias reservadas no Azure](../articles/billing/billing-manage-reserved-vm-instance.md)
- [Compreender a forma como é aplicado o desconto instância reservado](../articles/billing/billing-understand-vm-reservation-charges.md)
- [Compreender a utilização de instância reservado para a sua subscrição pay as you go](../articles/billing/billing-understand-reserved-instance-usage.md)
- [Compreender a utilização de instância reservado para a inscrição Enterprise](../articles/billing/billing-understand-reserved-instance-usage-ea.md)
- [Custos de software do Windows não incluídos com instâncias reservadas](../articles/billing/billing-reserved-instance-windows-software-costs.md)
- [Instâncias reservadas no programa de parceiro Center solução fornecedor Cloud (CSP)](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contactar o suporte

Se ainda tiver mais perguntas, [contacte o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para obter o seu problema resolvido rapidamente.