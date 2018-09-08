---
title: FAQ e problemas conhecidos com identidades geridas para recursos do Azure
description: Problemas conhecidos com identidades geridas para recursos do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.component: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: daveba
ms.openlocfilehash: 27f9e0a3875b8d7e9411517863f7d7c2f6982ab3
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158641"
---
# <a name="faqs-and-known-issues-with-managed-identities-for-azure-resources"></a>FAQ e problemas conhecidos com identidades geridas para recursos do Azure

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Perguntas Mais Frequentes (FAQs)

> [!NOTE]
> Identidades de gerido para recursos do azure é o novo nome para o serviço anteriormente conhecido como identidade de serviço gerida (MSI).

### <a name="does-managed-identities-for-azure-resources-work-with-azure-cloud-services"></a>Identidades geridas para recursos do Azure funciona com serviços Cloud do Azure?

Não, não existem não existem planos para oferecer suporte a identidades geridas para recursos do Azure nos serviços de Cloud do Azure.

### <a name="does-managed-identities-for-azure-resources-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>Identidades geridas para recursos do Azure funciona com o Active Directory Authentication Library (ADAL) ou o Microsoft Authentication Library (MSAL)?

Não, geridos identidades para recursos do Azure ainda não está integrado com o ADAL ou MSAL. Para obter detalhes sobre como adquirir um token para identidades geridas para recursos do Azure com o ponto final REST, veja [como utilizar identidades geridas para recursos do Azure numa VM do Azure para adquirir um token de acesso ](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-managed-identities-for-azure-resources"></a>O que é o limite de segurança de identidades geridas para recursos do Azure?

O limite de segurança da identidade é o recurso ao qual está ligado a. Por exemplo, o limite de segurança para uma Máquina Virtual com identidades geridas para recursos do Azure ativado, é a Máquina Virtual. Qualquer código em execução nessa VM, é capaz de chamar as identidades geridas para recursos do Azure, tokens de ponto final e a pedido. É a experiência de semelhante com outros recursos que oferecem suporte a identidades geridas para recursos do Azure.

### <a name="should-i-use-the-managed-identities-for-azure-resources-vm-imds-endpoint-or-the-vm-extension-endpoint"></a>Deve utilizar as identidades geridas para o ponto final VM IMDS de recursos do Azure ou o ponto de final de extensão VM?

Quando utiliza identidades geridas para recursos do Azure com VMs, recomendamos a utilização de identidades geridas para o ponto final IMDS de recursos do Azure. O serviço de metadados de instância do Azure é um ponto final REST acessíveis a todas as VMs de IaaS criadas através do Azure Resource Manager. Algumas das vantagens da utilização de identidades geridas para recursos do Azure através de IMDS são:

1. Todos os sistemas de operativos de IaaS do Azure suportada pode utilizar identidades geridas para recursos do Azure através de IMDS. 
2. Já não é necessário instalar uma extensão na sua VM para ativar identidades geridas para recursos do Azure. 
3. Os certificados utilizados por identidades geridas para recursos do Azure já não estão presentes na VM. 
4. O ponto de extremidade IMDS é um endereço IP bem conhecido não encaminháveis internos, apenas disponível a partir da VM. 

As identidades geridas para a extensão VM de recursos do Azure ainda está disponível para ser utilizada hoje. No entanto, mais adiante, será predefinida para utilizar o ponto de extremidade IMDS. As identidades geridas para a extensão VM de recursos do Azure vão ser preteridas em Janeiro de 2019. 

Para obter mais informações sobre o serviço de metadados de instância do Azure, consulte [documentação IMDS](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Quais são as distribuições suportadas de Linux?

Todas as distribuições de Linux suportadas pelo Azure IaaS podem ser utilizadas com identidades geridas para recursos do Azure através do ponto de extremidade IMDS. 

Nota: As identidades geridas para a extensão de VM (planeada para preterição em Janeiro de 2019) de recursos do Azure só suporta as seguintes distribuições de Linux:
- Stable do CoreOS
- CentOS 7.1
- Red Hat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Outras distribuições do Linux não são atualmente suportadas e extensão pode falhar em distribuições não suportadas.

A extensão funciona no CentOS 6.9. No entanto, devido à falta de suporte do sistema no 6.9, a extensão será não automaticamente reinício se travasse ou parado. Reinicia quando a VM é reiniciada. Para reiniciar manualmente a extensão, consulte [como é reiniciar as identidades geridas para a extensão de recursos do Azure?](#how-do-you-restart-the-managed-identities-for-Azure-resources-extension)

### <a name="how-do-you-restart-the-managed-identities-for-azure-resources-extension"></a>Como reiniciar as identidades geridas para a extensão de recursos do Azure?
No Windows e determinadas versões do Linux, se a extensão de parar, o cmdlet seguinte pode ser usado para reiniciar manualmente:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Em que: 
- Nome de extensão e tipo para Windows é: ManagedIdentityExtensionForWindows
- Nome de extensão e o tipo para Linux é: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Problemas conhecidos

### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>"Script de automação de" Falha ao tentar exportar de esquema para identidades geridas para a extensão de recursos do Azure

Quando as identidades geridas para recursos do Azure é ativada numa VM, o seguinte erro é apresentado durante a tentativa de utilizar a funcionalidade de "Script de automatização" para a VM ou o respetivo grupo de recursos:

![Erro de exportação de identidades geridas para o script de automação de recursos do Azure](./media/msi-known-issues/automation-script-export-error.png)

As identidades geridas para recursos do Azure a extensão VM (planeada para preterição em Janeiro de 2019) faz atualmente não suportam a capacidade de exportar o seu esquema para um modelo de grupo de recursos. Como resultado, o modelo gerado não mostra os parâmetros de configuração para ativar identidades geridas para recursos do Azure no recurso. Estas secções podem ser adicionadas manualmente ao seguir os exemplos [configurar geridos identidades para recursos do Azure na VM do Azure com um modelo de](qs-configure-template-windows-vm.md).

Quando a funcionalidade de exportação de esquema fica disponível para as identidades geridas para a extensão VM de recursos do Azure (planeada para preterição em Janeiro de 2019), serão listado na [e grupos de recursos de exportação, que contêm as extensões de VM](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Painel de configuração não é apresentado no portal do Azure

Se o painel de configuração de VM não aparecer na sua VM, em seguida, identidades geridas para recursos do Azure tem ainda não foi ativado no portal da sua região.  Verifique novamente mais tarde.  Também pode ativar identidades geridas para recursos do Azure para a sua VM, utilizando [PowerShell](qs-configure-powershell-windows-vm.md) ou o [CLI do Azure](qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Não é possível atribuir acesso a máquinas virtuais no painel de controlo de acesso (IAM)

Se **Máquina Virtual** não é apresentado no portal do Azure como uma opção para **atribuir acesso a** na **controlo de acesso (IAM)** > **adicionar permissões**, em seguida, identidades geridas para recursos do Azure tem ainda não foi ativado no portal da sua região. Verifique novamente mais tarde.  Pode ainda selecionar a identidade para a VM para a atribuição de função ao procurar as identidades geridas para recursos do Azure Principal de serviço.  Introduza o nome da VM no **selecione** campo e o Principal de serviço é apresentado no resultado da pesquisa.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>VM falha a iniciação após a ser movidos do grupo de recursos ou subscrição

Se mover uma VM em execução, ele continua a ser executado durante a mudança. No entanto, após a mudança, se a VM é parada e reiniciada, ele irá falhar iniciar. Este problema ocorre porque a VM não está a ser atualizada a referência para as identidades geridas para a identidade de recursos do Azure e continua para apontá-lo no grupo de recursos antigos.

**Solução** 
 
Acione uma atualização na VM para que ele pode obter valores corretos para as identidades geridas para recursos do Azure. Pode fazer uma alteração de propriedade VM para atualizar a referência para as identidades geridas para a identidade de recursos do Azure. Por exemplo, pode definir um novo valor de etiqueta na VM com o seguinte comando:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Este comando define uma nova etiqueta "fixVM" com um valor de 1 na VM. 
 
Definindo essa propriedade, a VM atualiza com as identidades geridas corretas para o URI do recurso de recursos do Azure e, em seguida, deve ser capaz de iniciar a VM. 
 
Assim que a VM é iniciada, a etiqueta pode ser removida ao utilizar o seguinte comando:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="known-issues-with-user-assigned-identities"></a>Problemas conhecidos relacionados com identidades atribuídas

- atribuições de identidades atribuídas só estão disponíveis para a VM e VMSS. Importante: atribuído ao utilizador atribuições de identidade serão alterado nos próximos meses.
- Duplicar Identidiades atribuídas na mesma VM/VMSS, fará com que a VM/VMSS falhe. Isto inclui a identidades que são adicionadas com maiúsculas/minúsculas diferentes. Por exemplo, MyUserAssignedIdentity e myuserassignedidentity. 
- Aprovisionamento da extensão VM (planeada para preterição em Janeiro de 2019) a uma VM pode falhar devido a falhas de pesquisa DNS. Reinicie a VM e tente novamente. 
- Adicionar uma identidade de utilizador atribuída 'não existente' fará com que a VM falhe. 
- Criar uma identidade de utilizador atribuída com carateres especiais (por exemplo, um caráter de sublinhado) no nome, não é suportada.
- nomes de identidade atribuída por utilizador estão limitados a 24 carateres para o cenário de ponta a ponta. identidiades atribuídas com mais de 24 carateres de nomes irão falhar a ser atribuída.
- Se utilizar a extensão de máquina virtual de identidade gerida (planeada para preterição em Janeiro de 2019), o limite suportado é 32 identidades geridas atribuído ao utilizador. Sem a extensão de máquina virtual de identidade gerida, o limite suportado é 512.  
- Ao adicionar uma identidade de utilizador atribuído segundo, o ID de cliente pode não estar disponível para tokens de pedidos para a extensão VM. Facilitar a reiniciar as identidades geridas para a extensão VM de recursos do Azure com os seguintes comandos de dois bash:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- Quando uma VM tem uma identidade de utilizador atribuída, mas nenhuma identidade atribuída de sistema, o portal que irá mostrar a interface do Usuário geridos identidades para recursos do Azure como desativados. Para ativar a identidade atribuída de sistema, utilize um modelo Azure Resource Manager, uma CLI do Azure ou um SDK.
