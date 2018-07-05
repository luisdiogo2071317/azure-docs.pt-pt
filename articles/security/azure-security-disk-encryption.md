---
title: Azure Disk Encryption para VMs de Linux IaaS e do Windows | Documentos da Microsoft
description: Este artigo fornece uma visão geral do Microsoft Azure disco encriptação para Windows e VMs de IaaS Linux.
services: security
documentationcenter: na
author: DevTiw
manager: avibm
editor: barclayn
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/13/2018
ms.author: devtiw
ms.openlocfilehash: f350716d0ca906376f3eadce9e117694ff14515c
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/04/2018
ms.locfileid: "35756401"
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a>Azure Disk Encryption para VMs de Linux IaaS e do Windows
O Microsoft Azure é vivamente compromisso de garantir que a privacidade dos dados, soberania de dados e possibilita que controle alojados no seu Azure dados através de uma variedade de tecnologias para encriptar, controlar e gerir chaves de encriptação, acesso de controlo e auditoria de dados. Isso fornece aos clientes Azure a flexibilidade de escolher a solução mais adequada às suas necessidades de negócios. Neste artigo, vamos apresentará uma nova solução de tecnologia "O Azure Disk Encryption para Windows e de Linux VM de IaaS" para ajudar a proteger e salvaguardar os seus dados para satisfazer os seus compromissos de conformidade e segurança organizacionais. O documento fornece orientações detalhadas sobre como utilizar as funcionalidades de encriptação de disco do Azure incluindo os cenários suportados e o utilizador experiências.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]


