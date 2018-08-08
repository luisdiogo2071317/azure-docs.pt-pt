---
title: Diretrizes para aplicativos de imagem corporativa | Documentos da Microsoft
description: Um guia abrangente para recursos dirigidos aos programadores do Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: celested
ms.reviewer: arielgo
ms.custom: aaddev, signin_art
ms.openlocfilehash: 78a0b9bf7f49cc41c8d78287bcbe54c5c88f0809
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/07/2018
ms.locfileid: "39597910"
---
# <a name="branding-guidelines-for-applications"></a>Diretrizes de imagem corporativa para aplicações

Este artigo discute as diretrizes de imagem corporativa que deve utilizar ao desenvolvimento de aplicativos com o Azure Active Directory (Azure AD). Essas diretrizes ajuda direcione seus clientes quando eles querem utilizar a respetiva conta escolar ou profissional, gerida no Azure AD, ou a respetiva conta para inscrição e início de sessão na sua aplicação.

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Contas pessoais versus trabalho contas escolares ou profissionais da Microsoft

Microsoft gerencia os dois tipos de contas de utilizador:

* **Contas pessoais** (anteriormente conhecido como Windows Live ID). Estas contas representam a relação entre *individuais* utilizadores e a Microsoft e são utilizados para aceder a dispositivos de consumo e serviços da Microsoft. Estas contas foram concebidas para utilização pessoal.
* **Contas escolares ou profissionais.** Estas contas são geridas pela Microsoft em nome de organizações que utilizam o Azure Active Directory. Essas contas são usadas para iniciar sessão no Office 365 e outros serviços empresariais da Microsoft.

Microsoft contas profissionais ou escolares, normalmente, são atribuídas aos utilizadores finais (funcionários, estudantes, colaboradores federais) por suas organizações (da empresa, escola, agência do Governo). Estas contas dominado diretamente na cloud (na plataforma do Azure AD) ou sincronizado com o Azure AD a partir de um diretório no local, como o Windows Server Active Directory. A Microsoft é o *conservador* do trabalho ou escola contas, mas as contas são propriedade e controlado pela organização.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Que faça referência a contas do Azure AD na sua aplicação

Microsoft não expõe aos utilizadores finais do Azure ou os nomes de marca do Active Directory e nenhum deles, deve.

* Assim que os utilizadores iniciar sessão, utilize o nome da organização e o logótipo tanto quanto possível. Isso é melhor do que usar genéricos termos como "sua organização."
* Quando os utilizadores não tiver iniciados sessão, consulte as respetivas contas como "trabalho contas profissionais ou escolares" e utilize o logótipo da Microsoft para transmitir a Microsoft gerir a estas contas. Não utilize termos como "conta de empresa", "conta de empresa" ou "conta empresarial," o que criar confusão de utilizador.

## <a name="user-account-pictogram"></a>Pictograma de conta de utilizador

Numa versão anterior destas diretrizes, recomendamos utilizar pictograma um "distintivo azul". Com base nos comentários de utilizador e o desenvolvedor, agora Recomendamos a utilização do logótipo da Microsoft em vez disso. O logótipo da Microsoft ajudará os usuários a compreender o que eles podem reutilizar a conta que utilizam com o Office 365 ou outro Microsoft serviços empresariais para iniciar sessão na sua aplicação.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Inscrever-se e iniciar sessão no Azure AD

Seu aplicativo pode apresentar os caminhos separados para inscrição e início de sessão e as secções seguintes fornecem orientações visual para ambos os cenários.

**Se a aplicação suporta a inscrição (por exemplo, livre para o modelo de avaliação ou freemium) de utilizador final**: pode mostrar um **início de sessão** botão que permite aos utilizadores aceder à sua aplicação com a respetiva conta profissional ou a conta pessoal. Azure AD mostra uma solicitação de consentimento pela primeira vez que aceder à sua aplicação.

**Se a sua aplicação necessita de permissões que apenas os administradores podem dar consentimento a ou se a sua aplicação requer licenciamento organizacional**: separe a aquisição de administração de início de sessão do utilizador. O **botão "obter esta aplicação"** redirecionará os administradores de iniciar sessão, em seguida, peça-lhe para conceder autorização em nome dos utilizadores na sua organização, que tem o benefício adicional de supressão dos pedidos de consentimento do utilizador final para a sua aplicação.

## <a name="visual-guidance-for-app-acquisition"></a>Documentação de orientação Visual para aquisição de aplicações

A ligação "obter a aplicação" tem de redirecionar o utilizador para a concessão de acesso do Azure AD (autorizar) para a página, para permitir que o administrador da organização autorizar a sua aplicação tenha acesso a dados da sua organização, o que são hospedados pela Microsoft. Obter detalhes sobre como pedir acesso são discutidas a [integrar aplicações com o Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md) artigo.

