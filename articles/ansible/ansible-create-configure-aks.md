---
title: Criar e configurar clusters do serviço Kubernetes do Azure no Azure com o Ansible
description: Saiba como utilizar o Ansible para criar e gerir um cluster do serviço Kubernetes do Azure no Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, aks, contentores, Kubernetes
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: 6d7c5f961256e0ae1831bd76353cadd761f4b8ac
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39009204"
---
# <a name="create-and-configure-azure-kubernetes-service-clusters-in-azure-using-ansible"></a>Criar e configurar clusters do serviço Kubernetes do Azure no Azure com o Ansible
Ansible permite-lhe automatizar a implementação e configuração de recursos no seu ambiente. Pode utilizar o Ansible para gerir o Azure Kubernetes Service (AKS). Este artigo mostra-lhe como utilizar o Ansible para criar e configurar um cluster do serviço Kubernetes do Azure.

## <a name="prerequisites"></a>Pré-requisitos
- **Subscrição do Azure** – se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
- **Configurar o Ansible** - [Azure criar credenciais e configurar o Ansible](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **O Ansible e os módulos do Azure Python SDK** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)
- **Principal de serviço do Azure** - quando [criar o principal de serviço](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal), tenha em atenção os seguintes valores: **appId**, **displayName**, **palavra-passe** , e **inquilino**.

> [!Note]
> 2.6 do Ansible é necessário para executar o seguinte os playbooks de exemplo neste tutorial. 

## <a name="create-a-managed-aks-cluster"></a>Criar um cluster do AKS gerido
O playbook de Ansible de exemplo seguinte cria um grupo de recursos e um cluster do AKS que reside no grupo de recursos:

  ```yaml
  - name: Create Azure Kubernetes Service
    hosts: localhost
    connection: local
    vars:
      resource_group: myResourceGroup
      location: eastus
      aks_name: myAKSCluster
      username: azureuser
      ssh_key: "your_ssh_key"
      client_id: "your_client_id"
      client_secret: "your_client_secret"
    tasks:
    - name: Create resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
    - name: Create a managed Azure Container Services (AKS) cluster
      azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 2
            vm_size: Standard_D2_v2
        tags:
          Environment: Production
  ```

A lista com marcas abaixo ajuda a explicar o código de manual de comunicação do Ansible anterior:
- A primeira seção **tarefas** define um grupo de recursos com o nome **myResourceGroup** dentro do **eastus** localização. 
- A segunda secção dentro **tarefas** define um cluster do AKS com o nome **myAKSCluster** dentro do **myResourceGroup** grupo de recursos. 

Para criar o cluster do AKS com o Ansible, guardar o playbook de exemplo anterior como `azure_create_aks.yml`, e executar o playbook com o seguinte comando:

  ```bash
  ansible-playbook azure_create_aks.yml
  ```

A saída do **ansible playbook* comando se parece com o seguinte que mostra que o cluster do AKS foi criado com êxito:

  ```bash
  PLAY [Create AKS] ****************************************************************************************

  TASK [Gathering Facts] ********************************************************************************************
  ok: [localhost]

  TASK [Create resource group] **************************************************************************************
  changed: [localhost]

  TASK [Create a Azure Container Services (AKS) cluster] ***************************************************
  changed: [localhost]

  PLAY RECAP *********************************************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0
  ```

## <a name="scale-aks-nodes"></a>Dimensionar nós do AKS

O playbook de exemplo na secção anterior define dois nós. Se precisar de mais ou menos cargas de trabalho de contentor no seu cluster, pode ajustar facilmente o número de nós. O playbook de exemplo nesta secção aumenta o número de nós de dois nós para três. Modificar a contagem de nós é feito alterando a **contagem** valor no **agent_pool_profiles** bloco. 

Introduza o seu próprio `ssh_key`, `client_id`, e `client_secret` no **service_principal** bloco:

```yaml
- name: Scale AKS cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Scaling an existed AKS cluster
    azure_rm_aks:
        name: "{{ aks_name }}"    
        location: "{{ location }}"
        resource_group: "{{ resource_group }}" 
        dns_prefix: "{{ aks_name }}" 
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 3
            vm_size: Standard_D2_v2
```

Para dimensionar o cluster do serviço Kubernetes do Azure com o Ansible, salvar o playbook anterior como *azure_configure_aks.yml*, e executar o playbook da seguinte forma:

  ```bash
  ansible-playbook azure_configure_aks.yml
  ```

O resultado seguinte mostra que o cluster do AKS foi criado com êxito:

  ```bash
  PLAY [Scale AKS cluster] ***************************************************************

  TASK [Gathering Facts] ******************************************************************
  ok: [localhost]

  TASK [Scaling an existed AKS cluster] **************************************************
  changed: [localhost]

  PLAY RECAP ******************************************************************************
  localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"] 
> [Tutorial: Dimensionar aplicação no Azure Kubernetes Service (AKS)](https://docs.microsoft.com/en-us/azure/aks/tutorial-kubernetes-scale)