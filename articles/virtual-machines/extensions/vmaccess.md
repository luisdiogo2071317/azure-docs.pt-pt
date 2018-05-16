---
title: Reponha o acesso a uma VM com Linux do Azure | Microsoft Docs
description: Como gerir os utilizadores administrativos e reponha o acesso em VMs do Linux utilizando a extensão VMAccess e o 2.0 CLI do Azure
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/10/2018
ms.author: danis
ms.openlocfilehash: c023f226894d2fabb90736513e49a1ecca179d4f
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="manage-administrative-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli-20"></a>Gerir utilizadores administrativos, SSH e verificação ou reparar discos em VMs do Linux com a extensão VMAccess 2.0 de CLI do Azure
## <a name="overview"></a>Descrição geral
O disco no Linux VM é Mostrar erros. Examinar repor a palavra-passe de raiz para a VM com Linux ou a chave privada SSH sejam eliminados acidentalmente. Se o que aconteceu volta nos dias do Centro de dados, terá de existir uma unidade e, em seguida, abra o KVM para obter na consola do servidor. Considere a extensão VMAccess do Azure como esse comutador KVM que lhe permite aceder à consola reponha o acesso ao Linux ou efetuar a manutenção de nível de disco.

Este artigo mostra como utilizar a extensão VMAccess do Azure para verificar ou reparar um disco, reponha o acesso de utilizador, gerir contas de utilizador administrativo ou atualizar a configuração de SSH no Linux quando estiverem a executar como máquinas virtuais do Azure Resource Manager. Se precisar de gerir máquinas virtuais clássicas - pode seguir as instruções encontradas no [clássica documentação de VM](../linux/classic/reset-access-classic.md). 

## <a name="prerequisites"></a>Pré-requisitos
### <a name="operating-system"></a>Sistema operativo

A extensão de acesso de VM pode ser executada em relação a estas distribuições do Linux:

| Distribuição | Versão |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS e 12.04 LTS |
| Debian | Debian 7.9 +, 8.2 + |
| RedHat | RHEL 6.7 + 7.1 + |
| Oracle Linux | 6.4+, 7.0+ |
| SUSE | 11 e 12 |
| OpenSuse | openSUSE bissexto 42.2 + |
| CentOS | CentOS 6.3 + 7.0 + |
| CoreOS | 494.4.0+ |

## <a name="ways-to-use-the-vmaccess-extension"></a>Formas de utilizar a extensão VMAccess
Existem duas formas que pode utilizar a extensão VMAccess em VMs do Linux:

