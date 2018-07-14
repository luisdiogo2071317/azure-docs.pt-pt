---
title: Gerir fórmulas no Azure DevTest Labs para criar VMs | Documentos da Microsoft
description: Saiba como atualizar e remover as fórmulas de Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: b7a68f545f60829e5da83f0734c57a4d210cb843
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39001486"
---
# <a name="manage-azure-devtest-labs-formulas"></a>Gerir fórmulas de Azure DevTest Labs

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

Este artigo ilustra como criar uma fórmula de uma base de (imagem personalizada, imagem do Marketplace ou outra fórmula) ou uma VM existente. Este artigo também o orienta por meio do gerenciamento de fórmulas existentes.

## <a name="create-a-formula"></a>Criar uma fórmula
Qualquer pessoa com o DevTest Labs *utilizadores* permissões é capaz de criar VMs com uma fórmula como base. Existem duas formas de criar fórmulas: 

* A partir de uma base - Use quando desejar definir todas as características da fórmula.
* A partir de um laboratório existente VM - utilização quando pretender criar uma fórmula baseada nas definições de uma VM existente.

Para obter mais informações sobre como adicionar utilizadores e permissões, consulte [adicionar proprietários e utilizadores no Azure DevTest Labs](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>Criar uma fórmula a partir de uma base
Os seguintes passos guiá-lo pelo processo de criação de uma fórmula de uma imagem personalizada, imagem do Marketplace ou outra fórmula.

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

2. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.

3. Na lista de laboratórios, selecione o laboratório pretendido.  

4. No painel do laboratório, selecione **fórmulas (reutilizáveis bases)**.
   
    ![Menu de fórmulas](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. Sobre o **fórmulas** painel, selecione **+ adicionar**.
   
    ![Adicionar uma fórmula](./media/devtest-lab-create-formulas/add-formula.png)

6. Sobre o **Vyberte bázi.** painel, selecione base (imagem personalizada, imagem do Marketplace ou fórmula) a partir do qual pretende cria a fórmula.
   
    ![Lista de base](./media/devtest-lab-create-formulas/base-list.png)

7. Sobre o **criar fórmula** painel, especifique os seguintes valores:
   
    * **Nome da fórmula** -introduza um nome para a sua fórmula. Este valor é apresentado na lista de imagens de base ao criar uma VM. O nome é validado como escrever e, se não for válido, uma mensagem indica os requisitos para um nome válido.
    * **Descrição** -introduza uma descrição relevante para a sua fórmula. Este valor é disponível no menu de contexto da fórmula quando cria uma VM.
    * **Nome de utilizador** -introduza um nome de utilizador que é concedido privilégios de administrador.
    * **Palavra-passe** – introduza - ou selecione na lista pendente - um valor que estão associado com o segredo (palavra-passe) que pretende utilizar para o utilizador especificado. Para saber mais sobre a guardar segredos num cofre de chaves e utilizá-las durante a criação de recursos do laboratório, consulte [Store segredos no Azure Key Vault](devtest-lab-store-secrets-in-key-vault.md).
    * **Tipo de disco da máquina virtual** - Especifique qualquer um dos HDD (unidade de disco rígido) ou SSD (unidade de estado sólido) para indicar o tipo de disco de armazenamento é permitida para as máquinas virtuais aprovisionadas com esta imagem de base.
    * * *: Máquina Virtual tamanho * * - selecione um dos itens predefinidos que especificam os núcleos de processador, o tamanho de RAM e o tamanho de disco rígido da VM para criar. 
    * **Artefactos** - Selecione para abrir o **adicione os artefactos** painel, em que selecionar e configurar os artefatos que pretende adicionar à imagem de base. Para obter mais informações sobre artefatos, consulte [criar artefactos personalizados para a máquina virtual do Azure DevTest Labs](devtest-lab-artifact-author.md).
    * **Definições avançadas** - Selecione para abrir o **Advanced** painel em que configure as seguintes definições:
        * **Rede virtual** -especifique a rede virtual pretendida.
        * **Sub-rede** -especifique a sub-rede pretendida.    
        * **Configuração do endereço IP** -Especifique se pretende que os endereços público, privado ou IP partilhados. Para obter mais informações sobre endereços IP partilhados, consulte [compreender partilhado endereços IP no Azure DevTest Labs](./devtest-lab-shared-ip.md).
        * **Tornar esta máquina reclamáveis** -tornar uma máquina "reclamáveis" significa que ele não será atribuído propriedade no momento da criação. Em vez disso, os utilizadores de laboratório poderá assumir a propriedade ("declaração") a máquina no painel do laboratório.     
    * **Imagem** -este campo apresenta o nome da imagem base que selecionou no painel anterior. 
     
       ![Criar a fórmula](./media/devtest-lab-create-formulas/create-formula.png)

8. Selecione **criar** para criar a fórmula.

9. Quando tiver sido criada a fórmula, será apresentada na lista na **fórmulas** painel.

### <a name="create-a-formula-from-a-vm"></a>Criar uma fórmula de uma VM
Os seguintes passos guiá-lo pelo processo de criação de uma fórmula com base numa VM existente. 

> [!NOTE]
> Para criar uma fórmula de uma VM, a VM tem de ter sido criada após 30 de Março de 2016. 
> 
> 

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.
3. Na lista de laboratórios, selecione o laboratório pretendido.  
4. O laboratório **descrição geral** painel, selecione a VM a partir do qual pretende criar a fórmula.
   
    ![VMs de laboratórios](./media/devtest-lab-create-formulas/my-vms.png)
5. No painel da VM, selecione **criar fórmula (base reutilizável)**.
   
    ![Criar a fórmula](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. Na **criar fórmula** painel, introduza um **nome** e **Descrição** para sua nova fórmula.
   
    ![Criar o painel de fórmula](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Selecione **OK** para criar a fórmula.

## <a name="modify-a-formula"></a>Modificar uma fórmula
Para modificar uma fórmula, siga estes passos:

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.
3. Na lista de laboratórios, selecione o laboratório pretendido.  
4. No painel do laboratório, selecione **fórmulas (reutilizáveis bases)**.
   
    ![Menu de fórmulas](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Sobre o **fórmulas de laboratório** painel, selecione a fórmula que pretende modificar.
6. Sobre o **Aktualizovat vzorec** painel, faça as edições pretendidas e selecione **atualização**.

## <a name="delete-a-formula"></a>Eliminar uma fórmula
Para eliminar uma fórmula, siga estes passos:

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.
3. Na lista de laboratórios, selecione o laboratório pretendido.  
4. O laboratório **configurações** painel, selecione **fórmulas**.
   
    ![Menu de fórmulas](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Sobre o **fórmulas de laboratório** painel, selecione as reticências à direita da fórmula que pretende eliminar.
   
    ![Menu de fórmulas](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. No menu de contexto a fórmula, selecione **eliminar**.
   
    ![Menu de contexto de fórmulas](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Selecione **Sim** na caixa de diálogo de confirmação de eliminação.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Postagens de blogs relacionados
* [Imagens personalizadas ou fórmulas?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Passos Seguintes
Depois de criar uma fórmula para utilização quando criar uma VM, a próxima etapa é [adicionar uma VM para o laboratório](devtest-lab-add-vm.md).

