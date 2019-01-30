---
title: Gerir o Cofre de chaves no Azure Stack através do portal | Documentos da Microsoft
description: Saiba como gerir o Cofre de chaves no Azure Stack com o portal
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: D4300668-461F-45F6-BF3B-33B502C39D17
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: bc73e80acddaac8676b4cb47c0b9ced2467cdc44
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245461"
---
# <a name="manage-key-vault-in-azure-stack-by-using-the-portal"></a>Gerir o Cofre de chaves no Azure Stack com o portal

Pode gerir o Cofre de chaves no Azure Stack com o portal do Azure Stack. Este artigo descreve como criar e gerir um cofre de chaves no Azure Stack.

## <a name="prerequisites"></a>Pré-requisitos

Tem de subscrever uma oferta que inclui o serviço Azure Key Vault.

## <a name="create-a-key-vault"></a>Criar um cofre de chaves

1. Inicie sessão para o [portal de utilizador](https://portal.local.azurestack.external).

2. A partir do dashboard, selecione **+ criar um recurso**, em seguida, **segurança + identidade**, em seguida, **Key Vault**.

    ![Ecrã do Key Vault](media/azure-stack-key-vault-manage-portal/image1.png)

3. Na **criar Key Vault** painel, atribuir um **nome** para o cofre. Nomes de cofre podem conter apenas carateres alfanuméricos e o caráter de hífen (-). Não deve começar com um número.

4. Escolher uma **subscrição** na lista de subscrições disponíveis. Todas as subscrições que oferecem o serviço de Key Vault são apresentadas na lista pendente.

5. Selecione um existente **grupo de recursos**, ou criar um novo.

6. Selecione o **escalão de preço**. No Azure Stack Development Kit (ASDK), suporte de cofres de chaves **padrão** apenas SKUs.

7. Escolha uma das existentes **políticas de acesso** ou criar um novo. Uma política de acesso permite-lhe conceder permissões de um utilizador, aplicação ou um grupo de segurança executar operações com este cofre.

8. Opcionalmente, escolha uma **política de acesso avançadas** para ativar o acesso aos recursos. Por exemplo: máquinas virtuais (VMs) para a implementação, Gestor de recursos para a implementação do modelo e o acesso ao Azure Disk Encryption para encriptação de volume.

9. Depois de configurar as definições, selecione **OK**e, em seguida, selecione **criar**. Esta ação inicia a implementação de Cofre de chaves.

## <a name="manage-keys-and-secrets"></a>Gerir chaves e segredos

Depois de criar um cofre, utilize o procedimento seguinte para criar e gerir chaves e segredos no cofre.

### <a name="create-a-key"></a>Criar uma chave

1. Inicie sessão para o [portal de utilizador](https://portal.local.azurestack.external).

2. A partir do dashboard, selecione **todos os recursos**, selecione o Cofre de chaves que criou anteriormente e, em seguida, selecione a **chaves** mosaico.

3. Na **chaves** painel, selecione **Add**.

4. Na **crie uma chave** painel, na lista de **opções**, escolha o método que pretende utilizar para criar uma chave. Pode **gerar** uma nova chave **carregar** existente de chaves ou utilizar **restaurar cópia de segurança** para selecionar uma cópia de segurança de uma chave.

5. Introduza um **nome** para a sua chave. O nome da chave pode conter apenas carateres alfanuméricos e o caráter de hífen (-).

6. Opcionalmente, configure as **definir data de ativação** e **definir data de expiração** valores para a sua chave.

7. Selecione **criar** para iniciar a implementação.

Depois da chave for criada com êxito, pode selecionar-o para baixo **chaves** e ver ou modificar as respetivas propriedades. A secção de propriedades contém os **identificador de chave**, que é um identificador URI (Uniform Resource) que aplicativos externos utilizam para aceder esta chave. Para limitar as operações nesta chave, configure as definições sob **permitido operações**.

![Chave URI](media/azure-stack-key-vault-manage-portal/image4.png)

### <a name="create-a-secret"></a>Criar um segredo

1. Inicie sessão para o [portal de utilizador](https://portal.local.azurestack.external).

2. A partir do dashboard, selecione **todos os recursos**, selecione o Cofre de chaves que criou anteriormente e, em seguida, selecione a **segredos** mosaico.

3. Sob **segredos**, selecione **Add**.

4. Sob **criar um segredo**, na lista de **opções de carregamento**, escolha uma opção com a qual pretende criar um segredo. Pode criar um segredo **manualmente** se introduzir um valor para o segredo ou o carregamento de um **certificado** do seu computador local.

5. Introduza um **nome** para o segredo. O nome secreto pode conter apenas carateres alfanuméricos e o caráter de hífen (-).

6. Opcionalmente, especifique a **tipo de conteúdo**e configurar os valores para **definir data de ativação** e **definir data de expiração** para o segredo.

7. Selecione **criar** para iniciar a implementação.

Após o segredo é criado com êxito, pode selecionar-o para baixo **segredos** e ver ou modificar as respetivas propriedades. O **identificador de segredo** é um URI que aplicativos externos podem utilizar para aceder Este segredo.

![Segredo do URI](media/azure-stack-key-vault-manage-portal/image5.png)

## <a name="next-steps"></a>Passos Seguintes

* [Implementar uma VM ao obter a palavra-passe armazenada no Cofre de chaves](azure-stack-key-vault-deploy-vm-with-secret.md)
* [Implementar uma VM com certificado armazenado no Cofre de chaves](azure-stack-key-vault-push-secret-into-vm.md)
