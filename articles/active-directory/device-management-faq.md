---
title: Gestão de dispositivos do Azure Active Directory FAQ | Microsoft Docs
description: Gestão de dispositivos do Azure Active Directory FAQ.
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
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 60b77f5956cb627905eb955995652098337c4dea
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309863"
---
# <a name="azure-active-directory-device-management-faq"></a>Gestão de dispositivos do Azure Active Directory FAQ



**P: como registar o dispositivo macOS?**

**R:** para registar o dispositivo macOS:

1.  [Criar uma política de conformidade](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [Definir uma política de acesso condicional para dispositivos de macOS](active-directory-conditional-access-azure-portal.md) 

**Comentários:**

- Os utilizadores que estão incluídos na sua política de acesso condicional tem um [a versão suportada do Office para macOS](active-directory-conditional-access-technical-reference.md#client-apps-condition) para aceder a recursos. 

- Durante a primeira tentativa de acesso, os utilizadores recebem um pedido para inscrever o dispositivo através do portal da empresa.

---

**P: Posso registado o dispositivo recentemente. Por que motivo não é possível ver o dispositivo nas minhas informações de utilizador no portal do Azure?**

**R:** dispositivos Windows 10 que são híbrida do Azure AD associado não mostrar dos dispositivos dos utilizadores.
Tem de utilizar a vista do todos os dispositivos no portal do Azure. Também pode utilizar o PowerShell [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.

Os seguintes dispositivos estão listados dos dispositivos dos utilizadores:

- Todos os dispositivos pessoais que não são híbrida do Azure AD associada. 
- Todos os não - Windows 10 / dispositivos do Windows Server 2016.
- Todos os dispositivos não Windows 

--- 

**P: como posso saber o que é o estado de registo do dispositivo do cliente?**

**R:** pode utilizar o portal do Azure, aceda a todos os dispositivos e procure o dispositivo com o ID de dispositivo. Verifique o valor na coluna de tipo de associação.

Se pretende verificar o estado de registo do dispositivo local de um dispositivo registado:

- Para Windows 10 e Windows Server 2016 ou posterior inscritos, execute dsregcmd.exe /status.
- Para versões de SO de nível inferior, execute "%programFiles%\Microsoft Join\autoworkplace.exe à área de trabalho"

---

**P: Posso ter eliminado no portal do Azure ou utilizar o Windows PowerShell, mas o estado local no dispositivo a indicar que permanece registado?**

**R:** isto é propositado. O dispositivo não terão acesso aos recursos na nuvem. 

Se pretender voltar a registar novamente, tem de ser uma ação manual para ser executada no dispositivo. 

Para limpar o estado de associação do Windows 10 e Windows Server 2016 que estão no local associados a um domínio do AD:

1.  Abra a linha de comandos como administrador.

2.  tipo `dsregcmd.exe /debug /leave`

3.  Terminar sessão e inicie sessão acionar a tarefa agendada que regista o dispositivo com o Azure AD novamente. 

Para versões de SO Windows de nível inferior que estão no local AD associados a um domínio:

1.  Abra a linha de comandos como administrador.
2.  Digite `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Digite `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---
* * P: Como posso anulação da associação a um dispositivo do Azure AD associados localmente no dispositivo?
**R:** 
- Para dispositivos do Azure AD associados híbrida, certifique-se desativar o registo automático para que a tarefa agendada não registar o dispositivo novamente. Em seguida, abra a linha de comandos como um administrador e escreva `dsregcmd.exe /debug /leave`. Em alternativa, este comando pode ser executado como um script em vários dispositivos ao desassociar em massa.

- Para puro associados do AD do Azure dispositivos, certifique-se de um administrador local offline conta ou criar um, como não conseguirá iniciar sessão com as credenciais de utilizador do Azure AD. Em seguida, aceda a **definições** > **contas** > **acesso ou escola**. Selecione a sua conta e clique em **desligar**. Siga as instruções e forneça as credenciais de administrador local quando lhe for pedido. Reinicie o dispositivo para concluir o processo de unjoin.

---

**P: por que razão vejo entradas de dispositivo duplicado no portal do Azure?**

**R:**

-   Para Windows 10 e Windows Server 2016, se existem tentativas repetidas de anulação da associação ao e voltar a associar o mesmo dispositivo, podem existir as entradas duplicadas. 

-   Se tiver utilizado a adicionar conta escolar ou profissional, cada utilizador do windows que utiliza a adicionar conta escolar ou profissional irá criar um novo registo de dispositivos com o mesmo nome de dispositivo.

-   Para versões de SO Windows de nível inferior que estão no local AD associados a um domínio utilizando o registo automático irá criar um novo registo de dispositivos com o mesmo nome de dispositivo para cada utilizador de domínio que inicia sessão no dispositivo. 

-   Um computador associado a do Azure AD que tenha sido apagado, reinstalado e novamente associado com o mesmo nome, será mostrado como outro registo com o mesmo nome de dispositivo.

---

**P: por que razão pode um utilizador ainda aceder aos recursos de um dispositivo que posso tiver desativado no portal do Azure?**

**R:** pode demorar até uma hora para um revogar sejam aplicadas.

>[!Note] 
>Para dispositivos inscritos, recomendamos que apagar o dispositivo para garantir que os utilizadores não é possível aceder aos recursos. Para obter mais detalhes, consulte [inscrever dispositivos para gestão no Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**P: por que razão os meus utilizadores vir "Que não pode chegar lá a partir daqui"?**

**R:** se configurou determinadas regras de acesso condicional para requerer um Estado de dispositivo específico e o dispositivo não cumpre os critérios, os utilizadores estão bloqueados e verá esta mensagem. Volte a avaliar as regras de política de acesso condicional e certifique-se de que o dispositivo é capaz de satisfazer os critérios para evitar esta mensagem.

---


**P: Posso ver o registo de dispositivos com as informações de utilizador no portal do Azure e pode ver o estado, tal como registado no dispositivo. Estou que posso configurar corretamente para utilizar o acesso condicional?**

**R:** o estado de associação do dispositivo, refletido pela deviceID, tem de corresponde à que sobre o Azure AD e cumprem os critérios de avaliação para o acesso condicional. Para obter mais detalhes, consulte [introdução ao registo de dispositivos do Azure Active Directory](active-directory-device-registration.md).

---

**P: por que motivo se receber uma mensagem "nome de utilizador ou palavra-passe está incorreto" para um dispositivo que posso ter apenas associado para o Azure AD?**

**R:** são razões comuns para este cenário:

- As suas credenciais de utilizador já não são válidos.

- O computador não consegue comunicar com o Azure Active Directory. Verifique a existência de quaisquer problemas de conectividade de rede.

- Não foram cumpridos os pré-requisitos associação do Azure AD. Certifique-se de que seguiu os passos para [Extending capacidades da nuvem para dispositivos Windows 10 através da associação do Azure Active Directory](active-directory-azureadjoin-overview.md).  

- Inícios de sessão federados requer o seu servidor de Federação para suportar um ponto final ativo de WS-Trust. 

- Ativou pass-through Authentication e o utilizador tem uma palavra-passe temporária que tem de ser alteradas no início de sessão.

---

**P: por que motivo consulte o "Oops... Ocorreu um erro!" diálogo ao tentar fazer do Azure AD associe o meu computador?**

**R:** este é um resultado de configuração de inscrição do Azure Active Directory com o Intune. Certifique-se de que o utilizador tentar fazer a associação do Azure AD tem a licença do Intune correta atribuída. Para obter mais detalhes, consulte [configurar a gestão de dispositivos Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**P: por que motivo meu tentativa de associação um PC falharem apesar de não recebi quaisquer informações de erro?**

**R:** uma causa provável é que o utilizador inicia sessão no dispositivo com a conta de administrador incorporada local. Crie uma conta local diferente antes de utilizar a associação do Azure Active Directory para concluir a configuração. 

---

**P: onde posso encontrar instruções para a configuração do registo automático de dispositivos?**

**R:** para obter instruções detalhadas, consulte [como configurar o registo automático de dispositivos associados a um domínio do Windows com o Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**P: onde posso encontrar a resolução de problemas de informações sobre o registo automático de dispositivos?**

**R:** para informações de resolução de problemas, consulte:

- [Resolução de problemas de registo automático de domínio associados a computadores com o Azure AD – Windows 10 e Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Registo automático de domínio de resolução de problemas associados a computadores com o Azure AD para clientes de nível inferior do Windows](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 
---

