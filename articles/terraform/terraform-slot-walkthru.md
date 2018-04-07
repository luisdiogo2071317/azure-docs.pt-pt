---
title: Terraform ranhura de implementação do fornecedor do Azure
description: Terraform com tutorial de ranhura de implementação do fornecedor do Azure
keywords: terraform, devops, máquina virtual, do Azure, ranhuras de implementação
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 4/05/2018
ms.topic: article
ms.openlocfilehash: 34b16b5fb2b5b574d166693db346ebba15eaa1f9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="using-terraform-to-provision-infrastructure-with-azure-deployment-slots"></a>Utilizar Terraform à infraestrutura de aprovisionar com as ranhuras de implementação do Azure

[Ranhuras de implementação do Azure](/azure/app-service/web-sites-staged-publishing) permitem-lhe trocar entre versões diferentes da sua aplicação - como produção ou transição - para minimizar o impacto das implementações interrompidas. Este artigo ilustra uma utilização de exemplo de ranhuras de implementação orientando-o através da implementação das duas aplicações através do GitHub e o Azure. Uma aplicação está alojada numa "ranhura de produção", enquanto a aplicação de segundo está alojada na ranhura de "transição". (Os nomes "production" e "transição" são arbitrários e podem ser qualquer coisa que pretende que representa o seu cenário.) Depois de tem configuradas as ranhuras de implementação, em seguida, pode utilizar Terraform ao trocar entre os dois blocos conforme necessário.

## <a name="prerequisites"></a>Pré-requisitos

