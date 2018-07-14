---
title: FAQ e problemas conhecidos com o Managed Service Identity (MSI) do Azure Active Directory
description: Problemas conhecidos com a identidade do serviço gerido do Azure Active Directory.
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
ms.openlocfilehash: c48d03b6e8a3d850d02d2c36c35915f8214b00e8
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035817"
---
# <a name="faqs-and-known-issues-with-managed-service-identity-msi-for-azure-active-directory"></a>FAQ e problemas conhecidos com o Managed Service Identity (MSI) do Azure Active Directory

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Perguntas Mais Frequentes (FAQs)

### <a name="does-msi-work-with-azure-cloud-services"></a>MSI funciona com serviços Cloud do Azure?

Não, não existem não existem planos para suportar o MSI nos serviços Cloud do Azure.

### <a name="does-msi-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>MSI funciona com o Active Directory Authentication Library (ADAL) ou o Microsoft Authentication Library (MSAL)?

Não, MSI ainda não estiver integrado com o ADAL ou MSAL. Para obter detalhes sobre como adquirir um token de MSI com o ponto final de REST de MSI, veja [como utilizar um Azure VM Managed Service Identity (MSI) para a aquisição do token](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-a-managed-service-identity"></a>O que é o limite de segurança de uma identidade do serviço gerido?

O limite de segurança da identidade é o recurso ao qual está ligado a. Por exemplo, o limite de segurança para um MSI de Máquina Virtual, é a Máquina Virtual. Qualquer código em execução nessa VM, é capaz de chamar o ponto de extremidade do MSI e solicitar tokens. É a experiência de semelhante com outros recursos que suportam o MSI.

### <a name="should-i-use-the-msi-vm-imds-endpoint-or-the-msi-vm-extension-endpoint"></a>Posso utilizar o ponto de extremidade do MSI VM IMDS ou o ponto de final de extensão de VM de MSI?

Ao utilizar o MSI com VMs, nós o encorajamos a utilizar o ponto de extremidade IMDS de MSI. O serviço de metadados de instância do Azure é um ponto final REST acessíveis a todas as VMs de IaaS criadas através do Azure Resource Manager. Algumas das vantagens de utilizar o MSI através de IMDS são:

1. Todos os sistemas de operativos de IaaS do Azure suportada pode utilizar o MSI sobre IMDS. 
2. Já não tem de instalar uma extensão na sua VM para ativar o MSI. 
3. Os certificados utilizados por MSI já não estão presentes na VM. 
4. O ponto de extremidade IMDS é um endereço IP bem conhecido não encaminháveis internos, apenas disponível a partir da VM. 

A extensão de VM de MSI ainda está disponível para ser utilizada hoje. No entanto, mais adiante, será predefinida para utilizar o ponto de extremidade IMDS. A extensão de VM de MSI será iniciado num plano de descontinuação em breve. 

Para obter mais informações sobre o serviço de metadados de instância do Azure, consulte [documentação IMDS](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)

### <a name="what-are-the-supported-linux-distributions"></a>Quais são as distribuições suportadas de Linux?

Todas as distribuições de Linux suportadas pelo Azure IaaS podem ser utilizadas com o MSI através do ponto de extremidade IMDS. 

Nota: A extensão de VM de MSI só suporta as seguintes distribuições de Linux:
- Stable do CoreOS
- CentOS 7.1
- Red Hat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Outras distribuições do Linux não são atualmente suportadas e extensão pode falhar em distribuições não suportadas.

A extensão funciona no CentOS 6.9. No entanto, devido à falta de suporte do sistema no 6.9, a extensão será não automaticamente reinício se travasse ou parado. Reinicia quando a VM é reiniciada. Para reiniciar manualmente a extensão, consulte [como é reiniciar a extensão MSI?](#how-do-you-restart-the-msi-extension)

### <a name="how-do-you-restart-the-msi-extension"></a>Como reiniciar a extensão MSI?
No Windows e determinadas versões do Linux, se a extensão de parar, o cmdlet seguinte pode ser usado para reiniciar manualmente:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Em que: 
- Nome de extensão e tipo para Windows é: ManagedIdentityExtensionForWindows
- Nome de extensão e o tipo para Linux é: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Problemas conhecidos

### <a name="automation-script-fails-when-attempting-schema-export-for-msi-extension"></a>"Script de automação de" Falha ao tentar exportar de esquema para a extensão MSI

Quando a identidade do serviço gerido estiver ativada numa VM, o seguinte erro é apresentado durante a tentativa de utilizar a funcionalidade de "Script de automatização" para a VM ou o respetivo grupo de recursos:

![Erro de exportação de script de automação de MSI](../managed-service-identity/media/msi-known-issues/automation-script-export-error.png)

A extensão de VM de identidade de serviço gerida não suporta atualmente a capacidade de exportar o seu esquema para um modelo de grupo de recursos. Como resultado, o modelo gerado não mostra os parâmetros de configuração para ativar a identidade do serviço gerido no recurso. Estas secções podem ser adicionadas manualmente ao seguir os exemplos [configurar uma identidade de serviço gerido da VM com um modelo](qs-configure-template-windows-vm.md).

Quando a funcionalidade de exportação de esquema fica disponível para a extensão de VM de MSI, serão listado na [e grupos de recursos de exportação, que contêm as extensões de VM](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>Painel de configuração não é apresentado no portal do Azure

Se o painel de configuração de VM não aparecer na sua VM, em seguida, MSI não foi ativado no portal da sua região ainda.  Verifique novamente mais tarde.  Também pode ativar o MSI para a sua VM, utilizando [PowerShell](qs-configure-powershell-windows-vm.md) ou o [CLI do Azure](qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>Não é possível atribuir acesso a máquinas virtuais no painel de controlo de acesso (IAM)

Se **Máquina Virtual** não é apresentado no portal do Azure como uma opção para **atribuir acesso a** na **controlo de acesso (IAM)** > **adicionar permissões**, em seguida, a identidade do serviço gerido tem ainda não foi ativada no portal da sua região. Verifique novamente mais tarde.  Pode ainda selecionar a identidade do serviço gerido para a atribuição de função ao pesquisar o Principal de serviço para o MSI.  Introduza o nome da VM no **selecione** campo e o Principal de serviço é apresentado no resultado da pesquisa.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>VM falha a iniciação após a ser movidos do grupo de recursos ou subscrição

Se mover uma VM em execução, ele continua a ser executado durante a mudança. No entanto, após a mudança, se a VM é parada e reiniciada, ele irá falhar iniciar. Este problema ocorre porque a VM não está a ser atualizada a referência para a identidade MSI e continua para apontá-lo no grupo de recursos antigos.

**Solução** 
 
Acione uma atualização na VM para que ele pode obter os valores corretos para o MSI. Pode fazer uma alteração de propriedade VM para atualizar a referência para a identidade MSI. Por exemplo, pode definir um novo valor de etiqueta na VM com o seguinte comando:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Este comando define uma nova etiqueta "fixVM" com um valor de 1 na VM. 
 
Definindo essa propriedade, a VM atualiza com o URI do recurso de MSI correto e, em seguida, deve ser capaz de iniciar a VM. 
 
Assim que a VM é iniciada, a etiqueta pode ser removida ao utilizar o seguinte comando:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

## <a name="known-issues-with-user-assigned-identities"></a>Problemas conhecidos relacionados com identidades de utilizador atribuído

- Atribuições de identidade atribuída de utilizador estão apenas disponíveis para a VM e VMSS. Importante: As atribuições de identidade de utilizador atribuída serão alterados nos próximos meses.
- Duplicado atribuído as identidades dos utilizadores na mesma VM/VMSS, fará com que a VM/VMSS falhe. Isto inclui a identidades que são adicionadas com maiúsculas/minúsculas diferentes. Por exemplo, MyUserAssignedIdentity e myuserassignedidentity. 
- Aprovisionamento da extensão VM a uma VM pode falhar devido a falhas de pesquisa DNS. Reinicie a VM e tente novamente. 
- Adicionar uma identidade atribuída ao utilizador "inexistente" fará com que a VM falhe. 
- Criar um identidade com carateres especiais (por exemplo, um caráter de sublinhado) no nome do atribuída ao utilizador, não é suportada.
- Atribuídos nomes de identidade do utilizador estão limitadas a 24 carateres para o cenário de ponta a ponta. Utilizador atribuído identidades com mais de 24 carateres de nomes irão falhar a ser atribuída.
- Se utilizar a extensão de máquina virtual de identidade gerida, o limite suportado é atribuída de identidades geridas ao utilizador 32. Sem a extensão de máquina virtual de identidade gerida, o limite suportado é 512.  
- Quando adicionar um segundo utilizador atribuído a identidade, o ID de cliente pode não estar disponível para tokens de pedidos para a extensão VM. Facilitar a reiniciar a extensão de VM de MSI com os seguintes comandos de dois bash:
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler disable"`
 - `sudo bash -c "/var/lib/waagent/Microsoft.ManagedIdentity.ManagedIdentityExtensionForLinux-1.0.0.8/msi-extension-handler enable"`
- Quando uma VM tem um identidade atribuída ao utilizador, mas nenhum sistema de identidade atribuída, o portal de interface do Usuário irá mostrar MSI como desativados. Para ativar o sistema de identidade atribuído, utilize um modelo Azure Resource Manager, uma CLI do Azure ou um SDK.
