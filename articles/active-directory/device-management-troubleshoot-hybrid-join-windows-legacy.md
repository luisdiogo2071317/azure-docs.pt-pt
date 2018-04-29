---
title: Resolução de problemas híbrida do Azure Active Directory dispositivos associados a um nível baixo | Microsoft Docs
description: Resolução de problemas híbrida do Azure Active Directory dispositivos associados a um nível inferior.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 43c1907bf3f9bb8eea92dc02889df24a5a0cc9e3
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Resolução de problemas híbrida do Azure Active Directory dispositivos associados a um nível baixo 

Este artigo é aplicável apenas para os seguintes dispositivos: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Para Windows 10 ou Windows Server 2016, consulte [híbrida de resolução de problemas do Azure Active Directory associados de dispositivos Windows 10 e Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).

Este artigo pressupõe que tem [dispositivos associados ao configurado híbrida do Azure Active Directory](device-management-hybrid-azuread-joined-devices-setup.md) para suportar os cenários seguintes:

- Acesso condicional baseado no dispositivo

- [Enterprise roaming das definições](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello para empresas](active-directory-azureadjoin-passport-deployment.md) 





Este artigo fornece orientações sobre como resolver potenciais problemas de resolução de problemas.  

**O que deve conhecer:** 

- O número máximo de dispositivos por utilizador está centrada nos dispositivos. Por exemplo, se *jdoe* e *jharnett* início de sessão num dispositivo, um registo separados (DeviceID) é criada para cada um no **utilizador** separador de informações.  

- O registo inicial / associação de dispositivos está configurada para efetuar uma tentativa de início de sessão ou bloquear / desbloquear. Pode haver atraso de 5 minutos acionado por uma tarefa de Programador de tarefas. 

- Reinstalar o sistema operativo ou re-registos manuais, pode criar um novo registo no Azure AD, o que resulta em várias entradas no separador de informações de utilizador no portal do Azure. 

## <a name="step-1-retrieve-the-registration-status"></a>Passo 1: Obter o estado do registo 

**Para verificar o estado do registo:**  

1. Início de sessão com a conta de utilizador que efetuou a associação do Azure AD híbrido.

2. Abra a linha de comandos como administrador 

3. tipo `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Este comando apresenta uma caixa de diálogo que fornece-lhe obter mais detalhes sobre o estado de associação.

![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Passo 2: Avaliar a Estado de associação do Azure AD de híbrida 

Se a associação do Azure AD híbrido não foi bem sucedida, a caixa de diálogo fornece detalhes sobre o problema ocorreu.

**Os problemas mais comuns são:**

- Uma configuração incorreta do AD FS ou o Azure AD

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Não tem sessão iniciada como um utilizador de domínio

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)
    
    Existem alguns diferentes motivos por que motivo esta situação pode ocorrer:
    
    - O utilizador com sessão iniciada não é um utilizador de domínio (por exemplo, um utilizador local). Associação do Azure AD híbrido nos dispositivos de nível inferior só é suportada para os utilizadores de domínio.
    
    - Autoworkplace.exe não consegue autenticar silenciosamente com o Azure AD ou AD FS. Isto pode dever-se um problemas de conectividade de rede out-vinculado aos URLs AD do Azure (Verifique os pré-requisitos). Também poderia ser que a autenticação multifator (MFA) é ativado/configurado para o utilizador e WIAORMUTLIAUTHN não está configurado no servidor de Federação (passos de configuração de verificação). Possibilidade de outra é nessa página do realm inicial (HRD) de deteção está a aguardar interação do utilizador, que impede que **autoworkplace.exe** obtenham silenciosamente um token.
    
    - A organização utiliza o Azure AD totalmente integrada Single Sign-On, `https://autologon.microsoftazuread-sso.com` não está presente i/e as definições no dispositivo da intranet, e **permita que as atualizações na barra de estado através do script** não está ativada para a zona de Intranet.

- Foi atingida a quota de uma

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- O serviço não está a responder 

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Também pode encontrar as informações de estado no registo de eventos em: **aplicações e serviços Log\Microsoft-Workplace Join**
  
**As causas mais comuns para uma associação de falha híbrida do Azure AD são:** 

- O computador não está ligado à rede interna da sua organização, nem para uma VPN com uma ligação para o seu local controlador de domínio do AD.

- Tem sessão iniciada num computador com uma conta de computador local. 

- Problemas de configuração do serviço: 

  - O servidor de Federação foi configurado para suportar **WIAORMULTIAUTHN**. 

  - Floresta do seu computador não tem qualquer objeto de ponto de ligação de serviço que aponta para o nome de domínio verificado no Azure AD 

  - Um utilizador atingiu o limite de dispositivos. 

## <a name="next-steps"></a>Passos Seguintes

Para perguntas, consulte o [perguntas mais frequentes sobre a gestão de dispositivos](device-management-faq.md)  
