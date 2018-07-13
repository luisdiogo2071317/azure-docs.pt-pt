---
title: Criar conjuntos de dimensionamento de máquinas virtuais no Azure com o Ansible
description: Saiba como utilizar o Ansible para criar e configurar um conjunto de dimensionamento no Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, manual de comunicação social, máquina virtual, o conjunto de dimensionamento de máquina virtual, vmss
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: 5f915f7b1b425a3bd6e5d62eb70bb3f633b7eda8
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009008"
---
# <a name="create-virtual-machine-scale-sets-in-azure-using-ansible"></a>Criar conjuntos de dimensionamento de máquinas virtuais no Azure com o Ansible
Ansible permite-lhe automatizar a implementação e configuração de recursos no seu ambiente. Pode utilizar o Ansible para gerir o seu conjunto de dimensionamento de máquinas virtuais (VMSS) no Azure, o mesmo ao gerenciamento de qualquer outro recurso do Azure. Este artigo mostra-lhe como utilizar o Ansible para criar e dimensionar um conjunto de dimensionamento de máquina virtual. 

## <a name="prerequisites"></a>Pré-requisitos
- **Subscrição do Azure** – se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
- **Configurar o Ansible** - [Azure criar credenciais e configurar o Ansible](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **O Ansible e os módulos do Azure Python SDK** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)

> [!Note]
> 2.6 do Ansible é necessário para executar o seguinte os playbooks de exemplo neste tutorial. 

## <a name="create-a-vmss"></a>Criar um VMSS
Esta secção apresenta um playbook de Ansible de exemplo que define os seguintes recursos:
- **Grupo de recursos** na qual todos os seus recursos vão ser implementados
- **Rede virtual** no espaço de endereços de 10.0.0.0/16
- **Sub-rede** dentro da rede virtual
- **Endereço IP público** esse wllows-o a aceder aos recursos na Internet
- **Grupo de segurança de rede** que controla o fluxo de tráfego de rede de entrada e saída de seu conjunto de dimensionamento de máquina virtual
- **Balanceador de carga** que distribui o tráfego por um conjunto de VMs definidas através de regras do Balanceador de carga
- **Conjunto de dimensionamento de máquina virtual** que utiliza todos os recursos criados

