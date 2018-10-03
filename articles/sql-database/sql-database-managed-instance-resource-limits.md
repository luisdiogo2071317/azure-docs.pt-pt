---
title: Limites de recursos de base de dados SQL do Azure - instância gerida | Documentos da Microsoft
description: Este artigo fornece uma visão geral dos limites de recursos do SQL Database do Azure para instâncias geridas.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop
manager: craigg
ms.date: 10/02/2018
ms.openlocfilehash: f2b1a8e18917c1c045c715bd3424d0bbfc0cdc67
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045405"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Limites de recursos de instância gerida da base de dados SQL do Azure de descrição geral

Este artigo fornece uma visão geral dos limites de recursos de instância gerida da base de dados SQL do Azure e fornece informações como criar o pedido para aumentar os limites de subscrição predefinidos. 

> [!NOTE]
> Para outras limitações de instância gerida, não é específicas para uma subscrição individual, veja [modelo de compra baseado em vCore](sql-database-managed-instance.md#vcore-based-purchasing-model) e [escalões de serviço da instância gerida](sql-database-managed-instance.md#managed-instance-service-tiers).

## <a name="default-subscription-level-limits-per-region"></a>Limites de nível de assinatura de predefinidos por região

Atualmente, a instância gerida suporta implementação apenas nos seguintes tipos de subscrições:

- [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Fornecedor de serviços cloud (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)

> [!NOTE]
> Esta limitação sobre o suporte para apenas alguns tipos de subscrição é temporária.

Instâncias geridas tem dois predefinido limites de nível de subscrição por região do Azure. Tipos de subscrição diferente têm limites regionais diferentes. Estes limites podem ser aumentados ao criar pedido de suporte especial no portal do Azure para a subscrição com o tipo de problema **Quota**:

- **Limite de sub-redes**: O número máximo de sub-redes em que são implementadas instâncias geridas
- **Limite de número de instância**: O número máximo de instâncias por região

> [!IMPORTANT]
> Quando planear as suas implementações, considere que uma instância de negócio críticos (BC) (devido a redundância adicionada) geralmente consome 4 x de uma instância de finalidade geral (GP) mais capacidade. Assim, para seus cálculos, 1 instância GP = 1 instância de unidade e a instância de 1 BC = 4 unidades de instância. Para simplificar a análise de consumo contra os limites predefinidos, resumir as unidades de instância em todas as sub-redes na região em que estão implementadas instâncias geridas e comparar os resultados com os limites de unidade de instância para o seu tipo de subscrição.

## <a name="default-limits-by-subscription-type"></a>Padrão de limites por tipo de subscrição

|Tipo de subscrição| Número máx. de sub-redes de instância gerida | Número máximo de instâncias |Número máx. de GP geridos instâncias *|Número máx. de BC geridos instâncias *|
| :---| :--- | :--- |:--- |:--- |
|"Pay-as-you-go"|1 *|4 *|4 *|1 *|
|CSP |1 *|4 *|4 *|1 *|
|EA|3 * *|12 * *|12 * *|3 * *|

\* Pode optar por implementar 1 BC ou 4 instâncias GP numa sub-rede, para que o número total de "unidades de instância" numa região nunca exceda 4.

* * Máximo número de instâncias numa camada de serviço aplica-se se não houver nenhuma instância da outra camada de serviço. No caso de pretender ao combinar as instâncias de GP e BC na mesma sub-rede, utilize a secção seguinte como uma referência para as combinações permitidas. Como regra simple, o número total de sub-redes não pode exceder os 3 e o número total de unidades de instância não pode exceder os 12.

## <a name="deployment-options-for-gp-and-bc-deployments-within-the-same-subnet"></a>Opções de implementação para implementações de GP e BC dentro da mesma sub-rede

Os exemplos seguintes abrangem casos de implantação com sub-redes de não vazios e misto GP e BC escalões de serviço.

|Número de sub-redes|sub-rede 1|Sub-rede 2|Sub-rede 3|
|:---|:---|:---|:---|
|1|0 BC e até 12 GP<br>1 BC e até 8 GP<br>2 BC e até 4 GP<br>3 BC|N/A| N/A|
|2|BC 0, até 4 GP|BC 0, até 8 GP<br>C. 1, até 4 GP<br>2 BC|N/A|
|2|1 BC|BC 0, até 8 GP<br>C. 1, até 4 GP<br>2 BC|N/A|
|2|2 BC|BC 0, até 8 GP<br>C. 1, até 4 GP<br>2 BC|N/A|
|3|C. 1, 0 GP|C. 1, 0 GP|BC 0, até 4 GP|
|3|1BC, 0 GP|BC 0, até 4 GP|C. 1, 0 GP|
|3|BC 0, até 4 GP|C. 1, 0 GP|1BC, 0 GP|

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>Obtendo uma quota maior para a instância gerida SQL

Para iniciar o processo de obtenção de uma quota maior:

1. Open **ajuda + suporte**e clique em **novo pedido de suporte**. 

   ![Ajuda e Suporte](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Na guia Noções básicas para o novo pedido de suporte:
   - Para **tipo de problema**, selecione **limites de serviço e subscrição (cotas)**.
   - Em **Subscrição**, selecione a sua subscrição.
   - Para **tipo de Quota**, selecione **SQL Database Managed Instance**.
   - Para **plano de suporte**, selecione o seu plano de suporte.

     ![Quota de tipo de problema](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. Clique em **Seguinte**.
4. No separador de problema para o novo pedido de suporte:
   - Para **gravidade**, selecione o nível de gravidade do problema.
   - Para **detalhes**, fornecem informações adicionais sobre o problema, incluindo mensagens de erro.
   - Para **carregamento de ficheiros**, anexar um ficheiro com mais informações (até 4 MB).

     ![Detalhes do problema](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Uma solicitação válida deve incluir:
     > - Região na qual assinatura limite tem de ser aumentado
     > - Aumentar o número necessário de instâncias, por camada de serviços em sub-redes existentes após a quota (se nenhuma das sub-redes existentes tem de ser expandido
     > - Necessário número de novas sub-redes e o número total de instâncias por escalão de serviço dentro as novas sub-redes (se precisar de implementar instâncias geridas nas novas sub-redes).
5. Clique em **Seguinte**.
6. No separador de informações de contacto para o novo pedido de suporte, introduza o método preferencial de contacto (e-mail ou telefone) e os detalhes de contactos.
7. Clique em **Criar**.

## <a name="next-steps"></a>Passos Seguintes

- Para obter mais informações sobre a instância gerida, veja [o que é uma instância gerida?](sql-database-managed-instance.md). 
- Para obter informações sobre preços, consulte [preços da SQL Database Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- Para saber como criar a sua primeira instância gerida, veja [guia de início rápido](sql-database-managed-instance-get-started.md).