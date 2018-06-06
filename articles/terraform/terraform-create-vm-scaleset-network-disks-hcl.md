---
title: Definir Terraform utilizado para criar um dimensionamento de máquina virtual do Azure
description: Tutorial sobre como utilizar Terraform para configurar e a versão um dimensionamento de máquina virtual do Azure, definir completo com uma rede virtual e geridos discos ligados
keywords: terraform, devops, a virtual máquina, do Azure, dimensionar conjunto, rede, armazenamento, módulos
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 06/04/2018
ms.topic: article
ms.openlocfilehash: b7cd9ad90198ead7c68d838547232429dbd1289f
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757325"
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale"></a>Utilize Terraform para criar um dimensionamento de máquina virtual do Azure

[Conjuntos de dimensionamento de máquina virtual do Azure](/azure/virtual-machine-scale-sets) permitem-lhe criar e gerir um grupo de idênticos, carregar máquinas virtuais com balanceamento onde o número de instâncias de máquina virtual pode automaticamente aumentar ou diminuir em resposta a pedido ou numa agenda definida. 

Neste tutorial, irá aprender a utilizar [Shell de nuvem do Azure](/azure/cloud-shell/overview) para realizar as seguintes tarefas:

> [!div class="checklist"]
> * Configurar uma implementação de Terraform
> * Utilizar variáveis e saídas para a implementação de Terraform 
> * Criar e implementar a infraestrutura de rede
> * Criar e implementar um conjunto de dimensionamento de máquina virtual e ligá-lo à rede
> * Criar e implementar uma jumpbox para ligar às VMs através de SSH

