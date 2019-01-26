---
author: yashesvi
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11/30/2018
ms.openlocfilehash: 1da2278eee6fcea5c013e9c2f5f4ad3e3013b590
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2019
ms.locfileid: "54906337"
---
# <a name="prepay-for-virtual-machines-with-azure-reserved-vm-instances"></a>Efetue o pré-pagamento de máquinas virtuais com instâncias de VM reservadas do Azure

Efetue o pré-pagamento de máquinas virtuais e poupe dinheiro com as instâncias de Máquina Virtual reservada do Azure (VM). Para obter mais informações, consulte [oferta do Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

Pode comprar uma instância de VM reservada [portal do Azure](https://portal.azure.com). Para comprar uma instância:

- Deve estar numa função de proprietário de, pelo menos, uma empresa ou uma subscrição pay as you go.
- Para subscrições Enterprise, compras de reserva tem de estar ativadas no [portal EA](https://ea.azure.com).
- Para o programa de fornecedor de soluções Cloud (CSP), apenas o admin de agentes ou agentes de vendas podem comprar reservas.

## <a name="determine-the-right-vm-size-before-you-buy"></a>Determinar o tamanho VM correto antes de comprar

Os campos de subcategoria do medidor e produto nos dados de utilização não distinguem entre tamanhos de VM que utilizam o armazenamento premium de VMs que não. Se utilizar estes campos para determinar o tamanho VM a utilizar para a reserva, pode comprar o tamanho errado e não obterá o desconto de reserva que espera. Utilize um dos seguintes métodos para determinar o tamanho VM correto ao comprar a reserva:

- Consulte o campo de AdditionalInfo no seu ficheiro de utilização ou a API de utilização para determinar o tamanho VM correto. Não utilize os valores dos campos de produto ou subcategoria do medidor. Estes campos não diferenciarem entre versões S e não-S de uma VM.
- Obter informações precisas de tamanho da VM com o Powershell, do Azure Resource Manager, ou a partir da VM detalhes no portal do Azure.

Instâncias de VM reservadas estão disponíveis para a maioria dos tamanhos de VM com algumas exceções:

- O desconto de reserva não se aplica para as VMs seguintes:
  - As VMs clássicas e serviços em nuvem
  - Série VM: Série A, série Av2 ou série G
  - VMs em pré-visualização: Qualquer série de VM ou o tamanho que está em pré-visualização
- Nuvens: As reservas não estão disponíveis para compra em regiões da Alemanha ou China.
- Quota insuficiente: Quota de vCPU disponível na subscrição para a nova RI tem de ter uma reserva de que tem um âmbito a uma subscrição individual. Por exemplo, se a subscrição de destino tem um limite de quota de 10 vCPUs para a série D, em seguida, é possível comprar uma reserva para 11 Standard_D1 instâncias. A verificação de quota para as reservas inclui VMs já implementadas na subscrição. Por exemplo, se a subscrição tem uma quota de 10 vCPUs para a série D e tem duas instâncias de standard_D1 implementadas, em seguida, pode comprar uma reserva para 10 instâncias standard_D1 nesta subscrição. 
- Restrições de capacidade: Em raras circunstâncias, a compra de reservas de novo para o subconjunto de tamanhos VM, devido à baixa capacidade numa região limites do Azure.

## <a name="buy-a-reserved-vm-instance"></a>Comprar uma instância VM reservadas

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços** > **reservas**.
3. Selecione **adicionar** para comprar uma reserva nova.
4. Preencha os campos obrigatórios. Instâncias de VM em execução que correspondam aos atributos que selecionar se qualificar para obter o desconto de reserva. O número real de suas instâncias de VM que obter o desconto depende do escopo e quantidade selecionado.

    | Campo      | Descrição|
    |:------------|:--------------|
    |Nome        |O nome desta reserva.| 
    |Subscrição|A subscrição utilizada para pagar a reserva. O método de pagamento da subscrição é cobrado os custos iniciais para a reserva. O tipo de subscrição tem de ser um contrato enterprise (número da oferta: MS-AZR-0017P) ou pay as you go (número da oferta: MS-AZR-0003P). Para uma subscrição Enterprise, os custos são deduzidos do saldo de fidelização monetária da inscrição ou cobrados como utilização excedida. Para a subscrição Pay As You Go, os custos são debitados no cartão de crédito ou cobrados de acordo com o método de pagamento indicado na subscrição.|    
    |Âmbito       |Âmbito da reserva pode abranger uma subscrição ou várias subscrições (âmbito partilhado). Se selecionar: <ul><li>Subscrição individual - o desconto de reserva é aplicada a VMs nesta subscrição. </li><li>Partilhado - o desconto de reserva é aplicado a VMs em execução no caso de subscrições no seu contexto de faturação. Para os clientes empresariais, o escopo compartilhado é a inscrição e inclui todas as subscrições (exceto as subscrições de desenvolvimento/teste) na inscrição. Para clientes pay as you go, o âmbito partilhado é todas as subscrições pay as you go a criada pelo administrador de conta.</li></ul>|
    |Região    |A região do Azure que é abrangida pela reserva.|    
    |Tamanho da VM     |O tamanho das instâncias de VM.|
    |Otimizar para     |Flexibilidade de tamanho de instância VM aplica o desconto de reserva às outras VMs no mesmo [grupo de tamanho VM](https://aka.ms/RIVMGroups). Prioridade de capacidade dá prioridade à capacidade do Centro de dados para as suas implementações. Isso oferece a confiança adicional na sua habilidade de iniciar as instâncias VM quando precisar delas. Prioridade de capacidade apenas está disponível quando o âmbito da reserva é subscrição única. |
    |Termo        |Um ano ou três anos.|
    |Quantidade    |O número de instâncias que está a ser comprado dentro a reserva. A quantidade é o número de instâncias VM que podem obter o desconto de faturação em execução. Por exemplo, se estiver a executar 10 Standard_D2 VMs nos EUA leste, em seguida, tem de especificar quantidade como 10 para maximizar o benefício de todas as máquinas em execução. |
5. Pode ver o custo da reserva quando seleciona **calcular o custo**.

    ![Captura de ecrã antes de submeter a compra de reserva](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvminstance-purchase.png)

6. Selecione **Comprar**.
7. Selecione **ver esta reserva** para ver o estado da sua compra.

    ![Captura de tela depois de submeter a compra de reserva](./media/virtual-machines-buy-compute-reservations/virtualmachines-reservedvmInstance-submit.png)

## <a name="cancellations-and-exchanges"></a>Cancelamentos e trocas

Se precisar de cancelar a sua reserva, pode ser cobrada uma taxa de cessação antecipada de 12%. Os reembolsos baseiam-se no preço mais baixo do preço de compra ou do preço atual da reserva. Os reembolsos estão limitados a 50 000 $ por ano. O reembolso que receber corresponde ao saldo rateado restante menos a taxa de cessação antecipada de 12%. Para pedir um cancelamento, vá para a reserva no portal do Azure e selecione **reembolsar** para criar um pedido de suporte.

Se precisar de alterar a sua reserva de Instâncias de VM Reservadas para outra região, grupo de tamanho de VM ou período, pode trocá-la por outra reserva de valor igual ou superior. A data de início do período da nova reserva não transita da reserva trocada. O período de 1 ou 3 anos começa a partir do momento em que cria a nova reserva. Para pedir uma troca, vá para a reserva no portal do Azure e selecione **Exchange** para criar um pedido de suporte.

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

## <a name="need-help-contact-us"></a>Precisa de ajuda? Contacte-nos.

Se tiver alguma dúvida ou precisar de ajuda, [criar um pedido de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
