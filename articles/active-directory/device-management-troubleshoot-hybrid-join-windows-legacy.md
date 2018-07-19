---
title: Resolução de problemas híbrida do Azure Active Directory dispositivos associados a um nível inferior | Documentos da Microsoft
description: Resolução de problemas híbrida do Azure Active Directory dispositivos associados a um nível inferior.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 3b99a82b59cbba22d30a4325e246c308a2042ad5
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136312"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Resolução de problemas híbrida do Azure Active Directory dispositivos associados a um nível inferior 

Este artigo é aplicável apenas aos seguintes dispositivos: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Para Windows 10 ou Windows Server 2016, veja [híbrido de resolução de problemas do Azure Active Directory associou-se os dispositivos Windows 10 e Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).

Este artigo pressupõe que tenha [dispositivos associados ao configurado híbrida do Azure Active Directory](device-management-hybrid-azuread-joined-devices-setup.md) para suportar os cenários seguintes:

- Acesso condicional com base no dispositivo

- [Definições de roaming empresarial](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello para empresas](active-directory-azureadjoin-passport-deployment.md) 





Este artigo fornece orientações sobre como resolver problemas potenciais de resolução de problemas.  

**O que deve saber:** 

- O número máximo de dispositivos por utilizador é centrada nos dispositivos. Por exemplo, se *jdoe* e *jharnett* início de sessão para um dispositivo, um registo separados (DeviceID) é criada para cada uma no **utilizador** separador informações.  

- O registo inicial / associação de dispositivos é configurada para executar uma tentativa de início de sessão ou bloqueio / desbloqueio. Pode haver atraso de 5 minutos, acionado por uma tarefa do agendador de tarefas. 

- Uma reinstalação do sistema operativo ou re-registo manual pode criar um novo registo no Azure AD, o que resulta em várias entradas sob o separador de informações do utilizador no portal do Azure. 

- Certifique-se [KB4284842](https://support.microsoft.com/en-us/help/4284842) estiver instalado, no caso do Windows 7 SP1 ou Windows Server 2008 R2 SP1. Esta atualização impede que as falhas de autenticação de futuras devido a perda de acesso do cliente para chaves protegidas depois de alterar a palavra-passe.

## <a name="step-1-retrieve-the-registration-status"></a>Passo 1: Obter o estado do registo 

**Para verificar o estado do registo:**  

1. Início de sessão com a conta de utilizador que efetuou a associação do Azure AD híbrido.

2. Abra a linha de comandos como administrador 

3. Tipo `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Este comando apresenta uma caixa de diálogo que lhe fornece mais detalhes sobre o estado de associação.

![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>Passo 2: Avaliar a Estado de associação ao Azure AD de híbrida 

Se a associação do Azure AD híbrido não foi concluída com êxito, a caixa de diálogo fornece detalhes sobre o problema que ocorreu.

**Os problemas mais comuns são:**

- Uma configuração incorreta do AD FS ou o Azure AD

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Não tem sessão iniciada como um utilizador de domínio

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)
    
    Há alguns motivos diferentes, por que isso pode ocorrer:
    
    - O utilizador com sessão iniciada não é um utilizador de domínio (por exemplo, um utilizador local). Associação do Azure AD híbrido em dispositivos de nível inferior é suportada apenas para utilizadores de domínio.
    
    - Autoworkplace.exe não conseguiu autenticar silenciosamente com o Azure AD ou AD FS. Isto pode dever-se um problemas de conectividade de rede out-vinculado aos URLs do Azure AD. Também pode ser que a autenticação multifator (MFA) é ativada ou não está configurado para o utilizador e WIAORMUTLIAUTHN não está configurado para o servidor de Federação. Outra possibilidade é essa página de deteção (HRD) de realm inicial está a aguardar interação do usuário, que impede **autoworkplace.exe** silenciosamente obtenham um token.
    
    - Sua organização utiliza o Azure AD totalmente integrada início de sessão único, `https://autologon.microsoftazuread-sso.com` ou `https://aadg.windows.net.nsatc.net` não estão presentes nas definições de intranet de IE do dispositivo, e **permitir atualizações à barra de estado por meio de script** não está ativada para a zona da Intranet.

- Foi atingida uma quota

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- O serviço não está a responder 

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Também pode encontrar as informações de estado no registo de eventos em: **aplicativos e serviços Log\Microsoft-Workplace Join**
  
**As causas mais comuns para uma associação do Azure AD híbrido de falha são:** 

- O computador não está ligado à rede interna da sua organização ou para uma VPN com uma ligação com o local do controlador de domínio do AD.

- Tiver sessão iniciada seu computador com uma conta de computador local. 

- Problemas de configuração do serviço: 

  - O servidor de Federação foi configurado para suportar **WIAORMULTIAUTHN**. 

  - Floresta do seu computador não tem qualquer objeto de ponto de ligação de serviço que aponta para o seu nome de domínio verificado no Azure AD 

  - Um utilizador atingir o limite de dispositivos. 

## <a name="next-steps"></a>Passos Seguintes

Para perguntas, consulte o [perguntas frequentes de gestão de dispositivos](device-management-faq.md)  
