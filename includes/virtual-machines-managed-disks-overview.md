---
title: incluir ficheiro
description: incluir ficheiro
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3e660dbcbfecd57c43ecec6006581364660adb44
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50737250"
---
# <a name="azure-managed-disks-overview"></a>Descrição geral do Managed Disks do Azure

Managed Disks do Azure simplifica a gestão de discos para VMs IaaS do Azure ao gerir o [contas de armazenamento](../articles/storage/common/storage-introduction.md) associadas aos discos VM. Só tem de especificar o tipo ([Standard HDD](../articles/virtual-machines/windows/standard-storage.md), [SSD padrão](../articles/virtual-machines/windows/disks-standard-ssd.md), ou [Premium SSD](../articles/virtual-machines/windows/premium-storage.md)) e o tamanho do disco é necessário e o Azure cria e gere o disco por si.

## <a name="benefits-of-managed-disks"></a>Benefícios dos discos geridos

Vamos dar uma olhada em alguns dos benefícios que ganha ao utilizar discos geridos, começando com este vídeo do Channel 9 [melhor resiliência da VM do Azure com os Managed Disks](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency).
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>Implantação de VM simples e dimensionável

Managed Disks processa o armazenamento para em segundo plano. Anteriormente, era necessário criar contas de armazenamento para conter os discos (ficheiros VHD) para as VMs do Azure. Quando o aumento vertical, era necessário certificar-se de que criou as contas de armazenamento adicionais, para não exceder o limite de IOPS de armazenamento com qualquer um dos seus discos. Com os Managed Disks lidar com armazenamento, já não está limitado por limites de conta de armazenamento (por exemplo, 20.000 IOPS / conta). Também já não terá de copiar as imagens personalizadas (ficheiros VHD) para várias contas de armazenamento. Pode geri-los num local central – uma conta de armazenamento por região do Azure – e usá-los para criar centenas de VMs numa subscrição.

Discos geridos, poderá criar a VM até 50 000 **discos** de um tipo de uma subscrição por região, que irá permitir-lhe criar milhares de **VMs** numa única subscrição. Esta funcionalidade também aumenta a escalabilidade do [conjuntos de dimensionamento de máquinas virtuais](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) , permitindo-lhe criar até mil VMs num conjunto de dimensionamento com uma imagem do Marketplace.

### <a name="better-reliability-for-availability-sets"></a>Melhor fiabilidade para conjuntos de disponibilidade

Discos geridos fornece melhor confiabilidade para conjuntos de disponibilidade ao garantir que os discos da [VMs num conjunto de disponibilidade](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) são suficientemente isolados uns dos outros para evitar pontos únicos de falha. Discos são colocados automaticamente em unidades de escala de armazenamento diferente (carimbos de data /). Se um carimbo de data / falhar devido uma falha de hardware ou software, apenas as instâncias VM com discos nesses carimbos falharem. Por exemplo, digamos que tenha um aplicativo em execução em VMs de cinco, e as VMs estão num conjunto de disponibilidade. Os discos para essas VMs não ser armazenadas no carimbo a mesmo, portanto, se um carimbo ficar inativo, as outras instâncias do aplicativo continuam a executar.

### <a name="highly-durable-and-available"></a>Elevada disponibilidade e durabilidade

Os Discos do Azure foram concebidos para garantir uma disponibilidade de 99,999%. Fique tranquilo ao saber que tem três réplicas dos dados, que permitem uma elevada durabilidade. Se uma ou até mesmo duas réplicas apresentarem problemas, as restantes ajudam a garantir a persistência dos dados e alta tolerância contra falhas. Esta arquitetura tem ajudado o Azure a garantir de forma consistente uma durabilidade de nível empresarial para discos IaaS, com uma Taxa de Falhas Anual de ZERO% líder da indústria. 

### <a name="granular-access-control"></a>Controlo de acesso granular

