---
title: Definir o Terraform de utilização para criar um dimensionamento de máquina virtual do Azure
description: Tutorial sobre como utilizar o Terraform para configurar e a versão um dimensionamento de máquina virtual do Azure, definir completo com uma rede virtual e geridos de discos anexados
keywords: terraform, devops, virtual machine, do Azure, dimensionar conjunto, rede, armazenamento, módulos
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 06/04/2018
ms.topic: article
ms.openlocfilehash: 5922bad24c50a9d315aae42ce11a33801b9dbcaf
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971838"
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale-set"></a>Definir o Terraform de utilização para criar um dimensionamento de máquina virtual do Azure

[Conjuntos de dimensionamento de máquina virtual do Azure](/azure/virtual-machine-scale-sets) permitem-lhe criar e gerir um grupo de idênticos, de carga balanceadas máquinas de virtuais em que o número de instâncias de máquina virtual pode automaticamente aumente ou diminua em resposta à procura ou a uma agenda definida. 

Neste tutorial, irá aprender a utilizar [Azure Cloud Shell](/azure/cloud-shell/overview) para realizar as seguintes tarefas:

> [!div class="checklist"]
> * Configurar uma implementação do Terraform
> * Utilizar variáveis e saídas para a implementação de Terraform 
> * Criar e implementar a infraestrutura de rede
> * Criar e implementar um conjunto de dimensionamento de máquina virtual e anexá-lo à rede
> * Criar e implementar uma jumpbox para ligar às VMs através de SSH