> [!NOTE]
> A versão mais recente do Terraform ficheiros de configuração utilizados neste artigo são no [Terraform extraordinário repositório no Github](https://github.com/Azure/awesome-terraform/tree/master/codelab-vmss).

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure**: se não tem uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

- **Instalar Terraform**: Siga as instruções no artigo, [Terraform e configurar o acesso para o Azure](/azure/virtual-machines/linux/terraform-install-configure)

- **Criar um par de chaves SSH**: Se ainda não tiver um SSH que par de chaves, siga as instruções no artigo, [como criar e utilizar um par de chaves público e privado SSH para VMs com Linux no Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys).

## <a name="create-the-directory-structure"></a>Criar a estrutura de diretórios

1. Navegue para o [portal do Azure](http://portal.azure.com).

1. Abra [em nuvem do Azure Shell](/azure/cloud-shell/overview). Se não selecionar um ambiente anteriormente, selecione **Bash** como o seu ambiente.

    ![Linha de Shell de nuvem](./media/terraform-create-vm-scaleset-network-disks-hcl/azure-portal-cloud-shell-button-min.png)

1. Altere os diretórios para o `clouddrive` diretório.

    ```bash
    cd clouddrive
    ```

1. Criar um diretório com o nome `vmss`.

    ```bash
    mkdir vmss
    ```

1. Altere os diretórios para o novo diretório:

    ```bash
    cd vmss
    ```

## <a name="create-the-variables-definitions-file"></a>Criar o ficheiro de definições de variáveis
Nesta secção, é possível definir as variáveis que personalizar os recursos criados pelo Terraform.

Na Shell de nuvem do Azure, execute os seguintes passos:

1. Crie um ficheiro denominado `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no editor:

  ```JSON
  variable "location" {
    description = "The location where resources will be created"
  }

  variable "tags" {
    description = "A map of the tags to use for the resources that are deployed"
    type        = "map"

    default = {
      environment = "codelab"
    }
  }

  variable "resource_group_name" {
    description = "The name of the resource group in which the resources will be created"
    default     = "myResourceGroup"
  }
  ```

1. Saída de modo de inserção, selecionando a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

## <a name="create-the-output-definitions-file"></a>Criar o ficheiro de definições de saída
Nesta secção, vai criar o ficheiro que descreve o resultado após a implementação.

Na Shell de nuvem do Azure, execute os seguintes passos:

1. Crie um ficheiro denominado `output.tf`.

    ```bash
    vi output.tf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no editor para expor o nome de domínio completamente qualificado (FQDN) para as máquinas virtuais. :

  ```JSON
    output "vmss_public_ip" {
        value = "${azurerm_public_ip.vmss.fqdn}"
    }
  ```

1. Saída de modo de inserção, selecionando a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

## <a name="define-the-network-infrastructure-in-a-template"></a>Definir a infraestrutura de rede num modelo
Nesta secção, crie a seguinte infraestrutura de rede num novo grupo de recursos do Azure: 

  - Uma rede virtual (VNET) com o espaço de endereços do 10.0.0.0/16 
  - Uma sub-rede com o espaço de endereços de 10.0.2.0/24
  - Dois endereços IP públicos. Um utilizado pelo balanceador de carga do conjunto a máquina virtual escala, a outro utilizado para estabelecer ligação com o jumpbox SSH.

Na Shell de nuvem do Azure, execute os seguintes passos:

1. Crie um ficheiro denominado `vmss.tf` descrever o dimensionamento da máquina virtual definir infraestrutura.

    ```bash
    vi vmss.tf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no fim do ficheiro para expor o nome de domínio completamente qualificado (FQDN) para as máquinas virtuais. 

  ```JSON
  resource "azurerm_resource_group" "vmss" {
    name     = "${var.resource_group_name}"
    location = "${var.location}"
    tags     = "${var.tags}"
  }

  resource "random_string" "fqdn" {
    length  = 6
    special = false
    upper   = false
    number  = false
  }

  resource "azurerm_virtual_network" "vmss" {
    name                = "vmss-vnet"
    address_space       = ["10.0.0.0/16"]
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    tags                = "${var.tags}"
  }

  resource "azurerm_subnet" "vmss" {
    name                 = "vmss-subnet"
    resource_group_name  = "${azurerm_resource_group.vmss.name}"
    virtual_network_name = "${azurerm_virtual_network.vmss.name}"
    address_prefix       = "10.0.2.0/24"
  }

  resource "azurerm_public_ip" "vmss" {
    name                         = "vmss-public-ip"
    location                     = "${var.location}"
    resource_group_name          = "${azurerm_resource_group.vmss.name}"
    public_ip_address_allocation = "static"
    domain_name_label            = "${random_string.fqdn.result}"
    tags                         = "${var.tags}"
  }
  ```

1. Saída de modo de inserção, selecionando a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

  ```bash
  :wq
  ```

## <a name="provision-the-network-infrastructure"></a>Aprovisionar a infraestrutura de rede
Utilizando a Shell de nuvem do Azure do diretório onde criou os ficheiros de configuração (.tf) execute os seguintes passos:

1. Inicializar Terraform.

  ```bash
  terraform init 
  ```

1. Execute o seguinte comando para implementar a infraestrutura definida no Azure.

  ```bash
  terraform apply
  ```

  Terraform pede-lhe um valor de "localização" como o **localização** variável é definida no `variables.tf`, mas nunca está definido. Pode introduzir qualquer localização válida - por exemplo, "EUA Oeste" seguido selecionando Enter. (Utilizar parênteses à volta de qualquer valor com espaços.)

1. Terraform imprima o resultado, tal como definido no `output.tf` ficheiro. Conforme mostrado na captura de ecrã seguinte, o FQDN assume a forma &lt;id >.&lt; localização >. cloudapp.azure.com. O valor de id é um valor calculado e a localização é o valor que fornece ao executar Terraform.

  ![Nome de domínio completamente qualificado para o endereço IP público do conjunto de dimensionamento da máquina virtual](./media/terraform-create-vm-scaleset-network-disks-hcl/fqdn.png)

1. No menu do portal do Azure, selecione **grupos de recursos** no menu principal.

1. No **grupos de recursos** separador, selecione **myResourceGroup** para ver os recursos que foram criados por Terraform.
  ![Recursos de rede de conjunto de dimensionamento de máquina virtual](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-resources.png)

## <a name="add-a-virtual-machine-scale-set"></a>Adicionar um conjunto de dimensionamento de máquina virtual

Nesta secção, saiba como adicionar os seguintes recursos para o modelo:

- Um balanceador de carga do Azure e as regras para servir a aplicação e anexe-o para o endereço IP público configurado anteriormente no artigo
- Conjunto de endereços de um back-end do Azure e atribua-o ao balanceador de carga 
- Uma porta de sonda de estado de funcionamento utilizada pela aplicação e configurado no balanceador de carga 
- Um conjunto junto por trás do Balanceador de carga que executa a vnet implementada anteriormente no artigo de dimensionamento de máquina virtual
- [Nginx](http://nginx.org/) em nós da escala de máquina virtual utilizando [nuvem init](http://cloudinit.readthedocs.io/en/latest/).

Na Shell de nuvem, execute os seguintes passos:

1. Abra o `vmss.tf` ficheiro de configuração.

  ```bash
  vi vmss.tf
  ```

1. Aceda ao fim do ficheiro e Inserir acrescentar modo ao selecionar a chave de um.

1. Cole o seguinte código no fim do ficheiro:

  ```JSON
  resource "azurerm_lb" "vmss" {
    name                = "vmss-lb"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"

    frontend_ip_configuration {
      name                 = "PublicIPAddress"
      public_ip_address_id = "${azurerm_public_ip.vmss.id}"
    }

    tags = "${var.tags}"
  }

  resource "azurerm_lb_backend_address_pool" "bpepool" {
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id     = "${azurerm_lb.vmss.id}"
    name                = "BackEndAddressPool"
  }

  resource "azurerm_lb_probe" "vmss" {
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id     = "${azurerm_lb.vmss.id}"
    name                = "ssh-running-probe"
    port                = "${var.application_port}"
  }

  resource "azurerm_lb_rule" "lbnatrule" {
      resource_group_name            = "${azurerm_resource_group.vmss.name}"
      loadbalancer_id                = "${azurerm_lb.vmss.id}"
      name                           = "http"
      protocol                       = "Tcp"
      frontend_port                  = "${var.application_port}"
      backend_port                   = "${var.application_port}"
      backend_address_pool_id        = "${azurerm_lb_backend_address_pool.bpepool.id}"
      frontend_ip_configuration_name = "PublicIPAddress"
      probe_id                       = "${azurerm_lb_probe.vmss.id}"
  }

  resource "azurerm_virtual_machine_scale_set" "vmss" {
    name                = "vmscaleset"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    upgrade_policy_mode = "Manual"

    sku {
      name     = "Standard_DS1_v2"
      tier     = "Standard"
      capacity = 2
    }

    storage_profile_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_profile_os_disk {
      name              = "osdisk"
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    storage_profile_data_disk {
      lun          = 0
      caching        = "ReadWrite"
      create_option  = "Empty"
      disk_size_gb   = 10
    }

    os_profile {
      computer_name_prefix = "vmlab"
      admin_username       = "${var.admin_user}"
      admin_password       = "${var.admin_password}"
      custom_data          = "${file("web.conf")}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    network_profile {
      name    = "terraformnetworkprofile"
      primary = true

      ip_configuration {
        name                                   = "IPConfiguration"
        subnet_id                              = "${azurerm_subnet.vmss.id}"
        load_balancer_backend_address_pool_ids = ["${azurerm_lb_backend_address_pool.bpepool.id}"]
      }
    }

    tags = "${var.tags}"
}
  ```

1. Saída de modo de inserção, selecionando a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Crie um ficheiro denominado `web.conf` para servir como a configuração de nuvem init para as máquinas virtuais que fazem parte do conjunto de dimensionamento. 

    ```bash
    vi web.conf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no editor:

  ```JSON
  #cloud-config
  packages:
    - nginx
  ```

1. Saída de modo de inserção, selecionando a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Abra o `variables.tf` ficheiro de configuração.

  ```bash
  vi variables.tf
  ```

1. Aceda ao fim do ficheiro e Inserir acrescentar modo ao selecionar a chave de um.

1. Personalize a implementação através da colagem o seguinte código no fim do ficheiro:

  ```JSON
  variable "application_port" {
      description = "The port that you want to expose to the external load balancer"
      default     = 80
  }

  variable "admin_user" {
      description = "User name to use as the admin account on the VMs that will be part of the VM Scale Set"
      default     = "azureuser"
  }

  variable "admin_password" {
      description = "Default password for admin account"
  }
  ``` 

1. Saída de modo de inserção, selecionando a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Crie um plano de Terraform para visualizar a implementação de conjunto de dimensionamento de máquina virtual. (Tem de especificar uma palavra-passe da sua escolha, bem como a localização para os seus recursos.)

  ```bash
  terraform plan
  ```

  O resultado do comando deve ser semelhante à captura de ecrã seguinte:

  ![Resultado da criação de conjunto de dimensionamento de máquina virtual](./media/terraform-create-vm-scaleset-network-disks-hcl/add-mvss-plan.png)

1. Implemente os novos recursos no Azure.

  ```bash
  terraform apply 
  ```

  O resultado do comando deve ser semelhante à captura de ecrã seguinte:

  ![Grupo de recursos de conjunto de dimensionamento de máquina virtual Terraform](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents.png)

1. Abra um browser e estabelecer ligação com o FQDN que foi devolvido pelo comando. 

  ![Resultados de navegação para FQDN](./media/terraform-create-vm-scaleset-network-disks-hcl/browser-fqdn.png)

## <a name="add-an-ssh-jumpbox"></a>Adicionar um jumpbox SSH
Um SSH *jumpbox* é um único servidor é "jump" através de para poder aceder a outros servidores na rede. Neste passo, configure os seguintes recursos:

- Uma interface de rede (ou jumpbox) ligado à mesma sub-rede que o conjunto de dimensionamento de máquina virtual.

- Uma máquina virtual ligada com esta interface de rede. Este jumpbox é acessível remotamente. Assim que estiver ligado, pode SSH para qualquer uma das máquinas virtuais no conjunto de dimensionamento.

1. Abra o `vmss.tf` ficheiro de configuração.

  ```bash
  vi vmss.tf
  ```

1. Aceda ao fim do ficheiro e Inserir acrescentar modo ao selecionar a chave de um.

1. Cole o seguinte código no fim do ficheiro:

  ```JSON
  resource "azurerm_public_ip" "jumpbox" {
    name                         = "jumpbox-public-ip"
    location                     = "${var.location}"
    resource_group_name          = "${azurerm_resource_group.vmss.name}"
    public_ip_address_allocation = "static"
    domain_name_label            = "${random_string.fqdn.result}-ssh"
    tags                         = "${var.tags}"
  }

  resource "azurerm_network_interface" "jumpbox" {
    name                = "jumpbox-nic"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"

    ip_configuration {
      name                          = "IPConfiguration"
      subnet_id                     = "${azurerm_subnet.vmss.id}"
      private_ip_address_allocation = "dynamic"
      public_ip_address_id          = "${azurerm_public_ip.jumpbox.id}"
    }

    tags = "${var.tags}"
  }

  resource "azurerm_virtual_machine" "jumpbox" {
    name                  = "jumpbox"
    location              = "${var.location}"
    resource_group_name   = "${azurerm_resource_group.vmss.name}"
    network_interface_ids = ["${azurerm_network_interface.jumpbox.id}"]
    vm_size               = "Standard_DS1_v2"

    storage_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_os_disk {
      name              = "jumpbox-osdisk"
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    os_profile {
      computer_name  = "jumpbox"
      admin_username = "${var.admin_user}"
      admin_password = "${var.admin_password}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    tags = "${var.tags}"
  }
  ```

1. Abra o `output.tf` ficheiro de configuração.

  ```bash
  vi output.tf
  ```

1. Aceda ao fim do ficheiro e Inserir acrescentar modo ao selecionar a chave de um.

1. Cole o seguinte código no fim do ficheiro para apresentar o nome de anfitrião do jumpbox quando a implementação está completa:

  ```
  output "jumpbox_public_ip" {
      value = "${azurerm_public_ip.jumpbox.fqdn}"
  }
  ```

1. Saída de modo de inserção, selecionando a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Implemente o jumpbox.

  ```bash
  terraform apply 
  ```

Depois de concluída a implementação, o conteúdo do grupo de recursos é semelhante que mostrado na captura de ecrã seguinte:

![Grupo de recursos de conjunto de dimensionamento de máquina virtual Terraform](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents-final.png)

> [!NOTE]
> A capacidade de iniciar sessão com uma palavra-passe está desativada no jumpbox e do conjunto de dimensionamento de máquina virtual que implementou. Inicie sessão com o SSH para aceder a AIS.

## <a name="environment-cleanup"></a>Limpeza de ambiente 

Para eliminar os recursos de Terraform que foram criados neste tutorial, introduza o seguinte comando na Shell de Cloud:

```bash
terraform destroy
```

O processo de destruição pode demorar vários minutos a concluir.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a utilizar Terraform para criar um conjunto de dimensionamento da máquina virtual do Azure. Seguem-se alguns recursos adicionais para o ajudar a saber mais sobre Terraform no Azure: 

 [Terraform Hub Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Documentação do fornecedor de Terraform Azure](http://aka.ms/terraform)  
 [Origem de fornecedor do Terraform Azure](http://aka.ms/tfgit)  
 [Módulos do Terraform Azure](http://aka.ms/tfmodules)