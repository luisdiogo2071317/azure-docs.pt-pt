---
title: Criar um cluster de Kubernetes com o serviço de Kubernetes do Azure (AKS) e Terraform
description: Tutorial que ilustra como criar um Cluster de Kubernetes com o serviço do Azure Kubernetes e Terraform
keywords: terraform devops, máquina virtual, azure, kubernetes
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 06/11/2018
ms.topic: article
ms.openlocfilehash: bd00a0cc8446802a03570edd58949a46c0769101
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35304204"
---
# <a name="create-a-kubernetes-cluster-with-azure-kubernetes-service-and-terraform"></a>Criar um cluster de Kubernetes com o serviço do Azure Kubernetes e Terraform
[Serviço de Kubernetes do Azure (AKS)](/azure/aks/) gere o seu ambiente alojado do Kubernetes, tornar mais rápida e fácil de implementar e gerir aplicações de sem conhecimentos de orquestração do contentor. Também põe fim às tarefas de operações e manutenções contínuas ao aprovisionar, atualizar e dimensionar recursos a pedido, sem que as aplicações sejam colocadas offline.

Neste tutorial, irá aprender a realizar as seguintes tarefas na criação de um [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) cluster utilizando [Terraform](http://terraform.io) e AKS:

> [!div class="checklist"]
> * Utilizar HCL (HashiCorp Language) para definir um cluster de Kubernetes
> * Utilizar Terraform e AKS para criar um cluster de Kubernetes
> * Utilize a ferramenta de kubectl para testar a disponibilidade de um cluster de Kubernetes

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure**: se não tem uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

- **Configurar Terraform**: Siga as instruções no artigo, [Terraform e configurar o acesso para o Azure](/azure/virtual-machines/linux/terraform-install-configure)

- **Principal de serviço do Azure**: Siga as indicações na secção de **criar o principal de serviço** secção do artigo, [criar um Azure principal de serviço com o Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal). Tome nota dos valores para o appId, displayName, palavra-passe e inquilinos.

## <a name="create-the-directory-structure"></a>Criar a estrutura de diretórios
O primeiro passo é criar o diretório que contém os ficheiros de configuração Terraform para o exercício.

1. Navegue para o [portal do Azure](http://portal.azure.com).

1. Abra [em nuvem do Azure Shell](/azure/cloud-shell/overview). Se não selecionar um ambiente anteriormente, selecione **Bash** como o seu ambiente.

    ![Linha de Shell de nuvem](./media/terraform-create-k8s-cluster-with-tf-and-aks/azure-portal-cloud-shell-button-min.png)

1. Altere os diretórios para o `clouddrive` diretório.

    ```bash
    cd clouddrive
    ```

1. Criar um diretório com o nome `terraform-aks-k8s`.

    ```bash
    mkdir terraform-aks-k8s
    ```

1. Altere os diretórios para o novo diretório:

    ```bash
    cd terraform-aks-k8s
    ```

## <a name="declare-the-azure-provider"></a>Declarar o fornecedor do Azure
Crie o ficheiro de configuração de Terraform que declara o fornecedor do Azure.

1. Na Shell de nuvem, crie um ficheiro denominado `main.tf`.

    ```bash
    vi main.tf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no editor:

    ```JSON
    provider "azurerm" {
        version = "=1.5.0"
    }

    terraform {
        backend "azurerm" {}
    }

    ```

1. Modo de inserção de saída, selecionando o **Esc** chave.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

## <a name="define-a-kubernetes-cluster"></a>Definir um cluster de Kubernetes
Crie o ficheiro de configuração de Terraform declara os recursos para o cluster Kubernetes.

1. Na Shell de nuvem, crie um ficheiro denominado `k8s.tf`.

    ```bash
    vi k8s.tf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no editor:

    ```JSON
    resource "azurerm_resource_group" "k8s" {
        name     = "${var.resource_group_name}"
        location = "${var.location}"
    }

    resource "azurerm_kubernetes_cluster" "k8s" {
        name                = "${var.cluster_name}"
        location            = "${azurerm_resource_group.k8s.location}"
        resource_group_name = "${azurerm_resource_group.k8s.name}"
        dns_prefix          = "${var.dns_prefix}"

        linux_profile {
            admin_username = "ubuntu"

            ssh_key {
            key_data = "${file("${var.ssh_public_key}")}"
            }
        }

        agent_pool_profile {
            name            = "default"
            count           = "${var.agent_count}"
            vm_size         = "Standard_D2"
            os_type         = "Linux"
            os_disk_size_gb = 30
        }

        service_principal {
            client_id     = "${var.client_id}"
            client_secret = "${var.client_secret}"
        }

        tags {
            Environment = "Development"
        }
    }
    ```

    O código anterior define o nome do cluster, a localização e o resource_group_name. Além disso, o valor de dns_prefix - o que faz parte do nome de domínio completamente qualificado (FQDN) utilizado para aceder ao cluster - está definido.

    O **linux_profile** registo permite-lhe configurar as definições que permitem a assinatura para nós de trabalho através de SSH.

    Com AKS, paga apenas os nós de trabalho. O **agent_pool_profile** registo configura os detalhes para estes nós de trabalho. O **agent_pool_profile registo** inclui o número de nós de trabalho para criar e o tipo de nós de trabalho. Se precisar de aumentar verticalmente ou reduzir verticalmente o cluster no futuro, modifique o **contagem** valor neste registo.

1. Modo de inserção de saída, selecionando o **Esc** chave.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

## <a name="declare-the-variables"></a>Declarar as variáveis

1. Na Shell de nuvem, crie um ficheiro denominado `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no editor:

    ```JSON
    variable "client_id" {}
    variable "client_secret" {}

    variable "agent_count" {
        default = 3
    }

    variable "ssh_public_key" {
        default = "~/.ssh/id_rsa.pub"
    }

    variable "dns_prefix" {
        default = "k8stest"
    }

    variable cluster_name {
        default = "k8stest"
    }

    variable resource_group_name {
        default = "azure-k8stest"
    }

    variable location {
        default = "Central US"
    }
    ```

1. Modo de inserção de saída, selecionando o **Esc** chave.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

## <a name="create-a-terraform-output-file"></a>Criar um ficheiro de saída Terraform
[Produz Terraform](https://www.terraform.io/docs/configuration/outputs.html) permitem-lhe definir os valores que irão ser realçados ao utilizador quando Terraform aplica-se de um plano e pode ser consultado utilizando o `terraform output` comando. Nesta secção, cria um ficheiro de saída que permite o acesso ao cluster com [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/).

1. Na Shell de nuvem, crie um ficheiro denominado `output.tf`.

    ```bash
    vi output.tf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no editor:

    ```JSON
    output "client_key" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_key}"
    }

    output "client_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate}"
    }

    output "cluster_ca_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate}"
    }

    output "cluster_username" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.username}"
    }

    output "cluster_password" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.password}"
    }

    output "kube_config" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config_raw}"
    }

    output "host" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.host}"
    }
    ```

1. Modo de inserção de saída, selecionando o **Esc** chave.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

## <a name="set-up-azure-storage-to-store-terraform-state"></a>Configurar o armazenamento do Azure para armazenar o estado de Terraform
Controla Terraform localmente estado através do `terraform.tfstate` ficheiro. Este padrão funciona bem num ambiente única pessoa. No entanto, num ambiente de várias pessoa mais prático, tem de controlar o estado a saber como utilizar o servidor [storage do Azure](/azure/storage/). Nesta secção, pode obter as informações de conta de armazenamento necessário (nome de conta e chave de conta) e criar um contentor de armazenamento na qual as informações de estado Terraform serão armazenadas.

1. No portal do Azure, selecione **todos os serviços** no menu esquerdo.

1. Selecione **contas do Storage**.

1. No **contas do Storage** separador, selecione o nome da conta de armazenamento na qual Terraform é para armazenar o estado. Por exemplo, pode utilizar a conta de armazenamento criada quando abrir a Shell de nuvem pela primeira vez.  O nome da conta de armazenamento criado por nuvem Shell normalmente começa com `cs` seguido por uma cadeia de números e letras aleatória. **Lembre-se o nome da conta de armazenamento que selecionar, conforme for necessário mais tarde.**

1. No separador de conta de armazenamento, selecione **chaves de acesso**.

    ![Menu da conta de armazenamento](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account.png)

1. Anote o **chave1** **chave** valor. (Selecionar o ícone para a direita da chave copia o valor para a área de transferência.)

    ![Chaves de acesso de conta de armazenamento](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account-access-key.png)

1. Na Shell de nuvem, criar um contentor na sua conta do storage do Azure (substituir a &lt;YourAzureStorageAccountName > e &lt;YourAzureStorageAccountAccessKey > marcadores de posição com os valores adequados para a sua conta de armazenamento do Azure ).

    ```bash
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Criar o cluster Kubernetes
Nesta secção, consulte como utilizar o `terraform init` comando para criar os recursos de definidos os ficheiros de configuração que criou nas secções anteriores.

1. Na Shell de nuvem, inicializar Terraform (substituir a &lt;YourAzureStorageAccountName > e &lt;YourAzureStorageAccountAccessKey > marcadores de posição com os valores adequados para a sua conta de armazenamento do Azure).

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    O `terraform init` comando apresenta a taxa de êxito de inicializar o plug-in de back-end e o fornecedor:

    ![Exemplo de resultados de "terraform init"](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-init-complete.png)

1. Execute o `terraform plan` comando para criar o plano de Terraform que define os elementos de infraestrutura. O comando solicitará dois valores: **var.client_id** e **var.client_secret**. Para o **var.client_id** variável, introduza o **appId** valor associado à sua principal de serviço. Para o **var.client_secret** variável, introduza o **palavra-passe** valor associado à sua principal de serviço.

    ```bash
    terraform plan -out out.plan
    ```

    O `terraform plan` comando apresenta os recursos que serão criados ao executar o `terraform apply` comando:

    ![Exemplo de resultados de "plano de terraform"](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-plan-complete.png)

1. Execute o `terraform apply` comando para aplicar o esquema para criar o cluster Kubernetes. O processo para criar um cluster de Kubernetes pode demorar vários minutos, causando a temporização de sessão de nuvem Shell enviados. Se a sessão de Shell de nuvem exceder o tempo limite, pode seguir os passos na secção ["Recuperar a partir de um tempo limite de Shell de Cloud"](#recover-from-a-dloud-shell-timeout) para permitir a concluir o tutorial.

    ```bash
    terraform apply out.plan
    ```

    O `terraform apply` comando apresenta os resultados da criação dos recursos definidos nos seus ficheiros de configuração:

    ![Exemplo de resultados "terraform aplicar"](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-apply-complete.png)

1. No portal do Azure, selecione **todos os serviços** no menu à esquerda para ver os recursos criados para o novo cluster de Kubernetese.

    ![Linha de Shell de nuvem](./media/terraform-create-k8s-cluster-with-tf-and-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Recuperar a partir de um tempo limite de Shell de nuvem
Se a sessão de Shell de nuvem exceder o tempo limite, pode efetuar os seguintes passos para recuperar:

1. Inicie uma sessão de Shell de nuvem.

1. Mude para o diretório que contém os ficheiros de configuração Terraform.

    ```bash
    cd /clouddrive/terraform-aks-k8s
    ```

1. Execute o seguinte comando:

    ```bash
    export KUBECONFIG=./azurek8s
    ```
    
## <a name="test-the-kubernetes-cluster"></a>Testar o cluster Kubernetes
As ferramentas de Kubernetes podem ser utilizadas para verificar o cluster recém-criado.

1. Obter a configuração de Kubernetes do Estado Terraform e armazená-las num ficheiro dessa kubectl pode ler.

    ```bash
    echo "$(terraform output kube_config)" > ./azurek8s
    ```

1. Defina uma variável de ambiente que kubectl escolherá a configuração correta.

    ```bash
    export KUBECONFIG=./azurek8s
    ```

1. Verifique o estado de funcionamento do cluster.

    ```bash
    kubectl get nodes
    ```

    Deverá ver os detalhes dos seus nós de trabalho e devem todos ter um estado **pronto**, conforme mostrado na imagem seguinte:

    ![A ferramenta de kubectl permite-lhe verificar o estado de funcionamento do seu cluster Kubernetes](./media/terraform-create-k8s-cluster-with-tf-and-aks/kubectl-get-nodes.png)

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a utilizar Terraform e AKS para criar um cluster de Kubernetes. Seguem-se alguns recursos adicionais para o ajudar a saber mais sobre Terraform no Azure: 

 [Terraform Hub Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Documentação do fornecedor de Terraform Azure](http://aka.ms/terraform)  
 [Origem de fornecedor do Terraform Azure](http://aka.ms/tfgit)  
 [Módulos do Terraform Azure](http://aka.ms/tfmodules)