---
title: "Utilizar uma imagem do Azure Marketplace para criar uma máquina de Virtual com Linux Terraform com a identidade de serviço geridas"
description: "Utilize a imagem do Marketplace para criar a máquina de Virtual com Linux Terraform com a gestão de identidade de serviço gerida e estado remoto facilmente implementar recursos para o Azure."
keywords: "terraform, devops, MSI, máquina virtual, o estado remoto, o azure"
author: VaijanathB
manager: rloutlaw
ms.author: tarcher
ms.date: 3/12/2018
ms.topic: article
ms.openlocfilehash: ce8a3e8b813bf4224a1fd77d60ec30f2f8e080a7
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="use-an-azure-marketplace-image-to-create-a-terraform-linux-virtual-machine-with-managed-service-identity"></a>Utilizar uma imagem do Azure Marketplace para criar uma máquina de Virtual com Linux Terraform com a identidade de serviço geridas

Este artigo mostra como utilizar um [imagem do Terraform Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.terraform?tab=Overview) para criar um `Ubuntu Linux VM (16.04 LTS)` com a versão mais recente [Terraform](https://www.terraform.io/intro/index.html) versão instalada e configurada com [(identidade de serviço geridas MSI)](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview). Esta imagem também configura um back-end remoto para permitir [estado remoto](https://www.terraform.io/docs/state/remote.html) gestão Terraform a utilizar. A imagem do Terraform Marketplace torna mais fácil começar a utilizar Terraform no Azure em minutos, sem ter de instalar Terraform e configurar manualmente a autenticação. 

Não existem sem custos de software para esta imagem de Terraform VM. Paga apenas as taxas de utilização de hardware do Azure são avaliadas com base no tamanho da máquina virtual aprovisionado. Obter mais detalhes sobre as taxas de computação podem ser encontrados no [página de preços de máquinas virtuais do Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/).

## <a name="prerequisites"></a>Pré-requisitos
Antes de poder criar uma Máquina Virtual do Linux Terraform, tem de ter uma subscrição do Azure. Se já tiver uma, consulte [hoje a criar a conta do Azure gratuita](https://azure.microsoft.com/free/).  

## <a name="create-your-terraform-virtual-machine"></a>Criar a Máquina Virtual de Terraform 

Eis os passos para criar uma instância de Máquina Virtual do Linux Terraform. 

1. Navegue para [crie um recurso](https://ms.portal.azure.com/#create/hub) listagem no portal do Azure.
2. Procurar `Terraform` no `Search the Marketplace` barra de pesquisa. Selecione o `Terraform` modelo. Selecione o **criar** botão no separador de detalhes de Terraform no lado direito na parte inferior.
![alt text](media\terraformmsi.png)
3. As secções seguintes fornecem entradas para todos os passos no assistente (**enumerado à direita**) para criar a máquina de Virtual Terraform Linux.  Seguem-se as entradas necessárias para configurar cada um destes passos

## <a name="details-in-create-terraform-tab"></a>Criar de detalhes no separador Terraform

Seguem-se os detalhes que precisam de ser introduzidos no separador Terraform criar.

a. **Noções básicas**
    
* **Nome**: nome da Máquina Virtual Terraform.
* **Nome de utilizador**: primeira conta de início de sessão ID.
* **Palavra-passe**: primeira conta palavra-passe (pode utilizar a chave pública SSH em vez da palavra-passe).
* **Subscrição**: Se tiver mais do que uma subscrição, selecione aquele no qual a máquina está a ser criado e cobrados. Tem de ter privilégios de criação de recursos para esta subscrição.
* **Grupo de recursos**: pode criar uma nova ou utilize um grupo existente.
* **Localização**: selecione o Centro de dados que é mais adequado. Normalmente, é o Centro de dados que a maioria dos seus dados, ou se aproxima-se à sua localização física para acesso de rede mais rápido.

b. **Definições adicionais**

* Tamanho: Tamanho da Máquina Virtual.
* Tipo de disco da VM: escolher entre SSD e HDD

c. **Resumo Terraform**

* Certifique-se de que todas as informações que introduziu estão corretas. 

d. **Comprar**

* Para iniciar o aprovisionamento, clique em comprar. É fornecida uma ligação para os termos da transação. A VM não tem quaisquer taxas adicionais para além de computação para o tamanho de servidor que selecionou no passo de tamanho.

A imagem de Terraform VM efetua os passos seguintes

* Cria uma VM com o sistema atribuído de identidade com base na imagem Ubuntu 16.04 LTS
* Instala a extensão do MSI da VM para permitir que os tokens de OAuth ao ser emitido para recursos do Azure
* Atribua permissões de RBAC para a identidade geridos, conceder direitos de proprietário para o grupo de recursos
* Cria uma pasta de modelo Terraform (tfTemplate)
* Pré-configura Terraform estado remoto com o back-end do Azure

## <a name="how-to-access-and-configure-linux-terraform-virtual-machine"></a>Como aceder e configurar a Máquina Virtual do Linux Terraform

Depois de criada a VM, pode iniciar sessão-lo através de SSH. Utilize as credenciais da conta que criou na secção Noções básicas do passo 3 para a interface de shell de texto. No Windows, pode transferir uma ferramenta de cliente SSH como [Putty](http://www.putty.org/)

Assim que tiver utilizado `SSH` para ligar à máquina Virtual, terá de conceder permissões de contribuinte para a subscrição completa a identidade de serviço gerido na máquina Virtual. Permissões de contribuinte ajuda-o MSI da VM para utilizar Terraform para criar recursos externos ao grupo de recursos da VM. Pode facilmente obter esta ação executando um script de uma vez. Eis o comando para fazê-lo.

`. ~/tfEnv.sh`

O script anterior utiliza [AZ CLI v 2.0 interativa início de sessão](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest#interactive-log-in) mecanismo para autenticar com o Azure e atribuir as permissões de contribuinte de identidade de serviço geridas da Máquina Virtual a subscrição completa. 

 A VM foi criada no back-end Terraform remoto estado e ativá-la na sua implementação Terraform, tem de copiar o ficheiro de remoteState.tf do tfTemplate directory para a raiz dos Terraform scripts.  

 `cp  ~/tfTemplate/remoteState.tf .`

 Pode ler mais sobre a gestão remota de estado [aqui](https://www.terraform.io/docs/state/remote.html). A chave de acesso de armazenamento está exposta neste ficheiro e tem de ser verificado cuidadosamente para controlo de origem.  

## <a name="next-steps"></a>Próximos Passos
Neste artigo, aprendeu como definir a segurança de um Terraform Máquina Virtual com Linux no Azure. Seguem-se alguns recursos adicionais para obter mais informações sobre Terraform no Azure. 

 [Terraform Hub Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Documentação do fornecedor de Terraform Azure](http://aka.ms/terraform)  
 [Origem de fornecedor do Terraform Azure](http://aka.ms/tfgit)  
 [Módulos do Terraform Azure](http://aka.ms/tfmodules)
 

















