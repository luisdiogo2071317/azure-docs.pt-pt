---
title: Utilizar o Ansible para gerir uma máquina virtual do Linux no Azure
description: Aprenda a utilizar o Ansible para gerir uma máquina virtual do Linux no Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, manual de procedimentos, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: 66106346b298fae22cce47081916a6c8eec8fd40
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/21/2018
ms.locfileid: "40250674"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>Utilizar o Ansible para gerir uma máquina virtual do Linux no Azure
O Ansible permite-lhe automatizar a implementação e a configuração de recursos no seu ambiente. Pode utilizar o Ansible para gerir máquinas virtuais do Azure, tal como faz em qualquer outro recurso. Este artigo mostra-lhe como utilizar o manual de procedimentos do Ansible para iniciar e parar uma máquina virtual do Linux. 

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure** - se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Configurar o Azure Cloud Shell** ou **Instalar e configurar o Ansible numa máquina virtual do Linux**

  **Configurar o Azure Cloud Shell**

  1. **Configurar o Azure Cloud Shell** - se estiver familiarizado com o Azure Cloud Shell, o artigo [Início rápido para Bash no Azure Cloud Shell](/azure/cloud-shell/quickstart), ilustra como iniciar e configurar o Cloud Shell. 

  1. **Máquina virtual do Linux** - se não tiver acesso a uma máquina virtual do Linux, pode [criar uma máquina virtual com o Ansible](ansible-create-vm.md).

  **--OU--**

  **Instalar e configurar o Ansible numa máquina virtual do Linux**

  1. **Instalar o Ansible** - Instale o Ansible numa [plataforma suportada do Linux](/azure/virtual-machines/linux/ansible-install-configure#install-ansible-on-an-azure-linux-virtual-machine).

  1. **Configurar o Ansible** - [Criar credenciais do Azure e configurar o Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="use-ansible-to-deallocate-stop-an-azure-virtual-machine"></a>Utilizar o Ansible para desalocar (parar) uma máquina virtual do Azure
Esta secção ilustra como utilizar o Ansible para desalocar (parar) uma máquina virtual do Azure

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Abra o [Cloud Shell](/azure/cloud-shell/overview).

1. Crie um ficheiro (para conter o manual de procedimentos) denominado `azure_vm_stop.yml` e abra-o no editor VI, da seguinte forma:

  ```azurecli-interactive
  vi azure_vm_stop.yml
  ```

1. Selecione a tecla **I** para entrar no modo de inserção.

1. Cole o seguinte código de exemplo no editor:

    ```yaml
    - name: Stop Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Deallocate the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
            allocated: no 
    ```

1. Saia do modo de inserção ao clicar na tecla **Esc**.

1. Guarde o ficheiro e saia do editor vi ao inserir o seguinte comando:

    ```bash
    :wq
    ```

1. Execute o manual de procedimentos de exemplo do Ansible.

  ```bash
  ansible-playbook azure_vm_stop.yml
  ```

1. O resultado é semelhante ao exemplo seguinte que mostra que a máquina virtual foi desalocada com êxito (parada):

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="use-ansible-to-start-a-deallocated-stopped-azure-virtual-machine"></a>Utilize o Ansible para iniciar uma máquina virtual do Azure desalocada (parada)
Esta secção ilustra como utilizar o Ansible para iniciar uma máquina virtual do Azure desalocada (parada)

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Abra o [Cloud Shell](/azure/cloud-shell/overview).

1. Crie um ficheiro (para conter o manual de procedimentos) denominado `azure_vm_start.yml` e abra-o no editor VI, da seguinte forma:

  ```azurecli-interactive
  vi azure_vm_start.yml
  ```

1. Selecione a tecla **I** para entrar no modo de inserção.

1. Cole o seguinte código de exemplo no editor:

    ```yaml
    - name: Start Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Start the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
    ```

1. Saia do modo de inserção ao clicar na tecla **Esc**.

1. Guarde o ficheiro e saia do editor vi ao inserir o seguinte comando:

    ```bash
    :wq
    ```

1. Execute o manual de procedimentos de exemplo do Ansible.

  ```bash
  ansible-playbook azure_vm_start.yml
  ```

1. O resultado será semelhante ao exemplo seguinte que mostra que a máquina virtual foi iniciada com êxito:

    O resultado será semelhante ao exemplo seguinte que mostra que a máquina virtual foi iniciada com êxito:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Utilizar o Ansible para gerir os seus inventários dinâmicos do Azure](../../ansible/ansible-manage-azure-dynamic-inventories.md)