---
title: Credenciais do Azure Stack store serviço principal no Key Vault | Documentos da Microsoft
description: Saiba como o Cofre de chaves armazena as credenciais do principal de serviço no Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2019
ms.author: sethm
ms.openlocfilehash: 570c1adc2f4615e78cbe5656c13b0e22b863baf7
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54192287"
---
# <a name="store-service-principal-credentials-in-key-vault"></a>Store credenciais do principal de serviço no Key Vault

Desenvolver aplicações no Azure Stack, normalmente, requer a criação de um serviço principal e usando essas credenciais para autenticar antes de implementar. No entanto, muitas vezes, as credenciais armazenadas para o principal de serviço são perdidas. Este artigo descreve como criar um serviço principal e armazenar os valores no Azure Key Vault para uma recuperação posterior.

Para obter mais informações sobre o Key Vault, consulte [este artigo](azure-stack-key-vault-intro.md).

## <a name="prerequisites"></a>Pré-requisitos

- Uma subscrição para uma oferta que inclui o serviço Azure Key Vault.
- PowerShell está configurado para utilização com o Azure Stack.

## <a name="key-vault-in-azure-stack"></a>Cofre de chaves no Azure Stack

Cofre de chaves no Azure Stack ajuda a salvaguardar chaves criptográficas e segredos na cloud a aplicações e serviços utilizam. Ao utilizar o Key Vault, pode encriptar chaves e segredos.

Para criar um cofre de chaves, siga estes passos:

1. Inicie sessão no portal do Azure Stack.

2. A partir do dashboard, selecione **+ criar um recurso**, em seguida, **segurança + identidade**, em seguida, selecione **Key Vault.**

   ![Criar o cofre de chaves](media/azure-stack-key-vault-store-credentials/create-key-vault.png)

3. Na **criar Key Vault** painel, atribuir um **nome** para o cofre. Nomes de cofre podem conter apenas carateres alfanuméricos e o caráter de hífen (-). Não deve começar com um número.

4. Escolha uma subscrição na lista de subscrições disponíveis.

5. Selecione um grupo de recursos existente ou crie um novo.

6. Selecione o escalão de preço.

7. Escolha uma das políticas de acesso existente ou crie um novo. Uma política de acesso permite-lhe conceder permissões de um utilizador, aplicação ou um grupo de segurança executar operações com este cofre.

8. Opcionalmente, escolha uma política de acesso avançadas para ativar o acesso aos recursos.

9. Depois de configurar as definições, selecione **OK**e, em seguida, selecione **criar**. Começa a implementação de Cofre de chaves.

## <a name="create-a-service-principal"></a>Criar um principal de serviço

1. Inicie sessão na sua conta do Azure através do portal do Azure.

2. Selecione **do Azure Active Directory**, em seguida, **registos das aplicações**, em seguida, **Add**.

3. Indique um nome e um URL para a aplicação. Selecione **aplicação Web / API** ou **nativo** para o tipo de aplicação que pretende criar. Depois de definir os valores, selecione **criar**.

4. Selecione **do Active Directory**, em seguida, **registos das aplicações**e selecione a aplicação.

5. Copie o **ID da Aplicação** e armazene-o no código da aplicação. As aplicações nas aplicações de exemplo utilizam **ID de cliente** quando nos Referimos ao **ID de aplicação**.

6. Para gerar uma chave de autenticação, selecione **Chaves**.

7. Forneça uma descrição para a chave e um período de tempo.

8. Selecione **Guardar**.

9. Copiar o **chave** que fica disponível depois que clicou **guardar**.

## <a name="store-the-service-principal-inside-key-vault"></a>Store o principal de serviço no Key Vault

1. Inicie sessão no portal de utilizador para o Azure Stack, em seguida, selecione o Cofre de chaves que criou anteriormente e, em seguida, selecione o **segredo** mosaico.

2. Na **segredo** painel, selecione **gerar/importar**.

3. Na **criar um segredo** painel, na lista de opções select **Manual**.

4. Introduza **o ID da aplicação** copiados do principal de serviço, como o nome para a sua chave. O nome da chave pode conter apenas carateres alfanuméricos e o caráter de hífen (-).

5. Cole o valor da chave do serviço principal para o **valor** separador.

6. Selecione **Principal de serviço** para o **tipo de conteúdo**.

7. Definir o **data de ativação** e **data de expiração** valores para a sua chave.

8. Selecione **criar** para iniciar a implementação.

Depois do segredo é criado com êxito, as informações do principal de serviço serão armazenadas aqui. Pode selecioná-lo em qualquer altura em **segredos**e ver ou modificar as respetivas propriedades. A secção de propriedades contém o identificador SECRETO, o que é um identificador URI (Uniform Resource) que aplicativos externos utilizam para aceder Este segredo.

## <a name="next-steps"></a>Passos Seguintes

- [Utilizar principais de serviço](azure-stack-create-service-principals.md)
- [Gerir o Cofre de chaves no Azure Stack pelo portal](azure-stack-key-vault-manage-portal.md)  
- [Gerir o Cofre de chaves no Azure Stack com o PowerShell](azure-stack-key-vault-manage-powershell.md)