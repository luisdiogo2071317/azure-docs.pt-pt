---
title: "Utilizar Ansible para gerir os inventários dinâmicos do Azure"
description: "Saiba como utilizar Ansible para gerir os inventários dinâmicos do Azure"
ms.service: ansible
keywords: "ansible, azure, devops, bash, cloudshell, inventário dinâmicas"
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/14/2018
ms.topic: article
ms.openlocfilehash: 799be6d2bb521de38af952376bf8ee14a18846de
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="use-ansible-to-manage-your-azure-dynamic-inventories"></a>Utilizar Ansible para gerir os inventários dinâmicos do Azure
Ansible pode ser utilizado para solicitar informações de inventário a partir de várias origens (incluindo origens de nuvem, como o Azure) para um *inventário dinâmicas*. Neste artigo, utilize o [Shell de nuvem do Azure](./ansible-run-playbook-in-cloudshell.md) para configurar um inventário dinâmicas de Ansible do Azure na qual criar duas máquinas virtuais, marcar uma dessas máquinas virtuais e instalar Nginx na máquina virtual marcada.

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure** - se não tiver uma subscrição do Azure, criar um [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

- **Credenciais do Azure** - [Azure criar credenciais e configurar Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="create-the-test-virtual-machines"></a>Criar as máquinas virtuais de teste

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Abra [nuvem Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview).

1. Crie um grupo de recursos do Azure para armazenar as máquinas virtuais para este tutorial.

    ```azurecli-interactive
    az group create --resource-group ansible-inventory-test-rg --location eastus
    ```

1. Crie duas máquinas virtuais do Linux no Azure utilizando um das técnicas seguintes:

    - **Manual de comunicação social Ansible** -o artigo [criar uma máquina virtual básica no Azure com Ansible](/azure/virtual-machines/linux/ansible-create-vm) ilustra como criar uma máquina virtual a partir de um manual de comunicação social Ansible. Se utilizar um manual de comunicação social para definir uma ou ambas as máquinas virtuais, certifique-se de que a ligação SSH é utilizada em vez de uma palavra-passe.

    - **CLI do Azure** -problema em cada um dos seguintes comandos na Shell de nuvem para criar as duas máquinas virtuais:

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm1 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

        ```azurecli-interactive
        az vm create --resource-group ansible-inventory-test-rg \
                     --name ansible-inventory-test-vm2 \
                     --image UbuntuLTS --generate-ssh-keys
        ```

## <a name="tag-a-virtual-machine"></a>Marcar uma máquina virtual
Pode [utilizar etiquetas para organizar os seus recursos do Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#azure-cli) por categorias definidas pelo utilizador. 

Introduza o seguinte [etiquetas de recurso az](/cli/azure/resource?view=azure-cli-latest.md#az_resource_tag) comando para marcar a máquina virtual `ansible-inventory-test-vm1` com a chave `nginx`:

```azurecli-interactive
az resource tag --tags nginx --id /subscriptions/<YourAzureSubscriptionID>/resourceGroups/ansible-inventory-test-rg/providers/Microsoft.Compute/virtualMachines/ansible-inventory-test-vm1
```

## <a name="generate-a-dynamic-inventory"></a>Gerar um inventário dinâmicas
Depois de ter as máquinas virtuais definido (e marcado), está na altura de gerar o inventário dinâmicas. Ansible fornece um script de Python chamado [azure_rm.py](https://github.com/ansible/ansible/blob/devel/contrib/inventory/azure_rm.py) que gera um inventário dinâmicas dos seus recursos do Azure ao efetuar pedidos de API para o Azure Resource Manager. Os seguintes passos guiá-lo utilizando o `azure_rm.py` testar o script para ligar a duas máquinas virtuais do Azure:

1. Utilize o GNU `wget` comando para obter o `azure_rm.py` script:

    ```azurecli-interactive
    wget https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/azure_rm.py
    ```

1. Utilize o `chmod` comando para alterar as permissões de acesso para o `azure_rm.py` script. O seguinte comando utiliza o `+x` parâmetro para permitir a execução (em execução) do ficheiro especificado (`azure_rm.py`):

    ```azurecli-interactive
    chmod +x azure_rm.py
    ```

1. Utilize o [ansible comando](https://docs.ansible.com/ansible/2.4/ansible.html) para ligar ao seu grupo de recursos: 

    ```azurecli-interactive
    ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
    ```

1. Assim que estiver ligado, verá os resultados semelhantes ao seguinte resultado:

    ```Output
    ansible-inventory-test-vm1 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ansible-inventory-test-vm2 | SUCCESS => {
        "changed": false,
        "failed": false,
        "ping": "pong"
    }
    ```

## <a name="enable-the-virtual-machine-tag"></a>Ativar a tag de máquina virtual
Assim que tiver configurado a tag pretendida, tem de "ativar" a etiqueta. Uma forma de ativar uma etiqueta ao exportar a tag para uma variável de ambiente denomina `AZURE_TAGS` através de **exportar** comando:

```azurecli-interactive
export AZURE_TAGS=nginx
```

Assim que a tag tenha sido exportada, pode experimentar o `ansible` novamente o comando:

```azurecli-interactive
ansible -i azure_rm.py ansible-inventory-test-rg -m ping 
```

Agora, ver apenas uma máquina virtual (um cujo tag corresponde ao valor de exportou para o **AZURE_TAGS** variável de ambiente):

```Output
ansible-inventory-test-vm1 | SUCCESS => {
    "changed": false,
    "failed": false,
    "ping": "pong"
}
```

## <a name="set-up-nginx-on-the-tagged-vm"></a>Configurar Nginx na VM marcada
O objetivo das etiquetas é ativar a capacidade de forma rápida e fácil trabalhar com os subgrupos das suas máquinas virtuais. Por exemplo, vamos supor que pretende instalar Nginx apenas em máquinas virtuais nos quais tiver atribuída uma etiqueta de `nginx`. Os passos seguintes ilustram como é fácil que está a realizar:

1. Criar um ficheiro (para conter o manual de comunicação social) com o nome `nginx.yml` da seguinte forma:

  ```azurecli-interactive
  vi nginx.yml
  ```

1. Inserir o seguinte código recém-criada `nginx.yml` ficheiro:

    ```yml
    - name: Install and start Nginx on an Azure virtual machine
    hosts: azure
    become: yes
    tasks:
    - name: install nginx
        apt: pkg=nginx state=installed
        notify:
        - start nginx

    handlers:
    - name: start nginx
        service: name=nginx state=started
    ```

1. Execute o `nginx.yml` manual de comunicação social:

  ```azurecli-interactive
  ansible-playbook -i azure_rm.py nginx.yml
  ```

1. Depois de executar o manual de comunicação social, poder ver os resultados semelhantes ao seguinte resultado:

    ```Output
    PLAY [Install and start Nginx on an Azure virtual machine] **********

    TASK [Gathering Facts] **********
    ok: [ansible-inventory-test-vm1]

    TASK [install nginx] **********
    changed: [ansible-inventory-test-vm1]

    RUNNING HANDLER [start nginx] **********
    ok: [ansible-inventory-test-vm1]

    PLAY RECAP **********
    ansible-inventory-test-vm1 : ok=3    changed=1    unreachable=0    failed=0
    ```

## <a name="test-nginx-installation"></a>Testar a instalação de Nginx
Esta secção ilustra uma técnica para testar se Nginx está instalado na sua máquina virtual.

1. Utilize o [az lista-endereços ip vm-](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az_vm_list_ip_addresses) comando para obter o endereço IP do `ansible-inventory-test-vm1` máquina virtual. O valor devolvido (endereço IP da máquina virtual), em seguida, é utilizado como parâmetro para o comando SSH para ligar à máquina virtual.

    ```azurecli-interactive
    ssh `az vm list-ip-addresses \
    -n ansible-inventory-test-vm1 \
    --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress -o tsv`
    ```

1. O [nginx - v](https://nginx.org/en/docs/switches.html) comando é geralmente utilizado para a versão de Nginx de impressão. No entanto, também pode ser utilizado para determinar se Nginx está instalado. Introduza-enquanto estiver ligado para o `ansible-inventory-test-vm1` máquina virtual.

    ```azurecli-interactive
    nginx -v
    ```

1. Depois de executar o `nginx -v` comando, consulte a versão de Nginx (segunda linha) que indica que Nginx está instalado.

    ```Output
    tom@ansible-inventory-test-vm1:~$ nginx -v

    nginx version: nginx/1.10.3 (Ubuntu)
    
    tom@ansible-inventory-test-vm1:~$
    ```

1. Prima a  **&lt;Ctrl > D** teclado combinação para desligar a sessão SSH.

1. Executar o anterior passos para o `ansible-inventory-test-vm2` máquina virtual gera uma mensagem meramente informativa que indica onde pode obter Nginx (o que significa que não o tiver instalado neste momento):

    ```Output
    tom@ansible-inventory-test-vm2:~$ nginx -v
    The program 'nginx' can be found in the following packages:
    * nginx-core
    * nginx-extras
    * nginx-full
    * nginx-lightTry: sudo apt install <selected package>
    tom@ansible-inventory-test-vm2:~$
    ```

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"] 
> [Criar uma máquina virtual básica no Azure com Ansible](/azure/virtual-machines/linux/ansible-create-vm)