## <a name="overview"></a>Descrição geral
O Azure Disk Encryption é um novo recurso que lhe permite encriptar discos da máquina virtual Windows e Linux IaaS. O Azure Disk Encryption tira partido da norma da indústria [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) recurso do Windows e o [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funcionalidade do Linux para fornecer encriptação de volume para o sistema operacional e os discos de dados. A solução está integrada [do Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) para o ajudar a controlar e gerir as chaves de encriptação de disco e segredos na sua subscrição do Cofre de chaves. A solução também garante que todos os dados nos discos de máquina virtual são encriptados em inatividade no armazenamento do Azure.

Azure disk encryption para Windows e VMs de IaaS Linux está agora na **disponibilidade geral** em todas as regiões públicas do Azure e regiões de AzureGov para VMs padrão e as VMs com o armazenamento premium.

> [!NOTE]
> Algumas recomendações podem aumentar de dados, a rede ou a utilização de recursos de computação, resultando em custos adicionais de licença ou subscrição.


### <a name="encryption-scenarios"></a>Cenários de encriptação
A solução Azure Disk Encryption suporta os seguintes cenários de cliente:

* Ativar a encriptação em novas VMs de IaaS criados a partir de VHD previamente encriptado e as chaves de encriptação
* Ativar a encriptação em novas VMs de IaaS criados a partir de imagens de galeria do Azure suportadas
* Ativar a encriptação em VMs de IaaS existentes em execução no Azure
* Desativar a encriptação em VMs de IaaS do Windows
* Desativar a encriptação em unidades de dados para VMs de IaaS Linux
* Ativar a encriptação de disco gerido de VMs
* Definições de encriptação de atualização de um existente premium encriptado e não premium armazenamento VM
* Cópia de segurança e restauro de VMs encriptadas

A solução suporta os seguintes cenários para IaaS VMs quando forem ativadas no Microsoft Azure:

* Integração com o Cofre de chaves do Azure
* Escalão Standard VMs: [A, D, DS, G, GS, F e assim por diante série IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Ativar a encriptação em Windows e VMs de IaaS Linux e VMs de disco gerido a partir das imagens da galeria do Azure suportadas
* Desativar a encriptação em unidades de SO e dados para VMs de IaaS do Windows e VMs de disco gerido
* Desativar a encriptação em unidades de dados para VMs de IaaS Linux e VMs de disco gerido
* Ativar a encriptação em VMs de IaaS com o sistema operativo de cliente do Windows
* Ativar a encriptação em volumes com caminhos de montagem
* Ativar a encriptação em VMs do Linux configurado com o disco com mdadm repartição (RAID)
* Ativar a encriptação em VMs do Linux com LVM para discos de dados
* Ativar a encriptação em Linux LVM 7.3 para discos de dados e SO 
* Ativar a encriptação em VMs do Windows configurado com espaços de armazenamento
* Definições de encriptação de atualização de um existente premium encriptado e não premium armazenamento VM
* Cópia de segurança e restauro de VMs encriptadas, para não-KEK e cenários KEK (KEK - chave de encriptação de chave)
* Todos os públicos do Azure e AzureGov regiões são suportadas

A solução não suporta os seguintes cenários, recursos e tecnologia:

* VMs de IaaS de escalão básico
* A desativação da encriptação numa unidade do sistema operacional para VMs de IaaS Linux
* A desativação da encriptação numa unidade de dados se a unidade do SO estiver encriptada para VMs de Iaas Linux
* VMs de IaaS que são criadas com o método de criação de VM clássico
* Ative a encriptação em Windows e VMs Linux IaaS em imagens personalizadas do cliente não é suportada.
* Integração com o seu serviço de gestão de chaves no local
* Ficheiros do Azure (sistema de ficheiros partilhados), o sistema de ficheiros de rede (NFS), volumes dinâmicos e VMs do Windows que estão configurados com sistemas RAID baseados em software

### <a name="encryption-features"></a>Recursos de criptografia
Quando ativa e implementa o Azure Disk Encryption para VMs IaaS do Azure, as seguintes funcionalidades estão ativadas, consoante a configuração fornecida:

* Encriptação do volume de sistema operacional para proteger o volume de arranque em repouso no armazenamento do
* Encriptação de volumes de dados para proteger os volumes de dados Inativos no seu armazenamento
* A desativação da encriptação nas unidades de SO e dados para VMs de IaaS do Windows
* A desativação da encriptação nos dados de unidades para VMs de IaaS Linux (apenas se o sistema operacional a unidade é não encriptado)
* Salvaguardar as chaves de encriptação e os segredos na sua subscrição do Cofre de chaves
* Relatório de status de encriptação de VM de IaaS encriptada
* Remoção das definições de configuração de encriptação de disco da máquina virtual IaaS
* Cópia de segurança e restauro de VMs encriptadas com o serviço de cópia de segurança do Azure

Inclui o Azure Disk Encryption para VMS de IaaS para a solução do Windows e Linux:

* A extensão de encriptação de disco para o Windows.
* A extensão de encriptação de disco para Linux.
* Os cmdlets do PowerShell de encriptação de disco.
* A disco de encriptação de cmdlets de interface de linha de comandos (CLI) do Azure.
* Os modelos do Azure Resource Manager de encriptação de disco.

A solução Azure Disk Encryption é suportada em VMs de IaaS que estão a executar o Windows ou o SO Linux. Para obter mais informações sobre os sistemas operativos suportados, consulte a secção "Pré-requisitos".

> [!NOTE]
> Não é sem custos adicionais para encriptar discos da VM com o Azure Disk Encryption.

### <a name="value-proposition"></a>Proposta de valor
Quando aplica a solução de gerenciamento de encriptação de disco do Azure, pode satisfazer as necessidades de negócio seguintes:

* VMs de IaaS são protegidas em repouso, porque pode usar a tecnologia de encriptação de norma da indústria para abordar os requisitos de segurança e conformidade organizacionais.
* Arranque de VMs de IaaS em chaves controlado pelo cliente e as políticas e pode auditar a utilização no seu Cofre de chaves.

### <a name="encryption-workflow"></a>Fluxo de trabalho de encriptação
Para ativar a encriptação de disco para VMs de Linux e do Windows, faça o seguinte:

1. Escolha um cenário de criptografia entre os cenários de encriptação anteriores.
2. Optar por ativar a encriptação através do modelo de encriptação de disco do Azure Resource Manager, cmdlets do PowerShell ou comando da CLI de disco e especificar a configuração de encriptação.

   * Para o cenário VHD encriptados de cliente, carrega o VHD encriptado para sua conta de armazenamento e o material de chave de encriptação para o seu Cofre de chaves. Em seguida, indique a configuração de criptografia para ativar a encriptação numa nova VM de IaaS.
   * Para novas VMs que são criadas a partir do Marketplace e as VMs existentes que já estão em execução no Azure, indique a configuração de criptografia para ativar a encriptação na IaaS VM.

3. Conceder acesso, a plataforma do Azure para ler o material de chave de encriptação (chaves de encriptação de disco BitLocker para sistemas Windows) e a frase de acesso para Linux a partir do seu Cofre de chaves para ativar a encriptação na IaaS VM.

4. Fornece a identidade de aplicação do Azure Active Directory (Azure AD) para escrever a chave de encriptação material para o seu Cofre de chaves. Se o fizer, ativa a encriptação na VM IaaS para os cenários mencionados no passo 2.

5. O Azure atualiza o modelo de serviço VM com a encriptação e a configuração de Cofre de chaves e configura a sua VM encriptada.

 ![Microsoft Antimalware no Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a>Fluxo de trabalho de desencriptação
Para desativar a encriptação de disco para IaaS VMs, conclua os seguintes passos de alto nível:

1. Optar por desativar a encriptação (desencriptação) numa VM de IaaS em execução no Azure, através do modelo de encriptação de disco do Azure Resource Manager ou cmdlets do PowerShell e especificar a configuração de desencriptação.

 Este passo desativa a encriptação de sistema operacional ou o volume de dados ou ambos na VM de IaaS do Windows em execução. No entanto, conforme mencionado na secção anterior, a desativação da encriptação de disco de SO para Linux não é suportada. O passo de desencriptação só é permitido para unidades de dados em VMs do Linux, desde que o disco do SO não está encriptado.
2. Azure atualiza o modelo de serviço VM e a VM de IaaS está marcada desencriptada. O conteúdo da VM já não é encriptado em inatividade.

> [!NOTE]
> A operação de criptografia de Desativação não elimina o seu Cofre de chaves e o material de chave de encriptação (chaves de encriptação de disco BitLocker para sistemas Windows) ou a frase de acesso para Linux.
 > Não é suportada a desativação da encriptação de disco de SO do Linux. O passo de desencriptação só é permitido para unidades de dados em VMs do Linux.
A desativação da encriptação de disco de dados para Linux não é suportada se a unidade do SO estiver encriptada.

## <a name="prerequisites"></a>Pré-requisitos
Antes de ativar o Azure Disk Encryption em VMs de IaaS do Azure para os cenários suportados que foram discutidos na seção "Descrição geral", consulte os seguintes pré-requisitos:

* Tem de ter uma subscrição do Azure Active Directory válida para criar recursos no Azure nas regiões suportadas.
* O Azure Disk Encryption é suportado nas seguintes versões do Windows Server: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016.
* O Azure Disk Encryption é suportado nas seguintes versões de cliente do Windows: cliente Windows 8 e o cliente do Windows 10.

> [!NOTE]
> Para o Windows Server 2008 R2, tem de ter o .NET Framework 4.5 instalado antes de ativar a encriptação no Azure. Pode instalá-lo do Windows Update através da instalação da atualização opcional Microsoft .NET Framework 4.5.2 para sistemas baseados em x64 do Windows Server 2008 R2 ([KB2901983](https://support.microsoft.com/kb/2901983)).

* O Azure Disk Encryption é apenas suportado em específico galeria do Azure com base em distribuições de servidor Linux e versões.  Para obter a lista de versões atualmente suportadas, consulte a [FAQ de encriptação de disco do Azure](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq).

* O Azure Disk Encryption requer que o Cofre de chaves e VMs residem na mesma região do Azure e subscrição.

> [!NOTE]
> Configurar os recursos em regiões separadas faz com que uma falha na ativação da funcionalidade do Azure Disk Encryption.

* Para definir e configurar o seu Cofre de chaves do Azure Disk Encryption, consulte a secção **definir a cópia de segurança e configurar o seu Cofre de chaves do Azure Disk Encryption** no *pré-requisitos* seção deste artigo.
* Para preparar e configurar a aplicação do Azure AD no Azure Active directory para a encriptação de disco do Azure, consulte a secção **configurar a aplicação do Azure AD no Azure Active Directory** no *pré-requisitos* secção Este artigo.
* Para definir e configurar a política de acesso do Cofre de chaves para a aplicação do Azure AD, consulte a secção **configurar a política de acesso do Cofre de chaves para a aplicação do Azure AD** no *pré-requisitos* seção deste artigo.
* Para preparar um VHD do Windows encriptadas, consulte a secção **preparar um VHD do Windows encriptadas** no *apêndice*.
* Para preparar um VHD do Linux encriptadas, consulte a secção **preparar um VHD do Linux encriptadas** no *apêndice*.
* A plataforma do Azure precisa de aceder a chaves de encriptação ou segredos no Cofre de chaves para que fiquem disponíveis para a máquina virtual quando ele é inicializado e desencripta o volume de sistema operacional da máquina virtual. Para conceder permissões para a plataforma do Azure, defina o **EnabledForDiskEncryption** propriedade no Cofre de chaves. Para obter mais informações, consulte **definir a cópia de segurança e configurar o seu Cofre de chaves do Azure Disk Encryption** no apêndice.
* O segredo do Cofre de chaves e URLs de KEK devem ter a versão. Azure impõe essa restrição de controle de versão. Para segredo válido e KEK URLs, consulte os exemplos seguintes:

  * Exemplo de um URL de segredo válido:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * Exemplo de um URL válido da KEK:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* O Azure Disk Encryption não suporta a especificação de números de porta como parte de segredos do Cofre de chaves e URLs de KEK. Para obter exemplos de URLs de Cofre de chaves suportadas e não suportados, consulte o seguinte:

  * URL do Cofre de chaves inaceitável  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*
  * URL do Cofre de chaves aceitável:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

* Para ativar o Azure Disk Encryption funcionalidade, as VMs de IaaS tem de cumprir os seguintes requisitos de configuração do ponto final de rede:
  * Para obter um token para ligar ao seu Cofre de chaves, a VM de IaaS tem de ser capaz de ligar a um ponto de extremidade do Azure Active Directory \[login.microsoftonline.com\].
  * Para escrever as chaves de encriptação para o seu Cofre de chaves, a VM de IaaS tem de conseguir ligar ao ponto final do Cofre de chaves.
  * A VM de IaaS tem de ser capaz de ligar a um ponto de extremidade de armazenamento do Azure que aloja o repositório de extensão do Azure e uma conta de armazenamento do Azure que aloja os ficheiros VHD.

  > [!NOTE]
  > Se a política de segurança limita o acesso a partir de VMs do Azure para a Internet, pode resolver o URI anterior e configurar uma regra específica para permitir a conectividade de saída para os IPs.
  >
  >Para configurar e aceder ao Azure Key Vault por trás de um firewall (https://docs.microsoft.com/azure/key-vault/key-vault-access-behind-firewall)

* Utilize a versão mais recente da versão do SDK do Azure PowerShell para configurar o Azure Disk Encryption. Baixe a versão mais recente do [versão do Azure PowerShell](https://github.com/Azure/azure-powershell/releases)

 > [!NOTE]
  > O Azure Disk Encryption não é suportado no [Azure PowerShell SDK versão 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016). Se estiver a receber um erro relacionado com a com o Azure PowerShell 1.1.0, consulte [Azure disco encriptação erro relacionados para o Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).

* Para executar qualquer comando da CLI do Azure e associá-lo a sua subscrição do Azure, primeiro tem de instalar o CLI do Azure:
  * Para instalar a CLI do Azure e associá-lo a sua subscrição do Azure, veja [como instalar e configurar a CLI do Azure](../cli-install-nodejs.md).
  * Para utilizar a CLI do Azure para Mac, Linux e Windows com o Azure Resource Manager, consulte [comandos da CLI do Azure no modo Resource Manager](../virtual-machines/azure-cli-arm-commands.md).

* Quando encriptar um disco gerido, é obrigatório pré-requisito para tirar um instantâneo do disco gerido ou uma cópia de segurança do disco fora do Azure Disk Encryption antes de ativar a encriptação.  Sem uma cópia de segurança em vigor, qualquer falha inesperada durante a encriptação pode ser apresentado o disco e a VM inacessíveis sem uma opção de recuperação.  Set-AzureRmVMDiskEncryptionExtension atualmente não fazer cópias de segurança de discos geridos e emite um erro se utilizada no âmbito de um disco gerido, a menos que o parâmetro - skipVmBackup foi especificado.  Este parâmetro é seguro utilizar, a menos que já foi efetuada uma cópia de segurança fora do Azure Disk Encryption.   Quando o parâmetro - skipVmBackup é especificado, o cmdlet não fará uma cópia de segurança do disco gerido antes de encriptação.  Por esse motivo, é considerado um pré-requisito obrigatório certificar-se de que uma cópia de segurança do disco gerido a VM está em vigor antes de ativar o Azure Disk Encryption no caso de recuperação é necessária mais tarde.  
> [!NOTE]
 > O parâmetro - skipVmBackup nunca deve ser usado, a menos que um instantâneo ou a cópia de segurança já foi efetuada fora do Azure Disk Encryption. 

* A solução Azure Disk Encryption utiliza o protetor de chave externo do BitLocker para VMs de IaaS do Windows. Para o domínio associado ao VMs, não enviar por push as políticas de grupo que impõem protetores TPM. Para informações sobre a política de grupo para "Permitir BitLocker sem um TPM compatível", consulte [referência de política de grupo de BitLocker](https://technet.microsoft.com/library/ee706521).
* Política do BitLocker em máquinas de virtuais associados a um domínio com a política de grupo personalizado tem de incluir a seguinte definição: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption irá falhar quando as definições de política de grupo personalizado para o Bitlocker são incompatíveis. Nas máquinas que não tinha a política correta a definição, a nova política, forçando a nova política de atualização (gpupdate.exe /force) e, em seguida, reiniciar poderão ser necessários.  
* Para criar uma aplicação do Azure AD, crie um cofre de chaves, ou configurar um cofre de chaves existente e ativar a encriptação, consulte a [script do PowerShell de pré-requisitos do Azure Disk Encryption](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).
* Para configurar os pré-requisitos de encriptação de disco com a CLI do Azure, veja [este script de Bash](https://github.com/ejarvi/ade-cli-getting-started).
* Para utilizar o serviço de cópia de segurança do Azure para criar cópias de segurança e restaurar VMs encriptadas, quando a encriptação está ativada com o Azure Disk Encryption, encripte as suas VMs com a configuração de chave de encriptação de disco do Azure. O serviço de cópia de segurança suporta VMs que são encriptadas com a KEK não ou configurações de KEK. Ver [encriptados de como fazer cópias de segurança e restaurar máquinas virtuais com a encriptação de cópia de segurança do Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption).

* Quando encriptar um volume do SO Linux, tenha em atenção que é atualmente necessário no final do processo de um reinício VM. Isso pode ser feito através do portal, o powershell ou a CLI.   Para acompanhar o progresso de criptografia, consultar periodicamente a mensagem de estado devolvida pelo Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.  Assim que a criptografia estiver concluída, a mensagem de estado devolvida por este comando irá indicar isto. Por exemplo, "ProgressMessage: disco do SO encriptados com êxito, reinicie a VM" nesse ponto, a VM pode ser reiniciada e utilizada.  

* O Azure Disk Encryption para o Linux requer discos de dados para ter um sistema de ficheiros instalado no Linux antes de encriptação

* Recursivamente montadas dados discos não são suportados pelo Azure Disk Encryption para Linux. Por exemplo, se o sistema de destino tem montado um disco na barra/foo/e, em seguida, outra em /foo/bar/baz, a criptografia de /foo/bar/baz será concluída com êxito, mas como a encriptação da barra/foo/irá falhar. 

* O Azure Disk Encryption apenas é suportado nas imagens da galeria do Azure suportada que cumprem os pré-requisitos mencionados anteriormente. Imagens personalizadas do cliente não são suportadas devido a esquemas de partição personalizados e comportamentos de processo que possam existir nessas imagens. Além disso, até mesmo Galeria baseada em imagem da VM que inicialmente cumpridos os pré-requisitos, mas que foram modificada após a criação poderão não ser compatíveis.  Para que o motivo, o procedimento sugerido para encriptar uma VM do Linux é de começar a partir de uma imagem de galeria limpa, encriptar a VM e, em seguida, adicionar software personalizado ou dados para a VM conforme necessário.  

* O Azure Disk Encryption e volume de dados local - Bek Volume para Windows e/mnt/azure_bek_disk para VMs de IaaS Linux armazenar em segurança a chave de encriptação. Não elimine ou edite qualquer conteúdo neste disco. Não desmonte o disco, uma vez que a presença de chaves de encriptação é necessária para quaisquer operações de encriptação na IaaS VM. Arquivo Leiame incluído no volume contém detalhes adicionais.

#### <a name="set-up-the-azure-ad-application-in-azure-active-directory"></a>Configurar a aplicação do Azure AD no Azure Active Directory
Quando precisar de criptografia para ser ativada numa VM em execução no Azure, o Azure Disk Encryption gera e escreve as chaves de encriptação para o seu Cofre de chaves. A gestão de chaves de encriptação no seu Cofre de chaves requer autenticação do Azure AD.

Para essa finalidade, crie uma aplicação do Azure AD. Pode encontrar passos detalhados para registar uma aplicação na secção "Obter uma identidade para a aplicação" a mensagem de blogue [do Azure Key Vault - passo a passo](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Esta mensagem também contém um número de exemplos úteis para definir e configurar o seu Cofre de chaves. Para fins de autenticação, pode utilizar a autenticação com base no segredo do cliente ou a autenticação de cliente baseada em certificado do Azure AD.

#### <a name="client-secret-based-authentication-for-azure-ad"></a>Autenticação com base no segredo do cliente para o Azure AD
As secções que se seguem podem ajudá-lo a configurar uma autenticação com base no segredo de cliente para o Azure AD.

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a>Criar uma aplicação do Azure AD com o Azure PowerShell
Utilize o seguinte cmdlet do PowerShell para criar uma aplicação do Azure AD:

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> $azureAdApplication.ApplicationId é o ID de cliente do Azure AD e $aadClientSecret é o segredo de cliente que deve usar mais tarde para ativar a encriptação de disco do Azure. Proteger adequadamente o segredo do cliente do Azure AD.

##### <a name="setting-up-the-azure-ad-client-id-and-secret-from-the-azure-portal"></a>Como configurar o ID de cliente do Azure AD e o segredo do portal do Azure
Pode também configurar seu ID de cliente do Azure AD e o segredo com o Portal do Azure. Para executar essa tarefa, faça o seguinte:

1. Selecione **todos os serviços > o Azure Active Directory**

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-service.png)

2. Selecione **registos de aplicações > novo registo de aplicação**

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-app-registration.png)

3. Forneça as informações pedidas e criar a aplicação:

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-create-app.png)

4. Selecione a aplicação criada recentemente para ver as respetivas propriedades, incluindo a ID da aplicação.  Para criar uma chave para a aplicação, selecione **definições > chaves**, adicione uma descrição e a expiração da chave e clique em **guardar**

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-create-pw.png)

5. Copie o valor secreto gerado e salvaguardá-la adequadamente.

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/aad-save-pw.png)


##### <a name="use-an-existing-application"></a>Utilizar uma aplicação existente
Para executar os comandos seguintes, obter e utilizar o [módulo do Azure AD PowerShell](https://technet.microsoft.com/library/jj151815.aspx).

> [!NOTE]
> Os seguintes comandos tem de ser executados a partir de uma nova janela do PowerShell. Não utilize o Azure PowerShell ou a janela do Azure Resource Manager para executar os comandos. Recomendamos que essa abordagem porque estes cmdlets no módulo MSOnline ou Azure AD PowerShell.

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a>Autenticação baseada em certificado para o Azure AD
> [!NOTE]
> Autenticação baseada em certificados. do Azure AD não é atualmente suportada em VMs do Linux.

As secções que se seguem mostram como configurar uma autenticação baseada em certificado para o Azure AD.

##### <a name="create-an-azure-ad-application"></a>Criar uma aplicação do Azure AD
Para criar uma aplicação do Azure AD, execute os seguintes cmdlets do PowerShell:

> [!NOTE]
> Substitua o seguinte `yourpassword` cadeia com a palavra-passe segura e salvaguardar a palavra-passe.

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

Depois de concluir este passo, carregar um ficheiro PFX para o seu Cofre de chaves e ativar a política de acesso necessária para implementar o certificado a uma VM.

##### <a name="use-an-existing-azure-ad-application"></a>Utilizar uma aplicação do Azure AD existente
Se estiver a configurar a autenticação baseada em certificado para uma aplicação existente, utilize os cmdlets do PowerShell mostrados aqui. Certifique-se de que executá-los a partir de uma nova janela do PowerShell.

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

Depois de concluir este passo, carregar um ficheiro PFX para o seu Cofre de chaves e ativar a política de acesso que é necessário para implementar o certificado a uma VM.

##### <a name="upload-a-pfx-file-to-your-key-vault"></a>Carregar um ficheiro PFX para o seu Cofre de chaves
Para obter uma explicação detalhada desse processo, consulte [o oficial do Azure Key Vault Blog da Equipe](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx). No entanto, os seguintes cmdlets do PowerShell são tudo o que precisa para a tarefa. Certifique-se de que executá-los a partir da consola do Azure PowerShell.

> [!NOTE]
> Substitua o seguinte `yourpassword` cadeia com a palavra-passe segura e salvaguardar a palavra-passe.

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-your-key-vault-to-an-existing-vm"></a>Implemente um certificado no seu Cofre de chaves de uma VM existente
Depois de concluir a carregar o PFX, implemente um certificado no Cofre de chaves para uma VM existente com o seguinte:
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-the-key-vault-access-policy-for-the-azure-ad-application"></a>Configurar a política de acesso do Cofre de chaves para a aplicação do Azure AD
A aplicação do Azure AD tem direitos de acesso a chaves ou segredos no cofre. Utilize o [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet para conceder permissões para a aplicação, com o ID de cliente (o que foi gerado quando o aplicativo foi registrado) como a _– ServicePrincipalName_ valor do parâmetro. Para obter mais informações, consulte a mensagem de blogue [do Azure Key Vault - passo a passo](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx). Eis um exemplo de como realizar esta tarefa através do PowerShell:

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> O Azure Disk Encryption exige que configure as seguintes políticas de acesso à sua aplicação de cliente do Azure AD: _WrapKey_ e _definir_ permissões.

## <a name="terminology"></a>Terminologia
Para compreender alguns dos termos comuns utilizados por essa tecnologia, utilize a seguinte tabela de terminologia:

| Terminologia | Definição |
| --- | --- |
| Azure AD | O Azure AD é [do Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Uma conta do Azure AD é um pré-requisito para autenticar, armazenar e obter segredos a partir de um cofre de chaves. |
| Azure Key Vault | Key Vault é um serviço de gestão chave criptográfica que se baseia em módulos de segurança de hardware validados por Federal Information Processing Standards FIPS, ajudar a salvaguardar as chaves criptográficas e segredos sensíveis. Para obter mais informações, consulte [Key Vault](https://azure.microsoft.com/services/key-vault/) documentação. |
| ARM | Azure Resource Manager |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) é uma reconhecidas no setor Windows volume tecnologia de encriptação que é utilizada para ativar a encriptação de disco em VMs de IaaS do Windows. |
| BEK | Chaves de encriptação do BitLocker são usadas para criptografar o volume de arranque do sistema operacional e os volumes de dados. As chaves do BitLocker são salvaguardadas num cofre de chaves como segredos. |
| CLI | Ver [interface de linha de comandos do Azure](../cli-install-nodejs.md). |
| DM-Crypt |[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) é o subsistema de encriptação de disco baseado em Linux, transparente, que é utilizado para ativar a encriptação de disco em VMs do Linux IaaS. |
| KEK | Chave de encriptação de chave é a chave assimétrica (RSA 2048) que pode utilizar para proteger ou encapsular o segredo. Pode fornecer uma segurança de hardware (HSM) de módulos-chave ou chave protegida por software protegidos. Para obter mais detalhes, consulte [do Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentação. |
| PS cmdlets | Ver [cmdlets do Azure PowerShell](/powershell/azure/overview). |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a>Definir e configurar o seu Cofre de chaves do Azure Disk Encryption
O Azure Disk Encryption ajuda a salvaguardar a encriptação de disco chaves e segredos no Cofre de chaves. Para configurar o seu Cofre de chaves do Azure Disk Encryption, conclua os passos em cada uma das seguintes secções.

#### <a name="create-a-key-vault"></a>Criar um cofre de chaves
Para criar um cofre de chaves, utilize uma das seguintes opções:

* ["101-Key-Vault-Create" modelo do Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [Cmdlets do Cofre de chaves do Azure PowerShell](/powershell/module/azurerm.keyvault/#key_vault)
* Azure Resource Manager
* Como [proteger o seu Cofre de chave](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)

> [!NOTE]
> Se já configurou um cofre de chaves para a sua subscrição, avance para a secção seguinte.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a>Configurar uma chave de encriptação de chave (opcional)
Se pretender utilizar um KEK para uma camada adicional de segurança para as chaves de encriptação do BitLocker, adicione um KEK ao seu Cofre de chaves. Utilize o [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet para criar uma chave de encriptação de chave no Cofre de chaves. Também pode importar um KEK de sua gestão de chaves HSM no local. Para obter mais detalhes, consulte [documentação do Cofre de chave](https://azure.microsoft.com/documentation/services/key-vault/).

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

Pode adicionar a KEK ao aceder ao Azure Resource Manager ou utilizando a interface do seu Cofre de chaves.

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a>Definir permissões do Cofre de chaves
A plataforma do Azure precisa de aceder a chaves de encriptação ou segredos no Cofre de chaves para que fiquem disponíveis para a VM para o arranque e a desencriptação de volumes. Para conceder permissões para a plataforma do Azure, defina o **EnabledForDiskEncryption** propriedade na chave do cofre com o cmdlet PowerShell do Cofre de chaves:

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

Também pode definir o **EnabledForDiskEncryption** propriedade visitando o [Explorador de recursos do Azure](https://resources.azure.com).

Como mencionado anteriormente, tem de definir o **EnabledForDiskEncryption** propriedade no seu Cofre de chaves. Caso contrário, a implementação irá falhar.

Pode definir políticas de acesso para a sua aplicação do Azure AD a partir da interface do Cofre de chaves, conforme mostrado aqui:

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

Sobre o **políticas de acesso avançadas** separador, certifique-se de que o seu Cofre de chaves está ativado para a encriptação de disco do Azure:

![Cofre de chaves do Azure](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a>Cenários de implementação de encriptação de disco e experiências de usuário
Pode ativar vários cenários de encriptação de disco e os passos podem variar de acordo com o cenário. As secções seguintes abrangem os cenários mais detalhadamente.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-the-marketplace"></a>Ativar a encriptação em novas VMs de IaaS que são criados a partir do Marketplace
Pode ativar a encriptação de disco na nova VM do Windows de IaaS do Marketplace no Azure utilizando o [modelo do Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).

1. No modelo de início rápido do Azure, clique em **implementar no Azure**, introduza a configuração de encriptação **parâmetros** painel e clique em **OK**.

2. Selecione a subscrição, grupo de recursos, localização do grupo de recursos, os termos legais e contrato e, em seguida, clique em **criar** para ativar a encriptação numa nova VM de IaaS.

> [!NOTE]
> Este modelo cria uma nova VM do Windows criptografado que utiliza a imagem de galeria do Windows Server 2012.

Pode ativar a encriptação de disco numa nova IaaS Red Hat Linux 7.2 com uma matriz de RAID-0 de 200 GB ao utilizar isto [modelo do Resource Manager](https://aka.ms/fde-rhel). Depois de implementar o modelo, verificar o estado de encriptação da VM com o `Get-AzureRmVmDiskEncryptionStatus` cmdlet, conforme descrito na [unidades de SO de encriptação numa VM do Linux em execução](#encrypting-os-drive-on-a-running-linux-vm). Quando a máquina retorna um status de _VMRestartPending_, reinicie a VM.

A tabela seguinte lista os parâmetros de modelo do Resource Manager para novas VMs o cenário de Marketplace através do ID de cliente do Azure AD:

| Parâmetro | Descrição |
| --- | --- |
| adminUserName | Nome de utilizador de administrador para a máquina virtual. |
| adminPassword | Senha de usuário de administrador para a máquina virtual. |
| newStorageAccountName | Nome da conta do storage para armazenar dados VHDs e SO. |
| vmSize | Tamanho da VM. Atualmente, a série A Standard apenas, D e G é suportado. |
| virtualNetworkName | Nome da VNet ao qual a NIC de VM devem pertencer à. |
| subnetName | Nome da sub-rede na VNet que a NIC de VM devem pertencer à. |
| AADClientID | ID de cliente da aplicação do Azure AD que tenha permissões para escrever segredos para o seu Cofre de chaves. |
| AADClientSecret | Segredo do cliente de aplicação do Azure AD que tenha permissões para escrever segredos para o seu Cofre de chaves. |
| keyVaultURL | URL do que a chave do BitLocker deve ser carregada para o Cofre de chaves. Pode obtê-lo utilizando o cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`. |
| keyEncryptionKeyURL | URL da chave de encriptação de chave que é utilizada para encriptar a chave gerada do BitLocker (opcional). |
| keyVaultResourceGroup | Grupo de recursos do Cofre de chaves. |
| vmName | Nome da VM que a operação de encriptação está a ser efetuada numa. |

> [!NOTE]
> _KeyEncryptionKeyURL_ é um parâmetro opcional. Pode levar seu próprio KEK para proteger ainda mais a chave de encriptação de dados (segredo da frase de acesso) no seu Cofre de chaves.

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a>Ativar a encriptação em novas VMs de IaaS que são criados a partir de VHD encriptados de cliente e as chaves de encriptação
Neste cenário, pode ativar a encriptação com o modelo do Resource Manager, cmdlets do PowerShell ou comandos da CLI. As secções seguintes explicam em maior detalhe o modelo do Resource Manager e os comandos da CLI.

Siga as instruções de uma destas secções para preparar imagens encriptadas que podem ser utilizadas no Azure. Depois da imagem é criada, pode utilizar os passos na secção seguinte para criar uma VM do Azure encriptadas.

* [Preparar um VHD do Windows encriptadas](#preparing-a-pre-encrypted-windows-vhd)
* [Preparar um VHD do Linux encriptadas](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-the-resource-manager-template"></a>Usando o modelo do Resource Manager
Pode ativar a encriptação de disco no seu VHD encriptada utilizando o [modelo do Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).

1. No modelo de início rápido do Azure, clique em **implementar no Azure**, introduza a configuração de encriptação **parâmetros** painel e clique em **OK**.

2. Selecione a subscrição, grupo de recursos, localização do grupo de recursos, os termos legais e contrato e, em seguida, clique em **criar** para ativar a encriptação na nova VM de IaaS.

A tabela seguinte lista os parâmetros de modelo do Resource Manager para o VHD encriptado:

| Parâmetro | Descrição |
| --- | --- |
| newStorageAccountName | Nome da conta do storage para armazenar o VHD do SO encriptados. Esta conta de armazenamento deve já ter foi criada no mesmo grupo de recursos e na mesma localização da VM. |
| osVhdUri | URI do VHD do SO da conta de armazenamento. |
| osType | Tipo de produto do sistema operacional (Windows/Linux). |
| virtualNetworkName | Nome da VNet ao qual a NIC de VM devem pertencer à. O nome deve já ter foi criado no mesmo grupo de recursos e na mesma localização da VM. |
| subnetName | Nome da sub-rede na VNet que a NIC de VM devem pertencer à. |
| vmSize | Tamanho da VM. Atualmente, a série A Standard apenas, D e G é suportado. |
| keyVaultResourceID | O ResourceID que identifica o recurso do Cofre de chaves no Azure Resource Manager. Pode obtê-lo utilizando o cmdlet do PowerShell `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`. |
| keyVaultSecretUrl | URL da chave de encriptação de disco que está configurada no Cofre de chaves. |
| keyVaultKekUrl | URL da chave de encriptação de chave para criptografar a chave de encriptação de disco gerado. |
| vmName | Nome da IaaS VM. |

#### <a name="using-powershell-cmdlets"></a>Com os cmdlets do PowerShell
Pode ativar a encriptação de disco no seu VHD encriptada utilizando o cmdlet do PowerShell [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).  

#### <a name="using-cli-commands"></a>Utilizar comandos da CLI
Para ativar a encriptação de disco para este cenário através dos comandos da CLI, faça o seguinte:

1. Definir políticas de acesso no Cofre de chaves:

   * Definir o **EnabledForDiskEncryption** sinalizador:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Definir permissões para a aplicação do Azure AD para escrever segredos para o seu Cofre de chaves:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Para ativar a encriptação numa VM existente ou em execução, escreva:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Obter o estado de encriptação:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Para ativar a encriptação numa nova VM a partir do VHD encriptado, utilize os seguintes parâmetros com o `azure vm create` comando:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a>Ativar a encriptação em existente ou executar as VM do Windows de IaaS no Azure
Neste cenário, pode ativar a encriptação com o modelo do Resource Manager, cmdlets do PowerShell ou comandos da CLI. As secções seguintes explicam detalhadamente como ativá-la utilizando o modelo do Resource Manager e os comandos da CLI.

#### <a name="using-the-resource-manager-template"></a>Usando o modelo do Resource Manager
Pode ativar a encriptação de disco no existente ou executar VMs do Windows de IaaS no Azure utilizando o [modelo do Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).

1. No modelo de início rápido do Azure, clique em **implementar no Azure**, introduza a configuração de encriptação **parâmetros** painel e clique em **OK**.

2. Selecione a subscrição, grupo de recursos, localização do grupo de recursos, os termos legais e contrato e, em seguida, clique em **criar** para ativar a encriptação na VM de IaaS existente ou em execução.

A tabela seguinte lista os parâmetros de modelo do Resource Manager para existente ou executar VMs com um ID de cliente do Azure AD:

| Parâmetro | Descrição |
| --- | --- |
| AADClientID | ID de cliente da aplicação do Azure AD que tenha permissões para escrever segredos para o Cofre de chaves. |
| AADClientSecret | Segredo do cliente de aplicação do Azure AD que tenha permissões para escrever segredos para o Cofre de chaves. |
| keyVaultName | Nome do Cofre de chaves que a chave do BitLocker deve ser enviada para. Pode obtê-lo utilizando o cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | URL da chave de encriptação de chave que é utilizada para encriptar a chave gerada do BitLocker. Este parâmetro é opcional se selecionou **nokek** na lista pendente UseExistingKek. Se selecionou **kek** na lista pendente UseExistingKek, tem de introduzir o _keyEncryptionKeyURL_ valor. |
| volumeType | Tipo de volume que a operação de criptografia é executada em. Os valores válidos são _SO_, _dados_, e _todos os_. |
| sequenceVersion | Versão de sequência da operação de disco BitLocker. Incremente este número de versão sempre que uma operação de encriptação de disco é realizada na mesma VM. |
| vmName | Nome da VM que a operação de encriptação está a ser efetuada numa. |

> [!NOTE]
> _KeyEncryptionKeyURL_ é um parâmetro opcional. Pode utilizar o seu próprio KEK para proteger ainda mais a chave de encriptação de dados (segredo de encriptação do BitLocker) no Cofre de chaves.

#### <a name="using-powershell-cmdlets"></a>Com os cmdlets do PowerShell
Para obter informações sobre como ativar a encriptação com Azure Disk Encryption utilizando cmdlets do PowerShell, consulte as mensagens de blogue [explorar o Azure Disk Encryption com o Azure PowerShell - parte 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) e [explorar o Azure Disk Encryption com o Azure PowerShell - parte 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

#### <a name="using-cli-commands"></a>Utilizar comandos da CLI
Para ativar a encriptação em existente ou a executar a VM do Windows de IaaS no Azure através de comandos da CLI, faça o seguinte:

1. Para definir políticas de acesso no Cofre de chaves:
   * Definir o **EnabledForDiskEncryption** sinalizador:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * Definir permissões para a aplicação do Azure AD para escrever segredos para o seu Cofre de chaves:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. Para ativar a encriptação numa VM existente ou em execução:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. Para obter o estado de encriptação:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. Para ativar a encriptação numa nova VM a partir do VHD encriptado, utilize os seguintes parâmetros com o `azure vm create` comando:

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a>Ativar a encriptação numa VM do Linux IaaS existente ou em execução no Azure
Pode ativar a encriptação de disco numa VM do Linux IaaS existente ou em execução no Azure utilizando o [modelo do Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).

1. Clique em **implementar no Azure** no modelo de início rápido do Azure, introduza a configuração de encriptação **parâmetros** painel e clique em **OK**.

2. Selecione a subscrição, grupo de recursos, localização do grupo de recursos, os termos legais e contrato e, em seguida, clique em **criar** para ativar a encriptação na VM de IaaS existente ou em execução.

A tabela seguinte lista os parâmetros de modelo do Resource Manager para existente ou executar VMs com um ID de cliente do Azure AD:

| Parâmetro | Descrição |
| --- | --- |
| AADClientID | ID de cliente da aplicação do Azure AD que tenha permissões para escrever segredos para o Cofre de chaves. |
| AADClientSecret | Segredo do cliente de aplicação do Azure AD que tenha permissões para escrever segredos para o seu Cofre de chaves. |
| keyVaultName | Nome do Cofre de chaves que a chave do BitLocker deve ser enviada para. Pode obtê-lo utilizando o cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`. |
|  keyEncryptionKeyURL | URL da chave de encriptação de chave que é utilizada para encriptar a chave gerada do BitLocker. Este parâmetro é opcional se selecionou **nokek** na lista pendente UseExistingKek. Se selecionou **kek** na lista pendente UseExistingKek, tem de introduzir o _keyEncryptionKeyURL_ valor. |
| volumeType | Tipo de volume que a operação de criptografia é executada em. Os valores suportados válidos são _SO_ ou _todos os_ (consulte suportado distribuições de Linux e suas versões de SO e discos de dados na secção de prerequisiteis anteriormente). |
| sequenceVersion | Versão de sequência da operação de disco BitLocker. Incremente este número de versão sempre que uma operação de encriptação de disco é realizada na mesma VM. |
| vmName | Nome da VM que a operação de encriptação está a ser efetuada numa. |
| frase de acesso | Escreva uma frase de acesso forte como a chave de encriptação de dados. |

> [!NOTE]
> _KeyEncryptionKeyURL_ é um parâmetro opcional. Pode levar seu próprio KEK para proteger ainda mais a chave de encriptação de dados (segredo da frase de acesso) no seu Cofre de chaves.

#### <a name="cli-commands"></a>Comandos da CLI
Pode ativar a encriptação de disco no seu VHD encriptado por instalar e utilizar o [comando da CLI](../cli-install-nodejs.md). Para ativar a encriptação em existente ou executar VMs do Linux IaaS no Azure utilizando os comandos da CLI, faça o seguinte:

1. Definir políticas de acesso no Cofre de chaves:

 * Definir o **EnabledForDiskEncryption** sinalizador:

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * Definir permissões para a aplicação do Azure AD para escrever segredos para o seu Cofre de chaves:

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. Para ativar a encriptação numa VM existente ou em execução:

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. Obter o estado de encriptação:

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. Para ativar a encriptação numa nova VM a partir do VHD encriptado, utilize os seguintes parâmetros com o `azure vm create` comando:
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-the-encryption-status-of-an-encrypted-iaas-vm"></a>Obter o estado de encriptação de uma VM de IaaS encriptada
Pode obter o estado de encriptação utilizando o Gestor de recursos do Azure, [cmdlets do PowerShell](/powershell/azure/overview), ou comandos da CLI. As secções seguintes explicam como utilizar o Portal do Azure e os comandos da CLI para obter o estado de encriptação.

#### <a name="get-the-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a>Obter o estado de encriptação de uma VM do Windows criptografado, com o Azure Resource Manager
Pode obter o estado de encriptação da IaaS VM do Azure Resource Manager, fazendo o seguinte:

1. Inicie sessão para o [Portal do Azure](https://portal.azure.com/)e, em seguida, clique em **máquinas virtuais** no painel da esquerda para ver uma vista de resumo das máquinas virtuais na sua subscrição. Pode filtrar a vista de máquinas virtuais ao selecionar o nome da subscrição no **subscrição** na lista pendente.

2. Na parte superior a **máquinas virtuais** página, clique em **colunas**.

3. Sobre o **coluna de escolha** painel, selecione **encriptação de disco**e, em seguida, clique em **atualização**. Deverá ver a coluna de encriptação de disco que mostra o estado de encriptação _Enabled_ ou _não ativada_ para cada VM, conforme mostrado na figura a seguir:

 ![Microsoft Antimalware no Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-the-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-the-disk-encryption-powershell-cmdlet"></a>Obter o estado de encriptação de uma VM de IaaS (Windows/Linux) encriptada utilizando o cmdlet do PowerShell de encriptação de disco
Pode obter o estado de encriptação da IaaS VM do cmdlet do PowerShell de encriptação de disco `Get-AzureRmVMDiskEncryptionStatus`. Para obter as definições de encriptação para a sua VM, introduza o seguinte:

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

Pode inspecionar a saída de _Get-AzureRmVMDiskEncryptionStatus_ para a encriptação de chave URLs.

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey

    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

Os valores de definições OSVolumeEncrypted e DataVolumesEncrypted são definidos como _Encrypted_, que mostra que ambos os volumes são encriptados com o Azure Disk Encryption. Para obter informações sobre como ativar a encriptação com Azure Disk Encryption utilizando cmdlets do PowerShell, consulte as mensagens de blogue [explorar o Azure Disk Encryption com o Azure PowerShell - parte 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) e [explorar o Azure Disk Encryption com o Azure PowerShell - parte 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).

> [!NOTE]
> Em VMs do Linux, demora três a quatro minutos o `Get-AzureRmVMDiskEncryptionStatus` cmdlet para comunicar o estado de encriptação.

#### <a name="get-the-encryption-status-of-the-iaas-vm-from-the-disk-encryption-cli-command"></a>Obter o estado de encriptação da IaaS VM do comando da CLI de encriptação de disco
Pode obter o estado de encriptação da IaaS VM com o comando da CLI de encriptação de disco `azure vm show-disk-encryption-status`. Para obter as definições de encriptação para a sua VM, introduza a sua sessão de CLI do Azure:

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a>Desativar a encriptação na VM de IaaS do Windows em execução
Pode desativar a encriptação num Windows em execução ou VM do IaaS Linux através do modelo de encriptação de disco do Azure Resource Manager ou cmdlets do PowerShell e especificar a configuração de desencriptação.

##### <a name="windows-vm"></a>VM do Windows
O passo da criptografia de desativar desativa a encriptação de sistema operacional, o volume de dados ou ambos na VM de IaaS do Windows em execução. Não é possível desativar o volume do sistema operacional e deixe o volume de dados encriptado. Quando o passo de desativar a encriptação é efetuado, o modelo de implementação clássica do Azure atualiza o modelo de serviço VM e a VM de IaaS do Windows está marcada como desencriptada. O conteúdo da VM já não é encriptado em inatividade. A desencriptação não elimina o seu Cofre de chaves e o material de chave de encriptação (chaves de encriptação de disco BitLocker para Windows e a frase de acesso para Linux).

##### <a name="linux-vm"></a>VM do Linux
O passo da criptografia de desativar desativa a encriptação do volume de dados na VM de IaaS Linux em execução. Este passo só funciona se o disco do SO não está encriptado.

> [!NOTE]
> A desativação da encriptação no disco do SO não é permitida em VMs do Linux.

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Desativar a encriptação numa VM do IaaS existente ou em execução
Pode desativar a encriptação de disco na execução de VMs de IaaS do Windows com o [modelo do Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).

1. No modelo de início rápido do Azure, clique em **implementar no Azure**, introduza a configuração de desencriptação **parâmetros** painel e clique em **OK**.

2. Selecione a subscrição, grupo de recursos, localização do grupo de recursos, os termos legais e contrato e, em seguida, clique em **criar** para ativar a encriptação numa nova VM de IaaS.

Para VMs do Linux, pode desativar a encriptação utilizando o [desativar a encriptação numa VM do Linux em execução](https://aka.ms/decrypt-linuxvm) modelo.

A tabela seguinte lista os parâmetros de modelo do Resource Manager para desativar a encriptação numa VM de IaaS em execução:

| Parâmetro | Descrição |
| --- | --- |
| vmName | Nome da VM que a operação de encriptação está a ser efetuada numa.
| volumeType | Tipo de volume que uma operação de desencriptação é executada em. Os valores válidos são _SO_, _dados_, e _todos os_. Não é possível desativar a encriptação sobre como executar o volume de sistema operacional de VM de IaaS do Windows/arranque sem a desativação da encriptação no _dados_ volume. Observe também que a desativação da encriptação no disco do SO não é permitida em VMs do Linux. |
| sequenceVersion | Versão de sequência da operação de disco BitLocker. Incremente este número de versão sempre que uma operação de desencriptação de disco é realizada na mesma VM. |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a>Desativar a encriptação numa VM do IaaS existente ou em execução
Para desativar a encriptação numa VM de IaaS em execução ou existente utilizando o cmdlet do PowerShell, consulte [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption). Este cmdlet suporta Windows e VMs do Linux. Para desativar a encriptação, ele instala uma extensão na máquina virtual. Se o _Name_ parâmetro não for especificado, uma extensão com o nome predefinido _AzureDiskEncryption para VMs Windows_ é criado.

Em VMs do Linux, é utilizada a extensão AzureDiskEncryptionForLinux.

> [!NOTE]
> Este cmdlet reinicia a máquina virtual.

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a>Ativar a encriptação em encriptadas de IaaS de VM com disco gerido do Azure
Utilize o modelo de ARM de disco gerido do Azure para criar uma VM encriptada a partir de um VHD encriptado utilizando o modelo ARM localizado em   
[Criar um novo disco gerido encriptado a partir de um blob VHD/armazenamento encriptado] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a>Ativar a encriptação numa nova VM do IaaS Linux com o disco gerido do Azure
Utilize o modelo de ARM de disco gerido do Azure para criar uma nova VM de IaaS Linux encriptada utilizando o modelo ARM localizado em   
[Implementação do RHEL 7.2 com a encriptação de disco completo] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a>Ativar a encriptação numa nova VM de IaaS do Windows com disco gerido do Azure
 Utilize o modelo de ARM de disco gerido do Azure para criar uma nova VM de IaaS Linux encriptada utilizando o modelo ARM localizado em   
 [Criar um novo Windows IaaS geridos disco VM encriptada da imagem da Galeria] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)

  > [!NOTE]
  >É obrigatório para instantâneo e/ou a cópia de segurança um disco gerido com base instância de VM fora do e antes de ativar o Azure Disk Encryption.  Pode ser criado um instantâneo do disco gerido a partir do portal ou cópia de segurança do Azure pode ser utilizada.  As cópias de segurança Certifique-se de que uma opção de recuperação é possível no caso de qualquer falha inesperada durante a encriptação.  Assim que for feita uma cópia de segurança, o cmdlet Set-AzureRmVMDiskEncryptionExtension pode servir para encriptar discos geridos ao especificar o parâmetro - skipVmBackup.  Este comando irá falhar em relação a da VM de disco gerido com base em até que foi efetuada uma cópia de segurança e este parâmetro foi especificado.    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a>Atualizar definições de encriptação da VM não premium encriptada existente
  Utilizar as interfaces de encriptação suportada de disco do Azure existentes para executar a VM [cmdlets de PS, modelos da CLI ou ARM] para atualizar as definições de encriptação, como o cliente do AAD ID/segredos, chave de encriptação da chave [KEK], chave de encriptação de disco BitLocker para VM do Windows ou a frase de acesso para VM do Linux etc. A atualização de definição de encriptação é suportada para premium e VMs de armazenamento não premium.

## <a name="appendix"></a>Anexo
### <a name="connect-to-your-subscription"></a>Estabelecer a ligação à subscrição
Antes de continuar, reveja os *pré-requisitos* secção deste artigo. Depois de assegurar que todos os pré-requisitos tiverem sido cumpridos, ligar à sua subscrição, fazendo o seguinte:

1. Inicie uma sessão do PowerShell do Azure e iniciar sessão na sua conta do Azure com o seguinte comando:

    `Connect-AzureRmAccount`

2. Se tiver várias subscrições e pretender especificar uma para utilizar, escreva o seguinte para ver as subscrições da sua conta:

    `Get-AzureRmSubscription`

3. Para especificar a subscrição que pretende utilizar, introduza:

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. Para verificar se a subscrição configurada está correta, escreva:

    `Get-AzureRmSubscription`

5. Para confirmar que os cmdlets do Azure Disk Encryption são instalados, escreva:

    `Get-command *diskencryption*`

6. O seguinte resultado confirma a instalação do PowerShell de encriptação de disco do Azure:

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a>Preparar um VHD do Windows encriptadas
As secções que se seguem são necessárias para preparar um VHD do Windows encriptadas para a implementação como um VHD encriptado no IaaS do Azure. Utilize as informações para preparar e arranque de uma VM de Windows atualizados (VHD) no Azure Site Recovery ou no Azure.

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a>Atualizar política de grupo para permitir não-TPM para proteção do sistema operacional
Configurar a definição de política de grupo do BitLocker **BitLocker Drive Encryption**, que encontrará sob **política de computador Local** > **configuração do computador**  >  **Modelos administrativos** > **componentes do Windows**. Alterar esta definição para **unidades de sistema operativo** > **exigir autenticação adicional no arranque** > **permitir BitLocker sem um TPM compatível**, conforme mostrado na figura a seguir:

![Microsoft Antimalware no Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a>Instalar componentes de funcionalidades do BitLocker
Para o Windows Server 2012 e posterior, utilize o seguinte comando:

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

Para o Windows Server 2008 R2, utilize o seguinte comando:

    ServerManagerCmd -install BitLockers

#### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a>Preparar o volume do SO do BitLocker utilizando `bdehdcfg`
Para compactar a partição do sistema operacional e preparar o computador para o BitLocker, execute o seguinte comando:

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-the-os-volume-by-using-bitlocker"></a>Proteger o volume do sistema operacional usando o BitLocker
Utilize o [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) comando para ativar a encriptação em volume de arranque com um protetor de chave externo. Também coloca a chave externa (ficheiro .bek) no volume ou unidade externa. A encriptação está ativada no volume de arranque do sistema após o reinício seguinte.

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> Prepare a VM com um dados/recurso separado VHD para obter a chave externa usando o BitLocker.

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a>Encriptar uma unidade de sistema operacional numa VM do Linux em execução

##### <a name="prerequisites-for-os-disk-encryption"></a>Pré-requisitos para a encriptação de disco do SO

* A VM tem de utilizar uma distribuição compatível com a encriptação de disco do sistema operacional conforme listado no [FAQ de encriptação de disco do Azure](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-faq#what-linux-distributions-does-azure-disk-encryption-support) 
* A VM tem de ser criada a partir da imagem do Marketplace no Azure Resource Manager.
* VM do Azure com, pelo menos, 4 GB de RAM (recomendado tamanho é de 7 GB).
* (Para RHEL e CentOS) Desative SELinux. Para desativar SELinux, consulte "4.4.2 criação. Desativar o SELinux"a [Guia do administrador e usuário SELinux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) na VM.
* Depois de desativar SELinux, reinicie a VM, pelo menos, uma vez.

##### <a name="steps"></a>Passos
1. Crie uma VM ao utilizar uma das distribuições especificadas anteriormente.

 CentOS 7.2, encriptação de disco do SO é suportada por meio de uma imagem especial. Para utilizar esta imagem, especifica "7.2n" como o SKU ao criar a VM:
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. Configure a VM, de acordo com suas necessidades. Se for encriptar todos os (SO + dados), unidades, as unidades de dados tem de ser especificado e montável de /etc/fstab.

 > [!NOTE]
 > Utilize o UUID =... para especificar a unidades de dados no/etc/fstab em vez de especificar o nome do dispositivo de blocos (por exemplo, / desenvolvimento/sdb1). Durante a encriptação, a ordem das alterações de unidades na VM. Se a VM se baseia numa ordem específica de dispositivos de bloco, ele falhará montá-los depois da encriptação.

3. Termine as sessões SSH.

4. Para encriptar o sistema operacional, especifique volumeType como **todos os** ou **SO** quando [ativar a encriptação](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).

 > [!NOTE]
 > Todos os processos de espaço do utilizador que não estiverem executando `systemd` serviços devem ser eliminados com um `SIGKILL`. Reinicie a VM. Quando ativa a encriptação de disco de SO numa VM em execução, planeje-se no período de indisponibilidade da VM.

5. Periodicamente, monitorizar o progresso de criptografia utilizando as instruções no [próxima seção](#monitoring-os-encryption-progress).

6. Depois de Get-AzureRmVmDiskEncryptionStatus mostra "VMRestartPending", reinicie a VM ao iniciar sessão para o mesmo ou ao utilizar o portal, o PowerShell ou a CLI.
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
Antes de reiniciar, recomendamos que guarde [diagnósticos de arranque](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/) da VM.

#### <a name="monitoring-os-encryption-progress"></a>Monitorizar o progresso de criptografia do sistema operacional
Pode monitorizar o progresso de criptografia do sistema operacional de três formas:

* Utilize o `Get-AzureRmVmDiskEncryptionStatus` cmdlet e inspecionar o campo de ProgressMessage:
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 Depois que a VM atinge "Iniciada de encriptação de disco de SO", demora cerca de 40 a 50 minutos num armazenamento Premium de segurança VM.

 Devido [emitir #388](https://github.com/Azure/WALinuxAgent/issues/388) no WALinuxAgent, `OsVolumeEncrypted` e `DataVolumesEncrypted` aparecem como `Unknown` em algumas distribuições. Com WALinuxAgent versão 2.1.5 e mais tarde, este problema é resolvido automaticamente. Se vir `Unknown` na saída, pode verificar o estado de encriptação de disco utilizando o Explorador de recursos do Azure.

 Aceda a [Explorador de recursos do Azure](https://resources.azure.com/)e, em seguida, expanda esta hierarquia no painel de seleção no lado esquerdo:

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 No InstanceView, role para baixo para ver o estado de encriptação das suas unidades.

 ![Vista de instância VM](./media/azure-security-disk-encryption/vm-instanceview.png)

* Examinar [diagnósticos de arranque](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/). Mensagens a partir da extensão de ADE devem ter o prefixo `[AzureDiskEncryption]`.

* Inicie sessão para a VM através de SSH e obter o registo da extensão de:

    /var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux

 Recomendamos que não iniciar sessão na VM, enquanto a encriptação de sistema operacional está em curso. Copie os registos apenas quando os outros dois métodos falharam.

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a>Preparar um VHD do Linux encriptadas
##### <a name="ubuntu-16"></a>16 de Ubuntu
Configure a encriptação durante a instalação de distribuição, fazendo o seguinte:

1. Selecione **configurar volumes criptografados** quando dividir os discos.

 ![Configuração do Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. Crie uma unidade de inicialização separado, que não tem de estar encriptada. Criptografe sua unidade de raiz.

 ![Configuração do Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. Forneça uma frase de acesso. Esta é a frase de acesso que é carregado para o Cofre de chaves.

 ![Configuração do Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. Conclua a criação de partições.

 ![Configuração do Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. Quando iniciar a VM e são-lhe pedido para uma frase de acesso, utilize a frase de acesso que indicou no passo 3.

 ![Configuração do Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. Preparar a VM para carregando-a no Azure com [estas instruções](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-ubuntu/). Não execute o último passo (desaprovisionar a VM) ainda.

Configure a encriptação funcione com o Azure, fazendo o seguinte:

1. Crie um ficheiro em /usr/local/sbin/azure_crypt_key.sh, com o conteúdo no seguinte script. Preste atenção às KeyFileName, porque é o nome de ficheiro da frase de acesso utilizado pelo Azure.

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. Alterar a configuração dos crypt no *nomedeanfitrião crypttab*. Deve ter o seguinte aspeto:
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. Se estiver a editar *azure_crypt_key.sh* no Windows e é copiado para o Linux, execute `dos2unix /usr/local/sbin/azure_crypt_key.sh`.

4. Adicione permissões executáveis para o script:
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. Editar */etc/initramfs-tools/modules* ao acrescentar linhas:
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. Execute `update-initramfs -u -k all` para atualizar o initramfs para tornar o `keyscript` entrem em vigor.

7. Agora pode desaprovisionar a VM.

 ![Configuração do Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. Continuar para o passo seguinte e [carregar o seu VHD](#upload-encrypted-vhd-to-an-azure-storage-account) para o Azure.

##### <a name="opensuse-132"></a>openSUSE 13.2
Para configurar a encriptação durante a instalação de distribuição, efetue o seguinte:
1. Quando a partição dos discos, selecione **criptografar o Volume grupo**e, em seguida, introduza uma palavra-passe. Esta é a palavra-passe que irá carregar para o seu Cofre de chaves.

 ![Configurar o openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. A VM com a sua palavra-passe de arranque.

 ![Configurar o openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. Preparar a VM para carregar para o Azure ao seguir as instruções em [preparar uma máquina virtual SLES ou openSUSE para o Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131). Não execute o último passo (desaprovisionar a VM) ainda.

Para configurar a encriptação funcione com o Azure, efetue o seguinte:
1. Edite o /etc/dracut.conf e adicione a seguinte linha:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. Comente estas linhas ao final do /usr/lib/dracut/modules.d/90crypt/module-setup.sh ficheiro:
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. Acrescente a seguinte linha no início do /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh ficheiro:
 ```
    DRACUT_SYSTEMD=0
 ```
E altere todas as ocorrências de:
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
para:
```
    if [ 1 ]; then
```
4. Editar /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh e acrescentá-lo a "# LUKS aberto a dispositivo":

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. Executar `/usr/sbin/dracut -f -v` para atualizar o initrd.

6. Agora, pode desaprovisionar a VM e [carregar o seu VHD](#upload-encrypted-vhd-to-an-azure-storage-account) para o Azure.

##### <a name="centos-7"></a>CentOS 7
Para configurar a encriptação durante a instalação de distribuição, efetue o seguinte:
1. Selecione **criptografar meus dados** quando dividir os discos.

 ![Configuração do centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. Certifique-se **Encrypt** está selecionada para a partição de raiz.

 ![Configuração do centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. Forneça uma frase de acesso. Esta é a frase de acesso que irá carregar para o seu Cofre de chaves.

 ![Configuração do centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. Quando iniciar a VM e são-lhe pedido para uma frase de acesso, utilize a frase de acesso que indicou no passo 3.

 ![Configuração do centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. Preparar a VM para carregar para o Azure ao utilizar as instruções de "CentOS 7.0 +" na [preparar uma máquina de virtual baseada em CentOS para o Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70). Não execute o último passo (desaprovisionar a VM) ainda.

6. Agora, pode desaprovisionar a VM e [carregar o seu VHD](#upload-encrypted-vhd-to-an-azure-storage-account) para o Azure.

Para configurar a encriptação funcione com o Azure, efetue o seguinte:

1. Edite o /etc/dracut.conf e adicione a seguinte linha:
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. Comente estas linhas ao final do /usr/lib/dracut/modules.d/90crypt/module-setup.sh ficheiro:
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. Acrescente a seguinte linha no início do /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh ficheiro:
```
    DRACUT_SYSTEMD=0
```
E altere todas as ocorrências de:
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
para
```
    if [ 1 ]; then
```
4. Editar /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh e acrescente isso depois de dispositivo"LUKS aberto de #":
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. Executar o "/ usr/sbin/dracut - f - v" para atualizar o initrd.

![Configuração do centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a>Carregar o VHD encriptado para uma conta de armazenamento do Azure
Depois de é ativada a encriptação do BitLocker ou encriptação de DM-Crypt, o VHD encriptado local tem de ser carregado para a sua conta de armazenamento.

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-the-disk-encryption-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a>Carregar o segredo de encriptação de disco para a VM encriptada para o seu Cofre de chaves
O segredo de encriptação de disco que obteve anteriormente deve ser carregado como um segredo no Cofre de chaves. O Cofre de chaves tem de ter a encriptação de disco e permissões ativadas para o cliente do Azure AD.

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a>Segredo de encriptação de disco não criptografado com uma KEK
Para configurar o segredo no Cofre de chaves, utilize [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret). Se tiver uma máquina virtual do Windows, o ficheiro de bek é codificado como uma cadeia base64 e, em seguida, é carregado para o seu Cofre de chaves com o `Set-AzureKeyVaultSecret` cmdlet. Para o Linux, a frase de acesso é codificado como uma cadeia base64 e, em seguida, é carregado para o Cofre de chaves. Além disso, certifique-se de que as etiquetas seguintes estão definidas quando cria o segredo no Cofre de chaves.

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

Utilize o `$secretUrl` no próximo passo para [anexar o disco do SO sem utilizar KEK](#without-using-a-kek).

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a>Segredo de encriptação de disco encriptado com uma KEK
Antes de carregar o segredo ao Cofre de chaves, opcionalmente, poderá criptografá-los com uma chave de encriptação de chave. Utilizar a moldagem [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) primeiro criptografar o segredo usando a chave de encriptação de chave. O resultado desta operação de moldagem é uma cadeia de URL codificado em base64 que, em seguida, pode carregar como um segredo ao utilizar o [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

Uso `$KeyEncryptionKey` e `$secretUrl` no passo seguinte para [anexar o disco do SO usando KEK](#using-a-kek).

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a>Especifique um URL de segredo quando anexa um disco de SO
#### <a name="without-using-a-kek"></a>Sem utilizar um KEK
Enquanto estão a anexar o disco do SO, precisa passar `$secretUrl`. O URL foi gerado na secção "segredo de encriptação de disco não criptografado com uma KEK".

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a>Usando um KEK
Ao anexar o disco do SO, passa `$KeyEncryptionKey` e `$secretUrl`. O URL foi gerado na secção "segredo de encriptação de disco não criptografado com uma KEK".

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="for-more-information"></a>Para obter mais informações:
[Explorar o Azure Disk Encryption com o Azure PowerShell - parte 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[Explorar o Azure Disk Encryption com o Azure PowerShell - parte 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
