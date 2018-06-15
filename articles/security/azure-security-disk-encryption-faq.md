---
title: Encriptação de disco do Azure FAQ | Microsoft Docs
description: Este artigo fornece respostas às perguntas mais frequentes sobre o Microsoft Azure disco encriptação para o Windows e as VMs de IaaS Linux.
services: security
documentationcenter: na
author: DevTiw
manager: avibm
editor: barclayn
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2018
ms.author: barclayn
ms.openlocfilehash: 47ccf91a64653c928cc4da01bc98535c97440d37
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
ms.locfileid: "32187696"
---
# <a name="azure-disk-encryption-faq"></a>Encriptação de disco do Azure FAQ

Este artigo fornece respostas às perguntas mais frequentes (FAQ) sobre a encriptação de disco do Azure para Windows e as VMs de IaaS Linux. Para obter mais informações sobre este serviço, consulte [encriptação de disco do Azure para Windows e as VMs de IaaS Linux](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Onde está o Azure Disk Encryption na disponibilidade geral (DG)?

Azure encriptação de disco para Windows e as VMs de IaaS Linux em geral é disponibilidade em todas as regiões públicas do Azure.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Ocorre no qual o utilizador que estão disponíveis com o Azure Disk Encryption?

GA de encriptação de disco do Azure suporta modelos Azure Resource Manager, do Azure PowerShell e CLI do Azure. Isto dá-lhe muita flexibilidade. Tem três opções diferentes para ativar a encriptação de disco para as suas VMs de IaaS. Para obter mais informações sobre a experiência de utilizador e orientações passo a passo disponíveis no Azure Disk Encryption, consulte [experiências e cenários de implementação do Azure Disk Encryption](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="how-much-does-azure-disk-encryption-cost"></a>Quanto custo Azure Disk Encryption?

Existem sem qualquer encargo para encriptar os discos da VM com o Azure Disk Encryption, mas existem custos associados a utilização do Cofre de chaves do Azure. Para obter mais informações sobre o Cofre de chaves do Azure, os custos consulte o [preços do Cofre de chaves](https://azure.microsoft.com/pricing/details/key-vault/) página.

## <a name="which-virtual-machine-tiers-does-azure-disk-encryption-support"></a>Os escalões de máquina virtual suporta a Azure Disk Encryption?

Encriptação de disco do Azure está disponível em VMs do escalão standard, incluindo [A, D, DS, G, GS e F](https://azure.microsoft.com/pricing/details/virtual-machines/) série VMs de IaaS. Também está disponível para VMs com o armazenamento premium. Não está disponível na camada básica VMs.

## <a name="what-linux-distributions-does-azure-disk-encryption-support"></a>As distribuições do Linux que suportam o Azure Disk Encryption?

Encriptação de disco do Azure é suportada no seguintes distribuições de servidor Linux e versões:

| Distribuição de Linux | Versão | Tipo de volume suportado para a encriptação|
| --- | --- |--- |
| Ubuntu | 16.04-DIARIAMENTE-LTS | Disco do SO e dados |
| Ubuntu | 14.04.5-DAILY-LTS | Disco do SO e dados |
| RHEL | 7.4 | Disco de dados * |
| RHEL | 7.3 | Disco de dados * |
| RHEL | 7.2 | Disco de dados * |
| RHEL | 6.8 | Disco de dados * |
| RHEL | 6.7 | Disco de dados * |
| CentOS | 7.3 | Disco do SO e dados |
| CentOS | 7.2N | Disco do SO e dados |
| CentOS | 6.8 | Disco do SO e dados |
| CentOS | 7.1 | Disco de dados |
| CentOS | 7.0 | Disco de dados |
| CentOS | 6.7 | Disco de dados |
| CentOS | 6.6 | Disco de dados |
| CentOS | 6.5 | Disco de dados |
| openSUSE | 13.2 | Disco de dados |
| SLES | 12 SP1 | Disco de dados |
| SLES | Prioridade: 12-SP1 | Disco de dados |
| SLES | HPC 12 | Disco de dados |
| SLES | Prioridade: 11-SP4 | Disco de dados |
| SLES | 11 SP4 | Disco de dados |

*__ADE é suportada para RHEL para o disco de dados. A implementação atual do ADE funcionar para o disco do SO, mas não é suportada atualmente jointly. Microsoft e Red Hat trabalhar para uma solução jointly suportada. No provisório, pode referenciar o documento técnico ADE para encriptação de disco de SO Linux [aqui](https://docs.microsoft.com/azure/security/azure-security-disk-encryption).__

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Como posso começar a utilizar o Azure Disk Encryption?

Para começar, leia o [encriptação de disco do Azure para Windows e as VMs de IaaS Linux](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) documento técnico.

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>Pode encriptar volumes de arranque e dados com o Azure Disk Encryption?

Sim, pode encriptar volumes de arranque e de dados para o Windows e as VMs de IaaS Linux. Para VMs do Windows, não é possível encriptar os dados sem primeiro encriptando o volume de SO. Para VMs com Linux, pode encriptar o volume de dados sem ter de encriptar o volume de SO primeiro. Depois de encriptou o volume de SO para Linux, a desativação da encriptação num volume SO para as VMs de IaaS Linux não é suportada.

## <a name="does-azure-disk-encryption-allow-you-to-bring-your-own-key-byok-capability"></a>Azure Disk Encryption permite-lhe traga a sua própria chave capacidade de (BYOK)?

Sim, pode fornecer as suas próprias chaves de encriptação de chaves. Estas chaves são salvaguardadas no Cofre de chaves do Azure, que é o armazenamento de chaves de encriptação de disco do Azure. Para obter mais informações sobre as chaves de encriptação de chaves suporta cenários, consulte [experiências e cenários de implementação do Azure Disk Encryption](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="can-i-use-an-azure-created-key-encryption-key"></a>Pode utilizar uma chave de encriptação de chaves do Azure-criar?

Sim, pode utilizar o Cofre de chaves do Azure para gerar uma chave de encriptação de chave para utilização de encriptação de disco do Azure. Estas chaves são salvaguardadas no Cofre de chaves do Azure, que é o armazenamento de chaves de encriptação de disco do Azure. Para obter mais informações sobre os cenários de suporte de chave de encriptação de chaves, consulte [experiências e cenários de implementação do Azure Disk Encryption](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="can-i-use-an-on-premises-key-management-service-or-hsm-to-safeguard-the-encryption-keys"></a>Pode utilizar um serviço de gestão de chaves no local ou HSM para salvaguardar as chaves de encriptação?

Não é possível utilizar o serviço de gestão de chaves no local ou HSM para salvaguardar as chaves de encriptação com o Azure Disk Encryption. Só pode utilizar o serviço Cofre de chaves do Azure para salvaguardar as chaves de encriptação. Para obter mais informações sobre os cenários de suporte de chave de encriptação de chaves, consulte [experiências e cenários de implementação do Azure Disk Encryption](azure-security-disk-encryption.md#disk-encryption-deployment-scenarios-and-user-experiences).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption"></a>Quais são os pré-requisitos para configurar a encriptação de disco do Azure?

Não há um script do PowerShell de pré-requisitos. Com este script, pode criar uma aplicação do Azure Active Directory, crie um novo cofre de chave ou configurar um cofre de chaves de acesso de encriptação de disco ativar a encriptação e salvaguarda segredos e as chaves. Para obter mais informações sobre os cenários de suporte de chave de encriptação de chaves, consulte [pré-requisitos do Azure Disk Encryption e cenários de implementação e experiências](azure-security-disk-encryption.md#prerequisites).

## <a name="where-can-i-get-more-information-on-how-to-use-powershell-for-configuring-azure-disk-encryption"></a>Onde posso obter mais informações sobre como utilizar o PowerShell para configurar a Azure Disk Encryption?

Temos algumas excelente artigos sobre a forma como pode efetuar tarefas básicas do Azure Disk Encryption, bem como em cenários mais avançados. Para as tarefas básicas, consulte [explorar o Azure Disk Encryption com o Azure PowerShell – parte 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Para cenários mais avançados, consulte [explorar o Azure Disk Encryption com o Azure PowerShell – parte 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Qual a versão do Azure PowerShell suportam o Azure Disk Encryption?

Utilize a versão mais recente do SDK do Azure PowerShell para configurar a Azure Disk Encryption. Transfira a versão mais recente do [Azure PowerShell](https://github.com/Azure/azure-powershell/releases). A encriptação de disco do Azure é *não* suportada pela versão 1.1.0 do Azure SDK.

> [!NOTE]
> A extensão de pré-visualização de encriptação de disco do Linux Azure foi preterida. Para obter mais informações, consulte [extensão pré-visualização de encriptação de disco do Azure descontinuar para as VMs de IaaS Linux](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

## <a name="can-i-apply-azure-disk-encryption-on-my-custom-linux-image"></a>Pode aplicar Azure Disk Encryption no meu imagem personalizada do Linux?

Não é possível aplicar a Azure Disk Encryption na sua imagem personalizada do Linux. Suportamos apenas as imagens de Linux Galeria para as distribuições suportadas que referida anteriormente. Não é suportada atualmente imagens personalizadas do Linux.

## <a name="can-i-apply-updates-to-a-linux-red-hat-vm-that-uses-the-yum-update"></a>Pode aplicar atualizações para a VM com Linux Red Hat que utiliza a atualização yum?

Sim, pode efetuar uma atualização ou correção uma VM do Red Hat com Linux. Para obter mais informações, consulte [aplicar atualizações às encriptados VM do Azure IaaS Red Hat utilizando a atualização yum](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/).

## <a name="what-is-the-recommended-azure-disk-encryption-workflow-for-linux"></a>O que é o fluxo de trabalho de encriptação de disco do Azure recomendado para Linux?

O seguinte fluxo de trabalho é recomendado ter os melhores resultados no Linux:
* Iniciar na imagem de galeria as cotações inalterado correspondente para o pretendido SO distro e a versão do
* Cópia de segurança de quaisquer unidades montadas que serão encriptadas.  Isto permite a recuperação em caso de falha, por exemplo, se a VM é reiniciada antes da encriptação foi concluída.
* Encriptar (pode demorar várias horas ou até mesmo dias dependendo das características da vm e o tamanho dos discos de dados anexados)
* Personalizar e adicione software à imagem, conforme necessário.

Se este fluxo de trabalho não for possível, depender [encriptação do serviço de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) (SSE) de armazenamento de plataforma camada de conta pode ser uma alternativa à utilização dm-crypt de encriptação de disco completa.

## <a name="what-is-the-disk-bek-volume-or-mntazurebekdisk"></a>O que é o disco "Bek Volume" ou "/ mnt/azure_bek_disk"?

"Volume Bek" para o Windows ou "/ mnt/azure_bek_disk" para o Linux é um volume de dados local que armazena em segurança as chaves de encriptação para encriptados VMs de IaaS do Azure.
> [!NOTE]
> Não elimine ou edite qualquer conteúdo este disco. Não é desmonte o disco, uma vez que a presença da chave de encriptação é necessária para as operações de encriptação na VM do IaaS.

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Em que pode ir para colocar questões ou comentários?

Pode colocar questões ou comentários sobre o [fórum do Azure Disk Encryption](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Passos Seguintes
Neste documento, aprendeu mais informações sobre as perguntas mais frequentes relacionadas com a Azure Disk Encryption. Para obter mais informações sobre este serviço e as respetivas capacidades, consulte os artigos seguintes:

- [Aplicar a encriptação de disco no Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Encriptar uma máquina virtual do Azure](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Encriptação de dados do Azure Inativos](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
