---
title: Gestão de aplicações com o Azure Active Directory | Documentos da Microsoft
description: Este artigo os benefícios da integração do Azure Active Directory com os seus locais, na cloud e aplicações SaaS.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: bf53829a2d2578132f9a3595c0bac5e8eb588916
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366776"
---
# <a name="managing-applications-with-azure-active-directory"></a>Gestão de aplicações com o Azure Active Directory
Além do fluxo de trabalho real ou conteúdo, as empresas têm dois requisitos básicos para todas as aplicações:

1. Para aumentar a produtividade, os aplicativos devem ser fáceis de detetar e aceder
2. Para ativar a segurança e governação, a organização necessita de controlo e supervisão sobre quem pode aceder e quem está a aceder, na verdade, cada aplicativo

No mundo de aplicações na cloud, isso pode melhor ser obtido com a identidade para o controlo "*quem tem permissão para fazer o que*."

Na terminologia de informática:

* *Quem* é conhecido como *identidade* -gestão de utilizadores e grupos
* *O que* é conhecido como *gestão de acesso* – a gestão de acesso a recursos protegidos

Os dois componentes em conjunto são conhecidos como *identidade e gestão de acessos (IAM)*, que é definido pela [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) grupo como "*a disciplina de segurança que permite que os indivíduos certos os recursos certos no lado direito de acesso vezes pelos motivos certos*".

OK, então, qual é o problema? Se for de IAM *não geridos* num único local com uma solução integrada:

* Os administradores de identidade têm individualmente, criar e atualizar as contas de utilizador em todos os aplicativos em separado, uma atividade demorada e redundante.
* Os utilizadores têm várias credenciais para aceder às aplicações que precisam para trabalhar com memorizar. Como resultado, os usuários tendem a anotar as senhas ou utilizar outras soluções de gestão de palavra-passe. Essas alternativas apresentam outros riscos de segurança de dados.
* Atividades demoradas e redundantes reduzem vezes os utilizadores e os administradores gastam trabalhar em atividades de negócios que aumentam a linha de na parte inferior da sua empresa.

Por isso, em geral, o que impede que as organizações de adoção de soluções integradas de IAM?

* Soluções técnicas mais baseiam-se em plataformas de software que tem de ser implementado e adaptada por cada organização para as suas aplicações.
* Aplicações na cloud são aprovadas, muitas vezes, a uma tarifa mais alta do que a organização de TI pode integrar com soluções IAM existentes.
* Segurança e as ferramentas de monitorização requerem personalização adicional e a integração para alcançar cenários de E2E abrangentes.

## <a name="azure-active-directory-integrated-with-applications"></a>O Azure Active Directory integrado em aplicações
Azure Active Directory é solução identidade da Microsoft como um serviço (IDaaS) completa que:

