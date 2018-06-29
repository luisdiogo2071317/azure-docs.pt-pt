---
title: Instalar e configurar Ansible para utilização com máquinas virtuais do Azure | Microsoft Docs
description: Saiba como instalar e configurar Ansible para gerir recursos do Azure no Ubuntu, CentOS e SLES
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/04/2018
ms.author: iainfou
ms.openlocfilehash: 262c72968ddb5985e0217bced1b83af6fdb7c03c
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052611"
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Instalar e configurar Ansible para gerir máquinas virtuais no Azure

Ansible permite-lhe automatizar a implementação e configuração de recursos no seu ambiente. Pode utilizar Ansible para gerir as máquinas virtuais (VMs) no Azure, os mesmos como faria com qualquer outro recurso. Este artigo fornece detalhes sobre como instalar Ansible e os módulos do Azure SDK do Python necessários para algumas da distros mais comuns do Linux. Pode instalar Ansible em outros distros ao ajustar os pacotes instalados para ajustar a sua plataforma específica. Para criar recursos do Azure de forma segura, também Saiba como criar e definir credenciais para Ansible a utilizar.

Para mais opções de instalação e passos para plataformas adicionais, consulte o [Ansible instalar guia](https://docs.ansible.com/ansible/intro_installation.html).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer que está a executar a CLI do Azure versão 2.0.30 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="install-ansible"></a>Instalar Ansible

Uma das formas mais fácil utilizar Ansible com o Azure é com a Shell de nuvem do Azure, uma experiência de shell baseada no browser para gerir e desenvolver recursos do Azure. Ansible é pré-instaladas na Shell de nuvem, para que possa avançar instruções sobre como instalar Ansible e aceda a [credenciais do Azure crie](#create-azure-credentials). Para obter uma lista das ferramentas adicionais também disponíveis na Shell de nuvem, consulte [funcionalidades e ferramentas para Bash na Shell de nuvem do Azure](../../cloud-shell/features.md#tools).

As instruções seguintes mostram como criar uma VM com Linux para vários distros e, em seguida, instalar Ansible. Se não precisa de criar uma VM com Linux, ignore este passo primeiro para criar um grupo de recursos do Azure. Se for necessário criar uma VM, crie primeiro um grupo de recursos com [criar grupo az](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Agora, selecione uma da distros seguintes para obter os passos sobre como criar uma VM, se necessário e, em seguida, instale Ansible:

- [CentOS 7.4](#centos-74)
- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Se necessário, crie uma VM com [az vm criar](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM chamada *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image OpenLogic:CentOS:7.4:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH para a sua VM ao utilizar o `publicIpAddress` anotou na saída da VM operação de criação:

```bash
ssh azureuser@<publicIpAddress>
```

A VM, instale os pacotes necessários para os módulos do SDK Python do Azure e Ansible da seguinte forma:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Mover para [credenciais do Azure crie](#create-azure-credentials).

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Se necessário, crie uma VM com [az vm criar](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM chamada *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH para a sua VM ao utilizar o `publicIpAddress` anotou na saída da VM operação de criação:

```bash
ssh azureuser@<publicIpAddress>
```

A VM, instale os pacotes necessários para os módulos do SDK Python do Azure e Ansible da seguinte forma:

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Mover para [credenciais do Azure crie](#create-azure-credentials).

### <a name="sles-12-sp2"></a>SLES 12 SP2

Se necessário, crie uma VM com [az vm criar](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM chamada *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image SUSE:SLES:12-SP2:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH para a sua VM ao utilizar o `publicIpAddress` anotou na saída da VM operação de criação:

```bash
ssh azureuser@<publicIpAddress>
```

A VM, instale os pacotes necessários para os módulos do SDK Python do Azure e Ansible da seguinte forma:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

Mover para [credenciais do Azure crie](#create-azure-credentials).

## <a name="create-azure-credentials"></a>Criar as credenciais do Azure

Ansible comunica com o Azure com um nome de utilizador e palavra-passe ou um principal de serviço. Um principal de serviço do Azure é uma identidade de segurança que pode utilizar com aplicações, serviços e ferramentas de automatização como Ansible. Controlar e definir as permissões para as operações que pode efetuar o principal de serviço no Azure. Para melhorar a segurança ao longo de apenas a fornecer um nome de utilizador e palavra-passe, este exemplo cria um serviço básico principal.

No computador anfitrião ou na Shell de nuvem do Azure, criar um principal de serviço através de [az ad sp criar-para-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). As credenciais que necessita de Ansible são enviadas para o ecrã:

```azurecli-interactive
az ad sp create-for-rbac --query '{"client_id": appId, "secret": password, "tenant": tenant}'
```

Segue-se um exemplo de saída a partir dos comandos do anteriores:

```json
{
  "client_id": "eec5624a-90f8-4386-8a87-02730b5410d5",
  "secret": "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Para autenticar para o Azure, também terá de obter o seu ID de subscrição do Azure utilizando [mostrar de conta az](/cli/azure/account#az-account-show):

```azurecli-interactive
az account show --query "{ subscription_id: id }"
```

Utilize o resultado destes dois comandos no próximo passo.

## <a name="create-ansible-credentials-file"></a>Criar ficheiro de credenciais Ansible

Para fornecer credenciais para Ansible, definir variáveis de ambiente ou criar um ficheiro de credenciais local. Para obter mais informações sobre como definir Ansible credenciais, consulte [fornecer credenciais para módulos do Azure](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

Para um ambiente de desenvolvimento, crie um *credenciais* ficheiro para Ansible no seu anfitrião VM. Crie um ficheiro de credenciais na VM onde instalou Ansible num passo anterior:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

O *credenciais* próprio ficheiro combina o ID de subscrição com o resultado da criação de um principal de serviço. O resultado da anterior [az ad sp criar-para-rbac](/cli/azure/ad/sp#create-for-rbac) comando é o mesmo conforme necessário para *client_id*, *segredo*, e *inquilino*. O exemplo seguinte *credenciais* ficheiro mostra estes valores que correspondam a saída anterior. Introduza os seus próprios valores da seguinte forma:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=eec5624a-90f8-4386-8a87-02730b5410d5
secret=531dcffa-3aff-4488-99bb-4816c395ea3f
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```

Guarde e feche o ficheiro.

## <a name="use-ansible-environment-variables"></a>Utilizar variáveis de ambiente de Ansible

Se pretender utilizar ferramentas como Ansible Torre ou Jenkins, terá de definir as variáveis de ambiente. Este passo pode ser ignorado se apenas pretender utilizar o cliente Ansible e o ficheiro de credenciais do Azure criado no passo anterior. As variáveis de ambiente de definir as mesmas informações que o ficheiro de credenciais do Azure:

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=eec5624a-90f8-4386-8a87-02730b5410d5
export AZURE_SECRET=531dcffa-3aff-4488-99bb-4816c395ea3f
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Passos Seguintes

Tem agora Ansible e o necessários SDK Python do Azure módulos instalados e as credenciais definidas para Ansible para utilizar. Saiba como [criar uma VM com Ansible](ansible-create-vm.md). Também pode aprender como [criar uma VM do Azure completa e que suporta a recursos com Ansible](ansible-create-complete-vm.md).
