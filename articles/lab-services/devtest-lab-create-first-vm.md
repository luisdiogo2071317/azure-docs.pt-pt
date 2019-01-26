---
title: Criar a sua primeira VM num laboratório no Azure DevTest Labs | Documentos da Microsoft
description: Saiba como criar a sua primeira máquina virtual num laboratório no Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: c72e10991e27f7d616703e635ee6e1a18122afc5
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078080"
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Criar a sua primeira VM num laboratório no Azure DevTest Labs

Ao acessar o DevTest Labs inicialmente e pretende criar a sua primeira VM, provavelmente farão isso usando um pré-carregados [imagem do base marketplace](devtest-lab-configure-marketplace-images.md). Mais tarde no, também poderá escolher entre uma [imagem personalizada e uma fórmula](devtest-lab-add-vm.md) durante a criação de mais VMs. 

Este tutorial explica-lhe utilizar o portal do Azure para adicionar a sua primeira VM a um laboratório no DevTest Labs.

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a>Passos para adicionar a sua primeira VM a um laboratório no Azure DevTest Labs
1. Inicie sessão no [portal do Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** no **DEVOPS** secção. Se selecionar * (em estrelas) junto a **DevTest Labs** no **DEVOPS** secção. Esta ação adiciona **DevTest Labs** para o menu de navegação à esquerda, para que pode acessá-lo facilmente na próxima vez. Em seguida, pode selecionar **DevTest Labs** no menu de navegação à esquerda.

    ![Todos os serviços - selecione o DevTest Labs](./media/devtest-lab-create-lab/all-services-select.png)
1. Na lista de laboratórios, selecione o laboratório em que pretende criar a VM.  
2. O laboratório **descrição geral** página, selecione **+ adicionar**.  

    ![Adicionar botão VM](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)
1. Sobre o **Vyberte bázi.** , selecione uma imagem do marketplace para a VM.
1. Sobre o **definições básicas** separador da **Máquina Virtual** página, efetue as seguintes ações: 
    1. Introduza um nome para a VM na **nome da Máquina Virtual** caixa de texto. A caixa de texto é previamente preenchida por si com um nome exclusivo gerado automaticamente. O nome corresponde ao nome de usuário dentro do seu endereço de e-mail, seguido por uma série de 3 dígitos exclusiva. Esse recurso poupa tempo para pensar num nome de máquina e escrevê-lo sempre que criar uma máquina. Se desejar, pode substituir este campo preenchido automaticamente com um nome à sua escolha. Para substituir o nome preenchido automaticamente para a VM, introduza um nome na **nome da Máquina Virtual** caixa de texto.
    2. Introduza um **nome de utilizador** que é concedido privilégios de administrador na máquina virtual. O **nome de utilizador** para a máquina é previamente preenchida com um nome exclusivo gerado automaticamente. O nome corresponde ao nome de usuário dentro do seu endereço de e-mail. Esse recurso poupa o tempo para decidir qual a um nome de utilizador sempre que criar uma nova máquina. Novamente, pode substituir este campo preenchido automaticamente com um nome de utilizador da sua preferência, se desejar. Para substituir o valor preenchido automaticamente para o nome de utilizador, introduza um valor na **nome de utilizador** caixa de texto. Este utilizador é concedido **administrador** privilégios na máquina virtual.
    3. Se estiver a criar primeira VM no laboratório, introduza um **palavra-passe** para o utilizador. Para guardar esta palavra-passe como uma palavra-passe predefinida no Cofre de chaves do Azure associada com o laboratório, selecione **guardar como palavra-passe predefinida**. A palavra-passe predefinida é guardado no Cofre de chaves com o nome: **VmPassword**. Quando tenta criar VMs subsequentes no laboratório, **VmPassword** é selecionado automaticamente para o **palavra-passe**. Para substituir o valor, desmarque a **utilizar um segredo guardado** caixa de verificação e introduza uma palavra-passe. 

        Pode também guardar segredos no Cofre de chaves em primeiro lugar e, em seguida, utilizá-lo ao criar uma VM no laboratório. Para obter mais informações, consulte [Store segredos num cofre de chaves](devtest-lab-store-secrets-in-key-vault.md). Para utilizar a palavra-passe armazenada no Cofre de chaves, selecione **utilizar um segredo guardado**e especifique um valor de chave que corresponda ao seu segredo (palavra-passe).
    4. Na **mais opções** secção, selecione **alterar tamanho**. Selecione um dos itens predefinidos que especificam os núcleos de processador, o tamanho de RAM e o tamanho de disco rígido da VM para criar.
    5. Selecione **adicionar ou remover artefactos**. Selecione e configure os artefatos que pretende adicionar à imagem de base.
    **Nota:** Se estiver familiarizado com o DevTest Labs ou configurar artefactos, consulte a [adicionar um artefato existente a uma VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) secção e, em seguida, volte aqui quando terminar.
2. Mude para o **definições avançadas** separador na parte superior, e fazer as seguintes ações:
    1. Para alterar a rede virtual que a VM está no, selecione **VNet alterar**. 
    2. Para alterar a sub-rede, selecione **alterar a sub-rede**. 
    3. Especifique se o endereço IP da VM **pública, privada ou partilhado**. 
    4. Para eliminar automaticamente a VM, especifique a **data de expiração e hora**. 
    5. Para fazer com que a VM reclamáveis por um utilizador de laboratório, selecione **Sim** para **conferir a esta máquina reclamáveis** opção. 
    6. Especifique o número do **instâncias de VM** que pretende disponibilizar aos seus utilizadores de laboratório. 
3. Selecione **criar** para adicionar a VM especificada para o laboratório.

   A página de laboratório apresenta o estado da criação da VM - primeiro como **Creating**, em seguida, como **em execução** depois da VM foi iniciada.

## <a name="next-steps"></a>Passos Seguintes
* Quando a VM tiver sido criada, pode ligar à VM selecionando **Connect** na página da VM.
* Confira [adicionar uma VM a um laboratório](devtest-lab-add-vm.md) para informações mais completas sobre como adicionar VMs subsequentes no seu laboratório.
* Explore os [Galeria de modelos de início rápido do DevTest Labs do Azure Resource Manager](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
