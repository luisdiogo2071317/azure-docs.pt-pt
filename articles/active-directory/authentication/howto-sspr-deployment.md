---
title: Guia de implementação de reposição personalizada de palavras-passe - Azure Active Directory
description: Sugestões para a implementação bem-sucedida da reposição de palavras-passe self-service do Azure AD
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: get-started-article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 3e14c51d644a29985e759da7c8a29927680d3891
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/14/2018
ms.locfileid: "39048956"
---
# <a name="how-to-successfully-roll-out-self-service-password-reset"></a>Como implementar com êxito a reposição personalizada de palavras-passe

Para garantir uma suave implementação da funcionalidade de reposição personalizada de palavra-passe (SSPR) do Azure Active Directory (Azure AD), a maioria dos clientes realizam os passos seguintes:

> [!VIDEO https://www.youtube.com/embed/OZn5btP6ZXw]

1. [Ative a reposição de palavras-passe no seu diretório](quickstart-sspr.md).
2. [Configurar as permissões do Active Directory no local para repetição de escrita de palavras-passe](howto-sspr-writeback.md#active-directory-permissions).
3. [Configurar a repetição de escrita de palavras-passe](howto-sspr-writeback.md#configure-password-writeback) para gravar palavras-passe do Azure AD no seu diretório local.
4. [Atribuir e verificar as licenças necessárias](concept-sspr-licensing.md).
5. Determinar se quer fazer uma implementação gradual. Se quiser implementar a SSPR gradualmente, pode limitar o acesso a um grupo de utilizadores, para que possa criar um programa-piloto com um grupo específico. Para implementar num grupo específico, defina o comutador **Reposição Personalizada de Palavras-passe Ativada** como **Selecionada** e selecione o grupo de segurança que vai poder utilizar a reposição de palavras-passe.  O aninhamento de grupos de segurança é suportado aqui.
6. Preencha os [dados de autenticação](howto-sspr-authenticationdata.md) necessários para os seus utilizadores se registarem, como o telefone do escritório, o número de telemóvel e o endereço de e-mail alternativo.
7. [Personalize a experiência de início de sessão do Azure AD para incluir a imagem corporativa da sua empresa](concept-sspr-customization.md).
8. Ensine os utilizadores a utilizar a SSPR. Envie-lhes instruções para aprenderem a registar-se e a repor as palavras-passe.
9. Determine quando pretende impor o registo. Pode optar por impor o registo em qualquer momento. Pode também exigir aos utilizadores que confirmem novamente as respetivas informações de autenticação deles após um determinado período de tempo.
10. Utilize a capacidade de relatórios. Ao longo do tempo, pode rever o registo dos utilizadores e a utilização com a [capacidade de relatórios que o Azure AD proporciona](howto-sspr-reporting.md).
11. Ative a reposição de palavras-passe. Quando estiver pronto, ative a reposição de palavras-passe para todos os utilizadores mediante a definição do comutador **Reposição Personalizada de Palavras-Passe Ativada** como **Todos**. 

   > [!NOTE]
   > Alterar esta opção de um grupo selecionado para todos os utilizadores, não invalida os dados de autenticação existentes que um utilizador registou como parte de um grupo de teste. Os utilizadores que estão configurados e têm dados de autenticação registados continuam a funcionar.

12. [Permita que os utilizadores do Windows 10 reponham as palavras-passe no ecrã de início de sessão](tutorial-sspr-windows.md).

   > [!IMPORTANT]
   > Teste a SSPR com um utilizador e não com um administrador, pois a Microsoft aplica requisitos de autenticação fortes às contas de administrador do Azure. Para obter mais informações sobre a política de palavras-passe de administrador, veja o nosso artigo sobre a [política de palavras-passe](concept-sspr-policy.md#administrator-password-policy-differences).

## <a name="email-based-rollout"></a>Implementação baseado em e-mails

Muitos clientes consideram que a forma mais fácil de pôr os utilizadores a utilizar a SSPR é criar uma campanha de e-mail que inclua instruções de utilização simples. [Criámos três e-mails simples que pode utilizar como modelos para ajudar na sua implementação](https://www.microsoft.com/download/details.aspx?id=56768):

* **Brevemente**: modelo de e-mail que pode utilizar nas semanas ou dias anteriores à implementação, para dizer aos utilizadores que precisam de fazer uma determinada coisa.
* **Disponível agora**: modelo de e-mail que pode utilizar no dia do lançamento do programa, de modo a levar os utilizadores a registarem-se e confirmarem os respetivos dados de autenticação. Se os utilizadores se registarem nesse momento, a SSPR estará à disposição deles quando precisarem.
* **Lembrete de inscrição**: modelo de e-mail para ser utilizado uns dias ou semanas após a implementação, para relembrar aos utilizadores para se registarem e confirmarem os respetivos dados de autenticação.

![E-mail][Email]

## <a name="create-your-own-password-portal"></a>Criar o seu próprio portal de palavras-passe

Muitos clientes optam por alojar uma página Web e criar uma entrada DNS de raiz, como https://passwords.contoso.com. Preenchem esta página com ligações para as informações seguintes:

* [Portal da reposição de palavras-passe do Azure AD - https://aka.ms/sspr](https://aka.ms/sspr)
* [Portal de registo da reposição de palavras-passe do Azure AD - https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)
* [Portal de alteração de palavras-passe do Azure AD - https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* Outras informações específicas da organização

Pode incluir, em qualquer comunicação ou panfleto por e-mail que distribuir, um URL corporativo e memorável, ao qual os utilizadores podem aceder quando precisam de utilizar os serviços. Para seu benefício, criámos uma [página de reposição de palavras-passe de exemplo](https://github.com/ajamess/password-reset-page) que pode utilizar e personalizar consoante as necessidades da sua organização.

## <a name="step-by-step-deployment-plan"></a>Plano de implementação passo a passo

O grupo de produtos do Azure Active Directory criou um [plano de implementação passo a passo](https://aka.ms/SSPRDeploymentPlan) que as organizações podem utilizar em paralelo com a documentação neste site para criar um caso comercial e um plano de implementação de reposição personalizada de palavra-passe.

## <a name="use-enforced-registration"></a>Utilizar o registo forçado

Se quiser que os utilizadores se registem na reposição de palavras-passe, pode obrigá-los a registarem-se quando iniciam sessão através do Azure AD. Pode ativar esta opção no painel **Reposição de palavras-passe** do seu diretório ao ativar a opção **Exigir que os Utilizadores se Registem Quando Iniciam Sessão**, no separador **Registo**.

Os administradores podem exigir que os utilizadores se voltem a registar após um determinado período de tempo. Podem definir a opção **Número de dias antes de ser pedido aos utilizadores que voltem a confirmar as informações de autenticação** entre 0 e 730 dias.

Depois de ativar esta opção, quando iniciam sessão, os utilizadores verão uma mensagem que os informa de que o administrador lhes exigiu que verifiquem as informações de autenticação.

## <a name="populate-authentication-data"></a>Preencher os dados de autenticação

Deve [preencher os dados de autenticação dos seus utilizadores](howto-sspr-authenticationdata.md). Desta forma, não precisam de se registar na reposição de palavras-passe antes de poderem utilizar a SSPR. Desde que os utilizadores tenham fornecido os dados de autenticação que cumpram a política de reposição de palavras-passe, podem repor as palavras-passe deles.

## <a name="disable-self-service-password-reset"></a>Desativar a reposição de palavras-passe self-service

É fácil desativar a reposição de palavras-passe self-service. Abra o seu inquilino do Azure AD e aceda a **Reposição de Palavras-passe** > **Propriedades** e selecione **Ninguém**, em **Reposição de Palavras-passe Self-Service Ativada**.

## <a name="next-steps"></a>Passos seguintes

* [Repor ou alterar a palavra-passe](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registar-se na reposição personalizada de palavra-passe](../user-help/active-directory-passwords-reset-register.md)
* [Tem alguma pergunta sobre licenciamento?](concept-sspr-licensing.md)
* [Que dados são utilizados pela SSPR e que dados devem ser preenchidos por si para os seus utilizadores?](howto-sspr-authenticationdata.md)
* [Quais são as opções de política da SSPR?](concept-sspr-policy.md)
* [O que é a repetição de escrita de palavras-passe e por que me deve interessar?](howto-sspr-writeback.md)
* [Como posso comunicar a atividade da SSPR?](howto-sspr-reporting.md)
* [Quais são todas as opções na SSPR e o que significam?](concept-sspr-howitworks.md)
* [Creio que algo está a funcionar incorretamente. Como posso resolver problemas da SSPR?](active-directory-passwords-troubleshoot.md)
* [Tenho uma pergunta que ainda não foi abordada](active-directory-passwords-faq.md)

[Email]: ./media/howto-sspr-deployment/sspr-emailtemplates.png "Personalizar estes modelos de e-mail para satisfazer as suas necessidades organizacionais"
