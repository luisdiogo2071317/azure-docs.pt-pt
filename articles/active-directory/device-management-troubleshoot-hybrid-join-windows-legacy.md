---
title: "Resolução de problemas híbrida do Azure Active Directory dispositivos associados a um nível baixo | Microsoft Docs"
description: "Resolução de problemas híbrida do Azure Active Directory dispositivos associados a um nível inferior."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 5657df412b1f2b7d4d43d7551289620ae4d77de2
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Resolução de problemas híbrida do Azure Active Directory dispositivos associados a um nível baixo 

Este tópico é apenas aplicável a dispositivos que se seguem: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Para Windows 10 ou Windows Server 2016, consulte [híbrida de resolução de problemas do Azure Active Directory associados de dispositivos Windows 10 e Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md).

Este tópico pressupõe que tem [dispositivos associados ao configurado híbrida do Azure Active Directory](device-management-hybrid-azuread-joined-devices-setup.md) para suportar os cenários seguintes:

- Acesso condicional baseado no dispositivo

- [Enterprise roaming das definições](active-directory-windows-enterprise-state-roaming-overview.md)

- [Windows Hello para empresas](active-directory-azureadjoin-passport-deployment.md) 





Este tópico fornece orientações sobre como resolver potenciais problemas de resolução de problemas.  

**O que deve conhecer:** 

- O número máximo de dispositivos por utilizador está centrada nos dispositivos. Por exemplo, se *jdoe* e *jharnett* início de sessão num dispositivo, um registo separados (DeviceID) é criada para cada um no **utilizador** separador de informações.  

- O registo inicial / associação de dispositivos está configurada para efetuar uma tentativa de início de sessão ou bloquear / desbloquear. Pode haver atraso de 5 minutos acionado por uma tarefa de Programador de tarefas. 

- Reinstalar o sistema operativo ou uma manual unregister e volte a registar podem criar um novo registo no Azure AD e resulta em várias entradas no separador de informações de utilizador no portal do Azure. 


## <a name="step-1-retrieve-the-registration-status"></a>Passo 1: Obter o estado do registo 

**Para verificar o estado do registo:**  

1. Abra a linha de comandos como administrador 

2. tipo `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

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
    
    1. Se o utilizador com sessão iniciado não é um utilizador de domínio (por exemplo, um utilizador local). Associação do Azure AD híbrido nos dispositivos de nível inferior só é suportada para os utilizadores de domínio.
    
    2. Se, por qualquer motivo, Autoworkplace.exe conseguir autenticar silenciosamente com o Azure AD ou AD FS. Algumas das razões possíveis podem ser problemas de conectividade de rede out-vinculado aos URLs de AD do Azure (Verifique os pré-requisitos) ou se o MFA é ativada/configurado para o utilizador, mas WIAORMUTLIAUTHN não está configurado no servidor de Federação (passos de configuração de verificação). Possibilidade de outra é nessa página do realm inicial (HRD) de deteção está a aguardar interação do utilizador, impedindo Autoworkplace.exe obtenham silenciosamente um 
    
    3. Se a organização está a utilizar o Azure AD totalmente integrada Single Sign-On, os URLs seguintes não estão presentes i/e as definições no dispositivo da intranet:
    - https://autologon.microsoftazuread-sso.com
    - https://aadg.windows.net.nsatc.net
    
    e a definição "Permitir que as atualizações à barra de estado através do script" tem de estar ativada para a zona de Intranet.

- Foi atingida a quota de uma

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- O serviço não está a responder 

    ![Associação à área de trabalho para Windows](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Também pode encontrar as informações de estado no registo de eventos em **aplicações e serviços Log\Microsoft-Workplace Join**.
  
**As causas mais comuns para uma associação de falha híbrida do Azure AD são:** 

- O computador não está na rede interna da organização ou uma VPN sem ligação a um local controlador de domínio do AD.

- Tem sessão iniciada num computador com uma conta de computador local. 

- Problemas de configuração do serviço: 

  - O servidor de Federação foi configurado para suportar **WIAORMULTIAUTHN**. 

  - Não há nenhum que aponta para o nome de domínio verificado no Azure AD na floresta do AD em que o computador pertence ao objeto de ponto de ligação de serviço.

  - Um utilizador atingiu o limite de dispositivos. 

## <a name="next-steps"></a>Passos Seguintes

Para perguntas, consulte o [perguntas mais frequentes sobre a gestão de dispositivos](device-management-faq.md)  
