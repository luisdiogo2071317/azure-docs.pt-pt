---
title: Requisitos de dados do Azure AD SSPR | Microsoft Docs
description: Requisitos de dados para a palavra-passe self-service do Azure AD como satisfazê-los e de reposição
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 01/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 5409bf198d0e3f6537619ef4698d9f2e31bd27c5
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34257593"
---
# <a name="deploy-password-reset-without-requiring-end-user-registration"></a>Implementar sem necessidade de registo de utilizador final de reposição de palavra-passe

Para implementar a reposição de palavra-passe self-service (SSPR) do Azure Active Directory (Azure AD), dados de autenticação tem de estar presente. Algumas organizações têm os seus utilizadores, introduza os seus dados de autenticação próprios. Mas, muitas organizações preferem sincronizar com dados que já existe no Active Directory. Os dados sincronizados são disponibilizados para o Azure AD e SSPR sem necessidade de interação do utilizador se a:
   * Formate corretamente os dados no seu diretório no local.
   * Configurar [do Azure AD Connect, utilizando as definições rápidas](./../connect/active-directory-aadconnect-get-started-express.md).

Para funcionar corretamente, números de telefone tem de estar no formato *+ indicativo do país PhoneNumber*, por exemplo, + 1 4255551234.

> [!NOTE]
> Tem de haver um espaço entre o indicativo do país e o número de telefone.
>
> Reposição de palavra-passe não suporta extensões telefónicas. Mesmo no formato + 1 4255551234 X 12345, as extensões são removidas antes da chamada é efetuada.

## <a name="fields-populated"></a>Campos preenchidos

Se utilizar as predefinições no Azure AD Connect, são efetuados os seguintes mapeamentos de:

| Active Directory no local | Azure AD |
| --- | --- |
| telephoneNumber | Telefone do emprego |
| Mobile | Número de telemóvel |

Quando o utilizador verifica o número de telemóvel, o campo de telefone em informações de contacto de autenticação no Azure AD também será preenchido com esse número.

## <a name="authentication-contact-info"></a>Informação de contacto de autenticação

Um Administrador Global pode definir manualmente as informações de contacto de autenticação para um utilizador, tal como apresentado na seguinte captura de ecrã.

![Contact][Contact]

Se o campo de telefone é preenchido e telemóvel está ativado na política de SSPR, o utilizador irá ver número na página de registo de reposição de palavra-passe e a palavra-passe durante o fluxo de trabalho de reposição. 

O campo de telefone alternativo não é utilizado para a reposição de palavra-passe.

Se o campo de correio eletrónico é preenchido e correio eletrónico está ativado na política de SSPR, o utilizador irá ver e-mail na página de registo de reposição de palavra-passe e a palavra-passe durante o fluxo de trabalho de reposição.

Se o campo de correio eletrónico alternativo é preenchido e correio eletrónico está ativado na política de SSPR, o utilizador será **não** ver que a página de registo de reposição de e-mail na palavra-passe, mas irão vê-lo durante a palavra-passe de reposição de fluxo de trabalho. 


## <a name="security-questions-and-answers"></a>Perguntas de segurança e respostas

