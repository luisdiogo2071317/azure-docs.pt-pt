---
title: "Registar para a reposição de palavra-passe self-service - Azure Active Directory"
description: "Registar dados de autenticação de palavra-passe self-service do Azure AD repor"
services: active-directory
keywords: 
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2018
ms.author: barlan
ms.custom: end-user;seohack1
ms.openlocfilehash: ffdffc0c471970e5a7e7a0bb291658cefe99ed71
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="register-for-self-service-password-reset"></a>Registar-se na reposição personalizada de palavra-passe

> [!IMPORTANT]
> Está aqui porque não é possível iniciar sessão? Se Sim, consulte [repor a palavra-passe de conta escolar ou profissional](active-directory-passwords-update-your-own-password.md).

Como um utilizador final, pode repor a palavra-passe ou desbloquear a conta por si, se utilizar a reposição de palavra-passe self-service do Azure Active Directory (Azure AD) (SSPR). Antes de poder utilizar esta funcionalidade, terá de registar os métodos de autenticação ou confirme os métodos de autenticação predefinido que o administrador tem preenchido.

## <a name="register-or-confirm-authentication-data-with-sspr"></a>Registar ou confirmar dados de autenticação com SSPR

1. Abra o browser no seu dispositivo e vá para o [página de registo de reposição de palavra-passe](http://aka.ms/ssprsetup).
2. Introduza o nome de utilizador e a palavra-passe que o administrador fornecido.
3. Dependendo de como a sua equipa de TI configurou coisas, um ou mais das seguintes opções estão disponíveis para configurar e verificar. Se o administrador tem permissão para utilizar as informações, pode de preencher algumas das informações por si.
    * **Telefone do escritório**: apenas o administrador pode definir esta opção.
    * **Telefone de autenticação**: defina esta opção para outro número de telefone que tenha acesso. Um exemplo é um telemóvel que pode receber um texto ou uma chamada.
    * **E-Mail de autenticação**: defina esta opção para um endereço de correio eletrónico alternativo que pode aceder sem utilizar a palavra-passe que pretende repor.
    * **Perguntas de segurança**: O administrador tiver aprovado esta lista de perguntas responder. Não é possível utilizar a mesma pergunta ou responder a mais do que uma vez.
4. Forneça e verifique as informações que o administrador necessita. Se estiver disponível mais do que uma opção, sugerimos que registar vários métodos. Isto dá-lhe flexibilidade quando um dos métodos não está disponível. Um exemplo é quando estiver a estiverem em deslocação e for possível aceder ao seu telefone do escritório.

    ![Registar métodos de autenticação e selecione concluir][Register]

5. Selecione **concluir**. Agora, pode utilizar SSPR quando for necessário no futuro.

Se introduzir dados de **telefone de autenticação** ou **correio eletrónico de autenticação**, não é visível no diretório global. As únicas pessoas que podem ver estes dados são os administradores. Só pode ver as respostas às perguntas de segurança.

Os administradores podem implicar a confirmar os métodos de autenticação após um período de tempo para se certificar de que tem dos métodos adequados registados.

## <a name="common-problems-and-their-solutions"></a>Problemas comuns e as respetivas soluções

 Eis alguns casos de erro comuns e as respetivas soluções:

| Caso de erro| O erro vir?| Solução |
| --- | --- | --- |
| Posso obter uma página ". Contacte o administrador" depois de introduzir o meu ID de utilizador | Contacte o administrador. <br> <br> Detetámos que a palavra-passe da conta de utilizador não é gerida pela Microsoft. Como resultado, não conseguimos automaticamente repor a palavra-passe. <br> <br> Contacte a equipa de TI para obter ajuda adicional. | Está a ver esta mensagem porque a sua equipa de TI gere a palavra-passe no seu ambiente no local e não permite-lhe repor a palavra-passe do **não é possível aceder à sua conta** ligação. <br> <br> Para repor a palavra-passe, contacte a equipa de TI diretamente para obter ajuda. Informe de que pretende repor a palavra-passe, pelo que podem ativar esta funcionalidade para si.|
| Posso obter um erro "a conta não está ativada para a reposição de palavra-passe" depois de introduzir o meu ID de utilizador | A conta não está ativada para a reposição de palavra-passe. <br> <br> Lamentamos, mas a sua equipa de TI não tenha configurado a sua conta para utilização com este serviço. <br> <br> Se quiser, podemos contactá um administrador na sua organização para repor a palavra-passe por si. | Está a ver esta mensagem porque a sua equipa de TI não activou a reposição da sua organização de palavra-passe a **não é possível aceder à sua conta** ligar ou ainda não está licenciado para utilizar a funcionalidade. <br> <br> Para repor a palavra-passe, selecione o **contacte um administrador** ligação. Mensagens de correio eletrónico serão enviadas para a sua empresa equipa de TI. Mensagem de correio eletrónico permite-lhes saber que pretende repor a palavra-passe, pelo que podem ativar esta funcionalidade para si. |
| Posso obter um erro de "pode não verificar a sua conta" depois de introduzir o meu ID de utilizador | Não foi possível verificar a sua conta. <br> <br> Se quiser, podemos contactá um administrador na sua organização para repor a palavra-passe por si. | Está a ver esta mensagem porque está a ser ativada para a reposição de palavra-passe, mas ainda não está registado para utilizar o serviço. Para registar para a reposição de palavra-passe, vá para o [página de registo de reposição de palavra-passe](http://aka.ms/ssprsetup) depois de ter recuperou o acesso à sua conta. <br> <br> Para repor a palavra-passe, selecione o **contacte um administrador** ligação para enviar uma mensagem de e-mail da sua empresa equipa de TI. |

## <a name="next-steps"></a>Passos Seguintes

* [Alterar a palavra-passe através da utilização de reposição de palavra-passe self-service](active-directory-passwords-update-your-own-password.md)
* [Página de registo de reposição de palavra-passe](http://aka.ms/ssprsetup)
* [Portal de reposição de palavra-passe](https://passwordreset.microsoftonline.com/)
* [Quando não é possível iniciar sessão na sua conta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant)

[Register]: ./media/active-directory-passwords-reset-register/register-2-methods.png "Página de registo de reposição de palavra-passe que mostra os métodos de registado e no botão Concluir"

