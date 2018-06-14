---
title: Terraform com ranhuras de implementação do fornecedor do Azure
description: Tutorial sobre como utilizar Terraform com as ranhuras de implementação do fornecedor do Azure
keywords: terraform, devops, máquina virtual, do Azure, ranhuras de implementação
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 4/05/2018
ms.topic: article
ms.openlocfilehash: 3a018dbaf90801604b13efcf8bd7afb6dbc68659
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
ms.locfileid: "31416868"
---
# <a name="use-terraform-to-provision-infrastructure-with-azure-deployment-slots"></a>Utilizar Terraform à infraestrutura de aprovisionar com as ranhuras de implementação do Azure

Pode utilizar [ranhuras de implementação do Azure](/azure/app-service/web-sites-staged-publishing) ao trocar entre versões diferentes da sua aplicação. Essa capacidade ajuda-o a minimizar o impacto das implementações interrompidas. 

Este artigo ilustra uma utilização de exemplo de ranhuras de implementação orientando-o através da implementação das duas aplicações através do GitHub e o Azure. Uma aplicação está alojada na ranhura de produção. A aplicação de segundo estiver alojada num bloco de transição. (Os nomes "production" e "transição" são arbitrários e podem ser qualquer coisa que pretende que representa o seu cenário.) Depois de configurar as ranhuras de implementação, pode utilizar Terraform ao trocar entre os dois blocos conforme necessário.

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure**: se não tem uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

