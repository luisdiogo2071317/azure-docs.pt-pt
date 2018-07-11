---
title: Instalar e configurar o Ansible para utilização com máquinas virtuais do Azure | Documentos da Microsoft
description: Saiba como instalar e configurar o Ansible para gerir recursos do Azure no Ubuntu, CentOS e SLES
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: e7d57ead2caff87db07380582b9085b831844f1e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930074"
---
# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Instalar e configurar o Ansible para gerir máquinas virtuais no Azure

Ansible permite-lhe automatizar a implementação e configuração de recursos no seu ambiente. Pode utilizar o Ansible para gerir máquinas virtuais (VMs) no Azure, o mesmo como faria com qualquer outro recurso. Este artigo fornece detalhes sobre como instalar o Ansible e os módulos necessários do SDK de Python do Azure para algumas das distribuições de Linux mais comuns. Pode instalar o Ansible em outras distribuições ao ajustar os pacotes instalados, de acordo com sua plataforma específica. Para criar recursos do Azure de forma segura, também aprenderá como criar e definir as credenciais para o Ansible utilizar.

Para obter mais opções de instalação e os passos para plataformas adicionais, consulte a [Ansible instalar guia](https://docs.ansible.com/ansible/intro_installation.html).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este artigo requer a execução da versão 2.0.30 da CLI do Azure ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

## <a name="install-ansible"></a>Instalar o Ansible

Uma das formas mais simples de utilizar o Ansible com o Azure é com o Azure Cloud Shell, uma experiência de shell baseada no browser para gerir e desenvolver recursos do Azure. Ansible está pré-instalado no Cloud Shell, para que possa avançar instruções sobre como instalar o Ansible e aceda a [credenciais do Azure criar](#create-azure-credentials). Para obter uma lista de ferramentas adicionais também está disponíveis no Cloud Shell, consulte [funcionalidades e ferramentas para o Bash no Azure Cloud Shell](../../cloud-shell/features.md#tools).

As instruções seguintes mostram-lhe como criar uma VM do Linux para várias distribuições e, em seguida, instalar o Ansible. Se não precisa de criar uma VM do Linux, ignore este primeiro passo para criar um grupo de recursos do Azure. Se for preciso criar uma VM, crie primeiro um grupo de recursos com [criar grupo az](/cli/azure/group#az_group_create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Agora, selecione uma da distros seguintes para obter passos sobre como criar uma VM, se for necessário e, em seguida, instale o Ansible:

- [CentOS 7.4](#centos-74)
- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Se for necessário, crie uma VM com [az vm criar](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM com o nome *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image OpenLogic:CentOS:7.4:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH para a VM com o `publicIpAddress` anotou na saída da VM de criação:

```bash
ssh azureuser@<publicIpAddress>
```

Na sua VM, instale os pacotes necessários para os módulos do Azure Python SDK e o Ansible da seguinte forma:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Agora continua [credenciais do Azure criar](#create-azure-credentials).

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Se for necessário, crie uma VM com [az vm criar](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM com o nome *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image Canonical:UbuntuServer:16.04-LTS:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH para a VM com o `publicIpAddress` anotou na saída da VM de criação:

```bash
ssh azureuser@<publicIpAddress>
```

Na sua VM, instale os pacotes necessários para os módulos do Azure Python SDK e o Ansible da seguinte forma:

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Agora continua [credenciais do Azure criar](#create-azure-credentials).

### <a name="sles-12-sp2"></a>SLES 12 SP2

Se for necessário, crie uma VM com [az vm criar](/cli/azure/vm#az_vm_create). O exemplo seguinte cria uma VM com o nome *myVMAnsible*:

```azurecli
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroup \
    --image SUSE:SLES:12-SP2:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH para a VM com o `publicIpAddress` anotou na saída da VM de criação:

```bash
ssh azureuser@<publicIpAddress>
```

Na sua VM, instale os pacotes necessários para os módulos do Azure Python SDK e o Ansible da seguinte forma:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

Agora continua [credenciais do Azure criar](#create-azure-credentials).

## <a name="create-azure-credentials"></a>Criar credenciais do Azure

Ansible comunica com o Azure com um nome de utilizador e palavra-passe ou um principal de serviço. Um principal de serviço do Azure é uma identidade de segurança que pode utilizar com aplicações, serviços e ferramentas de automatização, como Ansible. Pode controlar e define as permissões em relação às quais operações o principal de serviço pode executar no Azure. Para melhorar a segurança ao longo de apenas a dar um nome de utilizador e palavra-passe, este exemplo cria um serviço básico principal.

No computador anfitrião ou no Azure Cloud Shell, criar um principal de serviço através de [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac). As credenciais que precisa do Ansible são enviadas para o ecrã:

```azurecli-interactive
az ad sp create-for-rbac --query '{"client_id": appId, "secret": password, "tenant": tenant}'
```

Segue-se um exemplo da saída nos comandos anteriores:

```json
{
  "client_id": "eec5624a-90f8-4386-8a87-02730b5410d5",
  "secret": "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47"
}
```

Para autenticar para o Azure, também tem de obter o seu ID de subscrição do Azure com [show de conta de az](/cli/azure/account#az-account-show):

```azurecli-interactive
az account show --query "{ subscription_id: id }"
```

Utilize a saída destes dois comandos no próximo passo.

## <a name="create-ansible-credentials-file"></a>Criar ficheiro de credenciais do Ansible

Para fornecer credenciais para o Ansible, definir variáveis de ambiente ou crie um ficheiro de credenciais locais. Para obter mais informações sobre como definir as credenciais do Ansible, consulte [fornecer credenciais para módulos do Azure](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules).

Para um ambiente de desenvolvimento, criar um *credenciais* ficheiro para o Ansible no seu anfitrião VM. Crie um ficheiro de credenciais na VM onde instalou o Ansible no passo anterior:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

O *credenciais* próprio ficheiro combina o ID de subscrição com a saída de criação de um principal de serviço. Saída do anterior [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) comando é igual ao necessário para *client_id*, *segredo*, e *inquilino*. O exemplo a seguir *credenciais* ficheiro mostra estes valores que correspondem a saída anterior. Introduza os seus próprios valores da seguinte forma:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=eec5624a-90f8-4386-8a87-02730b5410d5
secret=531dcffa-3aff-4488-99bb-4816c395ea3f
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```

Guarde e feche o ficheiro.

## <a name="use-ansible-environment-variables"></a>Utilizar variáveis de ambiente do Ansible

Se pretender utilizar ferramentas como o Ansible Tower ou Jenkins, terá de definir as variáveis de ambiente. Este passo pode ser ignorado se apenas pretender utilizar o cliente do Ansible e o ficheiro de credenciais do Azure criado no passo anterior. Variáveis de ambiente definem as mesmas informações que o ficheiro de credenciais do Azure:

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=eec5624a-90f8-4386-8a87-02730b5410d5
export AZURE_SECRET=531dcffa-3aff-4488-99bb-4816c395ea3f
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Passos Seguintes

Tem agora Ansible e os módulos necessários do SDK de Python do Azure instalados e credenciais definidas para o Ansible para utilizar. Saiba como [criar uma VM com o Ansible](ansible-create-vm.md). Também pode saber como [criar uma VM do Azure completa e dar suporte a recursos com o Ansible](ansible-create-complete-vm.md).
