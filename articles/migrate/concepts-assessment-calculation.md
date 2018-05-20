---
title: Os cálculos de avaliação no Azure migrar | Microsoft Docs
description: Fornece uma descrição geral de cálculos de avaliação no serviço Azure migrar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: be4fb15d96f5598d4b1ddbbaa4befe7f6530152c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
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
Cliente Windows 7, 8 e 10 | O Azure oferece suporte com apenas a subscrição do Visual Studio. | Condicionalmente preparado para o Azure
Windows Vista, XP Professional | Estes sistemas operativos passaram respetivo fim de suporte de data, o computador pode efetuar o arranque no Azure, mas sem suporte de SO é fornecido pelo Azure. | Condicionalmente está pronto para o Azure, é recomendado para atualizar o sistema operativo antes de migrar para o Azure.
Linux | Azure patrocina estes [sistemas operativos Linux](../virtual-machines/linux/endorsed-distros.md). Outros sistemas de operativos Linux podem efetuar o arranque no Azure, mas é recomendado para atualizar o sistema operativo para uma versão endorsed antes de migrar para o Azure. | Pronto para o Azure, se a versão é aprovada.<br/><br/>Pronto condicionalmente se a versão não é aprovada.
Outros sistemas operativos<br/><br/> Por exemplo, Oracle Solaris, etc. de SO do Apple Mac, FreeBSD, etc. | Azure não apoia estes sistemas operativos. O computador pode efetuar o arranque no Azure, mas sem suporte de SO é fornecido pelo Azure. | Condicionalmente está pronto para o Azure, é recomendado para instalar um SO suportado antes de migrar para o Azure.  
SO especificado como *outros* no vCenter Server | Migrar do Azure não consegue identificar o SO neste caso. | Preparação desconhecida. Certifique-se de que o SO em execução dentro da VM é suportado no Azure.
sistemas operativos de 32 bits | O computador pode efetuar o arranque no Azure, mas o Azure poderá não oferecem suporte completo. | Condicionalmente está preparado para o Azure, considere atualizar sistema operativo da máquina do SO de 32 bits para 64 bits antes de migrar para o Azure.

## <a name="sizing"></a>Dimensionamento

Depois de uma máquina está marcada como pronta para o Azure, Azure migrar tamanhos da VM e os respetivos discos do Azure. Se o critério de dimensionamento especificado nas propriedades de avaliação é fazer com base no desempenho dimensionamento, o Azure migrar considera o histórico de desempenho da máquina para identificar um tamanho VM no Azure. Este método é útil em cenários em que alocou excessiva a VM no local, mas a utilização for baixa e pretende dimensionar as VMs do Azure para guardar o custo.

> [!NOTE]
> Migrar do Azure recolhe histórico do desempenho de VMs no local a partir do servidor vCenter. Para garantir a dimensionar exata, certifique-se de que a definição de estatísticas no vCenter Server está definida para o nível 3 e aguarde, pelo menos, um dia antes kicking desativar a deteção de VMs no local. Se a definição de estatísticas no vCenter Server for inferior ao nível 3, os dados de desempenho de disco e rede não são recolhidos.

Se não pretender considerar o histórico de desempenho para o dimensionamento de VM e pretende colocar a VM como-é para o Azure, pode especificar o critério de dimensionamento como *como no local* e migrar do Azure, em seguida, irá tamanho as VMs com base no no local configuração sem considerar os dados de utilização. Dimensionamento do disco, neste caso, irá ainda ser baseado em dados de desempenho.

### <a name="performance-based-sizing"></a>Dimensionamento com base no desempenho

Para dimensionamento com base no desempenho, começa a migrar do Azure com os discos ligados à VM, seguido de adaptadores de rede e, em seguida, mapas de uma VM do Azure com base nos requisitos da computação da VM no local.

