---
title: Diretrizes de imagem corporativa para aplicações | Microsoft Docs
description: Um guia completo de recursos orientados para programadores do Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 72f4e464-1352-4a49-a18f-c37f58e7d5c4
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: arielgo
ms.custom: aaddev, signin_art
ms.openlocfilehash: 6edcd5b3df7c3c58c6568434f82fbc52cb9e28df
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47068735"
---
# <a name="branding-guidelines-for-applications"></a>Diretrizes de imagem corporativa para aplicações

Ao desenvolver aplicações com o Azure Active Directory (Azure AD), irá precisar de orientar os seus clientes quando quiserem utilizar a respetiva conta escolar ou profissional (gerida no Azure AD), ou a respetiva conta pessoal para inscrição e início de sessão na sua aplicação.

Neste artigo, vai:

- Aprender sobre os dois tipos de contas de utilizador geridas pela Microsoft e como fazer referência a contas do Azure AD na sua aplicação
- Descubra o que precisa de fazer para adicionar o logótipo da Microsoft para utilizar na sua aplicação
- Transfira as imagens **Iniciar sessão** ou **Iniciar sessão com a Microsoft** oficiais para utilizar na sua aplicação
- Saiba mais sobre os prós e contras da imagem corporativa e da navegação

## <a name="personal-accounts-vs-work-or-school-accounts-from-microsoft"></a>Contas pessoais versus contas escolares ou profissionais da Microsoft

A Microsoft gere dois tipos de contas de utilizador:

- **Contas pessoais** (anteriormente conhecidas como Windows Live ID). Estas contas representam a relação entre utilizadores *individuais* e a Microsoft e servem para aceder a dispositivos de consumo e a serviços da Microsoft. Estas contas foram concebidas para utilização pessoal.
- **Contas escolares ou profissionais.** Estas contas são geridas pela Microsoft em nome de organizações que utilizam o Azure Active Directory. Estas contas servem para iniciar sessão no Office 365 e noutros serviços comerciais da Microsoft.

As contas escolares ou profissionais da Microsoft são, normalmente, atribuídas a utilizadores finais (colaboradores, estudantes, colaboradores do governo) pelas suas organizações (empresa, escola, agência governamental). Estas contas são controladas diretamente na cloud (na plataforma Azure AD) ou sincronizadas com o Azure AD a partir de um diretório no local, como o Windows Server Active Directory. A Microsoft é o *conservador* das contas escolares ou profissionais, mas as contas são detidas e controladas pela organização.

## <a name="referring-to-azure-ad-accounts-in-your-application"></a>Fazer referência a contas do Azure AD na sua aplicação

A Microsoft não expõe os utilizadores finais às marcas Azure ou Active Directory e o utilizador também não deve fazê-lo.

- Assim que os utilizadores iniciarem sessão, utilize o nome e o logótipo da organização tanto quanto possível. É melhor do que utilizar termos genéricos como "a sua organização".
- Quando os utilizadores não têm sessão iniciada, faça referência às respetivas contas como "Contas escolares ou profissionais" e utilize o logótipo da Microsoft para transmitir que a Microsoft gere estas contas. Não utilize termos como "conta empresarial", "conta de negócio" ou "conta de empresa", porque confundem o utilizador.

## <a name="user-account-pictogram"></a>Pictograma de conta de utilizador

Numa versão anterior destas diretrizes, recomendámos utilizar um pictograma de um "distintivo azul". Com base nos comentários dos utilizadores e programadores, recomendamos agora a utilização do logótipo da Microsoft. O logótipo da Microsoft vai ajudar os utilizadores a compreenderem que podem reutilizar a conta que utilizam com o Office 365 ou outros serviços comerciais da Microsoft para iniciarem sessão na sua aplicação.

## <a name="signing-up-and-signing-in-with-azure-ad"></a>Inscrever-se e iniciar sessão com o Azure AD

A sua aplicação pode apresentar caminhos diferentes para inscrição e início de sessão e as secções seguintes fornecem orientação visual para ambos os cenários.

**Se a aplicação suportar a inscrição de utilizadores finais (por exemplo, modelo Freemium ou de avaliação gratuita)**: pode mostrar um botão de **início de sessão** que permite aos utilizadores acederem à aplicação com a sua conta profissional ou a sua conta pessoal. O Azure AD irá mostrar um pedido de consentimento na primeira vez que acederem à aplicação.

**Se a aplicação exigir permissões a que apenas os administradores podem dar consentimento ou se a aplicação exigir licenciamento organizacional**: separe a aquisição de administrador do início de sessão do utilizador. O **botão "obter esta aplicação"** redireciona os administradores para o início de sessão e, em seguida, pede-lhes para darem consentimento em nome dos utilizadores na respetiva organização, o que tem o benefício adicional de suprimir os pedidos de consentimento do utilizador final para a sua aplicação.

## <a name="visual-guidance-for-app-acquisition"></a>Documentação de orientação visual para aquisição de aplicações

A hiperligação "obter a aplicação" tem de redirecionar o utilizador para a página de concessão de acesso (autorização) do Azure AD, para permitir que o administrador de uma organização autorize que a sua aplicação tenha acesso aos dados da respetiva organização, que são alojados pela Microsoft. O artigo [Integrar aplicações com o Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md) inclui detalhes sobre como pedir acesso.

