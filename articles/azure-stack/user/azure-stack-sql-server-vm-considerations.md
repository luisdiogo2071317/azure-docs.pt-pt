---
title: Melhores práticas do desempenho para o SQL Server na pilha de Virtual Machines do Azure
description: Apresenta as melhores práticas para otimizar o desempenho de SQL Server no Microsoft Azure pilha de máquinas virtuais.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: mabrigg
ms.reviewer: anajod
ms.openlocfilehash: 00c67503f5b9e0027cbb62520e392f56420a75e6
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34701402"
---
# <a name="optimize-sql-server-performance"></a>Otimizar o desempenho de SQL Server

Este artigo fornece orientação para otimizar o desempenho de SQL Server em máquinas virtuais de pilha do Microsoft Azure. Quando executar o SQL Server em máquinas virtuais de pilha do Azure, utilize as mesma base de dados otimização de desempenho opções aplicáveis ao SQL Server num ambiente de servidor no local. O desempenho da base de dados relacional numa nuvem do Azure pilha depende de vários fatores. Fatores incluem o tamanho da família de uma máquina virtual e a configuração dos discos de dados.

Quando criar imagens do SQL Server, [considerar o aprovisionamento de máquinas virtuais no portal do Azure pilha](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). Transfira a extensão de IaaS do SQL Server da gestão de Marketplace no Portal de administração de pilha do Azure e transferir a sua escolha de discos rígidos virtuais de SQL máquina virtual (VHD). Estes incluem SQL2014SP2, SQL2016SP1 e SQL2017.

> [!NOTE]  
> Enquanto o artigo descreve como aprovisionar uma máquina virtual de SQL Server utilizando o portal do Azure global, a documentação de orientação também se aplica a pilha do Azure com as seguintes diferenças: SSD não está disponível para o disco do sistema operativo, discos geridos não estão disponíveis, e Existem pequenas diferenças na configuração de armazenamento.

Obter o *melhor* desempenho para o SQL Server em máquinas virtuais de pilha do Azure é o objetivo deste artigo. Se a carga de trabalho for inferior pedir o seu trabalho, não poderá necessitar de cada otimização recomendada. Considere as necessidades de desempenho e padrões de carga de trabalho como avaliar estas recomendações.

> [!NOTE]  
> Para obter orientações de desempenho para o SQL Server em virtual machines do Azure, consulte [neste artigo](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance).

## <a name="before-you-begin"></a>Antes de começar
A lista de verificação seguinte é para um desempenho ideal do SQL Server em máquinas virtuais de pilha do Azure:


|Área|Otimizações de|
|-----|-----|
|Tamanho da máquina virtual |[DS3](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) ou superior para o SQL Server Enterprise edition.<br><br>[DS2](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) ou superior para o SQL Server Standard edition e Web edition.|
|Armazenamento |Utilize uma família de máquina virtual que suporta [armazenamento Premium](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-acs-differences).|
|Discos |Utilize um mínimo de discos de dados de dois (um para ficheiros de registo) e outra para o ficheiro de dados e o TempDB e escolha o tamanho do disco com base nas necessidades de capacidade. Predefinir as localizações de ficheiros de dados para estes discos durante a instalação do SQL Server.<br><br>Evite utilizar o sistema operativo ou discos temporários para o armazenamento de base de dados ou registo.<br>Stripe vários discos de dados do Azure para obter o aumento do débito de e/s através dos espaços de armazenamento.<br><br>O formato com o tamanho de alocação documentado.|
|E/S|Ative a inicialização instantânea de ficheiros para ficheiros de dados.<br><br>Limitar o aumento automático nas bases de dados com incrementos fixos razoavelmente pequenos (64 MB - 256 MB).<br><br>Desative autoshrink na base de dados.<br><br>Configure localizações de ficheiros de cópia de segurança e a base de dados predefinida em discos de dados, não o disco do sistema operativo.<br><br>Ative páginas bloqueadas.<br><br>Aplicam-se de que as do SQL Server service packs e atualizações cumulativas.|
|Funcionalidades específicas|Cópia de segurança diretamente para o blob storage (se suportado pela versão do SQL Server em utilização).|
|||

Para mais informações sobre *como* e *por que motivo* para efetuar estas otimizações, reveja os detalhes e as orientações fornecidas nas secções seguintes.