Pode usar [controlo de acesso de controlo (RBAC)](../articles/role-based-access-control/overview.md) para atribuir permissões específicas para um disco gerido a um ou mais utilizadores. Gerido discos expõe uma variedade de operações, incluindo de leitura, escrita (criar/atualizar), eliminar e obter um [assinatura de acesso partilhado (SAS) URI](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) para o disco. Pode conceder acesso a apenas as operações de que uma pessoa precisa de executar seu trabalho. Por exemplo, se não pretender que uma pessoa para copiar um disco gerido para uma conta de armazenamento, pode optar por não conceder acesso para a ação de exportação para esse disco gerido. Da mesma forma, se não pretender que uma pessoa a utilizar um URI de SAS para copiar um disco gerido, pode optar por não conceda essa permissão para o disco gerido.

### <a name="azure-backup-service-support"></a>Suporte de serviço de cópia de segurança do Azure

Utilize o serviço de cópia de segurança do Azure com os Managed Disks para criar uma tarefa de cópia de segurança com cópias de segurança baseados no tempo, fácil restauro de VM e políticas de retenção de cópia de segurança. Discos geridos só suportam o armazenamento localmente redundante (LRS) como a opção de replicação. Três cópias dos dados são mantidas numa única região. Para a recuperação de desastre regional, é necessário fazer backup os discos da VM numa região diferente com [serviço de cópia de segurança do Azure](../articles/backup/backup-introduction-to-azure-backup.md) e uma conta de armazenamento GRS como o Cofre de cópia de segurança. Cópia de segurança do Azure suporta atualmente os tamanhos de disco até 4 TB de discos. Precisa [atualização pilha de cópia de segurança de VM para V2](../articles/backup/backup-upgrade-to-vm-backup-stack-v2.md) para suporte de discos de 4 TB. Para obter mais informações, consulte [serviço através de cópia de segurança do Azure para VMs com discos geridos](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="pricing-and-billing"></a>Preços e Faturação

Ao utilizar o Managed Disks, aplicam-se as seguintes considerações de faturas:

* Tipo de armazenamento

* Tamanho do Disco

* Número de transações

* Transferências de dados de saída

* Instantâneos de disco (cópia de disco completa) geridos

Vamos dar uma olhada mais de perto essas opções.

**Tipo de armazenamento:** Managed Disks oferece 3 escalões de desempenho: [Standard HDD](../articles/virtual-machines/windows/standard-storage.md), [Standard SSD](../articles/virtual-machines/windows/disks-standard-ssd.md), e [Premium](../articles/virtual-machines/windows/premium-storage.md). A faturação de um disco gerido depende de qual tipo de armazenamento que selecionou para o disco.

**Tamanho do disco**: depende de faturação para discos geridos no tamanho do disco aprovisionado. Azure mapeia o tamanho aprovisionado (arredondado) para a opção de Managed Disks mais próxima, conforme especificado nas tabelas abaixo. Cada disco gerido é mapeado para um dos tamanhos suportados aprovisionados e é cobrado em conformidade. Por exemplo, se criar um disco gerido standard e especificar um tamanho aprovisionado de 200 GB, é-lhe cobrada de acordo com os preços do tipo de disco S15.

Seguem-se disponível para um disco gerido premium os tamanhos de disco:

| **SDD Premium gerido <br>tipo de disco** | **P4** | **P6** | **P10** | **P15** | **P20** | **P30** | **P40** | **P50** | **P60** | **P70** | **P80** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Tamanho do Disco        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1.024 giB (1 TiB) | 2.048 giB (2 TiB) | 4095 giB (4 TiB) | 8.192 giB (8 TiB) | 16,384 giB (TiB de 16) | 32.767 giB (TiB) |

Seguem-se disponível para um disco gerido de SSD padrão os tamanhos de disco:

| **SSD Standard gerido <br>tipo de disco** | **E10** | **E15** | **E20** | **E30** | **E40** | **E50** | **E60** | **E70** | **E80** |
|------------------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Tamanho do Disco        | 128 GiB | 256 GiB | 512 GiB | 1.024 giB (1 TiB) | 2.048 giB (2 TiB) | 4095 giB (4 TiB) | 8.192 giB (8 TiB) | 16,384 giB (TiB de 16) | 32.767 giB (TiB) |

Seguem-se disponível para um disco gerido de HDD padrão os tamanhos de disco:

| **HDD padrão gerido <br>tipo de disco** | **S4** | **S6** | **S10** | **S15** | **S20** | **S30** | **S40** | **S50** | **S60** | **S70** | **S80** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------|----------------|----------------|----------------|
| Tamanho do Disco        | 32 GiB  | 64 GiB  | 128 GiB | 256 GiB | 512 GiB | 1.024 giB (1 TiB) | 2.048 giB (2 TiB) | 4095 giB (4 TiB) | 8.192 giB (8 TiB) | 16,384 giB (TiB de 16) | 32.767 giB (TiB) |

**Número de transações**: É Faturado por número de transações que executar num disco gerido standard.

Discos de SSD Standard utilizam o tamanho da unidade de e/s de 256KB. Se os dados que está a ser transferidos são inferior a 256 KB, é considerado 1 unidade de e/s. Tamanhos de e/s maiores são contabilizados como várias e/SS de tamanho 256 KB. Por exemplo, uma e/s de 1,100 KB é contabilizado como cinco unidades de e/s.

Não existe nenhum custo para transações para um disco gerido premium.

**Transferências de dados de saída**: [transferências de dados de saída](https://azure.microsoft.com/pricing/details/data-transfers/) faturação para utilização de largura de banda de incorrer em (dados que saem de datacenters do Azure).

Para obter informações detalhadas sobre os preços do Managed Disks, consulte [preços de discos geridos](https://azure.microsoft.com/pricing/details/managed-disks).


## <a name="managed-disk-snapshots"></a>Instantâneos de disco gerido

Um instantâneo gerido é uma cópia completa só de leitura de um disco gerido, que é armazenado como um disco gerido standard por predefinição. Com instantâneos, pode fazer backup de seus discos geridos em qualquer ponto no tempo. Estes instantâneos existem independentemente do disco de origem e podem ser utilizados para criar novos discos geridos. Eles são faturados com base no tamanho utilizado. Por exemplo, se criar um instantâneo de um disco gerido com capacidade aprovisionada de 64 GiB e o tamanho de dados utilizados real de 10 GiB, instantâneo será cobrado apenas o tamanho de dados utilizados de 10 GiB.  

[Instantâneos incrementais](../articles/virtual-machines/windows/incremental-snapshots.md) não são atualmente suportadas para o Managed Disks.

Para saber mais sobre como criar instantâneos de discos geridos, consulte os seguintes recursos:

* [Criar uma cópia de um VHD armazenado como Disco Gerido com os Instantâneos no Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Criar uma cópia de um VHD armazenado como Disco Gerido com os Instantâneos no Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)

## <a name="images"></a>Imagens

Discos geridos também suportam a criação de uma imagem personalizada gerida. Pode criar uma imagem a partir do seu VHD personalizado numa conta de armazenamento ou diretamente a partir de uma VM generalizada (preparar sys). Este processo captura numa única imagem gerida todos os discos associados uma VM, incluindo o sistema operacional e os dados de discos. Esta imagem personalizada gerida permite criar centenas de VMs através da sua imagem personalizada sem a necessidade de copiar ou gerir as contas de armazenamento.

Para obter informações sobre a criação de imagens, consulte os artigos seguintes:

* [Como capturar uma imagem gerida de uma VM generalizada no Azure](../articles/virtual-machines/windows/capture-image-resource.md)
* [Como generalizar e capturar uma máquina virtual do Linux com a CLI do Azure](../articles/virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>Imagens com instantâneos

Muitas vezes, verá a palavra "imagem" utilizada com VMs e, agora verá "instantâneos" também. É importante compreender a diferença entre estes termos. Com os Managed Disks, pode tirar uma imagem de uma VM generalizada que foi desalocada. Esta imagem inclui todos os discos ligados à VM. Pode utilizar esta imagem para criar uma nova VM e irá incluir todos os discos.

Um instantâneo é uma cópia de um disco no ponto no tempo é necessário. Aplica-se apenas a um disco. Se tiver uma VM que tem apenas um disco (o SO), pode tirar um instantâneo ou uma imagem dele e criar uma VM a partir do instantâneo ou a imagem.

E se uma VM tem cinco discos e elas estão repartidas? Pode tirar um instantâneo de cada um dos discos, mas há sem reconhecimento de dentro da VM do estado dos discos – os instantâneos apenas sabem sobre o que um disco. Neste caso, os instantâneos precisariam ser coordenados entre si e que não é atualmente suportado.

## <a name="managed-disks-and-encryption"></a>Discos geridos e encriptação

Existem dois tipos de criptografia para discutir no contexto de discos geridos. A primeira é Storage Service Encryption (SSE), que é efetuada pelo serviço de armazenamento. A segunda é Azure Disk Encryption, que pode ativar nos discos de SO e dados para as suas VMs.

### <a name="storage-service-encryption-sse"></a>Encriptação do serviço de armazenamento (SSE)

[Encriptação do serviço de armazenamento do Azure](../articles/storage/common/storage-service-encryption.md) fornece encriptação em repouso e salvaguardar os seus dados para cumprir os compromissos de segurança e conformidade. O SSE está ativado por predefinição para todos os discos geridos, instantâneos e imagens em todas as regiões onde os discos geridos estão disponíveis. A partir de 10 de Junho de 2017, todos os novos geridos instantâneos/discos/imagens e novos dados escritos nos discos geridos existentes são automaticamente encriptados em repouso com chaves geridas pela Microsoft por predefinição. Visite o [página de FAQ do Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption) para obter mais detalhes.

### <a name="azure-disk-encryption-ade"></a>Azure Disk Encryption (ADE)

O Azure Disk Encryption permite que criptografe os discos de SO e os dados utilizados pela máquina Virtual IaaS. Essa criptografia inclui os discos geridos. Para Windows, as unidades são encriptadas com a tecnologia de encriptação do BitLocker de norma da indústria. Para o Linux, os discos são encriptados com a tecnologia de DM-Crypt. O processo de encriptação está integrado com o Azure Key Vault, para que possa controlar e gerir as chaves de encriptação de disco. Para obter mais informações, consulte [encriptação de disco do Azure para Windows e VMs de IaaS Linux](../articles/security/azure-security-disk-encryption.md).

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre os Managed Disks, consulte os artigos seguintes.

### <a name="get-started-with-managed-disks"></a>Introdução aos Discos Geridos

* [Criar uma VM com o Resource Manager e o PowerShell](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md)

* [Criar uma VM do Linux através da CLI do Azure](../articles/virtual-machines/linux/quick-create-cli.md)

* [Anexar um disco de dados geridos para uma VM do Windows com o PowerShell](../articles/virtual-machines/windows/attach-disk-ps.md)

* [Adicionar um disco gerido a uma VM do Linux](../articles/virtual-machines/linux/add-disk.md)

* [Scripts de exemplo do PowerShell de discos de geridos](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [Utilizar Managed Disks em modelos do Azure Resource Manager](../articles/virtual-machines/windows/using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>Compare as opções de armazenamento de discos geridos

* [Discos SSD Premium](../articles/virtual-machines/windows/premium-storage.md)

* [Discos Standard SSD e HDD](../articles/virtual-machines/windows/standard-storage.md)

### <a name="operational-guidance"></a>Orientação operacional

* [Migrar do AWS e de outras plataformas para os Managed Disks no Azure](../articles/virtual-machines/windows/on-prem-to-azure.md)

* [Converter VMs do Azure para managed disks no Azure](../articles/virtual-machines/windows/migrate-to-managed-disks.md)
