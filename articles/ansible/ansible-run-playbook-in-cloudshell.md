---
title: Executar Ansible na Shell em nuvem do Azure
description: "Saiba como efetuar várias tarefas de Ansible na Shell de nuvem do Azure"
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/14/2018
ms.topic: article
ms.openlocfilehash: d5a818616d382954d0880bcae58bb13b632ad757
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="run-ansible-in-the-azure-cloud-shell"></a>Executar Ansible na Shell em nuvem do Azure

Neste tutorial, irá aprender a efetuar várias tarefas de Ansible na Shell de nuvem do Azure. Estas tarefas incluem a ligar a uma máquina virtual e criar Ansible playbooks para criar e eliminar um grupo de recursos do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure** - se não tiver uma subscrição do Azure, criar um [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

- **Credenciais do Azure** - [Azure criar credenciais e configurar Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

- **Configurar a Shell de nuvem do Azure** - se estiver familiarizado com a Shell de nuvem do Azure, o artigo [início rápido para Bash na Shell de nuvem do Azure](https://docs.microsoft.com/en-us/azure/cloud-shell/quickstart) ilustra como iniciar e configurar a Shell de nuvem.

## <a name="use-ansible-to-connect-to-a-vm"></a>Utilizar Ansible para ligar a uma VM
Ansible criou um script de Python chamado [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) que gera um inventário dinâmicas dos seus recursos do Azure ao efetuar pedidos de API para o Azure Resource Manager. Os seguintes passos guiá-lo utilizando o `azure_rm.py` script para ligar a uma máquina virtual do Azure:

1. Abra a Shell de nuvem do Azure.

1. Se não tiver uma máquina virtual para utilizar, introduza os seguintes comandos para a Shell de nuvem para criar uma máquina virtual com o qual pretende testar:

  ```azurecli-interactive
  az group create --resource-group ansible-test-rg --location eastus
  ```

  ```azurecli-interactive
  az vm create --resource-group ansible-test-rg --name ansible-test-vm --image UbuntuLTS --generate-ssh-keys
  ```

1. Utilize o GNU `wget` comando para obter o `azure_rm.py` script:

  ```azurecli-interactive
  wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
  ```

1. Utilize o `chmod` comando para alterar as permissões de acesso para o `azure_rm.py` script. O seguinte comando utiliza o `+x` parâmetro para permitir a execução (em execução) do ficheiro especificado (`azure_rm.py`):

  ```azurecli-interactive
  chmod +x azure_rm.py
  ```

1. Utilize o [ansible comando](https://docs.ansible.com/ansible/2.4/ansible.html) para ligar à máquina virtual: 

  ```azurecli-interactive
  ansible -i azure_rm.py ansible-test-vm -m ping
  ```

  Assim que estiver ligado, deverá ver resultados semelhantes à saída de:

  ```Output
  The authenticity of host 'nn.nnn.nn.nn (nn.nnn.nn.nn)' can't be established.
  ECDSA key fingerprint is SHA256:&lt;some value>.
  Are you sure you want to continue connecting (yes/no)? yes
  test-ansible-vm | SUCCESS => {
      "changed": false,
      "failed": false,
      "ping": "pong"
  }
  ```

1. Se tiver criado um grupo de recursos e a máquina virtual nesta secção

  ```azurecli-interactive
  az group delete -n <resourceGroup>
  ```

## <a name="run-a-playbook-in-cloud-shell"></a>Executar um manual de comunicação social na Shell de nuvem
O [manual de comunicação social ansible](https://docs.ansible.com/ansible/2.4/ansible-playbook.html) comando executa playbooks Ansible, as tarefas em execução em anfitriões de destino. Esta secção explica como utilizar a Shell de nuvem para criar e executar dois playbooks - uma para criar um grupo de recursos e um segundo para eliminar o grupo de recursos. 

1. Crie um ficheiro denominado `rg.yml` da seguinte forma:

  ```azurecli-interactive
  vi rg.yml
  ```

1. Copie o seguinte conteúdo para a janela da Shell de nuvem (agora a alojar uma instância do VI editor):

  ```yml
  - name: My first Ansible Playbook
    hosts: localhost
    connection: local
    tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
          name: demoresourcegroup
          location: eastus
  ```

1. Guarde o ficheiro e saia do editor de VI introduzindo `:wq` e premindo &lt;Enter >.

1. Utilize o `ansible-playbook` comando a executar o `rg.yml` manual de comunicação social:

  ```azurecli-interactive
  ansible-playbook rg.yml
  ```

1. Deverá ver resultados semelhantes ao seguinte resultado:

  ```Output
  PLAY [My first Ansible Playbook] **********

  TASK [Gathering Facts] **********
  ok: [localhost]

  TASK [Create a resource group] **********
  changed: [localhost]

  PLAY RECAP **********
  localhost : ok=2 changed=1 unreachable=0 failed=0
  ```

1. Verificar a implementação:

  ```azurecli-interactive
  az group show -n demoresourcegroup
  ```

1. Agora que criou o grupo de recursos, crie uma segunda manual de Ansible para eliminar o grupo de recursos:

  ```azurecli-interactive
  vi rg2.yml
  ```

1. Copie o seguinte conteúdo para a janela da Shell de nuvem (agora a alojar uma instância do VI editor):

  ```yml
  - name: My second Ansible Playbook
    hosts: localhost
    connection: local
    tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
          name: demoresourcegroup
          state: absent
  ```

1. Guarde o ficheiro e saia do editor de VI introduzindo `:wq` e premindo &lt;Enter >.

1. Utilize o `ansible-playbook` comando a executar o `rg2.yml` manual de comunicação social:

  ```azurecli-interactive
  ansible-playbook rg.yml
  ```

1. Deverá ver resultados semelhantes ao seguinte resultado:

  ```Output
  The output is as following. 
  PLAY [My second Ansible Playbook] **********

  TASK [Gathering Facts] **********
  ok: [localhost]

  TASK [Delete a resource group] **********
  changed: [localhost]

  PLAY RECAP **********
  localhost : ok=2 changed=1 unreachable=0 failed=0
  ```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"] 
> [Criar uma máquina virtual básica no Azure com Ansible](/azure/virtual-machines/linux/ansible-create-vm)