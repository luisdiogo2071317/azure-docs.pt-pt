---
title: Aplicações de caráter universal no proxy de aplicações do Azure Active Directory | Microsoft Docs
description: Saiba como utilizar aplicações de caráter universal no proxy de aplicações do Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: d5450da1-9e06-4d08-8146-011c84922ab5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 61426d992e279cf88ae9750f0047b0cd81a797e3
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="wildcard-applications-in-the-azure-active-directory-application-proxy"></a>Aplicações de caráter universal no proxy de aplicações do Azure Active Directory 

No Azure Active Directory (Azure AD), configurar um grande número de no local aplicações pode rapidamente tornar-se unmanageable e introduz riscos desnecessários para erros de configuração, muitos dos mesmos exijam as mesmas definições. Com [Proxy de aplicações do Azure AD](manage-apps/application-proxy.md), pode resolver este problema através de publicação de aplicações de caráter universal para publicar e gerir muitas aplicações de uma só vez. Esta é uma solução que lhe permite:

-   Simplificar o overhead administrativo
-   Reduza o número de potenciais erros de configuração
-   Permitir que os utilizadores acedam em segurança às mais recursos

Este artigo fornece-lhe as informações necessárias configurar a publicação de aplicação de carateres universais no seu ambiente.


## <a name="create-a-wildcard-application"></a>Criar uma aplicação de carateres universais 

Pode criar uma aplicação de caráter universal (*) se tiver um grupo de aplicações com a mesma configuração. Possíveis candidatos para uma aplicação de carateres universais são as seguintes definições de partilha de aplicações:

- O grupo de utilizadores ter acesso aos mesmos
- O método SSO
- O protocolo de acesso (http, https)

Pode publicar aplicações com carateres universais se ambos os URLs internos e externos no seguinte formato:

> http(s)://*.\<domain\>

Por exemplo: `http(s)://*.adventure-works.com`. Enquanto os URLs internos e externos podem utilizar domínios diferentes, como melhor prática, deve ser igual. Quando publicar a aplicação, verá um erro se um dos URLs não tiver um caráter universal.

Se tiver aplicações adicionais com diferentes definições de configuração, tem de publicar estas exceções como aplicações separadas para substituir as predefinições definido para o caráter universal. Aplicações sem um caráter universal sempre precedência sobre as aplicações de caráter universal. Da perspetiva de configuração, estes são aplicações "apenas" regulares.

Criar uma aplicação de carateres universais baseia-se no mesmo [fluxo de publicação de aplicação](manage-apps/application-proxy-publish-azure-portal.md) que está disponível para todas as outras aplicações. A única diferença é que inclua um caráter universal nos URLs e potencialmente a configuração de SSO.


## <a name="prerequisites"></a>Pré-requisitos

### <a name="custom-domains"></a>Domínios personalizados

Enquanto [domínios personalizados](manage-apps/application-proxy-configure-custom-domain.md) são opcionais para todas as outras aplicações, são um pré-requisito para aplicações universais. A criação de domínios personalizados requer a:

1. Criar um domínio verificado no Azure 
2. Carregar um certificado SSL no formato PFX para o proxy de aplicações.

Deve considerar a utilização de um certificado de caráter universal para corresponder a aplicação que pretende criar. Em alternativa, também pode utilizar um certificado que apenas apresenta uma lista de aplicações específicas. Neste caso, apenas as aplicações listadas no certificado estarão acessíveis através desta aplicação de carateres universais.

Por motivos de segurança, isto é um requisito de disco rígido e não suporta carateres universais para aplicações que não é possível utilizar um domínio personalizado para o URL externo.

### <a name="dns-updates"></a>Atualizações de DNS

Quando utilizar domínios personalizados, terá de criar uma entrada DNS com um registo CNAME para o URL externo (por exemplo, `*.adventure-works.com`) que aponta para o URL externo do ponto final de proxy de aplicações. Para aplicações universais, o registo CNAME tem de apontar para os URLs externos relevantes:

> `<yourAADTenantId>.tenant.runtime.msappproxy.net`