- **Armazenamento**: Migrar Azure tenta mapear todos os discos ligados à máquina a um disco no Azure.

    > [!NOTE]
    > Suporta a migração do Azure geridos apenas discos para avaliação.

    - Para obter a e/s de disco Efetivo por segundo (IOPS) e débito (MBps), a Azure migrar multiplica o disco IOPS e o débito com o fator de comfort. Com base no IOPS Efetivo e valores de débito, Azure migrar identifica se o disco deve ser mapeado para um disco standard ou premium no Azure.
    - Se migrar do Azure não é possível localizar um disco com o IOPS & débito necessário, marca a máquina como unsuitable para o Azure. [Saiba mais](../azure-subscription-service-limits.md#storage-limits) sobre o Azure limita por disco e a VM.
    - Se encontrar um conjunto de discos adequados, Azure migrar seleciona aqueles que suportam o método de redundância do armazenamento e a localização especificada nas definições de avaliação.
    - Se existirem vários discos elegíveis, seleciona um com o menor custo.
    - Se a discos de dados de desempenho no disponível, todos os discos estão mapeados para os discos padrão no Azure.

- **Rede**: Migrar Azure tenta localizar uma VM do Azure que consiga suportar o número de adaptadores de rede ligados à máquina no local e o desempenho de que estes adaptadores de rede.
    - Para obter o desempenho de rede eficiente da VM no local, Azure migrar agrega os dados transmitidos por segundo (MBps) fora da máquina (rede out), através de todos os adaptadores de rede e aplica-se o fator de comfort. Este número é utilizado para localizar uma VM do Azure que consiga suportar o desempenho de rede necessários.
    - Juntamente com o desempenho da rede, também considera se a VM do Azure pode suportar necessários o número de adaptadores de rede.
    - Se não existem dados de desempenho de rede estiver disponíveis, apenas a contagem de adaptadores de rede é considerada para dimensionamento de VM.

- **Computação**: depois dos requisitos de armazenamento e rede são calculados, Azure migrar considera os requisitos de CPU e memória para localizar um tamanho adequado do VM no Azure.
    - Migrar do Azure analisa os núcleos sobreutilizados e a memória e aplica-se o fator de comfort para obter os núcleos efetivos e a memória. Com base nesse número, tenta localizar um tamanho adequado do VM no Azure.
    - Não se for encontrado nenhum tamanho adequado, a máquina está marcada como unsuitable para o Azure.
    - Se for encontrado um tamanho adequado, Azure migrar aplica-se os cálculos de armazenamento e de rede. Em seguida, aplica a localização e as definições de camada, para a recomendação de tamanho VM final de preços.
    - Se existirem vários tamanhos de VMs do Azure elegíveis, é recomendado aquele que tiver o custo mais baixo.

### <a name="as-on-premises-sizing"></a>Como no local de dimensionamento
Se for o critério de dimensionamento *tal como no local dimensionamento*, não considere que o histórico de desempenho de VMs e migrar Azure aloca VMs com base no tamanho alocado no local. No entanto, para dimensionamento de disco,-considere histórico do desempenho dos discos para recomendar discos Standard ou Premium.  
- **Armazenamento**: Migrar Azure mapeia cada disco ligado à máquina a um disco no Azure.

    > [!NOTE]
    > Suporta a migração do Azure geridos apenas discos para avaliação.

    - Para obter a e/s de disco Efetivo por segundo (IOPS) e débito (MBps), a Azure migrar multiplica o disco IOPS e o débito com o fator de comfort. Com base no IOPS Efetivo e valores de débito, Azure migrar identifica se o disco deve ser mapeado para um disco standard ou premium no Azure.
    - Se migrar do Azure não é possível localizar um disco com o IOPS & débito necessário, marca a máquina como unsuitable para o Azure. [Saiba mais](../azure-subscription-service-limits.md#storage-limits) sobre o Azure limita por disco e a VM.
    - Se encontrar um conjunto de discos adequados, Azure migrar seleciona aqueles que suportam o método de redundância do armazenamento e a localização especificada nas definições de avaliação.
    - Se existirem vários discos elegíveis, seleciona um com o menor custo.
    - Se a discos de dados de desempenho no disponível, todos os discos estão mapeados para os discos padrão no Azure.
- **Rede**: para cada adaptador de rede, recomenda-se um adaptador de rede no Azure.
- **Computação**: Migrar do Azure analisa o número de núcleos e tamanho da memória da VM no local e recomenda uma VM do Azure com a mesma configuração. Se existirem vários tamanhos de VMs do Azure elegíveis, é recomendado aquele que tiver o custo mais baixo. Dados de utilização de CPU e memória não são considerados para como no local dimensionamento.

### <a name="confidence-rating"></a>Classificação de confiança

Cada avaliação no Azure Migrate é associada a uma classificação de confiança de 1 a 5 estrelas (sendo que 1 estrela corresponde à mais baixa e 5 à mais alta). A classificação de confiança é alocada a uma avaliação com base na disponibilidade dos pontos de dados necessários para calcular a avaliação. A classificação de confiança de uma avaliação ajuda a calcular a fiabilidade das recomendações de tamanho fornecidas pelo Azure Migrate.

Para o dimensionamento com base no desempenho da VM, o Azure Migrate precisa dos dados de utilização relativos a CPU e memória. Além disso, para o dimensionamento de todos os discos ligados à VM, tem de leitura/escrita IOPS e débito. De forma semelhante, para cada adaptador de rede anexado à VM, o Azure Migrate requer a entrada/saída de rede para efetuar o dimensionamento com base em desempenho. Se algum dos números de utilização acima não estiver disponível no vCenter Server, a recomendação de tamanho feita pelo Azure Migrate poderá não ser fiável. Consoante a percentagem de pontos de dados disponíveis, a classificação de confiança para a avaliação é fornecida tal como indicado abaixo:

   **Disponibilidade de pontos de dados** | **Classificação de confiança**
   --- | ---
   0%-20% | 1 Estrela
   21%-40% | 2 Estrelas
   41%-60% | 3 Estrelas
   61%-80% | 4 Estrelas
   81%-100% | 5 Estrelas

Uma avaliação pode não ter todos os pontos de dados disponíveis por um dos seguintes motivos:
- A definição de estatísticas no vCenter Server não está definido para o nível 3. Se a definição de estatística no vCenter Server for inferior ao nível 3, os dados de desempenho para o disco e rede não são recolhidos do vCenter Server. Neste caso, a recomendação dada pelo Azure Migrate para disco e rede não se baseia na utilização. Sem considerar o IOPS/débito do disco, migrar do Azure não consegue identificar se o disco será necessário um disco de premium no Azure, por conseguinte, neste caso, migrar Azure recomenda discos padrão para todos os discos.
- A definição de estatística no vCenter Server foi definida como nível 3 para uma duração mais curta, antes de iniciar a deteção. Por exemplo, consideremos o cenário em que muda o nível de definição de estatística para 3 hoje e inicia a deteção com a aplicação recoletora amanhã (após 24 horas). Se estiver a criar uma avaliação para um dia, terá todos os pontos de dados e a classificação de confiança da avaliação será de 5 estrelas. No entanto, se estiver a mudar a duração do desempenho nas propriedades de avaliação para um mês, a classificação de confiança irá descer, uma vez que os dados de desempenho de rede e disco do último mês não estarão disponíveis. Se pretender considerar os dados de desempenho do último mês, é recomendado que mantenha a definição de estatística do vCenter Server no nível 3 durante um mês antes de iniciar a deteção.
- Poucas VMs foram encerradas durante o período para o qual a avaliação é calculada. Se houver VMs desligadas durante algum tempo, o vCenter Server não terá os dados de desempenho correspondentes a esse período.
- Poucas VMs foram criadas no período para o qual a avaliação é calculada. Por exemplo, se estiver a criar uma avaliação para o histórico de desempenho do último mês, mas poucas VMs tiverem sido criadas no ambiente há apenas uma semana. Em casos como este, o histórico de desempenho das novas VMs não estará lá para o período completo.

> [!NOTE]
> Se a classificação de confiança de qualquer avaliação for abaixo de 4 Estrelas, recomendamos que altere o nível das definições de estatística do vCenter Server para 3, aguarde o tempo que pretende considerar para avaliação (1 dia/1 semana/1 mês) e, em seguida, efetue a deteção e avaliação. Se não conseguir fazê-lo, o dimensionamento baseado em desempenho poderá não ser fiável e recomenda-se que mude para o *dimensionamento no local* ao alterar as propriedades de avaliação.

## <a name="monthly-cost-estimation"></a>Estimativa de custo mensal

Depois de concluídas as recomendações de dimensionamento, o Azure migrar calcula os custos de armazenamento e computação de pós-migração.

- **Custos de computação**: utilizar o tamanho recomendado da VM do Azure, Azure migrar utiliza a API de faturação para calcular o custo mensal para a VM. O cálculo demora o sistema operativo, software assurance da, localização e as definições de moeda na conta. Agrega o custo em todas as máquinas, para calcular o custo mensal total de computação.
- **Custo de armazenamento**: O armazenamento mensal de custos para uma máquina é calculada ao agregar o custo mensal de todos os discos ligados à máquina. Migrar do Azure calcula os custos de armazenamento mensal total por agregar os custos de armazenamento de todas as máquinas. Atualmente, o cálculo não demorar ofertas especificadas nas definições de avaliação na conta.

Os custos são apresentados na moeda especificada nas definições de avaliação.


## <a name="next-steps"></a>Passos Seguintes

[Criar uma avaliação para VMs de VMware no local](tutorial-assessment-vmware.md)
