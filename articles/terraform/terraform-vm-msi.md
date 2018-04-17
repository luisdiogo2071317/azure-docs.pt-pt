---
title: Utilizar uma imagem do Azure Marketplace para criar uma máquina virtual Terraform Linux com a identidade de serviço geridas
description: Utilize a imagem do Marketplace para criar máquina virtual Terraform Linux com uma identidade de serviço geridas e a gestão remota do Estado para facilmente implementar recursos no Azure.
keywords: terraform, devops, MSI, máquina virtual, o estado remoto, o azure
author: VaijanathB
manager: rloutlaw
ms.author: tarcher
ms.date: 3/12/2018
ms.topic: article
ms.openlocfilehash: 5f0ee2904c1072a5ad8c5f7ae1c90e649cc4813c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-service-identity"></a>Utilizar uma imagem do Azure Marketplace para criar uma máquina virtual Terraform Linux com a identidade de serviço geridas

Este artigo mostra como utilizar um [imagem do Terraform Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) para criar uma VM com Linux do Ubuntu (16.04 LTS) com a versão mais recente [Terraform](https://www.terraform.io/intro/index.html) versão instalada e configurada com [gerido Serviço de identidade (MSI)](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). Esta imagem também configura um remoto de back-end para ativar [estado remoto](https://www.terraform.io/docs/state/remote.html) gestão Terraform a utilizar. 

A imagem do Terraform Marketplace torna mais fácil começar a utilizar Terraform no Azure, sem ter de instalar e configurar Terraform manualmente. 

Não existem sem custos de software para esta imagem de Terraform VM. Paga apenas as taxas de utilização de hardware do Azure são avaliadas com base no tamanho da máquina virtual que está aprovisionado. Para obter mais informações sobre as taxas de processamento, consulte o [página de preços de máquinas virtuais do Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Pré-requisitos
Antes de poder criar uma máquina virtual do Linux Terraform, tem de ter uma subscrição do Azure. Se ainda não tiver um, consulte o artigo [hoje a criar a conta do Azure gratuita](https://azure.microsoft.com/free/).  

## <a name="create-your-terraform-virtual-machine"></a>Criar a máquina virtual de Terraform 

Eis os passos para criar uma instância de uma máquina virtual do Linux Terraform: 

1. No portal do Azure, visite o [crie um recurso](https://ms.portal.azure.com/#create/hub) listagem.

2. No **procurar no Marketplace** barra de pesquisa, procure **Terraform**. Selecione o **Terraform** modelo. 

3. No separador de detalhes do Terraform no canto inferior direito, selecione o **criar** botão.

    ![Criar uma máquina virtual de Terraform](media\terraformmsi.png)

4. As secções seguintes fornecem entradas para todos os passos no Assistente para criar a máquina virtual Terraform Linux. A secção seguinte apresenta uma lista de entradas que são necessárias para configurar cada um destes passos.

## <a name="details-on-the-create-terraform-tab"></a>Detalhes no separador Terraform criar

Introduza os seguintes detalhes o **criar Terraform** separador:

1. **Noções básicas**
    
   * **Nome**: O nome da máquina virtual Terraform.
   * **Nome de utilizador**: A primeira conta de início de sessão ID.
   * **Palavra-passe**: A palavra-passe de conta primeiro. (Pode utilizar uma chave pública SSH em vez de uma palavra-passe.)
   * **Subscrição**: A subscrição em que a máquina está a ser criado e cobrados. Tem de ter privilégios de criação de recursos para esta subscrição.
   * **Grupo de recursos**: um grupo de recursos novo ou existente.
   * **Localização**: O Centro de dados que é mais adequado. Normalmente, é o Centro de dados que tenha a maioria dos seus dados, ou um que seja mais próxima da sua localização física para acesso de rede mais rápido.

2. **Definições adicionais**

   * **Tamanho**: tamanho da máquina virtual. 
   * **Tipo de disco da VM**: SSD ou HDD.

3. **Resumo Terraform**

   * Certifique-se de que todas as informações que introduziu estão corretas. 

4. **Comprar**

   * Para iniciar o processo de aprovisionamento, selecione **comprar**. É fornecida uma ligação para os termos da transação. A VM não tem quaisquer taxas adicionais para além de computação para o tamanho de servidor que selecionou no passo de tamanho.

A imagem de Terraform VM efetua os seguintes passos:

* Cria uma VM com a identidade de sistema atribuída de mensagens em fila que se baseia a imagem de Ubuntu 16.04 LTS.
* Instala a extensão do MSI da VM para permitir que os tokens de OAuth ao ser emitido para recursos do Azure.
* Atribui permissões RBAC para a identidade gerida, conceder direitos de proprietário para o grupo de recursos.
* Cria uma pasta de modelo Terraform (tfTemplate).
* Pré-configura um Estado remoto Terraform com cópia de segurança do Azure final.

## <a name="access-and-configure-a-linux-terraform-virtual-machine"></a>Aceder e configurar uma máquina virtual do Linux Terraform

Depois de criar a VM, pode iniciar sessão-lo através de SSH. Utilize as credenciais da conta que criou na secção "Noções básicas" do passo 3 para a interface de shell de texto. No Windows, pode transferir uma ferramenta de cliente SSH como [Putty](http://www.putty.org/).

Depois de utilizar o SSH para ligar à máquina virtual, tem de conceder permissões de contribuinte para a subscrição completa a identidade de serviço geridas na máquina virtual. 

Permissões de contribuinte ajuda-o MSI da VM para utilizar Terraform para criar recursos externos ao grupo de recursos da VM. Pode facilmente obter esta ação, executando um script de uma vez. Utilize o seguinte comando:

`. ~/tfEnv.sh`

O script anterior utiliza o [AZ CLI v 2.0 interativa início de sessão](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest#interactive-log-in) mecanismo para autenticar com o Azure e atribuir a permissões de contribuinte de identidade de serviço geridas em toda a subscrição de máquina virtual. 

 A VM tem um Terraform estado remoto de back-end. Para ativá-la na sua implementação Terraform, copie o ficheiro de remoteState.tf do tfTemplate directory para a raiz dos Terraform scripts.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Para obter mais informações sobre a gestão remota de estado, consulte [nesta página sobre o estado de remoto Terraform](https://www.terraform.io/docs/state/remote.html). A chave de acesso de armazenamento está exposta neste ficheiro e tem de ser excluídos antes dos ficheiros de configuração do commiting Terraform para controlo de origem.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a configurar uma máquina virtual de Terraform Linux no Azure. Seguem-se alguns recursos adicionais para o ajudar a saber mais sobre Terraform no Azure: 

 [Terraform Hub Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Documentação do fornecedor de Terraform Azure](http://aka.ms/terraform)  
 [Origem de fornecedor do Terraform Azure](http://aka.ms/tfgit)  
 [Módulos do Terraform Azure](http://aka.ms/tfmodules)
 

