Para confirmar que tiver configurado o CNAME corretamente, pode utilizar [nslookup](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup) dos pontos finais de destino, por exemplo, `expenses.adventure-works.com`.  A resposta deve incluir o alias já mencionado (`<yourAADTenantId>.tenant.runtime.msappproxy.net`).


## <a name="considerations"></a>Considerações


### <a name="accepted-formats"></a>Formatos aceites

Para aplicações universais, o **URL interno** tem de ser formatado como `http(s)://*.<domain>`. 

![AppId](./media/active-directory-application-proxy-wildcard\22.png)


Quando configura um **URL externo**, tem de utilizar o seguinte formato: `https://*.<custom domain>` 

![AppId](./media/active-directory-application-proxy-wildcard\21.png)

Outras posições de caráter universal, vários carateres universais ou noutras cadeias de regex não são suportadas e estão a causar erros.


### <a name="excluding-applications-from-the-wildcard"></a>Excluir aplicações de caráter universal

Pode excluir uma aplicação de aplicação universal pelo

- Publicar a aplicação de exceção como aplicação regular 
- Ativar o caráter universal só para aplicações específicas através das definições de DNS  


Publicar uma aplicação como uma aplicação regular é o método preferencial para excluir uma aplicação a partir de um caráter universal. Deve publicar as aplicações excluídas antes das aplicações de carateres universais para se certificar de que as exceções são impostas a partir do início. A aplicação mais específica sempre irá ter precedência – uma aplicação publicada como `budgets.finance.adventure-works.com` tem precedência sobre a aplicação `*.finance.adventure-works.com`, que por sua vez, tem precedência sobre a aplicação `*.adventure-works.com`. 

Também pode limitar o caráter universal para funcionar apenas para aplicações específicas através da gestão de DNS. Como melhor prática, deve criar uma entrada CNAME que inclui um caráter universal e se corresponde ao formato do URL externo que configurou. No entanto, em vez disso, pode apontar URL de aplicação específicos para os carateres universais. Por exemplo, em vez de `*.adventure-works.com`, ponto `hr.adventure-works.com`, `expenses.adventure-works.com` e `travel.adventure-works.com individually` para `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net`. 

Se utilizar esta opção, também terá de outra entrada CNAME para o valor `AppId.domain`, por exemplo, `00000000-1a11-22b2-c333-444d4d4dd444.adventure-works.com`, também indo para a mesma localização. Pode encontrar o **AppId** na página de propriedades de aplicações da aplicação de carateres universais:

![AppId](./media/active-directory-application-proxy-wildcard\01.png)



### <a name="setting-the-homepage-url-for-the-myapps-panel"></a>Definir o URL de home page para o painel MyApps

