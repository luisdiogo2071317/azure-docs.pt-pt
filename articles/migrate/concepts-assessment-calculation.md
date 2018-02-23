---
title: "Os cálculos de avaliação no Azure migrar | Microsoft Docs"
description: "Fornece uma descrição geral de cálculos de avaliação no serviço Azure migrar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/02/2017
ms.author: raynew
ms.openlocfilehash: db09ff30ff9f3852e84162b8400572e76515230f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="assessment-calculations"></a>Cálculos de avaliação

[Migrar do Azure](migrate-overview.md) avalia cargas de trabalho no local para a migração para o Azure. Este artigo fornece informações sobre como avaliações são calculadas.


## <a name="overview"></a>Descrição geral

Uma avaliação de Azure migrar tem três fases. Avaliação começa com uma análise adequabilidade, seguida de dimensionamento, e por último, mensais custo estimativa. Uma máquina apenas move ao longo para posterior se tenha sido enviado anterior. Por exemplo, se um computador falhar a verificação de adequabilidade do Azure, está marcada como unsuitable para não ser efetuada do Azure e dimensionamento e custos. 


## <a name="azure-suitability-analysis"></a>Análise de adequabilidade do Azure

Nem todas as máquinas são adequadas para executar na nuvem como nuvem tem as suas próprias limitações e requisitos. Migrar do Azure avalia cada máquina no local, de adequação a migração para o Azure e categoriza as máquinas em um dos seguintes categorias:
- **Pronto para o Azure** -pode ser migrada a máquina como-é para o Azure sem quaisquer alterações. Arrancará no Azure com o suporte do Azure completo.
- **Condicionalmente está preparado para o Azure** -a máquina pode efetuar o arranque no Azure, mas não pode ter suporte do Azure completo. Por exemplo, uma máquina com uma versão antiga do SO do Windows Server não é suportada no Azure. Tem de ser cuidado antes de migrar estas máquinas para o Azure e siga as orientações de remediação sugerida na avaliação para corrigir os problemas de preparação antes de migrar.
- **Não pronto para o Azure** -a máquina não irão arrancar no Azure. Por exemplo, se uma máquina no local tem um disco de tamanho existe mais de 4 TB anexados, não pode ser alojado no Azure. Tem de seguir as orientações de remediação sugerida na avaliação para corrigir o problema de disponibilidade antes de migrar para o Azure. Dimensionar corretamente e de custo de estimativa não é feita nas máquinas que estão marcadas como não preparada para o Azure.
- **Preparação desconhecida** -migrar do Azure não foi possível localizar a preparação da máquina devido a dados insuficientes disponíveis no vCenter Server. 

Migrar do Azure analisa o sistema operativo para identificar a preparação do Azure da máquina no local e as propriedades da máquina.

### <a name="machine-properties"></a>Propriedades da máquina
Migrar do Azure analisa as seguintes propriedades da VM no local para identificar se uma VM pode ser executado no Azure.
 
**Propriedade** | **Detalhes** | **Estado de preparação do Azure**
--- | --- | ---
**Tipo de arranque** | Azure suporta VMs com o tipo de arranque BIOS e UEFI não. | Condicionalmente pronto para o Azure, se o tipo de arranque for UEFI. 
**Núcleos** | O número de núcleos nas máquinas tem de ser igual ou inferior ao número máximo de núcleos (32) suportado para uma VM do Azure.<br/><br/> Se está disponível o histórico de desempenho, Azure migrar considera os núcleos sobreutilizados para comparação. Se não for especificado um fator de comfort nas definições de avaliação, o número de núcleos sobreutilizados é multiplicado pelo fator de comfort.<br/><br/> Se não houver nenhum histórico de desempenho, migrar do Azure utiliza os núcleos alocados, sem aplicar o fator de comfort. | Não pronto se o número de núcleos é superior a 32. 
**Memória** | O tamanho de memória da máquina tem de ser igual ou menor do que o máximo de memória (448 GB) permitido para uma VM do Azure. <br/><br/> Se está disponível o histórico de desempenho, Azure migrar considera a memória sobreutilizada para comparação. Se não for especificado um fator de comfort, a memória sobreutilizada é multiplicada pelo fator de comfort.<br/><br/> Se não houver nenhum histórico é utilizada a memória alocada, sem aplicar o fator de comfort.<br/><br/> | Não pronto se o tamanho de memória é superior a 448 GB.
**Disco de armazenamento** | Tamanho alocado de um disco tem de ser 4 TB (4096 GB) ou menos.<br/><br/> O número de discos anexados à máquina tem de ser 65 ou inferior, incluindo o disco do SO. | Não pronto se qualquer disco tem um tamanho maior de 4 TB ou se existirem mais de 65 discos ligados à máquina. 
**Redes** | Uma máquina tem de ter 32 ou menos NICs anexados ao mesmo. | Não pronto se a máquina tem mais do que 32 NICs

