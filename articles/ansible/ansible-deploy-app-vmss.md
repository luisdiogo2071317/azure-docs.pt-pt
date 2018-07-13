---
title: Implementar aplicações em conjuntos de dimensionamento de máquinas virtuais no Azure com o Ansible
description: Saiba como utilizar o Ansible para configurar um conjunto de dimensionamento de máquinas virtuais e implementar a aplicação no conjunto de dimensionamento no Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, manual de comunicação social, máquina virtual, o conjunto de dimensionamento de máquina virtual, vmss
author: tomarcher
manager: jpconnock
editor: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.date: 07/11/2018
ms.author: tarcher
ms.openlocfilehash: b9c8058606e13c0db4908530e98cddb69d2caf50
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008854"
---
# <a name="deploy-applications-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Implementar aplicações em conjuntos de dimensionamento de máquinas virtuais no Azure com o Ansible
Ansible permite-lhe automatizar a implementação e configuração de recursos no seu ambiente. Pode utilizar o Ansible para implantar seus aplicativos para o Azure. Este artigo mostra-lhe como implementar uma aplicação de Java para um conjunto de dimensionamento de máquina virtual do Azure (VMSS).  

## <a name="prerequisites"></a>Pré-requisitos
- **Subscrição do Azure** – se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
- **Configurar o Ansible** - [Azure criar credenciais e configurar o Ansible](../virtual-machines/linux/ansible-install-configure.md#create-azure-credentials)
- **O Ansible e os módulos do Azure Python SDK** 
  - [CentOS 7.4](../virtual-machines/linux/ansible-install-configure.md#centos-74)
  - [Ubuntu 16.04 LTS](../virtual-machines/linux/ansible-install-configure.md#ubuntu-1604-lts)
  - [SLES 12 SP2](../virtual-machines/linux/ansible-install-configure.md#sles-12-sp2)
- **Conjunto de dimensionamento de máquina virtual** – se ainda não tiver um dimensionamento de máquinas virtuais definido, pode [criar um conjunto de dimensionamento com o Ansible](ansible-create-configure-vmss.md). 
- **Git** - [git](https://git-scm.com) é utilizada para transferir um exemplo de Java utilizado neste tutorial.
- **Java SE Development Kit (JDK)** -o JDK é usado para compilar o projeto de Java de exemplo.
- **Ferramentas de compilação do Apache Maven** – a [ferramentas de compilação do Apache Maven](https://maven.apache.org/download.cgi) são usados para compilar o projeto de Java de exemplo.

> [!Note]
> 2.6 do Ansible é necessário para executar o seguinte os playbooks de exemplo neste tutorial. 

## <a name="get-host-information"></a>Obter informações dos anfitriões

Esta secção ilustra como utilizar o Ansible para obter informações do anfitrião para um grupo de máquinas virtuais do Azure. Segue-se um manual de comunicação do Ansible de exemplo. O código obtém os endereços IP públicos e Balanceador de carga do especificado grupo de recursos e cria um grupo de anfitriões com o nome **saclesethosts** no inventário. 

Guardar o playbook de exemplo seguintes como `get-hosts-tasks.yml`: 

  ```yaml
  - name: Get facts for all Public IPs within a resource groups
    azure_rm_publicipaddress_facts:
      resource_group: "{{ resource_group }}"
    register: output_ip_address

  - name: Get loadbalancer info
    azure_rm_loadbalancer_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ loadbalancer_name }}"
    register: output

  - name: Add all hosts
    add_host:
      groups: scalesethosts
      hostname: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}_{{ item.properties.frontendPort }}"
      ansible_host: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}"
      ansible_port: "{{ item.properties.frontendPort }}"
      ansible_ssh_user: "{{ admin_username }}"
      ansible_ssh_pass: "{{ admin_password }}"
    with_items:
      - "{{ output.ansible_facts.azure_loadbalancers[0].properties.inboundNatRules }}"
  ```

## <a name="prepare-an-application-for-deployment"></a>Preparar um aplicativo para implantação  

Nesta secção, utilize o git para clonar um projeto de exemplo de Java do GitHub e crie o projeto. Guardar o playbook seguinte como `app.yml`:

  ```yaml
  - hosts: localhost
    vars:
      repo_url: https://github.com/spring-guides/gs-spring-boot.git
      workspace: ~/src/helloworld

    tasks: 
    - name: Git Clone sample app
      git:
        repo: "{{ repo_url }}"
        dest: "{{ workspace }}"

    - name: Build sample app
      shell: mvn package chdir="{{ workspace }}/complete"
  ```

Execute o manual de comunicação do Ansible de exemplo com o seguinte comando:

  ```bash
  ansible-playbook app.yml
  ```

Apresenta o resultado do comando ansible playbook saída semelhante ao seguinte, onde vê-la criado a aplicação de exemplo clonou do GitHub:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Git Clone sample app] ***************************************************************************
  changed: [localhost]

  TASK [Build sample app] ***************************************************
  changed: [localhost]

  PLAY RECAP ***************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0

  ```

## <a name="deploy-the-application-to-vmss"></a>Implementar a aplicação no VMSS

A seguinte secção em playbooks do Ansible instala o JRE (Java Runtime Environment) num grupo de anfitriões com o nome **saclesethosts**e implementa a aplicação de Java para um grupo de anfitriões com o nome **saclesethosts**: 

(Alterações a `admin_password` a sua própria palavra-passe.)

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      scaleset_name: myVMSS
      loadbalancer_name: myVMSSlb
      admin_username: azureuser
      admin_password: "your_password"
    tasks:   
    - include: get-hosts-tasks.yml

  - name: Install JRE on VMSS
    hosts: scalesethosts
    become: yes
    vars:
      workspace: ~/src/helloworld
      admin_username: azureuser

    tasks:
    - name: Install JRE
      apt:
        name: default-jre
        update_cache: yes

    - name: Copy app to Azure VM
      copy:
        src: "{{ workspace }}/complete/target/gs-spring-boot-0.1.0.jar"
        dest: "/home/{{ admin_username }}/helloworld.jar"
        force: yes
        mode: 0755

    - name: Start the application
      shell: java -jar "/home/{{ admin_username }}/helloworld.jar" >/dev/null 2>&1 &
      async: 5000
      poll: 0
  ```

Pode salvar o playbook de Ansible do exemplo anterior como `vmss-setup-deploy.yml`, ou [baixar o playbook de exemplo inteiro](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss). 

Para utilizar o ssh tipo de ligação com palavras-passe, tem de instalar o programa de sshpass. 
  - Para Ubunto 16.04, execute o comando `apt-get install sshpass`.
  - No CentOS 7.4, execute o comando `yum install sshpass`.

Poderá ver um erro, como **com uma palavra-passe SSH em vez de uma chave não é possível porque a chave de anfitrião a verificação está ativada e sshpass não o suporta.  Adicione a impressão digital este anfitrião a seu arquivo de known_hosts para gerir este anfitrião.** Se vir este erro, pode desativar a verificação, adicionando a seguinte linha a qualquer uma de chave de anfitrião a `/etc/ansible/ansible.cfg` ficheiro ou o `~/.ansible.cfg` ficheiro:
  ```bash
  [defaults]
  host_key_checking = False
  ```

Execute o playbook com o seguinte comando:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

O resultado da execução do comando do ansible playbook indica que a aplicação de Java de exemplo foi instalada para o grupo de anfitriões do conjunto de dimensionamento de máquina virtual:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Get facts for all Public IPs within a resource groups] **********************************************
  ok: [localhost]

  TASK [Get loadbalancer info] ****************************************************************************
  ok: [localhost]

  TASK [Add all hosts] *****************************************************************************
  changed: [localhost] ...

  PLAY [Install JRE on VMSS] *****************************************************************************

  TASK [Gathering Facts] *****************************************************************************
  ok: [40.114.30.145_50000]
  ok: [40.114.30.145_50003]

  TASK [Copy app to Azure VM] *****************************************************************************
  changed: [40.114.30.145_50003]
  changed: [40.114.30.145_50000]

  TASK [Start the application] ********************************************************************
  changed: [40.114.30.145_50000]
  changed: [40.114.30.145_50003]

  PLAY RECAP ************************************************************************************************
  40.114.30.145_50000        : ok=4    changed=3    unreachable=0    failed=0
  40.114.30.145_50003        : ok=4    changed=3    unreachable=0    failed=0
  localhost                  : ok=4    changed=1    unreachable=0    failed=0
  ```

Parabéns n!. A aplicação está em execução no Azure agora. Agora pode navegar para o URL do Balanceador de carga para o conjunto de dimensionamento de máquina virtual:

![Aplicação de Java em execução num conjunto de dimensionamento no Azure.](media/ansible-deploy-app-vmss/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"] 
> [Manual de comunicação do Ansible exemplo para o VMSS](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)