- **Conta GitHub**: É necessário um [GitHub](http://www.github.com) conta copiar e utilizar o teste de repositório do GitHub.

## <a name="create-and-apply-the-terraform-plan"></a>Criar e aplicar o esquema de Terraform

1. Navegue para o [portal do Azure](http://portal.azure.com).

1. Abra [em nuvem do Azure Shell](/azure/cloud-shell/overview). Se não selecionar um ambiente anteriormente, selecione **Bash** como o seu ambiente.

    ![Linha de Shell de nuvem](./media/terraform-slot-walkthru/azure-portal-cloud-shell-button-min.png)

1. Altere os diretórios para o `clouddrive` diretório.

    ```bash
    cd clouddrive
    ```

1. Criar um diretório com o nome `deploy`.

    ```bash
    mkdir deploy
    ```

1. Criar um diretório com o nome `swap`.

    ```bash
    mkdir swap
    ```

1. Utilize o `ls` bash comando para verificar que criou com êxito ambos os diretórios.

    ![Shell de nuvem depois de criar os diretórios](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. Altere os diretórios para o `deploy` diretório.

    ```bash
    cd deploy
    ```

1. Utilizando o [vi editor](https://www.debian.org/doc/manuals/debian-tutorial/ch-editor.html), crie um ficheiro denominado `deploy.tf`. Este ficheiro irá conter o [Terraform configuração](https://www.terraform.io/docs/configuration/index.html).

    ```bash
    vi deploy.tf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no editor:

    ```JSON
    # Configure the Azure provider
    provider "azurerm" { }

    resource "azurerm_resource_group" "slotDemo" {
        name = "slotDemoResourceGroup"
        location = "westus2"
    }

    resource "azurerm_app_service_plan" "slotDemo" {
        name                = "slotAppServicePlan"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        sku {
            tier = "Standard"
            size = "S1"
        }
    }

    resource "azurerm_app_service" "slotDemo" {
        name                = "slotAppService"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
    }

    resource "azurerm_app_service_slot" "slotDemo" {
        name                = "slotAppServiceSlotOne"
        location            = "${azurerm_resource_group.slotDemo.location}"
        resource_group_name = "${azurerm_resource_group.slotDemo.name}"
        app_service_plan_id = "${azurerm_app_service_plan.slotDemo.id}"
        app_service_name    = "${azurerm_app_service.slotDemo.name}"
    }
    ```

1. Selecione a tecla Esc para sair do modo de inserção.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Agora que criou o ficheiro, verifique o respetivo conteúdo.

    ```bash
    cat deploy.tf
    ```

1. Inicializar Terraform.

    ```bash
    terraform init
    ```

1. Crie o plano de Terraform.

    ```bash
    terraform plan
    ```

1. Aprovisionar os recursos que estão definidos no `deploy.tf` ficheiro de configuração. (Confirmar a ação, introduzindo `yes` na linha de.)

    ```bash
    terraform apply
    ```

1. Feche a janela da Shell de nuvem.

1. No menu principal do portal do Azure, selecione **grupos de recursos**.

    ![Seleção de "Grupos de recursos" no portal](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. No **grupos de recursos** separador, selecione **slotDemoResourceGroup**.

    ![Grupo de recursos criado pelo Terraform](./media/terraform-slot-walkthru/resource-group.png)

Agora, ver todos os recursos que criou Terraform.

![Recursos criados pelo Terraform](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>Copiar o projeto de teste

Antes de testar a criação e a troca de e para as ranhuras de implementação, terá de copiar o projeto de teste a partir do GitHub.

1. Navegue para o [extraordinário terraform repositório no GitHub](https://github.com/Azure/awesome-terraform).

1. Bifurcação o **extraordinário terraform** repositório.

    ![Copiar o repositório do GitHub extraordinário terraform](./media/terraform-slot-walkthru/fork-repo.png)

1. Siga as instruções para bifurcar ao seu ambiente.

## <a name="deploy-from-github-to-your-deployment-slots"></a>Implementar a partir do GitHub para as ranhuras de implementação

Depois de copiar o repositório de projeto de teste, configure os blocos de implementação através dos seguintes passos:

1. No menu principal do portal do Azure, selecione **grupos de recursos**.

1. Selecione **slotDemoResourceGroup**.

1. Selecione **slotAppService**.

1. Selecione **opções de implementação**.

    ![Opções de implementação para um recurso de serviço de aplicações](./media/terraform-slot-walkthru/deployment-options.png)

1. No **a opção de implementação** separador, selecione **Escolher origem**e, em seguida, selecione **GitHub**.

    ![Selecione a origem de implementação](./media/terraform-slot-walkthru/select-source.png)

1. Depois de Azure estabelece a ligação e apresenta todas as opções, selecione **autorização**.

1. No **autorização** separador, selecione **autorizar**e forneça as credenciais que o Azure necessita de aceder à sua conta do GitHub. 

1. Depois de Azure valida as credenciais do GitHub, uma mensagem é apresentada e indica que concluiu o processo de autorização. Selecione **OK** para fechar o **autorização** separador.

1. Selecione **escolha a sua organização** e selecione a sua organização.

1. Selecione **escolha projeto**.

1. No **escolha projeto** separador, selecione o **extraordinário terraform** projeto.

    ![Escolha o projeto de extraordinário terraform](./media/terraform-slot-walkthru/choose-project.png)

1. Selecione **escolha ramo**.

1. No **escolha ramo** separador, selecione **mestre**.

    ![Selecione o ramo principal](./media/terraform-slot-walkthru/choose-branch-master.png)

1. No **a opção de implementação** separador, selecione **OK**.

Neste momento, implementou a ranhura de produção. Para implementar o bloco de transição, execute todos os passos anteriores nesta secção com as seguintes alterações:

- No passo 3, selecione o **slotAppServiceSlotOne** recursos.

- Passo 13, selecione o ramo de trabalho em vez do ramo principal.

    ![Selecione o ramo de trabalho](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>Testar as implementações de aplicações

Nas secções anteriores, pode configura dois ranhuras –**slotAppService** e **slotAppServiceSlotOne**– para implementar a partir de diferentes ramos no GitHub. Vamos pré-visualizar as aplicações web para validar que que foram implementados com êxito.

Execute os seguintes passos duas vezes. No passo 3, selecione **slotAppService** pela primeira vez e, em seguida, selecione **slotAppServiceSlotOne** na segunda vez.

1. No menu principal do portal do Azure, selecione **grupos de recursos**.

1. Selecione **slotDemoResourceGroup**.

1. Selecione **slotAppService** ou **slotAppServiceSlotOne**.

1. Na página Descrição geral, selecione **URL**.

    ![Selecione o URL no separador de descrição geral para compor a aplicação](./media/terraform-slot-walkthru/resource-url.png)

> [!NOTE]
> Pode demorar alguns minutos para o Azure criar e implementar o site a partir do GitHub.
>
>

Para o **slotAppService** aplicação web, verá uma página com um título de página do azul **1 de aplicação de demonstração de ranhura**. Para o **slotAppServiceSlotOne** aplicação web, verá uma página com um título de página de verde **2 de aplicação de demonstração de ranhura**.

![Pré-visualize as aplicações para testar que foram implementados corretamente](./media/terraform-slot-walkthru/app-preview.png)

## <a name="swap-the-two-deployment-slots"></a>Trocar ranhuras de implementação de dois

Para testar a troca de ranhuras de implementação de dois, execute os seguintes passos:
 
1. Mudar para o separador do browser que está a executar **slotAppService** (a aplicação com a página azul). 

1. Regresse ao portal do Azure num separador à parte.

1. Abra a Shell de nuvem.

1. Altere os diretórios para o **clouddrive/troca** diretório.

    ```bash
    cd clouddrive/swap
    ```

1. Utilizando o editor de vi, crie um ficheiro denominado `swap.tf`.

    ```bash
    vi swap.tf
    ```

1. Introduza o modo de inserção, selecionando a chave.

1. Cole o seguinte código no editor:

    ```JSON
    # Configure the Azure provider
    provider "azurerm" { }

    # Swap the production slot and the staging slot
    resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {
        resource_group_name   = "slotDemoResourceGroup"
        app_service_name      = "slotAppService"
        app_service_slot_name = "slotappServiceSlotOne"
    }
    ```

1. Selecione a tecla Esc para sair do modo de inserção.

1. Guarde o ficheiro e saia do editor de vi introduzindo o seguinte comando:

    ```bash
    :wq
    ```

1. Inicializar Terraform.

    ```bash
    terraform init
    ```

1. Crie o plano de Terraform.

    ```bash
    terraform plan
    ```

1. Aprovisionar os recursos que estão definidos no `swap.tf` ficheiro de configuração. (Confirmar a ação, introduzindo `yes` na linha de.)

    ```bash
    terraform apply
    ```

1. Após a conclusão da Terraform troca de ranhuras, volte ao browser que estiver a compor o **slotAppService** aplicação web e atualize a página. 

A aplicação web na sua **slotAppServiceSlotOne** testes ranhura tem foram alternados com a ranhura de produção e agora é composto no verde. 

![As ranhuras de implementação tiverem sido alternadas](./media/terraform-slot-walkthru/slots-swapped.png)

Para voltar para a versão de produção original da aplicação, volte a aplicar o plano de Terraform que criou a partir de `swap.tf` ficheiro de configuração.

```bash
terraform apply
```

Depois da aplicação é alternada, consulte a configuração original.