- **Subscrição do Azure** - se não tiver uma subscrição do Azure, criar um [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

- **Conta GitHub** - um [GitHub](http://www.github.com) conta é necessária para copiar e utilizar o teste de repositório do GitHub.

## <a name="create-and-apply-the-terraform-plan"></a>Criar e aplicar o esquema de Terraform

1. Navegue para o [portal do Azure](http://portal.azure.com)

1. Abra [Shell de nuvem do Azure](/azure/cloud-shell/overview)e, se não o fez anteriormente - selecione **Bash** como o seu ambiente.

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

1. Certifique-se de que ambos os diretórios foram criados com êxito utilizando a `ls` bash comando.

    ![Shell de nuvem depois de criar os diretórios](./media/terraform-slot-walkthru/cloud-shell-after-creating-dirs.png)

1. Altere os diretórios para o `deploy` diretório.

    ```bash
    cd deploy
    ```

1. Utilizar o [vi editor](https://www.debian.org/doc/manuals/debian-tutorial/ch-editor.html), crie um ficheiro denominado `deploy.tf`, que irá conter o [Terraform configuração](https://www.terraform.io/docs/configuration/index.html).

    ```bash
    vi deploy.tf
    ```

1. Introduza o modo de inserção, premindo a letra `i` chave.

1. Cole o seguinte código no editor:

    ```JSON
    # Configure the Azure Provider
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

1. Prima a  **&lt;Esc >** chave para sair do modo de inserção.

1. Guarde o ficheiro e saia do editor de vi introduzindo o comando seguinte, em seguida, premindo  **&lt;Enter >**:

    ```bash
    :wq
    ```

1. Quando o ficheiro tiver sido criado, pode verificar o respetivo conteúdo.

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

1. Aprovisionar os recursos definidos no `deploy.tf` ficheiro de configuração. (Confirmar a ação, introduzindo `yes` na linha de.)

    ```bash
    terraform apply
    ```

1. Feche a janela da Shell de nuvem.

1. O menu principal portal do Azure, selecione **grupos de recursos**.

    ![Portal do Azure grupos de recursos](./media/terraform-slot-walkthru/resource-groups-menu-option.png)

1. No **grupos de recursos** separador, selecione **slotDemoResourceGroup**.

    ![Grupo de recursos criado pelo Terraform](./media/terraform-slot-walkthru/resource-group.png)

Quando terminar, verá todos os recursos criados pelo Terraform.

![Recursos criados pelo Terraform](./media/terraform-slot-walkthru/resources.png)

## <a name="fork-the-test-project"></a>Copiar o projeto de teste

Antes de testar a criação e a troca de e para as ranhuras de implementação, terá de copiar o projeto de teste a partir do GitHub.

1. Navegue para o [extraordinário terraform repositório no GitHub](https://github.com/Azure/awesome-terraform).

1. Bifurcação o **extraordinário terraform repositório**.

    ![Copiar o repositório do GitHub extraordinário terraform](./media/terraform-slot-walkthru/fork-repo.png)

1. Siga as instruções para bifurcar ao seu ambiente.

## <a name="deploy-from-github-to-your-deployment-slots"></a>Implementar a partir do GitHub para as ranhuras de implementação

Uma vez bifurcação o repositório de projeto de teste, configurar os blocos de implementação através dos seguintes passos:

1. O menu principal portal do Azure, selecione **grupos de recursos**.

1. Select **slotDemoResourceGroup**.

1. Selecione **slotAppService**.

1. Selecione **opções de implementação**.

    ![Opções de implementação para um recurso de serviço de aplicações](./media/terraform-slot-walkthru/deployment-options.png)

1. No **a opção de implementação** separador, selecione **Escolher origem**e, em seguida, selecione **GitHub**.

    ![Selecione a origem de implementação](./media/terraform-slot-walkthru/select-source.png)

1. Depois de Azure estabelece a ligação e apresenta todas as opções, selecione **autorização**.

1. No **autorização** separador, selecione **autorizar**e forneça as credenciais necessárias para o Azure aceder à sua conta do GitHub. 

1. Depois de Azure valida as credenciais do GitHub, apresentada uma mensagem a indicar que concluiu o processo de autorização. Selecione **OK** para fechar o **autorização** separador.

1. Selecione **escolha a sua organização** e selecione a sua organização.

1. Selecione **escolha projeto**.

1. No **escolha projeto** separador, selecione o **extraordinário terraform** projeto.

    ![Escolha o projeto de extraordinário terraform](./media/terraform-slot-walkthru/choose-project.png)

1. Selecione **escolha ramo**.

1. No **escolha ramo** separador, selecione **mestre**.

    ![Selecione o ramo principal](./media/terraform-slot-walkthru/choose-branch-master.png)

1. No **a opção de implementação** separador, selecione **OK**.

Neste momento, implementou a ranhura de produção. Para implementar o bloco de transição, execute todos os passos anteriores nesta secção com as seguintes alterações:

- No passo 3, **slotAppServiceSlotOne** recursos.

- Passo 13, selecione o ramo de "a trabalhar" em vez do ramo principal.

    ![Selecione o ramo de trabalho](./media/terraform-slot-walkthru/choose-branch-working.png)

## <a name="test-the-app-deployments"></a>Testar as implementações de aplicações

Nas secções anteriores, pode configura dois ranhuras - **slotAppService** e **slotAppServiceSlotOne** - para implementar a partir de diferentes ramos no GitHub. Vamos pré-visualizar as aplicações web para validar que que foram implementados com êxito.

Execute os seguintes passos duas vezes onde no passo 3 selecionar **slotAppService** pela primeira vez e, em seguida, selecione **slotAppServiceSlotOne** na segunda vez:

1. O menu principal portal do Azure, selecione **grupos de recursos**.

1. Select **slotDemoResourceGroup**.

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
 
1. Mudar para o separador do browser em execução **slotAppService** (a aplicação com a página azul). 

1. Regresse ao portal do Azure num separador separado.

1. Abra a Shell de nuvem.

1. Altere os diretórios para o **clouddrive/troca** diretório.

    ```bash
    cd clouddrive/swap
    ```

1. Utilizando o editor de vi, crie um ficheiro denominado `swap.tf`.

    ```bash
    vi swap.tf
    ```

1. Introduza o modo de inserção, premindo a letra `i` chave.

1. Cole o seguinte código no editor:

    ```JSON
    # Configure the Azure Provider
    provider "azurerm" { }

    # Swap the production slot and the staging slot
    resource "azurerm_app_service_active_slot" "slotDemoActiveSlot" {
        resource_group_name   = "slotDemoResourceGroup"
        app_service_name      = "slotAppService"
        app_service_slot_name = "slotappServiceSlotOne"
    }
    ```

1. Prima a  **&lt;Esc >** chave para sair do modo de inserção.

1. Guarde o ficheiro e saia do editor de vi introduzindo o comando seguinte, em seguida, premindo  **&lt;Enter >**:

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

1. Aprovisionar os recursos definidos no `swap.tf` ficheiro de configuração. (Confirmar a ação, introduzindo `yes` na linha de.)

    ```bash
    terraform apply
    ```

1. Depois de terminar Terraform troca de ranhuras, volte ao browser que estiver a compor o **slotAppService** aplicação web e atualize a página. 

A aplicação web na sua **slotAppServiceSlotOne** testes ranhura tem foram alternados com a ranhura de produção e agora composições verde. 

![As ranhuras de implementação tiverem sido alternadas](./media/terraform-slot-walkthru/slots-swapped.png)

Para voltar para a versão de produção original da aplicação, volte a aplicar o plano de Terraform criado a partir de `swap.tf` ficheiro de configuração.

```bash
terraform apply
```

Depois de alternados, consulte a configuração original.