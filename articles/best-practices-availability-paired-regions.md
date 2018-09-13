---
title: 'Negócios continuidade e recuperação após desastre (BCDR): regiões emparelhadas do Azure | Documentos da Microsoft'
description: Saiba mais sobre emparelhamento regional do Azure, para garantir que os aplicativos sejam resilientes durante falhas de datacenters.
author: rayne-wiselman
ms.service: multiple
ms.topic: article
ms.date: 07/03/2018
ms.author: raynew
ms.openlocfilehash: c500a1ecc817e4d95e39ca0ac985e1d90ffc8a78
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "44721268"
---
# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Negócios continuidade e recuperação após desastre (BCDR): regiões emparelhadas do Azure

## <a name="what-are-paired-regions"></a>Quais são as regiões emparelhadas?

O Azure opera em várias localizações geográficas em todo o mundo. Uma geografia do Azure é uma área definida de todo o mundo que contém pelo menos uma região do Azure. Uma região do Azure é uma área dentro de uma localização geográfica, que contém um ou mais datacenters.

Cada região do Azure é emparelhada com outra região na mesma geografia, em conjunto, tornando um par regional. A exceção é o sul do Brasil, que se encontra emparelhado com uma região fora da sua área geográfica. Entre os pares de região que Azure serializará plataforma atualiza (manutenção planeada), para que apenas uma região emparelhada será atualizada ao mesmo tempo. Além disso, em caso de interrupção que afetam várias regiões, pelo menos, uma região em cada par irá ser priorizada para recuperação.

