---
title: Inicie sessão para uma VM com Linux com credenciais do Azure Active Directory | Microsoft Docs
description: Este howto, irá aprender a criar e configurar uma VM com Linux para utilizar a autenticação do Azure Active Directory para inícios de sessão do utilizador
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2018
ms.author: iainfou
ms.openlocfilehash: 652f9867b7423ce4307dba1c77e8f38fcd596c67
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="log-in-to-a-linux-virtual-machine-in-azure-using-azure-active-directory-authentication-preview"></a>Inicie sessão na máquina virtual com Linux no Azure utilizando a autenticação do Azure Active Directory (pré-visualização)

Para melhorar a segurança de máquinas de virtuais (VMs) com Linux no Azure, pode integrar com a autenticação do Azure Active Directory (AD). Quando utiliza a autenticação do Azure AD para VMs com Linux, centralmente controlar e impor políticas que permitem ou negam o acesso às VMs. Este artigo mostra como criar e configurar uma VM com Linux para utilizar a autenticação do Azure AD.

> [!NOTE]
> Esta funcionalidade está em pré-visualização e não é recomendada para utilização com as máquinas virtuais de produção ou cargas de trabalho. Utilize esta funcionalidade numa máquina virtual de teste que pretende eliminar depois de teste.

Existem várias vantagens da utilização de autenticação do Azure AD para início de sessão para VMs com Linux no Azure, incluindo:

- **Segurança melhorada:**
  - Pode utilizar credenciais AD da sua empresa para iniciar sessão VMs do Linux do Azure. Não é necessário para criar contas de administrador local e gerir a duração da credencial.
  - Ao reduzir a dependência em contas de administrador local, não precisa de preocupar com perda/roubo de credenciais, os utilizadores a configurar credenciais fracas, etc.
  - A complexidade de palavra-passe e as políticas de duração de palavra-passe configuradas para o diretório do Azure AD ajudam a proteger VMs com Linux, bem como.
  - Para início de sessão mais seguro para máquinas virtuais do Azure, pode configurar a autenticação multifator.

- **Colaboração totalmente integrada:** controlo de acesso With Role-Based (RBAC), pode especificar quem pode iniciar sessão para uma VM indicada como um utilizador normal ou com privilégios de administrador. Quando os utilizadores aderir ou sair de equipa, pode atualizar a política RBAC para a VM conceder acesso conforme apropriado. Esta experiência é muito mais simples do que ter de eliminar as VMs para remover as de chaves públicas SSH desnecessárias. Quando os funcionários deixam a sua organização e a conta de utilizador é desativada ou removida do Azure AD, já não ter acesso aos recursos.

### <a name="supported-azure-regions-and-linux-distributions"></a>Regiões do Azure e as distribuições do Linux suportados

As distribuições de Linux seguintes são atualmente suportadas durante a pré-visualização desta funcionalidade:

| Distribuição | Versão |
| --- | --- |
| CentOS | CentOS 6.9 e CentOS 7.4 |
| VM de RedHat Enterprise Linux | RHEL 7 | 
| Ubuntu Server | Ubuntu 14.04 LTS, Ubuntu Server 16.04 e Ubuntu Server 17.10 |

As seguintes regiões do Azure atualmente são suportadas durante a pré-visualização desta funcionalidade:

- Todas as regiões do Azure públicas

>[!IMPORTANT]
> Para utilizar esta funcionalidade de pré-visualização, implementar apenas uma distro suportada de Linux e numa região do Azure suportada. A funcionalidade não é suportada no Azure Government ou sovereign nuvens.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial, necessita que está a executar a CLI do Azure versão 2.0.31 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="create-a-linux-virtual-machine"></a>Criar uma Máquina Virtual do Linux