## <a name="virtual-machine-size-guidance"></a>Orientações de tamanho de máquina virtual

Para aplicações sensíveis ao desempenho, as seguintes [tamanhos de máquina virtual](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) são recomendadas:

- **Edição do SQL Server para empresas:** DS3 ou superior

- **SQL Server Standard edition e Web edition:** DS2 ou superior

Com a pilha do Azure, não há qualquer diferença de desempenho entre a série DS e DS_v2 de família de máquina virtual.

## <a name="storage-guidance"></a>Orientações de armazenamento

Máquinas virtuais-série DS (juntamente com a série de série DSv2) na pilha do Azure fornecem a máxima do sistema operativo disco e os dados débito do disco (IOPS). Uma máquina virtual de série DS ou série DSv2 fornece IOPS até 1000 para o disco do sistema operativo e até 2,300 IOPS por disco de dados, independentemente do tipo ou tamanho do disco que escolheu.

Débito do disco de dados é determinado exclusivamente com base na série de família de máquina virtual. Pode [consulte este artigo](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) para identificar o débito de disco de dados por série família de máquina virtual.

> [!NOTE]  
> Para cargas de trabalho de produção, selecione uma máquina virtual-série DS ou série dsv2 para fornecer o IOPS máximo possível no sistema operativo discos de dados e de disco.

Ao criar uma conta do storage na pilha do Azure, a opção de replicação geográfica não tem efeito porque esta capacidade não está disponível na pilha do Azure.

## <a name="disks-guidance"></a>Documentação de orientação dos discos

Existem três tipos de disco principal numa máquina virtual do Azure de pilha:

- **Disco do sistema operativo:** quando cria uma máquina virtual de pilha do Azure, a plataforma anexa pelo menos um disco (identificados como o **C** unidade) para a máquina virtual para o disco de sistema operativo. Este disco é um VHD armazenado como um blob de página no armazenamento.

- **Disco temporário:** máquinas de virtuais do Azure pilha conter outro disco chamado o disco temporário (identificados como o **D** unidade). Este é um disco no nó que pode ser utilizado para o espaço scratch.

- **Os discos de dados:** pode anexar mais discos para a máquina virtual como discos de dados e estes discos são armazenados no armazenamento como blobs de páginas.

As secções seguintes descrevem recomendações para utilizar estes discos diferentes.

### <a name="operating-system-disk"></a>Disco do sistema operativo

Um disco de sistema operativo é um VHD que pode efetuar o arranque e montar como uma versão de um sistema operativo em execução e está assinalada como como **C** unidade.

### <a name="temporary-disk"></a>Disco temporário

A unidade de armazenamento temporário, identificada como o **D** unidade, não é persistente. Não armazene os dados que é recusou-se perder, tais como os ficheiros de base de dados de utilizador ou ficheiros de registo de transação de utilizador, no **D** unidade.

Recomendamos que armazenar TempDB num disco de dados, como cada disco de dados fornece um máximo de até 2,300 IOPS por disco de dados.

### <a name="data-disks"></a>Discos de dados

- **Utilize discos de dados para os ficheiros de dados e de registo.** Se não estiver a utilizar striping de disco, utilize dois discos de dados de uma máquina virtual que suporte o Premium storage, em que um disco contém os ficheiros de registo e o outro contém os dados e ficheiros de TempDB. Cada disco de dados fornece um número de IOPS e largura de banda (MB/s), dependendo da família de máquina virtual, conforme descrito em [tamanhos de Máquina Virtual suportados na pilha de Azure](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes). Se estiver a utilizar uma disco striping técnica, tais como espaços de armazenamento, coloque todos os ficheiros de dados e de registo na mesma unidade (incluindo TempDB). Esta configuração dá-lhe o número máximo de IOPS disponíveis para o SQL Server consumir, independentemente do ficheiro que necessita de-los em qualquer altura específica.

> [!NOTE]  
> Quando aprovisionar uma máquina virtual do SQL Server no portal, tem a opção de editar a configuração de armazenamento. Consoante a configuração, a pilha de Azure configura um ou mais discos. Vários discos são combinados num agrupamento de armazenamento única. Ambos os ficheiros de dados e de registo de residir em conjunto nesta configuração.

