---
description: incluir ficheiro
author: tomarcher
manager: rloutlaw
ms.service: multiple
ms.workload: web
ms.devlang: na
ms.topic: include
ms.date: 03/12/2018
ms.author: tarcher
ms.custom: Jenkins
ms.openlocfilehash: d5a832909f060ad8c8b3f0e7c7ea4504e5e5aadb
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36943475"
---
1. No seu browser, abra o [imagem do Azure Marketplace para Jenkins](https://azuremarketplace.microsoft.com/marketplace/apps/azure-oss.jenkins?tab=Overview).

1. Selecione **obter IT agora**.

    ![Selecione o GIT TI agora para iniciar o processo de instalação para a imagem do Jenkins Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-get-it-now.png)

1. Depois de rever as informações de termos e detalhes de preços, selecione **continuar**.

    ![Jenkins Marketplace termos de preços e as informações da imagem.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-pricing-and-terms.png)

1. Selecione **criar** para configurar o servidor de Jenkins no portal do Azure. 

    ![Instale a imagem do Jenkins Marketplace.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-create.png)

1. No **Noções básicas** separador, especifique os valores seguintes:

    - **Nome** -introduza `Jenkins`.
    - **Nome de utilizador** -introduza o nome de utilizador a utilizar quando o início de sessão na máquina virtual no qual Jenkins está em execução. O nome de utilizador tem de cumprir [requisitos específicos](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm).
    - **Tipo de autenticação** - selecione **chave pública SSH**.
    - **A chave pública SSH** -copiar e colar uma chave pública RSA no formato de linha única (começadas `ssh-rsa`) ou o formato PEM com várias linha. Pode gerar chaves SSH utilizando ssh-keygen no Linux e macOS ou PuTTYGen no Windows. Para obter mais informações sobre chaves SSH e o Azure, consulte o artigo [como chaves de utilizar o SSH com o Windows no Azure](/azure/virtual-machines/linux/ssh-from-windows).
    - **Subscrição** -selecionar a subscrição do Azure na qual pretende instalar Jenkins.
    - **Grupo de recursos** - selecione **criar nova**e introduza um nome para o grupo de recursos que funciona como um contentor para a coleção de recursos que compõem a sua instalação Jenkins lógico.
    - **Localização** - selecione **EUA Leste**.

    ![Introduza as informações do grupo de recursos e autenticação para Jenkins no separador básico.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-basic.png)

1. Selecione **OK** prossiga para o **definições adicionais** separador. 

1. No **definições adicionais** separador, especifique os valores seguintes:

    - **Tamanho** -selecionar a opção de dimensionamento adequado para a máquina virtual de Jenkins.
    - **Tipo de disco da VM** - Especifique um HDD (unidade de disco rígido) ou SSD (unidade de estado sólido) para indicar o tipo de disco de armazenamento é permitido para a máquina virtual de Jenkins.
    - **Rede virtual** -(opcional) selecione **rede Virtual** para modificar as predefinições.
    - **Sub-redes** - selecione **sub-redes**, verifique as informações e selecione **OK**.
    - **Endereço IP público** -nome do endereço IP está predefinida para o nome de Jenkins que especificou na página anterior com o sufixo - IP. Pode selecionar a opção para alterar esse predefinido.
    - **Etiqueta de nome de domínio** -especifique o valor para o URL para a máquina virtual de Jenkins completamente qualificado.
    - **Tipo de versão Jenkins** -selecione o tipo de versão pretendida entre as opções: `LTS`, `Weekly build`, ou `Azure Verified`. O `LTS` e `Weekly build` opções são explicadas no artigo, [Jenkins LTS versão linha](https://jenkins.io/download/lts/). O `Azure Verified` opção refere-se a um [versão Jenkins LTS](https://jenkins.io/download/lts/) que foi verificada para ser executada no Azure. 

    ![Introduza as definições de máquina virtual para Jenkins no separador Definições.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-settings.png)

1. Selecione **OK** prossiga para o **definições de integração** separador.

1. No **definições de integração** separador, especifique os valores seguintes:

    - **Principal de serviço** -o principal de serviço está adicionado à Jenkins como uma credencial para a autenticação com o Azure. `Auto` significa que o principal será criado pelo MSI (identidade de serviço geridos). `Manual` significa que o principal deve ser criado por si. 
        - **ID da aplicação** e **segredo** - se de que seleciona o `Manual` opção para o **Principal de serviço** opção, terá de especificar o `Application ID` e `Secret` para o principal de serviço. Quando [criar um principal de serviço](/cli/azure/create-an-azure-service-principal-azure-cli), tenha em atenção que a função predefinida **contribuinte**, que é suficiente para trabalhar com recursos do Azure.
    - **Ativar os agentes de nuvem** -especifique o modelo de nuvem predefinido para agentes onde `ACI` refere-se a instância de contentor do Azure, e `VM` refere-se às máquinas virtuais. Também pode especificar `No` se não pretender ativar um agente de nuvem.

1. Selecione **OK** prossiga para o **resumo** separador.

1. Quando o **resumo** separador apresenta, as informações que introduziu são validadas. Quando vir o **validação transmitida** mensagem (na parte superior do separador), selecione **OK**. 

    ![O separador Resumo mostra e valida as opções selecionadas.](./media/jenkins-install-from-azure-marketplace-image/jenkins-configure-summary.png)

1. Quando o **criar** separador apresenta, selecione **criar** para criar a máquina virtual de Jenkins. Quando o servidor estiver pronto, uma notificação é apresentada no portal do Azure.

    ![Jenkins é notificação pronta.](./media/jenkins-install-from-azure-marketplace-image/jenkins-install-notification.png)