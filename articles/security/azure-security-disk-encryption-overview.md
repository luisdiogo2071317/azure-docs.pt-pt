---
title: Azure Disk Encryption para descrição geral de VMs de IaaS | Documentos da Microsoft
description: Este artigo fornece uma visão geral do Microsoft Azure Disk Encryption para VMs de IaaS.
author: mestew
ms.service: security
ms.subservice: Azure Disk Encryption
ms.topic: article
ms.author: mstewart
ms.date: 09/10/2018
ms.openlocfilehash: ea72e4ac778e52d4f6e4f5597d38a1de59ba43fc
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348944"
---
# <a name="azure-disk-encryption-for-iaas-vms"></a>Azure Disk Encryption para IaaS VMs 
O Microsoft Azure é o compromisso de garantir que a privacidade dos dados, soberania de dados e permite-lhe controlo alojados no seu Azure dados através de uma variedade de tecnologias avançadas de encriptar, controlar e gerir chaves de encriptação e controlo e auditoria de acesso de dados. Esse controle fornece aos clientes Azure a flexibilidade de escolher a solução mais adequada às suas necessidades de negócios. Este artigo apresenta uma solução de tecnologia, o "Disco de encriptação para Windows e Linux VMs IaaS do Azure", para ajudar a proteger e salvaguardar os seus dados para satisfazer os seus compromissos de conformidade e segurança organizacionais. 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Descrição geral
Encriptação de disco do Azure (ADE) é uma funcionalidade que ajuda-o a encriptar discos da máquina virtual Windows e Linux IaaS. ADE tira partido da norma da indústria [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) recurso do Windows e o [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funcionalidade do Linux para fornecer encriptação de volume para os discos de SO e dados. A solução está integrada [do Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para o ajudar a controlar e gerir as chaves de encriptação de disco e segredos. A solução também garante que todos os dados nos discos de máquina virtual são encriptados em inatividade no armazenamento do Azure.

Azure disk encryption para VMs de IaaS de Linux e do Windows está em **disponibilidade geral** em todas as regiões públicas do Azure e regiões de AzureGov para VMs padrão e as VMs com o armazenamento premium. Quando aplica a solução de gerenciamento de encriptação de disco do Azure, pode satisfazer as necessidades de negócio seguintes:

* VMs de IaaS são protegidas em descanso ao utilizar a tecnologia de encriptação de norma da indústria para segurança organizacionais e requisitos de conformidade.
* Arranque de VMs de IaaS em chaves controlado pelo cliente e as políticas e pode auditar a utilização no seu Cofre de chaves.


Se utilizar o Centro de segurança do Azure, ele irá alertá-lo se tiver máquinas virtuais que não estão encriptadas. Estes alertas serão apresentados como de Gravidade Alta e a recomendação é de encriptar estas máquinas virtuais.
![Alerta de encriptação de disco do Centro de segurança do Azure](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Algumas recomendações podem aumentar de dados, a rede ou a utilização de recursos de computação, resultando em custos adicionais de licença ou subscrição.


## <a name="encryption-scenarios"></a>Cenários de encriptação
A solução Azure Disk Encryption suporta os seguintes cenários de cliente:

* Ativar a encriptação em novas VMs de IaaS do Windows criados a partir de encriptadas VHD e chaves de encriptação 
* Ativar a encriptação em novas VMs de IaaS criados a partir de imagens de galeria do Azure suportadas
* Ativar a encriptação em VMs de IaaS existentes em execução no Azure
* Ativar a encriptação em conjuntos de dimensionamento de máquinas virtuais do Windows
* Ativar a encriptação em unidades de dados para conjuntos de dimensionamento de máquinas virtuais do Linux
* Desativar a encriptação em VMs de IaaS do Windows
* Desativar a encriptação em unidades de dados para VMs de IaaS Linux
* Desativar a encriptação em conjuntos de dimensionamento de máquinas virtuais do Windows
* Desativar a encriptação em unidades de dados para conjuntos de dimensionamento de máquinas virtuais do Linux
* Ativar a encriptação de disco gerido de VMs
* Definições de encriptação de atualização de um existente premium encriptado e não premium armazenamento VM
* Cópia de segurança e restauro de VMs encriptadas

A solução suporta os seguintes cenários para IaaS VMs quando estão ativadas no Microsoft Azure:

* Integração com o Cofre de chaves do Azure
* Escalão Standard VMs: [A, D, DS, G, GS, F e assim por diante série IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)
    * [VMs do Linux](azure-security-disk-encryption-faq.md#bkmk_LinuxOSSupport) dentro estas camadas têm de cumprir o requisito de memória mínima de 7 GB
* Ativar a encriptação no Windows e VMs do Linux IaaS, disco gerido e dimensionamento de VMs do conjunto de imagens de galeria do Azure suportadas
* VMs do conjunto de encriptação de desativar em unidades de SO e dados para VMs de IaaS do Windows, dimensionamento e gerido de VMs de disco
* VMs do conjunto de encriptação de desativar em unidades de dados para VMs de IaaS Linux, dimensionamento e gerido de VMs de disco
* Ativar a encriptação em VMs de IaaS com o sistema operativo de cliente do Windows
* Ativar a encriptação em volumes com caminhos de montagem
* Ativar a encriptação em VMs do Linux configurado com o disco com mdadm repartição (RAID)
* Ativar a encriptação em VMs do Linux com LVM para discos de dados
* Ativar a encriptação em discos de dados e SO de VM do Linux 
* Ativar a encriptação em VMs do Windows configurado com espaços de armazenamento
* Definições de encriptação de atualização de um existente premium encriptado e não premium armazenamento VM
* Cópia de segurança e restauro de VMs encriptadas, para não-KEK e cenários KEK (KEK - chave de encriptação de chave)
* Todos os públicos do Azure e AzureGov regiões são suportadas

A solução não suporta os seguintes cenários, recursos e tecnologia:

* VMs de IaaS de escalão básico
* A desativação da encriptação numa unidade do sistema operacional para VMs de IaaS Linux
* A desativação da encriptação numa unidade de dados se a unidade do SO estiver encriptada para VMs de Iaas Linux
* VMs de IaaS que são criadas com o método de criação de VM clássico
* Ativar a encriptação em imagens personalizadas de cliente de VMs de IaaS Linux
* Integração com o seu serviço de gestão de chaves no local
* Ficheiros do Azure (sistema de ficheiros partilhados), o sistema de ficheiros de rede (NFS), volumes dinâmicos e VMs do Windows que estão configurados com sistemas RAID baseados em software

## <a name="encryption-features"></a>Recursos de criptografia
Quando ativa e implementa o Azure Disk Encryption para VMs IaaS do Azure, as seguintes funcionalidades estão ativadas, consoante a configuração fornecida:

* Encriptação do volume de sistema operacional para proteger o volume de arranque em repouso no armazenamento do
* Encriptação de volumes de dados para proteger os volumes de dados Inativos no seu armazenamento
* A desativação da encriptação nas unidades de SO e dados para VMs de IaaS do Windows
* Desativar a encriptação em unidades de dados para VMs de IaaS Linux (apenas se a unidade do SO não é encriptada)
* Salvaguardar as chaves de encriptação e os segredos na sua subscrição do Cofre de chaves
* Relatório de status de encriptação de VM de IaaS encriptada
* Remoção das definições de configuração de encriptação de disco da máquina virtual IaaS
* Cópia de segurança e restauro de VMs encriptadas com o serviço de cópia de segurança do Azure

Inclui o Azure Disk Encryption para VMS de IaaS para a solução do Windows e Linux:

* A extensão de encriptação de disco para o Windows.
* A extensão de encriptação de disco para Linux.
* A encriptação de disco, cmdlets do PowerShell.
* A encriptação de disco, os cmdlets de interface de linha de comandos (CLI) do Azure.
* Os modelos Azure Resource Manager de encriptação de disco.

A solução Azure Disk Encryption é suportada em VMs de IaaS que estão a executar o Windows ou o SO Linux. Para obter mais informações sobre os sistemas operativos suportados, consulte a [pré-requisitos](azure-security-disk-encryption-prerequisites.md) artigo.

> [!NOTE]
> Não é um valor adicional para encriptar discos da VM com o Azure Disk Encryption. Standard [preços do Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) aplica-se ao Cofre de chaves utilizado para armazenar as chaves de encriptação. 


## <a name="encryption-workflow"></a>Fluxo de trabalho de encriptação

 Para ativar a encriptação de disco para o Windows e VMs do Linux, siga os passos abaixo:

1. Escolha um cenário de criptografia entre os cenários de encriptação anteriores.
2. Optar por ativar a encriptação através do modelo de encriptação de disco do Azure Resource Manager, cmdlets do PowerShell ou comando da CLI de disco e especificar a configuração de encriptação.

   * Para o cenário VHD encriptados de cliente, carrega o VHD encriptado para sua conta de armazenamento e o material de chave de encriptação para o seu Cofre de chaves. Em seguida, indique a configuração de criptografia para ativar a encriptação numa nova VM de IaaS.
   * Para novas VMs que são criadas a partir do Marketplace e as VMs existentes que já estão em execução no Azure, indique a configuração de criptografia para ativar a encriptação na IaaS VM.

3. Conceder acesso, a plataforma do Azure para ler o material de chave de encriptação (chaves de encriptação de disco BitLocker para sistemas Windows) e a frase de acesso para Linux a partir do seu Cofre de chaves para ativar a encriptação na IaaS VM.

4. O Azure atualiza o modelo de serviço VM com a criptografia, a configuração de Cofre de chaves e configura a sua VM encriptada.

 ![Microsoft Antimalware no Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Fluxo de trabalho de desencriptação
Para desativar a encriptação de disco para IaaS VMs, conclua os seguintes passos de alto nível:

1. Optar por desativar a encriptação (desencriptação) numa VM de IaaS em execução no Azure e especificar a configuração de desencriptação. Pode desativar através do modelo de encriptação de disco do Azure Resource Manager, cmdlets do PowerShell ou da CLI do Azure.

 Este passo desativa a encriptação de sistema operacional ou o volume de dados ou ambos na VM de IaaS do Windows em execução. No entanto, conforme mencionado na secção anterior, a desativação da encriptação de disco de SO para Linux não é suportada. O passo de desencriptação só é permitido para unidades de dados em VMs do Linux, desde que o disco de SO não está encriptado.
2. Azure atualiza o modelo de serviço VM e a VM de IaaS está marcada desencriptada. O conteúdo da VM já não é encriptado em inatividade.

> [!NOTE]
> A operação de criptografia de Desativação não elimina o seu Cofre de chaves e o material de chave de encriptação (chaves de encriptação de disco BitLocker para sistemas Windows) ou a frase de acesso para Linux.
 > Não é suportada a desativação da encriptação de disco de SO do Linux. O passo de desencriptação só é permitido para unidades de dados em VMs do Linux.
A desativação da encriptação de disco de dados para Linux não é suportada se a unidade do SO estiver encriptada.


## <a name="encryption-workflow-previous-release"></a>Fluxo de trabalho de encriptação (versão anterior)

A nova versão de encriptação de disco do Azure elimina o requisito para fornecer um parâmetro de aplicação do Azure AD para ativar a encriptação de disco da VM. Com a nova versão, já não são necessários para fornecer uma credencial do Azure AD durante o passo da criptografia de ativação. Todas as novas VMs tem de estar encriptadas sem os parâmetros da aplicação do Azure AD com a nova versão. As VMs que já foram encriptadas com parâmetros de aplicação do Azure AD ainda são suportadas e devem continuar a ser mantido com a sintaxe do AAD. Para ativar a encriptação de disco para o Windows e VMs do Linux (versão anterior), siga os passos abaixo:

1. Escolha um cenário de criptografia entre os cenários de encriptação anteriores.
2. Optar por ativar a encriptação através do modelo de encriptação de disco do Azure Resource Manager, cmdlets do PowerShell ou comando da CLI de disco e especificar a configuração de encriptação.

   * Para o cenário VHD encriptados de cliente, carrega o VHD encriptado para sua conta de armazenamento e o material de chave de encriptação para o seu Cofre de chaves. Em seguida, indique a configuração de criptografia para ativar a encriptação numa nova VM de IaaS.
   * Para novas VMs que são criadas a partir do Marketplace e as VMs existentes que já estão em execução no Azure, indique a configuração de criptografia para ativar a encriptação na IaaS VM.

3. Conceder acesso, a plataforma do Azure para ler o material de chave de encriptação (chaves de encriptação de disco BitLocker para sistemas Windows) e a frase de acesso para Linux a partir do seu Cofre de chaves para ativar a encriptação na IaaS VM.

4. Fornece a identidade de aplicação do Azure Active Directory (Azure AD) para escrever a chave de encriptação material para o seu Cofre de chaves. Se o fizer, ativa a encriptação na VM IaaS para os cenários mencionados no passo 2.

5. O Azure atualiza o modelo de serviço VM com a encriptação e a configuração de Cofre de chaves e configura a sua VM encriptada.


## <a name="terminology"></a>Terminologia
Para compreender alguns dos termos comuns utilizados por essa tecnologia, utilize a seguinte tabela de terminologia:

| Terminologia | Definição |
| --- | --- |
| Azure AD | O Azure AD é [do Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Uma conta do Azure AD é utilizada para autenticar, armazenar e obter segredos a partir de um cofre de chaves. |
| Azure Key Vault | O Key Vault é um serviço de gestão de criptografia, chave com base no Federal Information Processing Standards (FIPS) validados módulos de segurança de hardware, que ajudam a salvaguardar as chaves criptográficas e segredos sensíveis. Para obter mais informações, consulte [Key Vault](https://azure.microsoft.com/services/key-vault/) documentação. |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) é uma reconhecidas no setor Windows volume tecnologia de encriptação que é utilizada para ativar a encriptação de disco em VMs de IaaS do Windows. |
| BEK | Chaves de encriptação do BitLocker são usadas para criptografar o volume de arranque do sistema operacional e os volumes de dados. As chaves do BitLocker são salvaguardadas num cofre de chaves como segredos. |
| CLI | Ver [interface de linha de comandos do Azure](/cli/azure/install-azure-cli).|
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) é o subsistema de encriptação de disco baseado em Linux, transparente, que é utilizado para ativar a encriptação de disco em VMs do Linux IaaS. |
| KEK | Chave de encriptação de chave é a chave assimétrica (RSA 2048) que pode utilizar para proteger ou encapsular o segredo. Pode fornecer um módulo de segurança de hardware (HSM)-chave ou chave protegida por software protegidos. Para obter mais informações, consulte [do Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentação. |
| PS cmdlets | Ver [cmdlets do Azure PowerShell](/powershell/azure/overview). |

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Pré-requisitos do Azure Disk Encryption](azure-security-disk-encryption-prerequisites.md)