Introduza a sua própria palavra-passe para o *admin_password* valor.

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      vmss_name: myVMSS
      vmss_lb_name: myVMSSlb
      location: eastus
      admin_username: azureuser
      admin_password: "your_password"
    tasks:
      - name: Create a resource group
        azure_rm_resourcegroup:
          name: "{{ resource_group }}"
          location: "{{ location }}"
      - name: Create virtual network
        azure_rm_virtualnetwork:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          address_prefixes: "10.0.0.0/16"
      - name: Add subnet
        azure_rm_subnet:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          address_prefix: "10.0.1.0/24"
          virtual_network: "{{ vmss_name }}"
      - name: Create public IP address
        azure_rm_publicipaddress:
          resource_group: "{{ resource_group }}"
          allocation_method: Static
          name: "{{ vmss_name }}"
      - name: Create Network Security Group that allows SSH
        azure_rm_securitygroup:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          rules:
            - name: SSH
              protocol: Tcp
              destination_port_range: 22
              access: Allow
              priority: 1001
              direction: Inbound

      - name: Create a load balancer
        azure_rm_loadbalancer:
          name: "{{ vmss_lb_name }}"
          location: "{{ location }}"
          resource_group: "{{ resource_group }}"
          public_ip: "{{ vmss_name }}"
          probe_protocol: Tcp
          probe_port: 8080
          probe_interval: 10
          probe_fail_count: 3
          protocol: Tcp
          load_distribution: Default
          frontend_port: 80
          backend_port: 8080
          idle_timeout: 4
          natpool_frontend_port_start: 50000
          natpool_frontend_port_end: 50040
          natpool_backend_port: 22
          natpool_protocol: Tcp

      - name: Create VMSS
        azure_rm_virtualmachine_scaleset:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          vm_size: Standard_DS1_v2
          admin_username: "{{ admin_username }}"
          admin_password: "{{ admin_password }}"
          ssh_password_enabled: true
          capacity: 2
          virtual_network_name: "{{ vmss_name }}"
          subnet_name: "{{ vmss_name }}"
          upgrade_policy: Manual
          tier: Standard
          managed_disk_type: Standard_LRS
          os_disk_caching: ReadWrite
          image:
            offer: UbuntuServer
            publisher: Canonical
            sku: 16.04-LTS
            version: latest
          load_balancer: "{{ vmss_lb_name }}"
          data_disks:
            - lun: 0
              disk_size_gb: 20
              managed_disk_type: Standard_LRS
              caching: ReadOnly
            - lun: 1
              disk_size_gb: 30
              managed_disk_type: Standard_LRS
              caching: ReadOnly
  ```

Para criar o ambiente de conjunto de dimensionamento de máquina virtual com o Ansible, guardar o playbook anterior como `vmss-create.yml`, ou [transferir o manual de comunicação do Ansible de exemplo](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-create.yml).

Para executar o playbook do Ansible, utilize o **ansible playbook** comando da seguinte forma:

  ```bash
  ansible-playbook vmss-create.yml
  ```

Depois de executar o playbook, o resultado semelhante ao seguinte exemplo mostra que o dimensionamento de máquinas virtuais conjunto criado com êxito:

  ```bash
  PLAY [localhost] ***********************************************************

  TASK [Gathering Facts] *****************************************************
  ok: [localhost]

  TASK [Create a resource group] ****************************************************************************
  changed: [localhost]

  TASK [Create virtual network] ****************************************************************************
  changed: [localhost]

  TASK [Add subnet] **********************************************************
  changed: [localhost]

  TASK [Create public IP address] ****************************************************************************
  changed: [localhost]

  TASK [Create Network Security Group that allows SSH] ****************************************************************************
  changed: [localhost]

  TASK [Create a load balancer] ****************************************************************************
  changed: [localhost]

  TASK [Create VMSS] *********************************************************
  changed: [localhost]

  PLAY RECAP *****************************************************************
  localhost                  : ok=8    changed=7    unreachable=0    failed=0

  ```

## <a name="scale-out-a-vmss"></a>Aumentar horizontalmente uma VMSS
O conjunto de dimensionamento de máquinas de virtuais criada tem duas instâncias. Se navega para o conjunto de dimensionamento no portal do Azure, verá **Standard_DS1_v2 (2 instâncias)**. Também pode utilizar o [Azure Cloud Shell](https://shell.azure.com/) executando o seguinte comando no Cloud Shell:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

O resultado deve ser semelhante ao seguinte:

  ```bash
  {
    "capacity": 2,
  }
  ```

Agora, vamos dimensionar de duas instâncias para três instâncias. O seguinte código de manual de comunicação do Ansible obtém informação sobre o dimensionamento de máquinas virtuais e altera a sua capacidade de duas a três. 

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      vmss_name: myVMSS
    tasks: 
      - name: Get scaleset info
        azure_rm_virtualmachine_scaleset_facts:
          resource_group: "{{ resource_group }}"
          name: "{{ vmss_name }}"
          format: curated
        register: output_scaleset

      - name: Dump scaleset info
        debug:
          var: output_scaleset

      - name: Modify scaleset (change the capacity to 3)
        set_fact:
          body: "{{ output_scaleset.ansible_facts.azure_vmss[0] | combine({'capacity': 3}, recursive=True) }}"

      - name: Update something in that VMSS
        azure_rm_virtualmachine_scaleset: "{{ body }}"
  ```

Para aumentar horizontalmente o conjunto de dimensionamento de máquina virtual que criou, guardar o playbook anterior como `vmss-scale-out.yml` ou [transferir o manual de comunicação do Ansible de exemplo](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss/vmss-scale-out.yml)). 

O seguinte comando será executado o playbook:

  ```bash
  ansible-playbook vmss-scale-out.yml
  ```

O resultado de executar o playbook do Ansible mostra que o conjunto de dimensionamento de máquina virtual foi dimensionado com êxito o:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Get scaleset info] ***************************************************************************
  ok: [localhost]

  TASK [Dump scaleset info] ***************************************************************************
  ok: [localhost] => {
      "output_scaleset": {
          "ansible_facts": {
              "azure_vmss": [
                  {
                      ......
                  }
              ]
          },
          "changed": false,
          "failed": false
      }
  }

  TASK [Modify scaleset (set upgradePolicy to Automatic and capacity to 3)] ***************************************************************************
  ok: [localhost]

  TASK [Update something in that VMSS] ***************************************************************************
  changed: [localhost]

  PLAY RECAP ****************************************************************
  localhost                  : ok=5    changed=1    unreachable=0    failed=0
  ```

Se navegar para o conjunto de dimensionamento de máquinas virtuais configuradas no portal do Azure, verá **Standard_DS1_v2 (3 instâncias)**. Também pode verificar a alteração com o [Azure Cloud Shell](https://shell.azure.com/) executando o seguinte comando:

  ```azurecli-interactive
  az vmss show -n myVMSS -g myResourceGroup --query '{"capacity":sku.capacity}' 
  ```

Os resultados da execução do comando no Cloud Shell mostra que existem agora três instâncias. 

  ```bash
  {
    "capacity": 3,
  }
  ```

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"] 
> [Manual de comunicação do Ansible exemplo para o VMSS](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)