As perguntas de segurança e respostas são armazenadas em segurança no seu inquilino do Azure AD e só estão acessíveis aos utilizadores através de [portal de registo SSPR](https://aka.ms/ssprsetup). Os administradores não é possível ver ou modificar o conteúdo da outra utilizadores perguntas e respostas.

### <a name="what-happens-when-a-user-registers"></a>O que acontece quando um utilizador se regista

Quando um utilizador se regista, a página de registo define os seguintes campos:

* **Telefone de autenticação**
* **E-Mail de autenticação**
* **Perguntas de segurança e respostas**

Se tiver fornecido um valor para **telemóvel** ou **e-mail alternativo**, os utilizadores imediatamente podem utilizar esses valores para repor as palavras-passe, mesmo que o se ainda não está registado para o serviço. Além disso, os utilizadores veem esses valores, quando estes registar pela primeira vez e podem modificá-los se pretenderem. Depois de se registar com êxito, estes valores serão ser persistente no **telefone de autenticação** e **correio eletrónico de autenticação** campos, respetivamente.

## <a name="set-and-read-the-authentication-data-through-powershell"></a>Definir e ler os dados de autenticação através do PowerShell

Os campos seguintes podem ser definidos através do PowerShell:

* **Correio eletrónico alternativo**
* **Telemóvel**
* **Telefone do escritório**: só pode ser definida se não estiver a sincronizar com um diretório no local

### <a name="use-powershell-version-1"></a>Utilizar o PowerShell versão 1

Para começar, terá de [transferir e instalar o módulo Azure AD PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule). Depois de ter instalado, pode utilizar os passos que se seguem para configurar cada campo.

#### <a name="set-the-authentication-data-with-powershell-version-1"></a>Definir os dados de autenticação com o PowerShell versão 1

```
Connect-MsolService

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"

Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com") -MobilePhone "+1 1234567890" -PhoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-1"></a>Ler os dados de autenticação com o PowerShell versão 1

```
Connect-MsolService

Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber

Get-MsolUser | select DisplayName,UserPrincipalName,AlternateEmailAddresses,MobilePhone,PhoneNumber | Format-Table
```

#### <a name="read-the-authentication-phone-and-authentication-email-options"></a>Ler as opções de telefone de autenticação e o E-Mail de autenticação

Para ler o **telefone de autenticação** e **correio eletrónico de autenticação** quando utilizar o PowerShell versão 1, utilize os seguintes comandos:

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

### <a name="use-powershell-version-2"></a>Utilizar o PowerShell versão 2

Para começar, terá de [transferir e instalar o módulo do PowerShell versão 2 do Azure AD](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0). Depois de ter instalado, pode utilizar os passos que se seguem para configurar cada campo.

Para instalar rapidamente das versões recentes do PowerShell que suportam o módulo de instalação, execute os seguintes comandos. (A primeira linha verifica se o módulo já está instalado.)

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="set-the-authentication-data-with-powershell-version-2"></a>Definir os dados de autenticação com o PowerShell versão 2

```
Connect-AzureAD

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 2345678901"
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"

Set-AzureADUser -ObjectId user@domain.com -OtherMails @("emails@domain.com") -Mobile "+1 1234567890" -TelephoneNumber "+1 1234567890"
```

#### <a name="read-the-authentication-data-with-powershell-version-2"></a>Ler os dados de autenticação com o PowerShell versão 2

```
Connect-AzureAD

Get-AzureADUser -ObjectID user@domain.com | select otherMails
Get-AzureADUser -ObjectID user@domain.com | select Mobile
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber

Get-AzureADUser | select DisplayName,UserPrincipalName,otherMails,Mobile,TelephoneNumber | Format-Table
```

## <a name="next-steps"></a>Passos Seguintes

* [Como posso concluir uma implementação com êxito da SSPR?](howto-sspr-deployment.md)
* [Repor ou alterar a palavra-passe](../active-directory-passwords-update-your-own-password.md)
* [Registar-se na reposição personalizada de palavra-passe](../active-directory-passwords-reset-register.md)
* [Tem alguma pergunta sobre licenciamento?](concept-sspr-licensing.md)
* [Que métodos de autenticação estão disponíveis para os utilizadores?](concept-sspr-howitworks.md#authentication-methods)
* [Quais são as opções de política da SSPR?](concept-sspr-policy.md)
* [O que é a repetição de escrita de palavras-passe e por que me deve interessar?](howto-sspr-writeback.md)
* [Como posso comunicar a atividade da SSPR?](howto-sspr-reporting.md)
* [Quais são todas as opções na SSPR e o que significam?](concept-sspr-howitworks.md)
* [Creio que algo está a funcionar incorretamente. Como posso resolver problemas da SSPR?](active-directory-passwords-troubleshoot.md)
* [Tenho uma pergunta que ainda não foi abordada](active-directory-passwords-faq.md)

[Contact]: ./media/howto-sspr-authenticationdata/user-authentication-contact-info.png "Os administradores globais podem modificar informações de contacto de autenticação de um utilizador"