* Utilize o 2.0 CLI do Azure e os parâmetros necessários.
* [Utilize ficheiros JSON em bruto que a extensão VMAccess processam](#use-json-files-and-the-vmaccess-extension) e, em seguida, agir em.

Os exemplos seguintes utilizam [utilizador de vm az](/cli/azure/vm/user) comandos. Para executar estes passos, terá da versão mais recente [Azure CLI 2.0](/cli/azure/install-az-cli2) instalado e registado para uma conta do Azure utilizando [início de sessão az](/cli/azure/reference-index#az_login).

## <a name="update-ssh-key"></a>Atualizar a chave SSH
O exemplo seguinte atualiza a chave SSH para o utilizador `azureuser` na VM com o nome `myVM`:

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

> **Nota:** o `az vm user update` comando acrescenta o novo texto da chave público para o `~/.ssh/authorized_keys` ficheiro para o utilizador de administrador na VM. Isto não substitui ou remover quaisquer chaves SSH existentes. Isto não irá remover as chaves anteriores definida no momento da implementação ou as atualizações subsequentes através da extensão VMAccess.

## <a name="reset-password"></a>Repor palavra-passe
O exemplo seguinte repõe a palavra-passe para o utilizador `azureuser` na VM com o nome `myVM`:

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>Reinício de SSH
O exemplo seguinte reinicia o daemon de SSH e repõe a configuração SSH para os valores predefinidos numa VM com o nome `myVM`:

```azurecli-interactive
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-an-administrativesudo-user"></a>Criar um utilizador administrativo/sudo
O exemplo seguinte cria um utilizador com o nome `myNewUser` com **sudo** permissões. A conta utiliza uma chave SSH para a autenticação na VM com o nome `myVM`. Este método é concebido para ajudar a recuperar o acesso a uma VM no caso de perda ou esquecidas credenciais atuais. Como melhor prática, contas com **sudo** permissões devem ser limitadas.

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a>Eliminar um utilizador
O exemplo seguinte elimina um utilizador com o nome `myNewUser` na VM com o nome `myVM`:

```azurecli-interactive
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```

## <a name="use-json-files-and-the-vmaccess-extension"></a>Utilize ficheiros JSON e a extensão VMAccess
Os exemplos seguintes utilizam ficheiros JSON não processados. Utilize [conjunto de extensão de vm az](/cli/azure/vm/extension#az_vm_extension_set) chamar, em seguida, os ficheiros JSON. Estes ficheiros JSON também podem ser chamados a partir de modelos do Azure. 

### <a name="reset-user-access"></a>Reponha o acesso de utilizador
Se tiver perdido o acesso de raiz na sua VM do Linux, pode iniciar um script de VMAccess para atualizar a chave SSH de um utilizador ou palavra-passe.

Para atualizar a chave pública SSH de um utilizador, crie um ficheiro denominado `update_ssh_key.json` e adicionar as definições no seguinte formato. Substitua os seus próprios valores para o `username` e `ssh_key` parâmetros:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

Execute o script de VMAccess com:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings update_ssh_key.json
```

Para repor uma palavra-passe do utilizador, crie um ficheiro denominado `reset_user_password.json` e adicionar as definições no seguinte formato. Substitua os seus próprios valores para o `username` e `password` parâmetros:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

Execute o script de VMAccess com:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>Reinício de SSH
Para reiniciar o daemon de SSH e repor a configuração SSH para os valores predefinidos, crie um ficheiro denominado `reset_sshd.json`. Adicione o seguinte conteúdo:

```json
{
  "reset_ssh": true
}
```

Execute o script de VMAccess com:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-administrative-users"></a>Gerir utilizadores administrativos

Para criar um utilizador com **sudo** permissões que utiliza uma chave SSH para a autenticação, crie um ficheiro denominado `create_new_user.json` e adicionar as definições no seguinte formato. Substitua os seus próprios valores para o `username` e `ssh_key` parâmetros. Este método é concebido para ajudar a recuperar o acesso a uma VM no caso de perda ou esquecidas credenciais atuais. Como melhor prática, contas com **sudo** permissões devem ser limitadas.

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

Execute o script de VMAccess com:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

Para eliminar um utilizador, crie um ficheiro denominado `delete_user.json` e adicione o seguinte conteúdo. Substitua o seu próprio valor para o `remove_user` parâmetro:

```json
{
  "remove_user":"myNewUser"
}
```

Execute o script de VMAccess com:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-the-disk"></a>Verificar ou reparar o disco
Utilizar o VMAccess também pode verificar e reparar um disco que adicionou à VM do Linux.

Para verificar e, em seguida, reparar o disco, crie um ficheiro denominado `disk_check_repair.json` e adicionar as definições no seguinte formato. Substitua o seu próprio valor para o nome do `repair_disk`:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

Execute o script de VMAccess com:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```
## <a name="troubleshoot-and-support"></a>Resolver problemas e suporte

### <a name="troubleshoot"></a>Resolução de problemas

É possível obter dados sobre o estado das implementações de extensão do portal do Azure e utilizando a CLI do Azure. Para ver o estado de implementação das extensões para uma determinada VM, execute o seguinte comando utilizando a CLI do Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

### <a name="support"></a>Suporte

Se precisar de mais ajuda, a qualquer altura neste artigo, pode contactar as especialistas do Azure no [fóruns do MSDN Azure e Stack Overflow](https://azure.microsoft.com/support/forums/). Em alternativa, pode ficheiro um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione o suporte de Get. Para informações sobre como utilizar o suporte do Azure, leia o [suporte do Microsoft Azure FAQ](https://azure.microsoft.com/support/faq/).
