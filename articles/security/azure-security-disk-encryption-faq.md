---
title: FAQ de encriptação de disco do Azure | Documentos da Microsoft
description: Este artigo fornece respostas para perguntas freqüentes sobre o Microsoft Azure disco encriptação para Windows e VMs de IaaS Linux.
services: security
documentationcenter: na
author: mestew
manager: MBaldwin
ms.assetid: 7188da52-5540-421d-bf45-d124dee74979
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/30/2018
ms.author: mstewart
ms.openlocfilehash: 98b8883a5ab0096102ab7daf90b5b2791a6f7e41
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389580"
---
# <a name="azure-disk-encryption-faq"></a>FAQ de encriptação de disco do Azure

Este artigo fornece respostas às perguntas mais frequentes (FAQ) sobre a encriptação de disco do Azure para Windows e VMs de IaaS Linux. Para obter mais informações sobre este serviço, consulte [encriptação de disco do Azure para Windows e VMs de IaaS Linux](azure-security-disk-encryption-overview.md).

## <a name="where-is-azure-disk-encryption-in-general-availability-ga"></a>Onde está o Azure Disk Encryption em disponibilidade geral (GA)?

Encriptação de disco para Windows e Linux VMs de IaaS do Azure está em disponibilidade geral em todas as regiões públicas do Azure.

## <a name="what-user-experiences-are-available-with-azure-disk-encryption"></a>Ocorre com o qual o utilizador que estão disponíveis com o Azure Disk Encryption?

Disponibilidade geral de encriptação de disco do Azure oferece suporte a modelos do Azure Resource Manager, do Azure PowerShell e CLI do Azure. As várias experiências de utilizador dão-lhe flexibilidade. Tem três opções diferentes para ativar a encriptação de disco para as VMs de IaaS. Para obter mais informações sobre a experiência de utilizador e a orientação passo a passo disponível no Azure Disk Encryption, consulte [ativar o Azure disco encriptação para Windows](azure-security-disk-encryption-windows.md) e [ativar o Azure Disk Encryption para Linux](azure-security-disk-encryption-linux.md).

## <a name="how-much-does-azure-disk-encryption-cost"></a>Quanto custa o Azure Disk Encryption?

Não existe nenhum custo para encriptar discos da VM com o Azure Disk Encryption, mas existem encargos associados à utilização do Cofre de chaves do Azure. Para obter mais informações sobre os custos do Azure Key Vault, consulte a [preços do Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) página.


## <a name="which-virtual-machine-tiers-does-azure-disk-encryption-support"></a>As camadas de máquina virtual suporta o Azure Disk Encryption?

