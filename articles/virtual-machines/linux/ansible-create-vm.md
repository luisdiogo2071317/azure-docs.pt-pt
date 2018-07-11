---
title: Utilizar o Ansible para criar uma VM do Linux básica no Azure | Documentos da Microsoft
description: Saiba como utilizar o Ansible para criar e gerir uma máquina virtual do Linux básica no Azure
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
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 35dfe8348718e0edf8683f7eeddf286831697d89
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37931434"
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Criar uma máquina virtual básica no Azure com o Ansible
Ansible permite-lhe automatizar a implementação e configuração de recursos no seu ambiente. Pode utilizar o Ansible para gerir máquinas virtuais (VMs) no Azure, o mesmo como faria com qualquer outro recurso. Este artigo mostra-lhe como criar uma VM básica com o Ansible. Também pode saber como [criar um ambiente completo de VM com o Ansible](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Pré-requisitos
Para gerir recursos do Azure com o Ansible, precisa do seguinte:

- O Ansible e os módulos do Azure Python SDK instalados no sistema anfitrião.
    - Instalar o Ansible no [CentOS 7.4](ansible-install-configure.md#centos-74), [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), e [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Credenciais do Azure e o Ansible configurado para utilizá-los.
    - [Criar credenciais do Azure e configurar o Ansible](ansible-install-configure.md#create-azure-credentials)
- CLI do Azure versão 2.0.4 ou posterior. Executar `az --version` para localizar a versão. 
    - Se precisar de atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). Também pode utilizar o [Azure Cloud Shell](/azure/cloud-shell/quickstart) de seu navegador da web.


## <a name="create-supporting-azure-resources"></a>Criar recursos do Azure de suporte
Neste exemplo, vai criar um runbook que implementa uma VM numa infraestrutura existente. Primeiro, crie o grupo de recursos com [criar grupo az](/cli/azure/group#az-group-create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Criar uma rede virtual para a sua VM com [vnet de rede de az criar](/cli/azure/network/vnet#az-network-vnet-create). O exemplo seguinte cria uma rede virtual denominada *myVnet* e uma sub-rede denominada *mySubnet*:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Criar e executar o Ansible playbook
Criar playbooks do Ansible com o nome *azure_create_vm.yml* e cole o seguinte conteúdo. Este exemplo cria uma VM única e configura as credenciais SSH. Introduza os dados de chave públicos completos na *key_data* emparelhar da seguinte forma:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys: 
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.5'
        version: latest
```

Para criar a VM com o Ansible, execute o playbook da seguinte forma:

```bash
ansible-playbook azure_create_vm.yml
```

O resultado será semelhante ao seguinte exemplo mostra que a VM tiver sido criada com êxito:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a>Passos Seguintes
Este exemplo cria uma VM no grupo de recursos existente e com uma rede virtual já implementado. Para obter um exemplo mais detalhado sobre como utilizar o Ansible para criar recursos de suporte, como uma rede virtual e regras do grupo de segurança de rede, consulte [criar um ambiente completo de VM com o Ansible](ansible-create-complete-vm.md).