Depois dos administradores de consentimento à sua aplicação, pode escolher para o adicionar à experiência de iniciador de aplicações dos utilizadores do Office 365 (acessível a partir do waffle e do [ https://portal.office.com/myapps ](https://portal.office.com/myapps)). Se quiser anunciar esta capacidade, pode usar termos como "Adicionar esta aplicação para a sua organização" e mostrar um botão, como o exemplo seguinte:

![Tipos de aplicativos e cenários](./media/howto-add-branding-in-azure-ad-apps/add-to-my-org.png)

No entanto, recomendamos que escreve o texto explicativo em vez de depender de botões. Por exemplo:

> *Se já utiliza o Office 365 ou outro serviço de negócios da Microsoft, pode conceder acesso de < your_app_name > aos dados da sua organização. Isso permitirá que os utilizadores acedam aos < your_app_name > com as suas contas de trabalho existente.*

Para transferir o logótipo da Microsoft oficial para utilização na sua aplicação, com o botão direito aquele que pretende utilizar e, em seguida, guarde-o para o seu computador.

| Recurso                                | Formato PNG | Formato SVG |
| ------------------------------------ | ---------- | ---------- |
| Logótipo da Microsoft  | ![Logótipo da Microsoft PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_MSSymbol_19.png) | ![Logótipo da Microsoft SVG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_MSSymbol_19.svg) |

## <a name="visual-guidance-for-sign-in"></a>Guia do Visual para início de sessão

Ele deverá exibir um botão de início de sessão que redireciona os usuários para o início de sessão no ponto final que corresponde ao protocolo que usar para integrar com o Azure AD. A secção seguinte fornece detalhes sobre o que esse botão deve ser semelhante.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictograma e "Sign in with Microsoft"

É a associação do logótipo da Microsoft e os termos de "Sign in with Microsoft" que representam exclusivamente do Azure AD, entre outros fornecedores de identidade pode suportar a sua aplicação. Se não tiver espaço suficiente para "Sign in with Microsoft", há problema para Encurte-a para "Iniciar sessão." Pode usar um esquema de cores simples ou escuro para os botões.

O diagrama seguinte mostra que o recomendadas pela Microsoft redlines ao usar os ativos com a sua aplicação. O redlines aplicam-se a "Sign in with Microsoft" ou a versão de "Iniciar sessão" mais curta.

![Redlines Sign in with Microsoft](./media/howto-add-branding-in-azure-ad-apps/Sign-in-with-Microsoft-redlines.png)

Para transferir as imagens oficiais para utilização na sua aplicação, com o botão direito aquele que pretende utilizar e, em seguida, guarde-o para o seu computador.

| Recurso                                | Formato PNG | Formato SVG |
| ------------------------------------ | ---------- | ---------- |
| Inicie sessão com a Microsoft (tema escuro)  | ![Inicie sessão no tema escuro do botão PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark.png) | ![Inicie sessão com o tema no escuro Microsoft botão SVG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark.svg) |
| Inicie sessão com a Microsoft (tema claro) | ![Inicie sessão no tema claro do botão PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light.png) | ![Inicie sessão com o tema claro de botão Microsoft SVG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light.svg) |
| Iniciar sessão (tema escuro)                 | ![Iniciar sessão, em Resumo, tema escuro do botão PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark_short.png) | ![Iniciar sessão, em Resumo, tema escuro do botão SVG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_dark_short.svg) |
| Iniciar sessão (tema claro)                | ![Iniciar sessão, em Resumo, tema claro do botão PNG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light_short.png) | ![Iniciar sessão, em Resumo, tema claro do botão SVG](./media/howto-add-branding-in-azure-ad-apps/MS-SymbolLockup_SignIn_light_short.svg) |


## <a name="branding-dos-and-donts"></a>Imagem corporativa afazeres e que não deve fazer

**FAZER** utilizar "conta escolar ou profissional" em combinação com o botão "Sign in with Microsoft" para fornecer uma explicação adicional para ajudar os utilizadores finais reconhecer se eles podem utilizá-lo. **Não** utilizar outros termos como "conta da empresa", "conta de empresa" ou "conta empresarial".

**Não** utilizar "ID do Office 365" ou "ID do Azure." O Office 365 também é o nome de um consumidor de oferta da Microsoft, que não utilize o Azure AD para autenticação.

**Não** alterar o logótipo da Microsoft.

**Não** expor aos utilizadores finais as marcas do Azure ou do Active Directory. Há problema entretanto para utilizar estes termos com programadores, profissionais de TI e administradores.

## <a name="navigation-dos-and-donts"></a>Navegação afazeres e que não deve fazer

**FAZER** proporcionam uma forma para os utilizadores terminar sessão e mudar para outra conta de utilizador. Embora a maioria das pessoas tem uma única conta pessoal do Microsoft/Facebook/Google/Twitter, as pessoas costumam ser associadas a mais do que uma organização. Suporte para vários utilizadores com sessão iniciada está disponível em breve.