- **Striping disco:** para obter mais débito, pode adicionar discos de dados adicionais e utilizar striping de disco. Para determinar o número de discos de dados que precisa de, analisar o número de IOPS e largura de banda necessária para os ficheiros de registo e para os seus dados e ficheiros de TempDB. Tenha em atenção que são os limites IOPS por disco de dados com base na família de série de máquina virtual e não com base no tamanho da máquina virtual. No entanto, os limites de largura de banda de rede, são baseados no tamanho da máquina virtual. Consulte as tabelas no [tamanhos de Máquina Virtual na pilha de Azure](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) para obter mais detalhes. Utilize as seguintes diretrizes:

    - Para o Windows Server 2012 ou posterior, utilize [espaços de armazenamento](https://technet.microsoft.com/library/hh831739.aspx) com as seguintes diretrizes:

        1.  Defina a intercalação (tamanho do stripe) para 64 KB (65,536 bytes) para cargas de trabalho (OLTP) e 256 KB (262.144 bytes) para cargas de trabalho de armazenamento de dados para evitar o impacto no desempenho devido a desalinhamento partition de processamento de transações online. Isto deve ser definido com o PowerShell.

        2.  Definir o número de colunas = número de discos físicos. Utilize o PowerShell quando configurar mais do que oito discos (não IU do Gestor de servidor).

            Por exemplo, o PowerShell seguinte cria um novo agrupamento de armazenamento com o tamanho de intercalação definido para 64 KB e o número de colunas para 2:

          ```PowerShell  
          $PoolCount = Get-PhysicalDisk -CanPool $True
          $PhysicalDisks = Get-PhysicalDisk | Where-Object {$_.FriendlyName -like "*2" -or $_.FriendlyName -like "*3"}

          New-StoragePool -FriendlyName "DataFiles" -StorageSubsystemFriendlyName "Storage Spaces*" -PhysicalDisks $PhysicalDisks | New-VirtualDisk -FriendlyName "DataFiles" -Interleave 65536 -NumberOfColumns 2 -ResiliencySettingName simple –UseMaximumSize |Initialize-Disk -PartitionStyle GPT -PassThru |New-Partition -AssignDriveLetter -UseMaximumSize |Format-Volume -FileSystem NTFS -NewFileSystemLabel "DataDisks" -AllocationUnitSize 65536 -Confirm:$false
          ```

- Determine o número de discos associados ao seu agrupamento de armazenamento com base nas suas expectativas de carga. Tenha em atenção que os tamanhos de outra máquina virtual permitem números diferentes de discos de dados anexados. Para obter mais informações, consulte [tamanhos de Máquina Virtual suportados na pilha de Azure](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes).
- Para obter o IOPS máximo possível para discos de dados, recomenda-se adicionar o número máximo de discos de dados suportados pela sua [tamanho da máquina virtual](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-vm-sizes) e utilizar striping de disco.
- **Tamanho de unidade de alocação de NTFS:** quando formatar o disco de dados, é recomendado que utilize um tamanho de unidade de alocação de 64 KB para ficheiros de registo e dados, bem como TempDB.
- **Disco práticas de gestão:** ao remover um disco de dados, pare o serviço do SQL Server durante a alteração. Além disso, não altere as definições de cache nos discos como não fornece quaisquer melhorias de desempenho.

> [!WARNING]  
> Falha ao parar o serviço SQL Server durante estas operações pode danificá-la.

## <a name="io-guidance"></a>Orientações de e/s

- Considere ativar a inicialização instantânea de ficheiros reduzir o tempo necessário para alocação de ficheiro inicial. Para tirar partido de inicialização instantânea de ficheiros, pode conceder a conta de serviço do SQL Server (MSSQLSERVER) com **SE_MANAGE_VOLUME_NAME** e adicioná-la para o **efetuar tarefas de manutenção de Volume** segurança política. Se estiver a utilizar uma imagem de plataforma do SQL Server para o Azure, a predefinição conta de serviço (**NT Service\MSSQLSERVER**) não está adicionada ao **efetuar tarefas de manutenção de Volume** política de segurança. Por outras palavras, inicialização instantânea de ficheiros não está ativada numa imagem de plataforma do Azure do SQL Server. Após a adição da conta de serviço do SQL Server para o **efetuar tarefas de manutenção de Volume** política de segurança, reinicie o serviço do SQL Server. Podem existir considerações de segurança para utilizar esta funcionalidade. Para obter mais informações, consulte [inicialização de ficheiro de base de dados](https://msdn.microsoft.com/library/ms175935.aspx).
- **O aumento automático** é o plano de contingência para o crescimento inesperado. Não é gerir o seu crescimento de dados e de registo numa base diária com o aumento automático. Se for utilizada a opção autogrow, previamente aumente o ficheiro utilizando o **tamanho** mudar.
- Certifique-se **autoshrink** está desativada para evitar custos desnecessários que podem afetar negativamente o desempenho.
- Configure localizações de ficheiros de cópia de segurança e a base de dados predefinidos. Utilize as recomendações neste artigo e efetue as alterações na janela de propriedades do servidor. Para obter instruções, consulte [ver ou alterar as localizações predefinidas para dados e ficheiros de registo (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Captura de ecrã seguinte demonstra onde pretende efetuar estas alterações:

    > ![Ver ou alterar as localizações predefinidas](./media/sql-server-vm-considerations/image1.png)

- Ative páginas bloqueadas reduzir e/s e quaisquer atividades de paginação. Para obter mais informações, consulte [ativar as páginas de bloqueio na opção de memória (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Considere a compressão de quaisquer ficheiros de dados ao transferir/fora da pilha do Azure, incluindo as cópias de segurança.

## <a name="feature-specific-guidance"></a>Documentação de orientação específica à funcionalidade

Algumas implementações poderão alcançar os benefícios de desempenho adicionais utilizando técnicas de configuração mais avançadas. A lista seguinte destaca algumas funcionalidades do SQL Server que podem ajudar a melhorar o desempenho:

- **Criar uma cópia de segurança do Azure** **armazenamento.** Quando efetua cópias de segurança para o SQL Server em execução em máquinas virtuais de pilha do Azure, pode utilizar a cópia de segurança do SQL Server para o URL. Esta funcionalidade está disponível a partir de com o SQL Server 2012 SP1 CU2 e recomendada para cópia de segurança nos discos de dados anexados.

    Quando a cópia de segurança ou restauro utilizando o armazenamento do Azure, siga as recomendações fornecidas [cópia de segurança do SQL Server para o URL de melhores práticas e resolução de problemas](https://msdn.microsoft.com/library/jj919149.aspx) e [restauro de cópias de segurança armazenadas no Microsoft Azure](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/restoring-from-backups-stored-in-microsoft-azure?view=sql-server-2017). Também pode automatizar estas cópias de segurança utilizando [cópia de segurança automatizada do SQL Server em Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup).

-   **Cópia de segurança para armazenamento de pilha do Azure.** Pode criar cópias de segurança para armazenamento de pilha do Azure de forma semelhante tal como acontece com a cópia de segurança em armazenamento do Azure. Quando cria uma cópia de segurança dentro do SQL Server Management Studio (SSMS), tem de introduzir manualmente as informações de configuração. Não é possível utilizar o SSMS para criar o contentor de armazenamento ou a assinatura de acesso partilhado. SSMS liga-se apenas a subscrições do Azure, não as subscrições de pilha do Azure. Em vez disso, terá de criar a conta de armazenamento, o contentor e a assinatura de acesso partilhado no portal do Azure pilha ou com o PowerShell.

    Quando colocar as informações para a caixa de diálogo de cópia de segurança do SQL Server:

    ![Cópia de segurança do SQL Server](./media/sql-server-vm-considerations/image3.png)

    > [!NOTE]  
    > A assinatura de acesso partilhado é o token SAS no portal do Azure pilha, sem a esquerda '?' na cadeia. Se utilizar a função de cópia a partir do portal, tem de eliminar à esquerda '?' para o token funcionar dentro do SQL Server.

    Assim que tiver o destino de cópia de segurança, configurar e configurado no SQL Server, pode, em seguida, criar cópias de segurança para armazenamento de Blobs do Azure pilha.

## <a name="next-steps"></a>Passos Seguintes

[Utilizando os serviços ou na criação de aplicações para a pilha do Azure](azure-stack-considerations.md)