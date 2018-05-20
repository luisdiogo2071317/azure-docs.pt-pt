---
title: Utilizar as aplicações no Azure Blockchain Workbench
description: Como utilizar a aplicação contrai no Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/16/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: b15d4980875b805b49f7dc1cdb941e607232f3cb
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="using-applications-in-azure-blockchain-workbench"></a>Utilizar as aplicações no Azure Blockchain Workbench

Pode utilizar o Blockchain Workbench para criar e executar ações contratos. Também pode ver detalhes, tais como o histórico de estado e a transação de contrato.

## <a name="prerequisites"></a>Pré-requisitos

* Uma implementação de Blockchain Workbench. Para obter mais informações, consulte [implementação do Azure Blockchain Workbench](blockchain-workbench-deploy.md) para obter detalhes sobre a implementação
* Uma aplicação implementada blockchain Blockchain Workbench. Consulte [criar uma aplicação de blockchain no Azure Blockchain Workbench]()

[Abrir o Blockchain Workbench](blockchain-workbench-deploy.md#blockchain-workbench-web-url) no seu browser.

![Blockchain Workbench](media/blockchain-workbench-use/workbench.png)

Terá de iniciar sessão como membro do Blockchain Workbench. Se não houver nenhuma aplicação listada, é um membro do Blockchain Workbench mas não um membro de quaisquer aplicações. O administrador do Blockchain Workbench pode atribuir membros para aplicações.

## <a name="create-new-contract"></a>Criar novo contrato 

Para criar um novo contrato, tem de ser um membro especificado como um contrato **iniciador**. Para definir funções da aplicação e os iniciadores para o contrato de informações, consulte [fluxos de trabalho na descrição geral configuração](blockchain-workbench-configuration-overview.md#workflows). Para obter informações sobre a atribuição de membros para funções da aplicação, consulte [adicionar um membro à aplicação](blockchain-workbench-manage-users.md#add-member-to-application).

1. Na secção de aplicação Blockchain Workbench, selecione o mosaico de aplicação que contém o contrato que pretende criar. Uma lista de contratos de Active Directory são apresentados.

2. Para criar um novo contrato, selecione **novo contrato**.

    ![Novo botão de contrato](media/blockchain-workbench-use/contract-list.png)

3. O **novo contrato** é apresentado o painel. Especifique os valores de parâmetros inicial. Selecione **Criar**.

    ![Novo painel do contrato](media/blockchain-workbench-use/new-contract.png)

    O contrato criado recentemente, é apresentado na lista com os outros contratos de Active Directory.

    ![Lista de contratos de Active Directory](media/blockchain-workbench-use/active-contracts.png)

## <a name="take-action-on-contract"></a>Ação no contrato

Dependendo do Estado o contrato é, os membros podem efetuar ações para transição para o estado próximo do contrato. As ações são definidas como [transições](blockchain-workbench-configuration-overview.md#transitions) dentro de um [estado](blockchain-workbench-configuration-overview.md#states). Os membros que pertencem a uma função de aplicação ou instância permitida para a transição poderá demorar a ação. 

1. Na secção de aplicação Blockchain Workbench, selecione o mosaico de aplicação que contém o contrato a ação.
2. Selecione o contrato na lista. São apresentados os detalhes sobre o contrato nas secções diferentes. 

    ![Detalhes do contrato ](media/blockchain-workbench-use/contract-details.png)

    | Section  | Descrição  |
    |---------|---------|
    | Estado | Apresenta o progresso atual dentro as fases de contrato |
    | Detalhes | Os valores atuais do contrato |
    | Ação | Detalhes sobre a última ação |
    | Atividade | Histórico de transação do contrato |
    
3. No **ação** secção, selecione **ação**.

4. São apresentados os detalhes sobre o estado atual do contrato num painel. Escolha a ação que pretende colocar na lista pendente. 

    ![Escolha a ação](media/blockchain-workbench-use/choose-action.png)

5. Selecione **ação** para iniciar a ação.
6. Se os parâmetros são necessários para a ação, especifique os valores para a ação.

    ![Tomar medidas](media/blockchain-workbench-use/take-action.png)

7. Selecione **ação** para executar a ação.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Como resolver problemas do Azure Blockchain Workbench](blockchain-workbench-troubleshooting.md)