### <a name="guest-operating-system"></a>Sistema operativo convidado 
Juntamente com propriedades VM, migrar do Azure também analisa o SO convidado da VM no local para identificar se a VM pode ser executado no Azure.

> [!NOTE]
> Migrar do Azure considera o SO especificado no vCenter Server para efetuar a análise seguinte. Uma vez que a deteção efetuada pelo Azure migrar com base no dispositivo, não tem uma forma de verificar se o SO em execução dentro da VM é igual ao especificado num vCenter Server.

A seguinte lógica é utilizada pelo Azure migrar para identificar a preparação do Azure da VM com base no sistema operativo.

**Sistema operativo** | **Detalhes** | **Estado de preparação do Azure**
--- | --- | ---
Windows Server 2016 & todos os SPs | O Azure oferece suporte completo. | Preparado para o Azure
Windows Server 2012 R2 e SPs todos os | O Azure oferece suporte completo. | Preparado para o Azure
Windows Server 2012 e SPs todos os | O Azure oferece suporte completo. | Preparado para o Azure
Windows Server 2008 R2 com todos os SPs | O Azure oferece suporte completo.| Preparado para o Azure
Windows Server 2003-2008 R2 | Estes sistemas operativos passaram respetivo fim do suporte de data e a necessidade de um [contrato suporta personalizada (CSA)](https://aka.ms/WSosstatement) para suporte no Azure. | Condicionalmente está preparado para o Azure, considere atualizar o sistema operativo antes de migrar para o Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Estes sistemas operativos passaram respetivo fim de suporte de data, o computador pode efetuar o arranque no Azure, mas sem suporte de SO é fornecido pelo Azure. | Condicionalmente está pronto para o Azure, é recomendado para atualizar o sistema operativo antes de migrar para o Azure.
Cliente Windows 7, 8 e 10 | O Azure oferece suporte com apenas a subscrição do Visual Studio. | Condicionalmente está preparado para o Azure
Windows Vista, XP Professional | Estes sistemas operativos passaram respetivo fim de suporte de data, o computador pode efetuar o arranque no Azure, mas sem suporte de SO é fornecido pelo Azure. | Condicionalmente está pronto para o Azure, é recomendado para atualizar o sistema operativo antes de migrar para o Azure.
Linux | Azure patrocina estes [sistemas operativos Linux](../virtual-machines/linux/endorsed-distros.md). Outros sistemas de operativos Linux podem efetuar o arranque no Azure, mas é recomendado para atualizar o sistema operativo para uma versão endorsed antes de migrar para o Azure. | Pronto para o Azure, se a versão é aprovada.<br/><br/>Pronto condicionalmente se a versão não é aprovada.
Outros sistemas operativos<br/><br/> Por exemplo, Oracle Solaris, etc. de SO do Apple Mac, FreeBSD, etc. | Azure não apoia estes sistemas operativos. O computador pode efetuar o arranque no Azure, mas sem suporte de SO é fornecido pelo Azure. | Condicionalmente está pronto para o Azure, é recomendado para instalar um SO suportado antes de migrar para o Azure.  
SO especificado como *outros* no vCenter Server | Migrar do Azure não consegue identificar o SO neste caso. | Preparação desconhecida. Certifique-se de que o SO em execução dentro da VM é suportado no Azure. 
sistemas operativos de 32 bits | O computador pode efetuar o arranque no Azure, mas o Azure poderá não oferecem suporte completo. | Condicionalmente está preparado para o Azure, considere atualizar sistema operativo da máquina do SO de 32 bits para 64 bits antes de migrar para o Azure.

## <a name="sizing"></a>Dimensionamento

Depois de uma máquina está marcada como pronta para o Azure, Azure migrar tamanhos da VM e os respetivos discos do Azure. Se o critério de dimensionamento especificado nas propriedades de avaliação é fazer com base no desempenho dimensionamento, o Azure migrar considera o histórico de desempenho da máquina para identificar um tamanho VM no Azure. Este método é útil em cenários em que alocou excessiva a VM no local, mas a utilização for baixa e pretende dimensionar as VMs do Azure para guardar o custo.

> [!NOTE]
> Migrar do Azure recolhe histórico do desempenho de VMs no local a partir do servidor vCenter. Para garantir a dimensionar exata, certifique-se de que a definição de estatísticas no vCenter Server está definida para o nível 3 e aguarde, pelo menos, um dia antes kicking desativar a deteção de VMs no local. Se a definição de estatísticas no vCenter Server for inferior ao nível 3, os dados de desempenho de disco e rede não são recolhidos. 

Se não pretender considerar o histórico de desempenho e pretende colocar a VM como-é para o Azure, pode especificar o critério de dimensionamento como *como no local* e migrar do Azure, em seguida, irá tamanho as VMs com base na configuração no local sem Tendo em conta os dados de utilização.

### <a name="performance-based-sizing"></a>Dimensionamento com base no desempenho

Para dimensionamento com base no desempenho, começa a migrar do Azure com os discos ligados à VM, seguido de adaptadores de rede e, em seguida, mapas de uma VM do Azure com base nos requisitos da computação da VM no local. 
 
- **Discos**: Migrar Azure tenta mapear todos os discos ligados à máquina a um disco no Azure. 
    
    > [!NOTE]
    > Suporta a migração do Azure geridos apenas discos para avaliação.
    
    - Para obter a e/s de disco Efetivo por segundo (IOPS) e débito (MBps), a Azure migrar multiplica o disco IOPS e o débito com o fator de comfort. Com base no IOPS Efetivo e valores de débito, Azure migrar identifica se o disco deve ser mapeado para um disco standard ou premium no Azure.
    - Se migrar do Azure não é possível localizar um disco com o IOPS & débito necessário, marca a máquina como unsuitable para o Azure. [Saiba mais](../azure-subscription-service-limits.md#storage-limits) sobre o Azure limita por disco e a VM.
    - Se encontrar um conjunto de discos adequados, Azure migrar seleciona aqueles que suportam o método de redundância do armazenamento e a localização especificada nas definições de avaliação.
    - Se existirem vários discos elegíveis, seleciona um com o menor custo.
    - Se a discos de dados de desempenho no disponível, todos os discos estão mapeados para os discos padrão no Azure.

- **Adaptadores de rede**: Migrar Azure tenta localizar uma VM do Azure que consiga suportar o número de adaptadores de rede ligados à máquina no local e o desempenho de que estes adaptadores de rede.
    - Para obter o desempenho de rede eficiente da VM no local, Azure migrar agrega os dados transmitidos por segundo (MBps) fora da máquina (rede out), através de todos os adaptadores de rede e aplica-se o fator de comfort. Este número é utilizado para localizar uma VM do Azure que consiga suportar o desempenho de rede necessários.
    - Juntamente com o desempenho da rede, também considera se a VM do Azure pode suportar necessários o número de adaptadores de rede.
    - Se não existem dados de desempenho de rede estiver disponíveis, apenas a contagem de adaptadores de rede é considerada para dimensionamento de VM.

- **Tamanho da VM**: depois dos requisitos de armazenamento e rede são calculados, Azure migrar considera os requisitos de CPU e memória para localizar um tamanho adequado do VM no Azure.
    - Migrar do Azure analisa os núcleos sobreutilizados e a memória e aplica-se o fator de comfort para obter os núcleos efetivos e a memória. Com base nesse número, tenta localizar um tamanho adequado do VM no Azure.
    - Não se for encontrado nenhum tamanho adequado, a máquina está marcada como unsuitable para o Azure.
    - Se for encontrado um tamanho adequado, Azure migrar aplica-se os cálculos de armazenamento e de rede. Em seguida, aplica a localização e as definições de camada, para a recomendação de tamanho VM final de preços.
    - Se existirem vários tamanhos de VM do Azure elegíveis, recomenda-se a uma com o menor custo.

### <a name="as-on-premises-sizing"></a>Como no local de dimensionamento
Se for o critério de dimensionamento *tal como no local dimensionamento*, não considere que o histórico de desempenho de VMs e migrar Azure aloca VMs e discos com base no tamanho alocado no local.
- **Armazenamento**: para cada disco, recomendamos um disco padrão no Azure com o mesmo tamanho que o disco no local.
- **Rede**: para cada adaptador de rede, recomenda-se um adaptador de rede no Azure.
- **Computação**: Migrar do Azure analisa o número de núcleos e tamanho da memória da VM no local e recomenda uma VM do Azure com a mesma configuração. Se existirem vários tamanhos de VM do Azure elegíveis, recomenda-se a uma com o menor custo.
 
### <a name="confidence-rating"></a>Classificação de confiança

Cada assessment no Azure migrar está associado uma classificação de confiança que varia de 1 estrela a 5 estrelas (1 estrela a ser mais baixo e 5 estrelas a ser mais alto). A classificação de confiança é atribuída a uma avaliação com base na disponibilidade de pontos de dados necessários para a avaliação de computação. Ajuda a estimar a fiabilidade das recomendações de tamanho fornecida pelo Azure migrar. 

Classificação de confiança é útil quando estão a fazer *com base no desempenho dimensionamento* uma vez que nem todos os pontos de dados poderão estar disponíveis. Para *tal como no local dimensionamento*, a classificação de confiança é sempre estrela 5 como migrar do Azure tem todos os dados que necessita para o tamanho da VM. 

Para dimensionamento, com base no desempenho do Azure migrar tem dos dados de utilização de CPU e memória. Para cada disco ligado à VM, tem de leitura/escrita IOPS e débito para efetuar o dimensionamento com base no desempenho. Da mesma forma para cada adaptador de rede ligado à VM, migrar do Azure necessita de rede/out fazer com base no desempenho dimensionamento. Se qualquer um dos números de utilização acima não estão disponíveis no vCenter Server, a recomendação de tamanho efetuada ao migrar de Azure poderá não ser fiável. Consoante a percentagem de pontos de dados disponíveis, é fornecida a classificação de confiança para a avaliação:

   **Disponibilidade de pontos de dados** | Classificação de confiança
   --- | ---
   0%-20% | 1 estrela
   21%-40% | 2 estrelas
   41%-60% | 3 estrelas
   61%-80% | 4 estrelas
   81%-100% | 5 estrelas

Uma avaliação poderá não ter todos os pontos de dados disponíveis devido a um dos seguintes motivos:
- A definição de estatísticas no vCenter que Server não está definido para o nível 3 e o tipo de avaliação tem com base no desempenho dimensionamento como o critério de dimensionamento. Se a definição de estatísticas no vCenter Server for inferior ao nível 3, os dados de desempenho de disco e rede não são recolhidos do vCenter Server. Neste caso, a recomendação fornecida pelo Azure migrar para o disco e rede apenas baseia-se no que foi alocado no local. Para armazenamento, o Azure migrar recomenda discos padrão vez que não há nenhuma forma de identificar se o disco tem IOPS/débito alto e tem os discos premium.
- A definição de estatísticas no vCenter Server foi definido para o nível 3 durante um período pequeno, antes de kicking desativar a deteção. Por exemplo, se alterar o nível de definição de estatísticas para 3 hoje e de reunião inicial desativar a deteção a utilizar a aplicação de recoletor amanhã (após 24 horas), se estiver a criar uma avaliação durante um dia, terá todos os pontos de dados. Mas, se estiver a alterar a duração de desempenho nas propriedades de avaliação para um mês, a classificação de confiança fica inativo, como o disco e dados de desempenho de rede para o último mês de um não estão disponíveis. Se pretender que a ter em consideração os dados de desempenho de um mês, recomenda-se que mantenha a vCenter estatísticas definição do servidor para o nível 3 durante um mês antes de pode iniciar a deteção. 
- Algumas VMs sejam desligadas durante o período para os quais é calculada a avaliação. Se quaisquer VMs foram desligadas durante algum período de tempo, o vCenter Server não terão os dados de desempenho durante esse período. 
- Foram criadas algumas VMs entre o período para os quais é calculada a avaliação. Por exemplo, se estiver a criar uma avaliação para o histórico de desempenho de um mês, mas algumas VMs criadas no ambiente apenas uma semana há. Nestes casos, o histórico de desempenho de VMs novas não serão existe para a duração de toda.

> [!NOTE]
> Se a classificação de confiança de qualquer avaliação é inferior a 3 estrelas, recomendamos-lhe alterar o nível de definições de estatísticas do servidor vCenter e 3, aguardaremos durante o período de tempo que pretende que a considerar para a avaliação de (1 dia/semana/1 mês) e, em seguida, efetue a deteção e de avaliação. Se precedente não pode ser efetuada, com base no desempenho dimensionamento pode não ser fiável e recomenda-se para mudar para *tal como no local dimensionamento* alterando as propriedades de avaliação.

## <a name="monthly-cost-estimation"></a>Estimativa de custo mensal

Depois de concluídas as recomendações de dimensionamento, o Azure migrar calcula os custos de armazenamento e computação de pós-migração.

- **Custos de computação**: utilizar o tamanho recomendado da VM do Azure, Azure migrar utiliza a API de faturação para calcular o custo mensal para a VM. O cálculo demora o sistema operativo, software assurance da, localização e as definições de moeda na conta. Agrega o custo em todas as máquinas, para calcular o custo mensal total de computação.
- **Custo de armazenamento**: O armazenamento mensal de custos para uma máquina é calculada ao agregar o custo mensal de todos os discos ligados à máquina. Migrar do Azure calcula os custos de armazenamento mensal total por agregar os custos de armazenamento de todas as máquinas. Atualmente, o cálculo não demorar ofertas especificadas nas definições de avaliação na conta.

Os custos são apresentados na moeda especificada nas definições de avaliação. 


## <a name="next-steps"></a>Passos Seguintes

[Criar uma avaliação para VMs de VMware no local](tutorial-assessment-vmware.md)
