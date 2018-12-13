---
title: Gestão de dispositivos do Azure Active Directory perguntas frequentes | Documentos da Microsoft
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
ms.openlocfilehash: a0cfd65aa2444956336e5363d20acab61a404c68
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309183"
---
# <a name="azure-active-directory-device-management-faq"></a>Gestão de dispositivos do Azure Active Directory FAQ

**P: Recentemente eu registado o dispositivo. Por que motivo não vejo o dispositivo em minhas informações de utilizador no portal do Azure? Ou, por que é proprietário do dispositivo marcado como n/d para dispositivos associados ao Azure AD de híbrida? ** 
 **R:** Dispositivos Windows 10 que estão associados ao Azure AD híbrido não aparecem em dispositivos de utilizador.
Tem de utilizar a vista de todos os dispositivos no portal do Azure. Também pode utilizar o PowerShell [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.

Apenas os seguintes dispositivos estão listados nos dispositivos de utilizador:

- Todos os dispositivos pessoais que não são do Azure AD híbrido associado. 
- Todos os não - Windows 10 / dispositivos do Windows Server 2016.
- Todos os dispositivos não Windows 

--- 

**P: Como posso saber o que é o estado de registo de dispositivo do cliente?**

**R:** Pode utilizar o portal do Azure, aceda a todos os dispositivos e procure o dispositivo com o ID de dispositivo. Verifique o valor na coluna de tipo de associação. Às vezes, o dispositivo foi foram repor ou recriar a imagem. Por isso, é essencial para também demasiado a verificar o estado de registo do dispositivo no dispositivo:

- Para Windows 10 e Windows Server 2016 ou posterior inscritos, execute dsregcmd.exe /status.
- Para versões de SO de nível inferior, execute "%programFiles%\Microsoft Join\autoworkplace.exe à área de trabalho"

---

**P: Posso ver o registo de dispositivo sob as informações de utilizador no portal do Azure e pode ver o estado como registrado no dispositivo. Estou posso configurar corretamente para utilizar o acesso condicional?**

**R:** O estado de associação de dispositivo, refletido pela deviceID, tem de corresponder com isso no Azure AD e satisfazer quaisquer critérios de avaliação para o acesso condicional. Para obter mais informações, consulte [exigir dispositivos para aceder à aplicação de cloud com o acesso condicional geridos](../conditional-access/require-managed-devices.md).

---

**P: Posso ter eliminado no portal do Azure ou utilizando o Windows PowerShell, mas o estado local no dispositivo diz que permanece registado?**

**R:** Esta ação é propositada. O dispositivo não terão acesso aos recursos na cloud. 

Se pretender voltar a registar novamente, uma ação manual tem de ser a executar no dispositivo. 

Para limpar o estado de associação do Windows 10 e Windows Server 2016 que estão no local associados a um domínio do AD:

1.  Abra a linha de comandos como administrador.

2.  Tipo `dsregcmd.exe /debug /leave`

3.  Terminar sessão e inicie sessão acionar a tarefa agendada que regista o dispositivo com o Azure AD novamente. 

Para versões de SO do Windows de nível inferior que estão no local AD associados a um domínio:

1.  Abra a linha de comandos como administrador.
2.  Digite `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Digite `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---

**P: Por que vejo as entradas de dispositivo duplicado no portal do Azure?**

**R:**

-   Para Windows 10 e Windows Server 2016, se existem tentativas repetidas de anulação da associação ao e voltar a participar no mesmo dispositivo, poderá haver entradas duplicadas. 

-   Se tiver utilizado a adicionar conta profissional ou escolar, cada utilizador do windows que utiliza a conta escolar ou profissional adicionar irá criar um novo registo de dispositivo com o mesmo nome de dispositivo.

-   Para versões de SO do Windows de nível inferior que estão no local AD associados a um domínio com o registo automático criará um novo registo de dispositivo com o mesmo nome de dispositivo para cada utilizador de domínio que inicia sessão no dispositivo. 

-   Um computador associado a do Azure AD que tenha sido apagado, reinstalado e reagregado com o mesmo nome, será apresentado como outro registo com o mesmo nome de dispositivo.

---

**P: Por que pode um utilizador ainda aceder aos recursos de um dispositivo que desativou no portal do Azure?**

**R:** Pode demorar até uma hora para um revogar a ser aplicado.

>[!Note] 
>Para dispositivos inscritos, é recomendável limpar o dispositivo para garantir que os utilizadores não podem aceder os recursos. Para obter mais informações, consulte [inscrever dispositivos para gestão no Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 

---

## <a name="azure-ad-join-faq"></a>FAQ de associação do Azure AD

**P: Como, desassocie um dispositivo do Azure AD associado localmente no dispositivo?**

**R:** 
- Para dispositivos do Azure AD associado híbrida, certifique-se desativar o registro automático para que a tarefa agendada não volte a registar o dispositivo. Em seguida, abra a linha de comandos como administrador e tipo `dsregcmd.exe /debug /leave`. Em alternativa, este comando pode ser executado como um script em vários dispositivos de anulação da associação em massa.

- Para puro associado do AD Azure dispositivos, certifique-se de que tem um administrador local offline de conta ou criar uma, como não será possível iniciar sessão com qualquer credenciais de utilizador do Azure AD. Em seguida, aceda a **configurações** > **contas** > **acesso profissional ou escolar**. Selecione a sua conta e clique em **desligar**. Siga as instruções e forneça as credenciais de administrador local quando lhe for pedido. Reinicie o dispositivo para concluir o processo de unjoin.

---

**P: Podem os meus utilizadores iniciar sessão em dispositivos associados ao Azure AD que foi eliminados ou desativados no Azure AD? ** 
 **R:** Sim. Windows tem em cache capacidade de início de sessão para permitir que anteriormente registadas em que os utilizadores acedam a área de trabalho rapidamente, mesmo sem conectividade de rede. Quando um dispositivo é eliminado ou desativado no Azure AD, não é conhecido no dispositivo do Windows. Então, anteriormente registado nos utilizadores podem continuar a aceder a área de trabalho com o início de sessão em cache. No entanto, à medida que o dispositivo é eliminado ou desativado, os utilizadores não podem aceder a quaisquer recursos protegidos pelo acesso condicional com base no dispositivo. 

Os utilizadores que ainda não tiver sessão iniciada não é possível aceder ao dispositivo, porque não existe nenhum início de sessão em cache ativado para os mesmos. 

---

**P: Podem desativados ou eliminados utilizadores iniciar sessão em dispositivos associados ao Azure AD? ** 
 **R:** Sim, mas apenas por um período limitado. Quando um utilizador é eliminado ou desativado no Azure AD, ele não é imediatamente conhecido para o dispositivo do Windows. Então, anteriormente registado nos usuários podem acessar a área de trabalho com o início de sessão em cache. Depois do dispositivo estiver ciente de que o estado do utilizador (normalmente em menos de 4 horas), o Windows bloqueia os utilizadores acedam ao ambiente de trabalho. À medida que o utilizador é eliminado ou desativado no Azure AD, todos os seus tokens vão ser revogados, pelo que não é possível aceder a quaisquer recursos. 

Os utilizadores de desativada ou eliminados que ainda não iniciou sessão anteriormente não é possível aceder um dispositivo porque não existe nenhum início de sessão em cache ativado para os mesmos. 

---

**P: Os meus utilizadores não consegue pesquisar impressoras dos dispositivos associados do Azure AD. Como posso ativar a impressão a partir de dispositivos associados do Azure AD?**

**R:** Para implantar impressoras para dispositivos associados do Azure AD, consulte [impressão de cloud híbrida](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). Precisará de um servidor do Windows no local para implementar a impressão de cloud híbrida. Atualmente, o serviço de impressão baseado na nuvem não está disponível. 

---

**P: Como posso ligar a um remoto do Azure AD associado ao dispositivo? ** 
 **R:** Veja o artigo https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc para obter detalhes.

---

**P: Por que os meus utilizadores veem "não pode aceder aí a partir daqui"?**

**R:** Se tiver configurado determinadas regras de acesso condicional para requerer um Estado de dispositivo específico e o dispositivo não cumpre os critérios, os utilizadores são bloqueados e ver esta mensagem. Avalie as regras de política de acesso condicional e certifique-se de que o dispositivo é capaz de satisfazer os critérios para evitar esta mensagem.

---

**P: Por que alguns dos meus fazer os utilizadores não get MFA solicita no Azure AD, a dispositivos associados ao?**

**R:** Se o usuário ingressa ou regista um dispositivo com o Azure AD com o multi-factor Authentication, o próprio dispositivo irá tornar-se um segundo fator fidedigno para esse usuário específico. Posteriormente, sempre que o mesmo utilizador inicia sessão no dispositivo e acede a uma aplicação, o Azure AD considera o dispositivo como um segundo fator e permite que o utilizador de forma totalmente integrada aceder às suas aplicações sem pedidos adicionais do MFA. Este comportamento não é aplicável a qualquer outro utilizador iniciar sessão nesse dispositivo, para que todos os outros utilizadores aceder a que o dispositivo ainda poderia ser pedidos com um desafio MFA antes de aceder a aplicações que necessitam de MFA.

---

**P: Por que motivo recebo uma mensagem de "nome de utilizador ou palavra-passe está incorreto" para um dispositivo que tem apenas associados ao Azure AD?**

**R:** Motivos comuns para este cenário são:

- As suas credenciais de utilizador já não são válidos.

- O computador não consegue comunicar com o Azure Active Directory. Verificar a existência de quaisquer problemas de conectividade de rede.

- Inícios de sessão federados requerem o seu servidor de Federação para suportar pontos de extremidade do WS-Trust habilitados e acessíveis. 

- Ativou a Pass-through Authentication e o utilizador tem uma palavra-passe temporária que precisa ser alterada no início de sessão.

---

**P: Por que razão vejo o "UPS... Ocorreu um erro!" caixa de diálogo quando tento fazer do Azure AD associar o meu PC?**

**R:** Este é um resultado de configuração de inscrição do Azure Active Directory com o Intune. Certifique-se de que o usuário tentar fazer a associação do Azure AD tem correta licença do Intune atribuída. Para obter mais informações, consulte [configurar a gestão de dispositivos do Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**P: Por que minha tentativa para o Azure AD associar uma falha de PC embora eu não recebeu qualquer informação de erro?**

**R:** Uma causa provável é que o utilizador inicia sessão no dispositivo com a conta interna de administrador local. Crie uma conta local diferente antes de utilizar a associação do Azure Active Directory para concluir a configuração. 

---

## <a name="hybrid-azure-ad-join-faq"></a>FAQ de associação do Azure AD híbrido

**P: Onde posso encontrar a resolução de problemas de informações para diagnosticar falhas de associação do Azure AD híbrido?**

**R:** Para informações de resolução de problemas, consulte:

- [Resolução de problemas do registo automático de domínio associados a computadores ao Azure AD – Windows 10 e Windows Server 2016](troubleshoot-hybrid-join-windows-current.md)

- [Resolução de problemas do registo automático de domínio associados a computadores ao Azure AD para clientes de nível inferior do Windows](troubleshoot-hybrid-join-windows-legacy.md)
 

---

## <a name="azure-ad-register-faq"></a>FAQ de registo do Azure AD

**P: Pode registar dispositivos BYOD de Android ou iOS?**

**R:** Sim, mas apenas com o serviço de registo de dispositivos do Azure e para clientes híbridos. Não é suportada com o serviço de registo de dispositivos no local no AD FS.

**P: Como posso registrar um dispositivo macOS?**

**R:** Para registar o dispositivo macOS:

1.  [Criar uma política de conformidade](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [Definir uma política de acesso condicional para dispositivos macOS](../active-directory-conditional-access-azure-portal.md) 

**Observações:**

- Os utilizadores que estão incluídos na sua política de acesso condicional tem um [uma versão suportada do Office para macOS](../conditional-access/technical-reference.md#client-apps-condition) para aceder aos recursos. 

- Durante a primeira tentativa de acesso, os utilizadores são-lhe pedidos para inscrever o dispositivo com o portal da empresa.

---