O Azure Disk Encryption está disponível em VMs de escalão standard, incluindo [, D, DS, G, GS e F](https://azure.microsoft.com/pricing/details/virtual-machines/) série VMs de IaaS. Também está disponível para VMs com o armazenamento premium. Não está disponível no escalão básico VMs.

## <a name="bkmk_LinuxOSSupport"></a> Distribuições do Linux que suporta o Azure Disk Encryption?

O Azure Disk Encryption é suportado nas seguintes distribuições de servidor do Linux e versões:

| Distribuição do Linux | Versão | Tipo de volume suportado para a encriptação|
| --- | --- |--- |
| Ubuntu | DIARIAMENTE-16.04-LTS | Disco de SO e dados |
| Ubuntu | 14.04.5-DAILY-LTS | Disco de SO e dados |
| RHEL | 7.5 | Disco de dados * |
| RHEL | 7.4 | Disco de dados * |
| RHEL | 7.3 | Disco de dados * |
| RHEL | 7.2 | Disco de dados * |
| RHEL | 6.8 | Disco de dados * |
| RHEL | 6.7 | Disco de dados * |
| CentOS | 7.4 | Disco de SO e dados |
| CentOS | 7.3 | Disco de SO e dados |
| CentOS | 7.2N | Disco de SO e dados |
| CentOS | 6.8 | Disco de SO e dados |
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


*__ADE é suportada para RHEL para o disco de dados. A implementação atual do ADE funciona o disco do SO, mas não é suportada atualmente em conjunto. Microsoft e a Red Hat estão a trabalhar numa solução em conjunto, suportada. Até lá, pode fazer referência a [do Azure Disk Encryption para Linux](azure-security-disk-encryption-linux.md) artigo.__

## <a name="how-can-i-start-using-azure-disk-encryption"></a>Como posso começar a utilizar o Azure Disk Encryption?

Para começar, leia os [descrição geral do Azure Disk Encryption](azure-security-disk-encryption-overview.md).

## <a name="can-i-encrypt-both-boot-and-data-volumes-with-azure-disk-encryption"></a>Pode criptografar volumes de arranque e dados com o Azure Disk Encryption?

Sim, pode criptografar volumes de arranque e de dados para VMs de IaaS de Linux e Windows. Para VMs do Windows, não é possível encriptar os dados sem primeiro criptografar o volume do sistema operacional. Para VMs do Linux, é possível criptografar o volume de dados sem ter de criptografar o volume do sistema operacional pela primeira vez. Depois de criptografia do volume do sistema operacional para Linux, desativar a encriptação num volume do sistema operacional para VMs de IaaS Linux não é suportada.

## <a name="does-azure-disk-encryption-allow-you-to-bring-your-own-key-byok-capability"></a>O Azure Disk Encryption permite traga seu próprio capacidades-chave (BYOK)?

Sim, pode fornecer suas próprias chaves de encriptação de chave. Estas chaves são salvaguardadas no Azure Key Vault, que é o armazenamento de chaves do Azure Disk Encryption. Para obter mais informações sobre as chaves de encriptação de chave oferecer suporte a cenários, consulte [pré-requisitos do Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md).

## <a name="can-i-use-an-azure-created-key-encryption-key"></a>Pode utilizar uma chave de encriptação de chave criadas pelo Azure?

Sim, pode utilizar o Azure Key Vault para gerar uma chave de encriptação de chave para utilização de encriptação de disco do Azure. Estas chaves são salvaguardadas no Azure Key Vault, que é o armazenamento de chaves do Azure Disk Encryption. Para obter mais informações sobre a chave de encriptação de chave, consulte [pré-requisitos do Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md).

## <a name="can-i-use-an-on-premises-key-management-service-or-hsm-to-safeguard-the-encryption-keys"></a>Posso utilizar um serviço de gestão de chaves no local ou o HSM para salvaguardar as chaves de encriptação?

Não é possível utilizar o serviço de gestão de chaves no local ou o HSM para salvaguardar as chaves de encriptação com o Azure Disk Encryption. Só pode utilizar o serviço Azure Key Vault para salvaguardar as chaves de encriptação. Para obter mais informações sobre os cenários de suporte de chave de encriptação de chave, consulte [pré-requisitos do Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md).

## <a name="what-are-the-prerequisites-to-configure-azure-disk-encryption"></a>Quais são os pré-requisitos para configurar a encriptação de disco do Azure?

Existem pré-requisitos para a encriptação de disco do Azure. Consulte a [pré-requisitos do Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md) artigo para criar uma aplicação do Azure Active Directory, criar um novo cofre de chaves, ou configurar um cofre de chaves existente para o acesso de encriptação de disco ativar a encriptação e salvaguardar segredos e chaves. Para obter mais informações sobre os cenários de suporte de chave de encriptação de chave, consulte [descrição geral do Azure Disk Encryption](azure-security-disk-encryption-overview.md).

## <a name="where-can-i-get-more-information-on-how-to-use-powershell-for-configuring-azure-disk-encryption"></a>Onde posso obter mais informações sobre como utilizar o PowerShell para configurar a encriptação de disco do Azure?

Há alguns artigos sobre como executar tarefas básicas do Azure Disk Encryption, bem como cenários mais avançados. Para as tarefas básicas, consulte [explorar o Azure Disk Encryption com o Azure PowerShell – parte 1](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/explore-azure-disk-encryption-with-azure-powershell/). Para cenários mais avançados, consulte [explorar o Azure Disk Encryption com o Azure PowerShell – parte 2](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2/).

## <a name="what-version-of-azure-powershell-does-azure-disk-encryption-support"></a>Qual a versão do Azure PowerShell suporta o Azure Disk Encryption?

Utilize a versão mais recente do SDK do PowerShell do Azure para configurar o Azure Disk Encryption. Baixe a versão mais recente do [do Azure PowerShell](https://github.com/Azure/azure-powershell/releases). É o Azure Disk Encryption *não* suportados pelo SDK do Azure versão 1.1.0.

> [!NOTE]
> A extensão de pré-visualização de encriptação de disco do Linux do Azure foi preterida. Para obter detalhes, consulte [extensão de pré-visualização do Azure descontinuar disco encryption para VMs de IaaS Linux](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/12/deprecating-azure-disk-encryption-preview-extension-for-linux-iaas-vms/).

## <a name="can-i-apply-azure-disk-encryption-on-my-custom-linux-image"></a>Pode aplicar encriptação de disco do Azure na minha imagem personalizada do Linux?

Não é possível aplicar o Azure Disk Encryption em sua imagem personalizada do Linux. São suportadas apenas as imagens de Linux de galeria para as distribuições suportadas descritas anteriormente. Imagens do Linux personalizadas não são atualmente suportadas.

## <a name="can-i-apply-updates-to-a-linux-red-hat-vm-that-uses-the-yum-update"></a>Pode aplicar atualizações para uma VM do Linux Red Hat que utiliza a atualização de yum?

Sim, pode efetuar uma atualização ou uma VM do Red Hat Linux de patches. Para obter mais informações, consulte [aplicar atualizações ao encriptados VM do Azure IaaS Red Hat, utilizando a atualização de yum](https://blogs.msdn.microsoft.com/azuresecurity/2017/07/13/applying-updates-to-a-encrypted-azure-iaas-red-hat-vm-using-yum-update/).

## <a name="what-is-the-recommended-azure-disk-encryption-workflow-for-linux"></a>O que é o fluxo de trabalho de encriptação de disco do Azure recomendado para o Linux?

O seguinte fluxo de trabalho é recomendado ter os melhores resultados no Linux:
* Começar a partir da imagem de galeria sem modificações de stock correspondente para a distribuição de SO pretendida e a versão
* Cópia de segurança de quaisquer unidades montadas que serão encriptadas.  Isto permite a recuperação se ocorrer uma falha, por exemplo, se a VM é reiniciada antes de encriptação foi concluída.
* Encriptar (pode demorar várias horas ou até mesmo dias dependendo características da vm e o tamanho de quaisquer discos de dados anexados)
* Personalizar e adicione software à imagem, conforme necessário.

Se este fluxo de trabalho não for possível, contando [Storage Service Encryption](../storage/common/storage-service-encryption.md) (SSE) no armazenamento de plataforma camada da conta pode ser uma alternativa para encriptação de disco completa com dm-crypt.

## <a name="what-is-the-disk-bek-volume-or-mntazurebekdisk"></a>O que é o disco "Bek Volume" ou "/ mnt/azure_bek_disk"?

"Bek volume" para o Windows ou "/ mnt/azure_bek_disk" para Linux é um volume de dados local que armazena em segurança as chaves de encriptação para as VMs de IaaS do Azure encriptadas.
> [!NOTE]
> Não elimine ou edite qualquer conteúdo neste disco. Não desmonte o disco, uma vez que a presença de chaves de encriptação é necessária para quaisquer operações de encriptação na IaaS VM.

## <a name="where-can-i-go-to-ask-questions-or-provide-feedback"></a>Onde posso ir para colocar questões ou comentários?

Pode fazer perguntas ou enviar comentários sobre o [fórum do Azure Disk Encryption](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureDiskEncryption).

## <a name="next-steps"></a>Passos Seguintes
Neste documento, ficou a saber mais sobre as perguntas mais frequentes relacionadas com o Azure Disk Encryption. Para obter mais informações sobre este serviço e as respetivas capacidades, consulte os artigos seguintes:

- [Aplicar encriptação de disco no Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-apply-disk-encryption)
- [Encriptar uma máquina virtual do Azure](https://docs.microsoft.com/azure/security-center/security-center-disk-encryption)
- [Encriptação de dados do Azure em repouso](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