A aplicação de carateres universais é representada com apenas um mosaico no [MyApps painel](https://myapps.microsoft.com). Por predefinição, este mosaico está oculto. Para mostrar o mosaico e têm de telefone de utilizadores numa página específica:

1. Siga as diretrizes para [definir um URL de home page](application-proxy-office365-app-launcher.md).
2. Definir **Mostrar aplicação** para **verdadeiro** na página de propriedades da aplicação.

### <a name="kerberos-constrained-delegation"></a>Delegação restringida de Kerberos

Para aplicações utilizando [restrita de kerberos (KCD) de delegação como método de SSO](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md), o SPN listados para o método SSO também poderá ter um caráter universal. Por exemplo, o SPN pode ser: `HTTP/*.adventure-works.com`. Ainda precisa de ter os SPNs individuais configurados nos seus servidores de back-end (por exemplo, `http://expenses.adventure-works.com and HTTP/travel.adventure-works.com`).



## <a name="scenario-1-general-wildcard-application"></a>Cenário 1: Aplicação de caráter universal de geral

Neste cenário, terá três aplicações diferentes que pretende publicar:

- `expenses.adventure-works.com`
- `hr.adventure-works.com`
- `travel.adventure-works.com`

Todos os três aplicações:

- São utilizados por todos os utilizadores
- Utilize *autenticação integrada do Windows* 
- Tem as mesmas propriedades


Pode publicar a aplicação de carateres universais utilizando os passos descritos em [publicar aplicações através do Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md). Este cenário pressupõe:

- Um inquilino com o seguinte ID: `000aa000-11b1-2ccc-d333-4444eee4444e` 

- Um domínio verificado chamado `adventure-works.com` foi configurado.

- A **CNAME** entrada que aponta `*.adventure-works.com` para `000aa000-11b1-2ccc-d333-4444eee4444e.tenant.runtime.msappproxy.net` foi criado.

Seguir o [documentados passos](manage-apps/application-proxy-publish-azure-portal.md), criar uma nova aplicação de proxy de aplicação no seu inquilino. Neste exemplo, o caráter universal tem os seguintes campos:

- URL interno:

    ![URL interno](./media/active-directory-application-proxy-wildcard\42.png)


- URL externo:

    ![URL externo](./media/active-directory-application-proxy-wildcard\43.png)

 
- SPN de aplicação interna: 

    ![Configuração de SPN](./media/active-directory-application-proxy-wildcard\44.png)


Por publicar a aplicação de carateres universais, pode agora aceder aos seus três aplicações navegando para os URLs que é utilizados para (por exemplo, `travel.adventure-works.com`).

A configuração implementa a estrutura seguinte:

![AppId](./media/active-directory-application-proxy-wildcard\05.png)

| Cor | Descrição |
| ---   | ---         |
| Azul  | Aplicações explicitamente publicadas e visíveis no Portal do Azure. |
| Cinzento  | As aplicações pode acessível através da aplicação principal. |




## <a name="scenario-2-general-wildcard-application-with-exception"></a>Cenário 2: Aplicação de caráter universal de geral com a exceção

Neste cenário, terá de além de três aplicações de gerais outra aplicação, `finance.adventure-works.com`, que só devem estar acessíveis através da divisão do departamento financeiro. Com a estrutura de aplicação atual, a aplicação de finanças seria acessível através da aplicação de carateres universais e por todos os funcionários. Para alterar esta definição, excluir a aplicação da sua universal configurando financeiros como uma aplicação separada com permissões mais restritivas.



Tem de certificar-se de que os registos CNAME existem esse pontos `finance.adventure-works.com` para o aplicação ponto final específico, especificado na página de Proxy de aplicações para a aplicação. Para este cenário, `finance.adventure-works.com` aponta para `https://finance-awcycles.msappproxy.net/`. 

Seguir o [documentados passos](manage-apps/application-proxy-publish-azure-portal.md), este cenário requer as seguintes definições:


- No **URL interno**, definir **financeiro** em vez de um caráter universal. 

    ![URL interno](./media/active-directory-application-proxy-wildcard\52.png)

- No **URL externo**, definir **financeiro** em vez de um caráter universal. 

    ![URL externo](./media/active-directory-application-proxy-wildcard\53.png)

- SPN de aplicação interna que definir **financeiro** em vez de um caráter universal.

    ![Configuração de SPN](./media/active-directory-application-proxy-wildcard\54.png)


Esta configuração implementa o seguinte cenário:

![Cenário](./media/active-directory-application-proxy-wildcard\09.png)

Porque `finance.adventure-works.com` é um URL mais específico que `*.adventure-works.com`, tem precedência. Os utilizadores ao navegar para `finance.adventure-works.com` tem experiência especificada na aplicação financeiro recursos. Neste caso, apenas os funcionários do departamento financeiro são capazes de aceder `finance.adventure-works.com`.

Se tiver várias aplicações publicadas financeiro e tiver `finance.adventure-works.com` como um domínio verificado, pode publicar outra aplicação de carateres universais `*.finance.adventure-works.com`. Porque esta é a mais específica que genérica `*.adventure-works.com`, tem precedência, se um utilizador acede a uma aplicação no domínio financeiros.


## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre:

- **Domínios personalizados**, consulte [trabalhar com domínios personalizados no Proxy de aplicações do Azure AD](manage-apps/application-proxy-configure-custom-domain.md).

- **Publicar aplicações**, consulte [publicar aplicações através do Proxy de aplicações do Azure AD](manage-apps/application-proxy-publish-azure-portal.md)