![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Figura 1 – pares regionais do Azure

| Geografia | Regiões emparelhadas |  |
|:--- |:--- |:--- |
| Ásia |Ásia Oriental |Sudeste Asiático |
| Austrália |Leste da Austrália |Sudeste da Austrália |
| Austrália |Austrália Central |Austrália Central 2 |
| Brasil |Sul do Brasil 2 |EUA Centro-Sul |
| Canadá |Canadá Central |Leste do Canadá |
| China |China Norte |Leste da China|
| Europa |Europa do Norte |Europa Ocidental |
| Alemanha |Alemanha Central |Alemanha Nordeste |
| Índia |Índia Central |Sul da Índia |
| Índia |Índia Ocidental (1) |Sul da Índia |
| Japão |Leste do Japão |Oeste do Japão |
| Coreia |Coreia do Sul Central |Coreia do Sul |
| América do Norte |EUA Leste |EUA Oeste |
| América do Norte |EUA Leste 2 |EUA Central |
| América do Norte |EUA Centro-Norte |EUA Centro-Sul |
| América do Norte |EUA Oeste 2 |EUA Centro-Oeste 
| RU |Reino Unido Oeste |Reino Unido Sul |
| Departamento de Defesa dos E.U.A. |US DoD Leste |US DoD Centro |
| Governo dos Estados Unidos da América |Gov (US) - Arizona |Gov (US) - Texas |
| Governo dos Estados Unidos da América |US Gov Iowa (3) |Gov (US) - Virginia |
| Governo dos Estados Unidos da América |US Gov Virgínia (4) |Gov (US) - Texas |

Tabela 1 - mapeamento de pares regionais do Azure

- (1) Índia Ocidental é diferente porque ele é emparelhado com outra região em apenas uma direção. É de região secundária do oeste da Índia sul da Índia, mas a região secundária do Sul da Índia é Índia Central.
- (2) sul do Brasil é exclusivo, porque ele está emparelhado com uma região fora do seu próprio geografia. Região secundária do Sul do Brasil é Centro-Sul, mas da Sul E.u.a. região secundária não é sul do Brasil.
- (3) a região secundária do Iowa de Governo dos EUA é Virgínia gov (US), mas a região secundária do de Virginia gov (US) não é Iowa gov (US).
- (4) a região secundária de Virgínia do Governo dos EUA é US Gov Texas, mas a região secundária do US Gov Texas není Virgínia gov (US).


Recomendamos que configure a recuperação de desastres de continuidade de negócio (BCDR) através de pares regionais para beneficiar de políticas de isolamento e a disponibilidade do Azure. Para aplicações que suportam várias regiões do Active Directory, recomendamos que utilize ambas as regiões num par de região sempre que possível. Isto irá garantir a disponibilidade ideal para aplicações e de tempo de recuperação minimizado em caso de desastre. 

## <a name="an-example-of-paired-regions"></a>Um exemplo de regiões emparelhadas
Figura 2 abaixo mostra um aplicativo hipotético que usa o par regional para recuperação após desastre. Os números verde realçam as atividades entre regiões de três serviços do Azure (Azure computação, armazenamento e base de dados) e como estão configurados para replicar em várias regiões. As vantagens exclusivas da implementação em várias regiões emparelhadas estão realçadas através dos números de cor de laranja.

![Descrição geral dos benefícios de região emparelhada](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Figura 2 – hipotético par regional do Azure

## <a name="cross-region-activities"></a>Atividades em várias regiões
Como referido na figura 2.

![PaaS](./media/best-practices-availability-paired-regions/1Green.png) **(PaaS) de computação do Azure** – terá de aprovisionar recursos de computação adicionais com antecedência para garantir que recursos estão disponíveis noutra região durante um desastre. Para obter mais informações, consulte [orientações técnicas sobre resiliência do Azure](resiliency/resiliency-technical-guidance.md).

![Armazenamento](./media/best-practices-availability-paired-regions/2Green.png) **armazenamento do Azure** -armazenamento Georredundante (GRS) está configurado por predefinição, quando é criada uma conta de armazenamento do Azure. Com a GRS, os dados automaticamente são replicados três vezes numa região principal e três vezes na região associada. Para obter mais informações, consulte [opções de redundância de armazenamento do Azure](storage/common/storage-redundancy.md).

![SQL do Azure](./media/best-practices-availability-paired-regions/3Green.png) **base de dados do Azure SQL** – com o Azure SQL da base de dados-Georreplicação, pode configurar a replicação assíncrona de transações em qualquer região do mundo; no entanto, é recomendável implementar estes recursos num região emparelhada na maioria dos cenários de recuperação após desastre. Para obter mais informações, consulte [Georreplicação no Azure SQL Database](sql-database/sql-database-geo-replication-overview.md).

![Gestor de recursos](./media/best-practices-availability-paired-regions/4Green.png) **do Azure Resource Manager** -Resource Manager inerentemente fornece isolamento lógico de componentes do serviço de gestão em várias regiões. Isso significa que as falhas de lógicas numa região são menor probabilidade de afetar o outro.

## <a name="benefits-of-paired-regions"></a>Benefícios das regiões emparelhadas
Como referido na figura 2.  

![Isolamento](./media/best-practices-availability-paired-regions/5Orange.png)
**isolamento físico** -se possível, Azure prefere pelo menos 300 quilómetros de separação entre centros de dados num par regional, embora não seja possível ou prático em todas as localizações geográficas. Separação de Data Center físico reduz a probabilidade de desastres naturais, conflitos civis, falhas de energia ou falhas de rede física que afetem as duas regiões ao mesmo tempo. Isolamento está sujeita as restrições na geografia (tamanho de geografia, disponibilidade da infraestrutura de rede/power, regulamentos, etc.).  

![A replicação](./media/best-practices-availability-paired-regions/6Orange.png)
**replicação fornecidos pelo plataforma** -alguns serviços, como o armazenamento Georredundante fornecem replicação automática para a região emparelhada.

![Recuperação](./media/best-practices-availability-paired-regions/7Orange.png)
**ordem de recuperação de região** – no caso de uma falha abrangente, a recuperação de uma região tem prioridade em cada par. Aplicações que são implementadas em regiões emparelhadas garantia para uma das regiões recuperados com a prioridade. Se uma aplicação é implementada em regiões que não estão emparelhados, pode estar atrasada recuperação – no pior caso que as regiões escolhidas podem ser os dois últimos a ser recuperado.

![As atualizações](./media/best-practices-availability-paired-regions/8Orange.png)
**atualizações sequenciais** – atualizações do sistema Azure planeadas são implementadas em regiões emparelhadas sequencialmente (e não ao mesmo tempo) para minimizar o período de indisponibilidade, o efeito de erros e falhas de lógicas na rara eventualidade de uma má Atualize.

![Dados](./media/best-practices-availability-paired-regions/9Orange.png)
**residência dos dados** – uma região reside dentro da mesma geografia do respetivo par (à exceção do Sul do Brasil), para atender aos requisitos de residência de dados para efeitos de jurisdição de imposição de imposto e lei.
