---
title: Utilizar o Ansible para criar uma VM de Linux completa no Azure | Documentos da Microsoft
description: Saiba como utilizar o Ansible para criar e gerir um ambiente completo de máquina virtual do Linux no Azure
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
ms.openlocfilehash: 63228f8bf8729f1bf3796a77516490ae7088d5ed
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37930849"
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a>Criar um ambiente completo de máquina virtual do Linux no Azure com o Ansible
Ansible permite-lhe automatizar a implementação e configuração de recursos no seu ambiente. Pode utilizar o Ansible para gerir máquinas virtuais (VMs) no Azure, o mesmo como faria com qualquer outro recurso. Este artigo mostra-lhe como criar um ambiente completo do Linux e de recursos com o Ansible de suporte. Também pode saber como [criar uma VM básica com o Ansible](ansible-create-vm.md).


## <a name="prerequisites"></a>Pré-requisitos
Para gerir recursos do Azure com o Ansible, precisa do seguinte:

- O Ansible e os módulos do Azure Python SDK instalados no sistema anfitrião.
    - Instalar o Ansible no [CentOS 7.4](ansible-install-configure.md#centos-74), [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), e [SLES 12 SP2](ansible-install-configure.md#sles-12-sp2)
- Credenciais do Azure e o Ansible configurado para utilizá-los.
    - [Criar credenciais do Azure e configurar o Ansible](ansible-install-configure.md#create-azure-credentials)
- CLI do Azure versão 2.0.4 ou posterior. Executar `az --version` para localizar a versão. 
    - Se precisar de atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli). Também pode utilizar [Cloud Shell](/azure/cloud-shell/quickstart) partir do seu browser.


## <a name="create-virtual-network"></a>Criar a rede virtual
Vamos examinar cada seção do playbooks do Ansible e criar recursos do Azure individuais. Para o playbook completado, consulte [esta secção do artigo](#complete-ansible-playbook).

A seguinte secção em playbooks do Ansible cria uma rede virtual denominada *myVnet* no *10.0.0.0/16* espaço de endereços:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.0.0.0/16"
```

Para adicionar uma sub-rede, a seção a seguir cria uma sub-rede denominada *mySubnet* no *myVnet* rede virtual:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a>Criar endereço IP público
Para acessar recursos na Internet, criar e atribuir um endereço IP público à VM. A seguinte secção em playbooks do Ansible cria um endereço IP público com o nome *myPublicIP*:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a>Criar grupo de segurança de rede
Grupos de segurança de rede controlar o fluxo de tráfego de rede e para a sua VM. A seguinte secção em playbooks do Ansible cria um grupo de segurança de rede com o nome *myNetworkSecurityGroup* e define uma regra para permitir o tráfego SSH na porta TCP 22:

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: Tcp
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-virtual-network-interface-card"></a>Criar placa de interface de rede virtual
Uma placa de interface de rede virtual (NIC) liga-se a sua VM para uma determinada rede virtual, o endereço IP público e o grupo de segurança de rede. A seguinte secção em playbooks do Ansible cria uma NIC virtual com o nome *myNIC* ligado para os recursos de rede virtual que criou:

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```


## <a name="create-virtual-machine"></a>Criar a máquina virtual
A etapa final é criar uma VM e utilizar todos os recursos criados. A seguinte secção em playbooks do Ansible cria uma VM com o nome *myVM* e anexa o NIC virtual com o nome *myNIC*. Introduza os dados de chave públicos completos na *key_data* emparelhar da seguinte forma:

```yaml
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
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.5'
      version: latest
```

## <a name="complete-ansible-playbook"></a>Concluir o manual de comunicação do Ansible
Para reunir todas estas secções, criar playbooks do Ansible com o nome *azure_create_complete_vm.yml* e cole o seguinte conteúdo. Introduza os dados de chave públicos completos na *key_data* par:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
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
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.5'
        version: latest
```

Ansible precisa para implementar todos os seus recursos num grupo de recursos. Crie um grupo de recursos com [az group create](/cli/azure/group#az-group-create). O exemplo seguinte cria um grupo de recursos com o nome *myResourceGroup* na localização *eastus*:

```azurecli
az group create --name myResourceGroup --location eastus
```

Para criar o ambiente de VM completado com o Ansible, execute o playbook da seguinte forma:

```bash
ansible-playbook azure_create_complete_vm.yml
```

O resultado será semelhante ao seguinte exemplo mostra que a VM tiver sido criada com êxito:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create virtual network] *********************************************
changed: [localhost]

TASK [Add subnet] *********************************************************
changed: [localhost]

TASK [Create public IP address] *******************************************
changed: [localhost]

TASK [Create Network Security Group that allows SSH] **********************
changed: [localhost]

TASK [Create virtual network inteface card] *******************************
changed: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=7    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Passos Seguintes
Este exemplo cria um ambiente de VM completo, incluindo os recursos de rede virtual necessários. Para obter um exemplo mais direto criar uma VM nos recursos de rede existente com as opções predefinidas, consulte [criar uma VM](ansible-create-vm.md).
