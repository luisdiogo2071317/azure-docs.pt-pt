---
title: Gerir definições de verificação de dois passos - do Azure AD | Microsoft Docs
description: Gerir a forma como utiliza o Azure multi-factor Authentication, incluindo alterar as informações de contacto ou configurar os seus dispositivos.
services: multi-factor-authentication
keywords: cliente de autenticação multifator, problema de autenticação, ID de correlação
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: lizross
ms.custom: end-user
ms.openlocfilehash: 81c2323dd65ee18d42a231306840eeb65bd09313
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37127463"
---
# <a name="manage-your-settings-for-two-step-verification"></a>Gerir as definições de verificação em dois passos
Este artigo responde a questões sobre como atualizar as definições de autenticação em dois passos de verificação ou multifator. Se estiver a ter problemas de início de sessão na sua conta, consulte [problemas com verificação de dois passos](multi-factor-authentication-end-user-troubleshoot.md) para resolução de problemas de ajuda.

## <a name="where-to-find-the-settings-page"></a>Onde encontrar a página de definições
Dependendo da forma como a sua empresa configurada da multi-factor Authentication do Azure, existem alguns locais onde pode alterar as definições, como o número de telefone.

Se suportar a sua empresa enviado um URL específico ou passos para gerir a verificação de dois passos, siga as instruções. Caso contrário, as seguintes instruções deverão funcionar para everybody pessoa. Se tiver siga estes passos, mas não vê as mesmas opções, que significa que a sua empresa ou escola personalizada os seus próprios portal. Peça ao seu administrador para a ligação ao portal do Azure multi-factor Authentication.

**Ir para a página de verificação de segurança adicionais**

- Aceda a https://aka.ms/MFASetup.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

Se clicar nessa ligação não resolver o problema, pode também aceder ao **verificação adicional de segurança** página seguindo estes passos:

1. Inicie sessão no [https://myapps.microsoft.com](https://myapps.microsoft.com)  

2. Selecione o nome da sua conta na parte superior direita, em seguida, selecione **perfil**.

3. Selecione **verificação adicional de segurança**.  

    ![MyApps](./media/multi-factor-authentication-end-user-manage-settings/myapps1.png)

4. Carrega a página de verificação de segurança adicional com as suas definições.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Quero alterar os meus número de telefone ou adicionar um número secundário
É importante configurar um número de telefone de autenticação secundária.  Porque o número de telefone principal e a sua aplicação móvel estão provavelmente no mesmo telefone, o número de telefone secundário é a única forma que vão conseguir aceder novamente à sua conta caso o seu telefone for perdido ou roubado.

> [!NOTE]
> Se não tiver acesso para o seu número de telefone principal e precisar de ajuda para obter sua conta, consulte o [problemas com verificação de dois passos](multi-factor-authentication-end-user-troubleshoot.md) artigo para obter mais ajuda.  

**Para alterar o número de telefone principal:**  

1. No **verificação adicional de segurança** página, selecione a caixa de texto com o número de telefone atuais e editá-lo com o novo número de telefone.  
2. Selecione **Guardar**.  
3. Se este número de telefone é o número que utilizar para a opção de verificação preferida, terá de verificar o número de novo antes de pode guardá-lo.  

**Para adicionar um número de telefone secundário:**  

1. Na página de verificação adicional de segurança, selecione a caixa junto **telefone de autenticação alternativo.**  
2. Introduza o seu número de telefone secundário na caixa de texto.  
3. Selecione **guardar** e terminar as suas alterações.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>Exigir verificação de dois passos novamente num dispositivo que tiver marcado como fidedigna

Dependendo das suas definições de organização, pode ter uma caixa de verificação que indica "não perguntar novamente durante **X** dias" quando efetuar verificação de dois passos no seu browser. Se esta caixa de verificação e, em seguida, perder o dispositivo ou considerar que a sua conta for comprometida, deve restaurar a verificação de dois passos para todos os seus dispositivos.

1. Na página de verificação adicional de segurança, selecione **restauro a autenticação multifator nos dispositivos anteriormente fidedignos**.
2. Da próxima vez que iniciar sessão em qualquer dispositivo, será solicitado para efetuar a verificação de dois passos.

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Como limpar Microsoft Authenticator a partir do meu dispositivo antigo e mover para um novo?
Quando desinstalar a aplicação do seu dispositivo ou repor o dispositivo, não remove a ativação no back-end. Para obter mais informações, consulte [Microsoft Authenticator](../../../../multi-factor-authentication/end-user/microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Passos Seguintes
* Obter sugestões de resolução de problemas e ajuda na [problemas com verificação de dois passos](multi-factor-authentication-end-user-troubleshoot.md)
* Configurar [palavras-passe de aplicação](../../../../multi-factor-authentication/end-user/multi-factor-authentication-end-user-app-passwords.md) para todas as aplicações que não suportam a verificação de dois passos.