> [!NOTE]
> A versão mais recente do Terraform ficheiros de configuração usados neste artigo estão na [Terraform Formidável repositório no Github](https://github.com/Azure/awesome-terraform/tree/master/codelab-vmss).

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure**: se não tem uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

- **Instalar o Terraform**: Siga as instruções no artigo, [Terraform e configurar o acesso ao Azure](/azure/virtual-machines/linux/terraform-install-configure)

- **Criar um par de chaves SSH**: Se ainda não tiver um SSH que par de chaves, siga as instruções no artigo [como criar e utilizar um par de chaves público e privado SSH para VMs do Linux no Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys).

## <a name="create-the-directory-structure"></a>Criar a estrutura de diretórios

1. Navegue para o [portal do Azure](http://portal.azure.com).

1. Open [Azure Cloud Shell](/azure/cloud-shell/overview). Se não tiver selecionado anteriormente um ambiente, selecione **Bash** como seu ambiente.

    ![Linha de comandos do cloud Shell](./media/terraform-create-vm-scaleset-network-disks-hcl/azure-portal-cloud-shell-button-min.png)

1. Altere os diretórios para o `clouddrive` diretório.

    ```bash
    cd clouddrive
    ```

1. Criar um diretório chamado `vmss`.

    ```bash
    mkdir vmss
    ```

1. Altere os diretórios para o novo diretório:

    ```bash
    cd vmss
    ```

## <a name="create-the-variables-definitions-file"></a>Criar o ficheiro de definições de variáveis
Nesta secção, vai definir as variáveis que personalizam os recursos criados por Terraform.

No Azure Cloud Shell, execute os seguintes passos:

1. Crie um ficheiro denominado `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Entrar no modo de inserção ao selecionar a chave.

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

1. Saída inserir modo ao selecionar a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

## <a name="create-the-output-definitions-file"></a>Criar o ficheiro de definições de saída
Nesta secção, vai criar o ficheiro que descreve o resultado após a implementação.

No Azure Cloud Shell, execute os seguintes passos:

1. Crie um ficheiro denominado `output.tf`.

    ```bash
    vi output.tf
    ```

1. Entrar no modo de inserção ao selecionar a chave.

1. Cole o seguinte código no editor para expor o nome de domínio completamente qualificado (FQDN) para as máquinas virtuais. :

  ```JSON
    output "vmss_public_ip" {
        value = "${azurerm_public_ip.vmss.fqdn}"
    }
  ```

1. Saída inserir modo ao selecionar a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

## <a name="define-the-network-infrastructure-in-a-template"></a>Definir a infraestrutura de rede num modelo
Nesta secção, vai criar a seguinte infraestrutura de rede num novo grupo de recursos do Azure: 

  - Uma rede virtual (VNET) com o espaço de endereços de 10.0.0.0/16 
  - Uma sub-rede com o espaço de endereços de 10.0.2.0/24
  - Dois endereços IP públicos. Um utilizado pelo balanceador de carga do conjunto a máquina virtual de dimensionamento, a outra utilizadas para estabelecer ligação à SSH jumpbox.

No Azure Cloud Shell, execute os seguintes passos:

1. Crie um ficheiro denominado `vmss.tf` descrever o dimensionamento de máquinas virtuais de infraestrutura definida.

    ```bash
    vi vmss.tf
    ```

1. Entrar no modo de inserção ao selecionar a chave.

1. Cole o seguinte código ao final do ficheiro para expor o nome de domínio completamente qualificado (FQDN) para as máquinas virtuais. 

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

1. Saída inserir modo ao selecionar a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

  ```bash
  :wq
  ```

## <a name="provision-the-network-infrastructure"></a>Aprovisionar a infraestrutura de rede
Utilizar o Azure Cloud Shell a partir do diretório onde criou os ficheiros de configuração (.tf) execute os seguintes passos:

1. Inicialize o Terraform.

  ```bash
  terraform init 
  ```

1. Execute o seguinte comando para implementar a infraestrutura definida no Azure.

  ```bash
  terraform apply
  ```

  Terraform pede-lhe um valor de "localização" como o **localização** variável está definida no `variables.tf`, mas nunca é definida. Pode introduzir qualquer localização válida - por exemplo, "EUA Oeste" seguido selecionando Enter. (Utilize parênteses qualquer valor com espaços.)

1. Terraform imprime a saída, conforme definido no `output.tf` ficheiro. Conforme mostrado na captura de ecrã seguinte, o FQDN assume a forma &lt;id >.&lt; localização >. cloudapp.azure.com. O valor de id é um valor calculado e localização é o valor que é fornecer ao executar o Terraform.

  ![Nome de domínio completamente qualificado para o endereço IP público do conjunto de dimensionamento de máquina virtual](./media/terraform-create-vm-scaleset-network-disks-hcl/fqdn.png)

1. No menu do portal do Azure, selecione **grupos de recursos** no menu principal.

1. Sobre o **grupos de recursos** separador, selecione **myResourceGroup** para ver os recursos que foram criados pela Terraform.
  ![Recursos de rede de conjunto de dimensionamento de máquina virtual](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-resources.png)

## <a name="add-a-virtual-machine-scale-set"></a>Adicionar um conjunto de dimensionamento de máquinas virtuais

Nesta secção, saiba como adicionar os seguintes recursos para o modelo:

- Um balanceador de carga e regras para servir a aplicação e anexá-lo para o endereço IP público configurado anteriormente no artigo
- Conjunto de endereços de um back-end do Azure e atribua-o ao balanceador de carga 
- Uma porta de sonda de estado de funcionamento usados pelo aplicativo e configurado no balanceador de carga 
- Um conjunto sentado por detrás do Balanceador de carga que é executado na VNET implementada anteriormente no artigo de dimensionamento de máquina virtual
- [Nginx](http://nginx.org/) em nós do uso de dimensionamento de máquina virtual [cloud-init](http://cloudinit.readthedocs.io/en/latest/).

No Cloud Shell, execute os seguintes passos:

1. Abra o `vmss.tf` ficheiro de configuração.

  ```bash
  vi vmss.tf
  ```

1. Vá para o fim do ficheiro e introduza o modo de acréscimo ao selecionar a chave de um.

1. Cole o seguinte código ao final do ficheiro:

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

1. Saída inserir modo ao selecionar a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Crie um ficheiro denominado `web.conf` para servir como a configuração de cloud-init para as máquinas virtuais que fazem parte do conjunto de dimensionamento. 

    ```bash
    vi web.conf
    ```

1. Entrar no modo de inserção ao selecionar a chave.

1. Cole o seguinte código no editor:

  ```JSON
  #cloud-config
  packages:
    - nginx
  ```

1. Saída inserir modo ao selecionar a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Abra o `variables.tf` ficheiro de configuração.

  ```bash
  vi variables.tf
  ```

1. Vá para o fim do ficheiro e introduza o modo de acréscimo ao selecionar a chave de um.

1. Personalize a implantação ao colar o código seguinte ao final do ficheiro:

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

1. Saída inserir modo ao selecionar a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Crie um plano de Terraform para visualizar a implementação de conjunto de dimensionamento de máquina virtual. (Tem de especificar uma palavra-passe da sua escolha, bem como a localização para os seus recursos.)

  ```bash
  terraform plan
  ```

  A saída do comando deve ser semelhante à captura de ecrã seguinte:

  ![Saída de criar o conjunto de dimensionamento de máquinas virtuais](./media/terraform-create-vm-scaleset-network-disks-hcl/add-mvss-plan.png)

1. Implemente os novos recursos no Azure.

  ```bash
  terraform apply 
  ```

  A saída do comando deve ser semelhante à captura de ecrã seguinte:

  ![Grupo de recursos do conjunto de dimensionamento de máquina virtual do Terraform](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents.png)

1. Abra um browser e ligue-se para o FQDN que foi devolvido pelo comando. 

  ![Resultados de navegação para FQDN](./media/terraform-create-vm-scaleset-network-disks-hcl/browser-fqdn.png)

## <a name="add-an-ssh-jumpbox"></a>Adicionar uma jumpbox SSH
Um SSH *jumpbox* é um único servidor que "saltar" por meio de para acessar outros servidores na rede. Neste passo, configure os seguintes recursos:

- Uma interface de rede (ou jumpbox) ligado à mesma sub-rede que o conjunto de dimensionamento de máquina virtual.

- Uma máquina virtual ligada com esta interface de rede. Este "jumpbox" é acessível remotamente. Assim que estiver ligado, pode encaminhar o SSH para qualquer uma das máquinas virtuais no conjunto de dimensionamento.

1. Abra o `vmss.tf` ficheiro de configuração.

  ```bash
  vi vmss.tf
  ```

1. Vá para o fim do ficheiro e introduza o modo de acréscimo ao selecionar a chave de um.

1. Cole o seguinte código ao final do ficheiro:

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

1. Vá para o fim do ficheiro e introduza o modo de acréscimo ao selecionar a chave de um.

1. Cole o seguinte código ao final do ficheiro para apresentar o nome do anfitrião da jumpbox quando a implementação estiver concluída:

  ```
  output "jumpbox_public_ip" {
      value = "${azurerm_public_ip.jumpbox.fqdn}"
  }
  ```

1. Saída inserir modo ao selecionar a tecla Esc.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Implemente a jumpbox.

  ```bash
  terraform apply 
  ```

Depois de concluída a implementação, o conteúdo do grupo de recursos é semelhante ao mostrado na captura de ecrã seguinte:

![Grupo de recursos do conjunto de dimensionamento de máquina virtual do Terraform](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents-final.png)

> [!NOTE]
> A capacidade de iniciar sessão com uma palavra-passe está desabilitada na jumpbox e conjunto de dimensionamento da máquina virtual que implementou. Inicie sessão com o SSH para aceder a máquinas virtuais.

## <a name="environment-cleanup"></a>Limpeza de ambiente 

Para eliminar os recursos do Terraform que foram criados neste tutorial, introduza o seguinte comando no Cloud Shell:

```bash
terraform destroy
```

O processo de destruição pode demorar vários minutos a concluir.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a utilizar o Terraform para criar um conjunto de dimensionamento de máquina virtual do Azure. Aqui estão alguns recursos adicionais para o ajudar a saber mais acerca do Terraform no Azure: 

 [Hub do Terraform no Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Documentação do fornecedor do Azure do Terraform](http://aka.ms/terraform)  
 [Origem de fornecedor do Azure do Terraform](http://aka.ms/tfgit)  
 [Módulos do Azure do Terraform](http://aka.ms/tfmodules)