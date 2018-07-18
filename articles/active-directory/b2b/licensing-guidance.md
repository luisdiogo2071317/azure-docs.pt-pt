---
title: Orientação de licenciamento a colaboração B2B do Active Directory do Azure | Documentos da Microsoft
description: O Azure Active Directory B2B não necessita de colaboração pago licenças do Azure AD, mas pode também obter funcionalidades pagas para utilizadores convidados B2B
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 08/09/2017
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 2050952eb924e1eee5e6d7d6312d9cd02f475d10
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39093123"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Orientação de licenciamento de colaboração do Azure Active Directory B2B

Pode usar os recursos de colaboração B2B do Azure AD para convidar utilizadores no inquilino do Azure AD para lhes permitir aceder a serviços do Azure AD e outros recursos na sua organização. Se quiser fornecer acesso às funcionalidades pagas do Azure AD, B2B utilizadores convidados de colaboração têm de estar licenciados com apropriado de licenças do Azure AD. 

Especificamente:
* Capacidades de gratuita do Azure AD estão disponíveis para utilizadores convidados sem licenças adicionais.
* Se quiser fornecer acesso às funcionalidades pagas do Azure AD aos utilizadores B2B, tem de ter licenças suficientes para oferecer suporte a esses utilizadores B2B.
* Um inquilino de convite com um Azure AD, uma licença paga tem direitos para um adicionais cinco utilizadores convidados de B2B convidado para o inquilino de uso a colaboração B2B.
* O cliente que é proprietário do inquilino de convite deve ser o para determinar a colaboração B2B quantos utilizadores têm de pago capacidades do Azure AD. Consoante a paga do Azure AD recursos desejados para seus usuários de convidado, tem de ter o suficiente do Azure AD pagos licenças para abranger os utilizadores de colaboração do B2B na mesma proporção 5:1.

Um utilizador de convidados de colaboração do B2B é adicionado como um usuário numa empresa parceira, não um funcionário da sua organização ou um funcionário de uma empresa diferente no seu conglomerado. Um utilizador de convidados B2B pode iniciar a sessão com credenciais externas ou credenciais pertencentes à sua organização, tal como descrito neste artigo. 

Em outras palavras, licenciamento B2B está definido, não pela forma como o utilizador é autenticado, mas em vez disso, a relação do utilizador para a sua organização. Se estes utilizadores não são parceiros, eles são tratados de forma diferente em termos de licenciamento. Eles não são considerados como um utilizador de colaboração do B2B para efeitos de licenciamento, mesmo que seus UserType é marcado como "Guest". Deve ser licenciados normalmente, numa licença por utilizador. Estes utilizadores incluem:
* Seus funcionários
* Equipe de sessão através de identidades externas
* Um funcionário de uma empresa diferente no seu conglomerado


## <a name="licensing-examples"></a>Exemplos de licenciamento
- Um cliente deseja convidá-lo 100 utilizadores de colaboração B2B ao seu inquilino do Azure AD. O cliente atribui a gestão de acesso e aprovisionamento para todos os utilizadores, mas 50 utilizadores também exigem MFA e o acesso condicional. Esse cliente tem de comprar 10 licenças do Azure AD básico e 10 licenças do Azure AD Premium P1 para abranger esses utilizadores B2B corretamente. Se o cliente planeia utilizar funcionalidades de proteção de identidade com utilizadores B2B, tem de ter licenças do Azure AD Premium P2 para abranger os utilizadores convidados a mesma proporção 5:1.
- Um cliente tiver 10 funcionários que têm a licença todas atualmente com o Azure AD Premium P1. Agora pretende convidar utilizadores B2B 60, que requerem autenticação multifator (MFA). Sob a regra de licenciamento de 5:1, o cliente tem de ter, pelo menos, 12 do Azure AD Premium P1 licenças para abranger todos os utilizadores de colaboração B2B 60. Uma vez que já têm 10 licenças Premium P1 para os 10 funcionários, quais têm direitos de convidar 50 utilizadores B2B com funcionalidades Premium P1, como o MFA. Neste exemplo, em seguida, eles têm licenças 2 adicionais Premium P1 para abranger todos os utilizadores de colaboração do B2B 10 restantes.

> [!NOTE]
> Não existe nenhuma forma de atribuir licenças diretamente aos utilizadores B2B para permitir que estes direitos de utilizador de colaboração do B2B.

O cliente que é proprietário do inquilino de convite deve ser o para determinar a colaboração B2B quantos utilizadores têm de pago capacidades do Azure AD. Dependendo do que funcionalidades pagas do Azure AD que pretende para os utilizadores convidados, tem de ter o suficiente paga do Azure AD licenças para abranger os utilizadores de colaboração do B2B na proporção 5:1. 

## <a name="additional-licensing-details"></a>Detalhes de licenciamento adicionais
- Não é necessário, na verdade, atribuir licenças a contas de utilizador B2B. Com base na proporção 5:1, licenciamento é automaticamente calculada e reportada.
- Se não, paga a licença do Azure AD existe no inquilino, cada utilizador convidado obtém os direitos que oferece a edição gratuita do Azure AD.
- Se um utilizador de colaboração já tem um pago do Azure AD de B2B de licença de sua organização, eles não consomem uma das licenças de colaboração B2B do inquilino de convite.

## <a name="advanced-discussion-what-are-the-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Advanced discussão: quais são as considerações de licenciamento quando adicionamos os utilizadores de uma organização conglomerate como "membros" utilizando as suas APIs?
Um utilizador de convidados B2B é aquele que é convidado a partir de uma organização parceira para trabalhar com a organização de anfitrião. Normalmente, qualquer outro caso não se qualifica como B2B-lo ainda utiliza recursos de B2B. Vejamos dois casos em particular:

1. Se um anfitrião convida um funcionário utilizando um endereço de consumidor
  * Este cenário não está em conformidade com as nossas políticas de licenciamento e não é recomendado.

2. Se uma organização de anfitrião adiciona um utilizador de outra organização conglomerate
  1. Neste caso, o utilizador é convidado com APIs de B2B, mas neste caso, não é tradicionalmente B2B. Idealmente, deve temos essas organizações convidar outros utilizadores de organizações como membros (nossa API permite que). Neste caso, licenças têm de ser atribuídos a esses membros para os mesmos para aceder a recursos na organização do convite.

  2. Algumas organizações poderão querer adicionar utilizadores de outra organização para ser adicionado como "Convidado" como uma política. Existem dois casos aqui:
      * A organização conglomerate já está a utilizar do Azure AD e os utilizadores convidados são licenciados da outra organização: neste caso, não Esperamos que os utilizadores convidados têm de seguir a fórmula de 1:5 descrita anteriormente neste documento. 

      * A organização conglomerate não está a utilizar o Azure AD ou não tem as licenças adequadas: neste caso, siga a fórmula de 1:5 descrita anteriormente neste documento.

## <a name="next-steps"></a>Passos Seguintes

Veja os artigos seguintes na colaboração B2B do Azure AD:

* [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
* [A colaboração do Azure Active Directory B2B perguntas mais frequentes (FAQ)](faq.md)