Após o consentimento dos administradores para a sua aplicação, estes podem optar por adicioná-la à experiência do iniciador de aplicações do Office 365 dos respetivos utilizadores (acessível a partir do ícone de waffle e de [https://portal.office.com/myapps](https://portal.office.com/myapps)). Se quiser anunciar esta capacidade, pode utilizar termos como "Adicionar esta aplicação à sua organização" e mostrar um botão como no exemplo seguinte:

![Tipos de Aplicação e cenários](./media/howto-add-branding-in-azure-ad-apps/add-to-my-org.png)

No entanto, recomendamos que escreva um texto explicativo em vez de utilizar botões. Por exemplo:

> *Se já utiliza o Office 365 ou outro serviço comercial da Microsoft, pode conceder a <your_app_name> acesso aos dados da sua organização. Isto permitirá que os utilizadores acedam a <your_app_name> com as suas contas profissionais existentes.*

Para transferir o logótipo oficial da Microsoft para utilizar na sua aplicação, clique com o botão direito do rato naquele que pretende utilizar e, em seguida, guarde-o no seu computador.

| Recurso                                | Formato PNG | Formato SVG |
| ------------------------------------ | ---------- | ---------- |
| Logótipo da Microsoft  | ![PNG do logótipo da Microsoft](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_mssymbol_19.png) | ![SVG do logótipo da Microsoft](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_mssymbol_19.svg) |

## <a name="visual-guidance-for-sign-in"></a>Orientação visual para início de sessão

A sua aplicação deve apresentar um botão de início de sessão que redirecione os utilizadores para o ponto final de início de sessão que corresponde ao protocolo que utiliza para fazer a integração com o Azure AD. A secção seguinte fornece detalhes sobre o aspeto que esse botão deve ter.

### <a name="pictogram-and-sign-in-with-microsoft"></a>Pictograma e “Iniciar sessão com a Microsoft”

É a associação do logótipo da Microsoft e dos termos "Iniciar sessão com a Microsoft" que representam exclusivamente o Azure AD entre outros fornecedores de identidade que a sua aplicação pode suportar. Se não tiver espaço suficiente para "Iniciar sessão com a Microsoft", pode encurtar para "Iniciar sessão". Pode utilizar um esquema de cores claras ou escuras para os botões.

O diagrama seguinte mostra as marcas de revisão recomendadas pela Microsoft ao utilizar os recursos com a sua aplicação. As marcas de revisão aplicam-se a "Iniciar sessão com a Microsoft" ou à versão "Iniciar sessão" mais curta.

![Marcas de revisão de Iniciar sessão com a Microsoft](./media/howto-add-branding-in-azure-ad-apps/sign-in-with-microsoft-redlines.png)

Para transferir as imagens oficiais da Microsoft para utilizar na sua aplicação, clique com o botão direito do rato naquela que pretende utilizar e, em seguida, guarde-a no seu computador.

| Recurso                                | Formato PNG | Formato SVG |
| ------------------------------------ | ---------- | ---------- |
| Iniciar sessão com a Microsoft (tema escuro)  | ![PNG do tema escuro do botão Iniciar sessão](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_dark.png) | ![SVG do tema escuro do botão Iniciar sessão com a Microsoft](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_dark.svg) |
| Iniciar sessão com a Microsoft (tema claro) | ![PNG do tema claro do botão Iniciar sessão](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_light.png) | ![SVG do tema claro do botão Iniciar sessão com a Microsoft](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_light.svg) |
| Iniciar sessão (tema escuro)                 | ![PNG do tema escuro do botão abreviado Iniciar sessão](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_dark_short.png) | ![SVG do tema escuro do botão abreviado Iniciar sessão](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_dark_short.svg) |
| Iniciar sessão (tema claro)                | ![PNG do tema claro do botão abreviado Iniciar sessão](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_light_short.png) | ![SVG do tema claro do botão abreviado Iniciar sessão](./media/howto-add-branding-in-azure-ad-apps/ms-symbollockup_signin_light_short.svg) |

## <a name="branding-dos-and-donts"></a>O que fazer e não fazer com a imagem corporativa

**UTILIZE** "conta escolar ou profissional" em combinação com o botão "Iniciar sessão com a Microsoft" para fornecer uma explicação adicional para ajudar os utilizadores finais a reconhecerem se podem utilizá-lo. **NÃO** utilize outros termos como "conta empresarial", "conta de negócio" ou "conta de empresa".

**NÃO** utilize "ID do Office 365" ou "ID do Azure". Office 365 também é o nome de uma oferta de consumidor da Microsoft, que não utiliza o Azure AD para autenticação.

**NÃO** altere o logótipo da Microsoft.

**NÃO** exponha os utilizadores finais às marcas Azure ou Active Directory. No entanto, pode utilizar estes termos com programadores, profissionais de TI e administradores.

## <a name="navigation-dos-and-donts"></a>O que fazer e não fazer com a navegação

**PROPORCIONE** uma forma para os utilizadores terminarem sessão e mudarem para outra conta de utilizador. Embora a maioria das pessoas tenha uma única conta pessoal da Microsoft/Facebook/Google/Twitter, as pessoas costumam estar associadas a mais do que uma organização. O suporte de vários utilizadores com sessão iniciada estará disponível em breve.