Criar um grupo de recursos com [criar grupo az](/cli/azure/group#az-group-create), em seguida, crie uma VM com [az vm criar](/cli/azure/vm#az-vm-create) utilizando um distro suportado e numa região suportada. O exemplo seguinte implementa uma VM chamada *myVM* que utiliza *Ubuntu 16.04 LTS* para um grupo de recursos denominado *myResourceGroup* no *southcentralus*  região. Nos exemplos a seguir, pode fornecer o seu grupo de recursos e nomes VM conforme necessário.

```azurecli-interactive
az group create --name myResourceGroup --location southcentralus

az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Demora alguns minutos para criar a VM e os recursos de suporte.

## <a name="install-the-azure-ad-login-vm-extension"></a>Instalar o início de sessão do Azure AD extensão da VM

Para iniciar sessão para uma VM com Linux com credenciais do Azure AD, instale o registo do Azure Active Directory na extensão VM. Extensões VM são aplicações de pequenas a indicar tarefas de configuração e a automatização de pós-implementação em máquinas virtuais do Azure. Utilize [conjunto de extensão de vm az](/cli/azure/vm/extension#az-vm-extension-set) para instalar o *AADLoginForLinux* extensão da VM com o nome *myVM* no *myResourceGroup* recursos grupo:

```azurecli-interactive
az vm extension set \
    --publisher Microsoft.Azure.ActiveDirectory.LinuxSSH \
    --name AADLoginForLinux \
    --resource-group myResourceGroup \
    --vm-name myVM
```

O *provisioningState* de *com êxito* é apresentada assim que a extensão é instalada na VM.

## <a name="configure-role-assignments-for-the-vm"></a>Configurar as atribuições de função da VM

Política de controlo de acesso baseado em funções (RBAC) do Azure determina quem pode iniciar sessão para a VM. Duas funções RBAC são utilizadas para autorizar o início de sessão VM:

- **Início de sessão do administrador do Virtual Machine**: os utilizadores com esta função atribuída podem iniciar sessão a uma máquina virtual do Azure com privilégios de utilizador de raiz do administrador do Windows ou Linux.
- **Início de sessão de utilizador de máquina virtual**: os utilizadores com esta função atribuída podem iniciar sessão a uma máquina virtual do Azure com privilégios de utilizador normais.

> [!NOTE]
> Para permitir que um utilizador inicie sessão VM através de SSH, tem de atribuir um o *início de sessão do administrador do Virtual Machine* ou *início de sessão de utilizador de Máquina Virtual* função. Um utilizador do Azure com o *proprietário* ou *contribuinte* funções atribuídas para uma VM não automaticamente dispõe de privilégios de início de sessão para a VM através de SSH.

O exemplo seguinte utiliza [de criação da atribuição de função az](/cli/azure/role/assignment#az-role-assignment-create) para atribuir a *início de sessão do administrador do Virtual Machine* função para a VM para o utilizador do Azure atual. O nome de utilizador da sua conta do Azure Active Directory é obtida com [mostrar de conta az](/cli/azure/account#az-account-show)e o *âmbito* está definido para a VM criada no passo anterior com [mostrar de vm az](/cli/azure/vm#az-vm-show). O âmbito também foi possível atribuir a um nível de grupo ou de subscrição de recursos e aplicam permissões de herança RBAC normais. Para obter mais informações, consulte [controlos de acesso baseado em funções](../../azure-resource-manager/resource-group-overview.md#access-control)

```azurecli-interactive
username=$(az account show --query user.name --output tsv)
vm=$(az vm show --resource-group myResourceGroup --name myVM --query id -o tsv)

az role assignment create \
    --role "Virtual Machine Administrator Login" \
    --assignee $username \
    --scope $vm
```

Para obter mais informações sobre como utilizar o RBAC para gerir o acesso aos recursos da sua subscrição do Azure, consulte utilizar o [Azure CLI 2.0](../../role-based-access-control/role-assignments-cli.md), [portal do Azure](../../role-based-access-control/role-assignments-portal.md), ou [Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

Também pode configurar o Azure AD para exigir a autenticação multifator para um utilizador específico iniciar sessão para a máquina virtual Linux. Para obter mais informações, consulte [introdução ao Azure multi-factor Authentication na nuvem](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="log-in-to-the-linux-virtual-machine"></a>Inicie sessão na máquina de virtual com Linux

Em primeiro lugar, ver o endereço IP público da sua VM com [mostrar de vm az](/cli/azure/vm#az-vm-show):

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Inicie sessão máquina virtual Linux do Azure com as suas credenciais do Azure AD. O `-l` parâmetro permite-lhe especificar os seus próprios endereços de conta do Azure AD. Especifique o endereço IP público da sua VM como saída no comando anterior:

```azurecli-interactive
ssh -l azureuser@contoso.onmicrosoft.com publicIps
```

Lhe for pedido para iniciar sessão Azure AD com um código de utilização única no [ https://microsoft.com/devicelogin ](https://microsoft.com/devicelogin). Copie e cole o código de utilização única a página de início de sessão de dispositivo, conforme mostrado no exemplo seguinte:

```bash
~$ ssh -l azureuser@contoso.onmicrosoft.com 13.65.237.247
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJS3K6X4D to authenticate. Press ENTER when ready.
```

Quando lhe for pedido, introduza as credenciais de início de sessão do AD do Azure na página de início de sessão. A seguinte mensagem é apresentada no browser quando tem de ser autenticadas com êxito:

    You have signed in to the Microsoft Azure Linux Virtual Machine Sign-In application on your device.

Feche a janela do browser, voltar para o SSH linha de comandos e prima a **Enter** chave. Tem agora sessão iniciada para a máquina virtual do Linux do Azure com as permissões de função conforme atribuído, tais como *VM utilizador* ou *VM administrador*. Se a sua conta de utilizador está atribuída a *início de sessão do administrador do Virtual Machine* função, pode utilizar o `sudo` para executar comandos que necessitam de privilégios de raiz.

## <a name="troubleshoot-sign-in-issues"></a>Resolver problemas de início de sessão

Alguns erros comuns ao tentar SSH com credenciais do Azure AD incluem não funções do RBAC atribuídas e repetido pedidos para iniciar sessão. Utilize as secções seguintes para corrigir estes problemas.

### <a name="access-denied-rbac-role-not-assigned"></a>Acesso negado: função RBAC não atribuída

Se vir o seguinte erro na sua linha SSH, certifique-se de que tem [RBAC políticas configuradas](#configure-rbac-policy-for-the-virtual-machine) para a VM que concede o utilizador está a *início de sessão do administrador do Virtual Machine* ou *Virtual Início de sessão do utilizador do computador* função:

```bash
login as: azureuser@contoso.onmicrosoft.com
Using keyboard-interactive authentication.
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJX327AXD to authenticate. Press ENTER when ready.
Using keyboard-interactive authentication.
Access denied:  to sign-in you be assigned a role with action 'Microsoft.Compute/virtualMachines/login/action', for example 'Virtual Machine User Login'
Access denied
```

### <a name="continued-ssh-sign-in-prompts"></a>SSH contínuo início de sessão dos avisos

Se concluir com êxito o passo de autenticação num web browser, poderá ser imediatamente a pedido para voltar a iniciar sessão com um código de raiz. Normalmente, este erro é causado por um erro de correspondência entre o nome de início de sessão que especificou na linha de SSH e a conta que iniciou sessão com o Azure AD com. Para corrigir este problema:

- Certifique-se de que o nome de início de sessão que especificou na linha de SSH está correto. Um erro de digitação no nome de início de sessão pode causar um erro de correspondência entre o nome de início de sessão que especificou na linha de SSH e a conta que iniciou sessão com o Azure AD com. Por exemplo, o que escreveu *azuresuer@contoso.onmicrosoft.com* em vez de *azureuser@contoso.onmicrosoft.com*.
- Se tiver múltiplas contas de utilizador, certifique-se que não fornecer uma conta de utilizador diferente na janela do browser quando iniciar sessão no Azure AD.
- Linux é um sistema de operativo maiúsculas e minúsculas. Há uma diferença entre 'Azureuser@contoso.onmicrosoft.com'e'azureuser@contoso.onmicrosoft.com', que pode causar um erro de correspondência. Certifique-se de que especificou o UPN com a sensibilidade correta na linha de SSH.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure Active Directory, consulte [que é o Azure Active Directory](../../active-directory/active-directory-whatis.md) e [como começar com o Azure Active Directory](../../active-directory/get-started-azure-ad.md)