* Permite IAM como um serviço em nuvem 
* Fornece gestão de acesso central, início de sessão único (SSO) e relatórios 
* Suporta a gestão de acesso integrada para [milhares de aplicações](https://azure.microsoft.com/marketplace/active-directory/) na Galeria de aplicações, incluindo o Salesforce, Google Apps, caixa, Concur e muito mais. 

Com o Azure Active Directory, todas as aplicações publicar para os seus parceiros e clientes (consumidor ou negócio) tenham a mesma identidade e acesso capacidades de gestão.<br> Isto permite-lhe reduzir significativamente os custos operacionais.

E se precisar de implementar um aplicativo que ainda não está listado na Galeria de aplicações? Embora esse seja um pouco mais morosa do que configurar o SSO para aplicações a partir da Galeria de aplicações, o Azure AD fornece um assistente que o ajuda a com a configuração.

O valor do Azure AD vai além de "apenas" as aplicações na cloud. Também pode utilizá-lo com aplicações no local ao fornecer acesso remoto seguro. Acesso remoto seguro, pode eliminar a necessidade de VPNs ou outras implementações de gestão de acesso remoto tradicional.

Ao fornecer gestão de acesso central e início de sessão único (SSO) para todas as suas aplicações, o Azure AD fornece a solução para a segurança de dados principal e problemas de produtividade.

* Os utilizadores podem aceder a várias aplicações com um início de sessão dando mais tempo para receitas de atividades de operações de gerar ou comercial feitas.
* Os administradores de identidade podem gerir o acesso às aplicações num único local.

O benefício para o utilizador e para a sua empresa é óbvio. Vamos dar uma olhada mais de perto os benefícios para um administrador de identidade e a organização.

## <a name="integrated-application-benefits"></a>Benefícios da aplicação integrada
O processo SSO tem duas etapas:

* Autenticação, o processo de validar a identidade do utilizador.
* Autorização, a decisão de permitir ou bloquear o acesso a um recurso com uma política de acesso.

Quando utilizar o Azure AD para gerir aplicações e ativar o SSO:

* A autenticação é realizada do usuário (por exemplo, AD) no local ou conta do Azure AD.
* Autorização é executado sobre a política do Azure AD de atribuição e proteção garantir a experiência do utilizador final consistente e que lhe permite adicionar condições MFA, atribuição e localizações em qualquer aplicativo, independentemente de seus recursos internos.

É importante compreender que a forma como a autorização é elaborada na aplicação de destino varia consoante a forma como o aplicativo foi integrado no Azure AD.

* **As aplicações previamente integradas ao fornecedor de serviços** , como o Office 365 e o Azure, são criados diretamente no Azure AD e da entidade confiadora no mesmo para as suas capacidades de gestão de identidades e acessos abrangentes de aplicativos. Acesso a esses aplicativos está ativado por meio de informações do diretório e emissão de token.
* **As aplicações previamente integradas da Microsoft e aplicações personalizadas** são independentes de nuvem aplicativos que dependem de um diretório de aplicação interna e podem funcionar independentemente do Azure AD. Acesso a esses aplicativos é ativado através da emissão de uma credencial específico do aplicativo mapeada para uma conta de aplicativos. Consoante as capacidades de aplicativo, a credencial pode ser um token de Federação ou o nome de utilizador e a palavra-passe para uma conta que tenha sido anteriormente aprovisionada no aplicativo.
* **Aplicações no local** aplicações publicadas através do proxy de aplicações do Azure AD, principalmente permitir o acesso a aplicações no local. Estes aplicativos dependem de um diretório central no local, como o Windows Server Active Directory. Acesso a esses aplicativos está ativado ao acionar o proxy para entregar o conteúdo da aplicação para o utilizador final ao respeitar o requisito de início de sessão no local.

Por exemplo, se um utilizador junta-se a sua organização, terá de criar uma conta para o utilizador no Azure AD para as operações de início de sessão principal. Se esse usuário exigir acesso a uma aplicação gerida como o Salesforce, terá também de criar uma conta para este utilizador no Salesforce e ligá-lo para a conta do Azure para tornar o SSO de trabalho. Quando o utilizador sai da organização, é aconselhável eliminar a conta do Azure AD e todas as contas de contraparte do IAM armazena os aplicativos o usuário tivesse acesso a.

## <a name="access-detection"></a>Deteção de acesso
Nas empresas modernas, a departamentos de TI, muitas vezes, não têm conhecimento de todos os aplicativos de nuvem que estão sendo usados. Em conjunto com o Cloud App Discovery, do Azure AD fornece uma solução para detectar esses aplicativos.

## <a name="account-management"></a>Gestão de contas
Tradicionalmente, o gerenciamento de contas em vários aplicativos é um processo manual realizado por IT ou pessoal de suporte da organização. O Azure AD totalmente automatiza o gerenciamento de conta em aplicações integradas por fornecedores de serviços e aplicações pré-integradas pela Microsoft que suporta o aprovisionamento automatizado do utilizador ou SAML ativaram o aprovisionamento.

## <a name="automated-user-provisioning"></a>Aprovisionamento automatizado do utilizador
Alguns aplicativos fornecem interfaces de automação para a criação e remoção ou desativação das contas. Se um fornecedor de oferecer uma interface desse tipo, ele é aproveitado pelo Azure AD. Isso reduz os custos operacionais porque tarefas administrativas ocorrem automaticamente e melhora a segurança do seu ambiente, como ela diminui a probabilidade de acesso não autorizado.

## <a name="access-management"></a>Gestão de acesso
Com o Azure AD pode gerir o acesso a aplicações com individuais ou a regra base em atribuições. Também pode delegar o acesso de gestão para as pessoas corretas na organização garantindo a melhor supervisão e reduzindo a carga sobre o suporte técnico.

## <a name="on-premises-applications"></a>Aplicações no local
O proxy de aplicações incorporada permite-lhe publicar as suas aplicações no local para os seus utilizadores, resultando em ambos consistente experiência com a aplicação de cloud modernas e os benefícios de acesso de capacidades de monitorização, relatórios e segurança do Azure AD.

## <a name="reporting-and-monitoring"></a>Relatórios e monitorização
Do Azure AD fornece relatórios previamente integradas e monitorização capacidades que permitem que sabe quem tem acesso a aplicações e quando eles realmente usaram.

## <a name="related-capabilities"></a>Recursos relacionados
Com o Azure AD pode proteger as suas aplicações com políticas de acesso granular e MFA previamente integrada. Para saber mais sobre MFA do Azure, consulte [MFA do Azure](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Introdução
Para começar a integrar aplicações com o Azure AD, veja a [guia de introdução ao integrar o Azure Active Directory com aplicativos introdução](plan-an-application-integration.md).

## <a name="see-also"></a>Consulte também
* [Índice de Artigos da Gestão da Aplicação no Azure Active Directory](../active-directory-apps-index.md)
* [Plano de implementação passo a passo para SSO numa aplicação SaaS](http://aka.ms/ssodeploymentplan)

