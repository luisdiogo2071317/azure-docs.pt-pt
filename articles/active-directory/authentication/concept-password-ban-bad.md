---
title: Palavras-passe banidas dinamicamente no Azure AD
description: Banir senhas fracas do seu ambiente com palavras-passe do Azure AD banida dinamicamente
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 1ad499e2703ff8376c063d933c0cc1f03765fc23
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "42054348"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Eliminar palavras-passe incorretas na sua organização

|     |
| --- |
| Proteção de palavra-passe do Azure AD e a lista de palavras-passe banidas personalizadas são funcionalidades de pré-visualização pública do Azure Active Directory. Para obter mais informações sobre pré-visualizações, consulte [termos de utilização suplementares para pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Líderes da indústria a informá-lo para não utilizar a mesma palavra-passe em vários locais, para que seja complexo e não torná-lo simples como Password123. Como as organizações a garantir que seus usuários estão seguindo orientação? Como eles tornar-se de que os utilizadores não estiverem a utilizar as palavras-passe ou palavras-passe que são conhecidas a serem incluídos na recente violações de dados comuns?

## <a name="global-banned-password-list"></a>Lista de palavra-passe banidas global

A Microsoft está sempre a trabalhar para estar um passo à frente dos cibercriminosos. Portanto a equipa do Azure AD Identity Protection continuamente procure palavras-passe comprometidas e comumente utilizadas. Eles bloqueiam, em seguida, essas palavras-passe que são consideradas muito comuns em que é chamado a lista de palavra-passe banidas global. Os cibercriminosos também utilizam estratégias semelhantes em seus ataques, portanto Microsoft não publica o conteúdo desta lista publicamente. Estas palavras-passe vulneráveis é bloqueados antes de se tornarem uma ameaça real aos clientes da Microsoft. Para obter mais informações sobre os esforços de segurança atual, consulte a [Microsoft Security Intelligence Report](https://www.microsoft.com/security/intelligence-report).

## <a name="preview-custom-banned-password-list"></a>Pré-visualização: Personalizado banida lista de palavra-passe

Algumas organizações poderão querer tirar ainda mais a etapa de segurança um adicionando suas próprias personalizações na parte superior da lista de palavra-passe banidas global em que a Microsoft chama a lista de palavras-passe banidas personalizado. Clientes empresariais, como a Contoso, em seguida, pode optar por bloquear variantes de seus nomes de marca, termos específicos da empresa ou outros itens.

O personalizado banido a lista de palavra-passe e a capacidade de ativar no local do Active Directory, integração é gerida no portal do Azure.

![Modificar a lista de palavras-passe banidas personalizadas em métodos de autenticação no portal do Azure](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>Cenários híbridos no local

Proteger contas apenas na cloud é útil, mas muitas organizações mantêm cenários híbridos, incluindo no local Windows Server Active Directory. É possível instalar a proteção de palavra-passe do Azure AD para o Windows Server Active Directory (pré-visualização) agentes no local expandir as listas de palavra-passe banidas à sua infraestrutura existente. Agora, os utilizadores e administradores que alterar, definir ou repor palavras-passe no local é necessário para estar em conformidade com a mesma política de palavra-passe como os utilizadores apenas na cloud.

## <a name="how-does-the-banned-password-list-work"></a>Como funciona a lista de palavra-passe banidas

A lista de palavra-passe banidas corresponde à palavras-passe na lista convertendo a cadeia de caracteres em minúsculas e comparando para as palavras-passe banidas conhecidas dentro de uma distância de edição de 1 com correspondência difusa.

Exemplo: A palavra-passe do word é bloqueada para uma organização
   - Um usuário tentar definir a palavra-passe como "P@ssword" que é convertido em "palavra-passe" e porque é uma variante de palavra-passe é bloqueado.
   - Um administrador tenta definir uma palavra-passe de utilizadores para "Password123!" que convertido em "password123!" e porque é uma variante de palavra-passe está bloqueada.

Sempre que um utilizador repõe ou altera a palavra-passe do Azure AD, flui por meio desse processo para confirmar que não está na lista de palavra-passe banidas. Esta verificação está incluída no híbrida repor cenários usando a palavra-passe self-service, sincronização de hash de palavra-passe e autenticação pass-through.

## <a name="license-requirements"></a>Requisitos de licença

|   | Proteção de palavra-passe do Azure AD com a lista de palavra-passe banidas global | Proteção de palavra-passe do Azure AD com a lista de palavras-passe banidas personalizado|
| --- | --- | --- |
| Utilizadores apenas na cloud | Azure AD Gratuito | Azure AD Básico |
| Os utilizadores sincronizados no local Windows Server Active Directory | O Azure AD Premium P1 ou P2 | O Azure AD Premium P1 ou P2 |

Informações de licenciamento adicionais, incluindo os custos, podem ser encontradas no [do Azure Active Directory preços site](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>O que os utilizadores veem

Quando um utilizador tenta repor uma palavra-passe para algo que deve ser excluído, verão a seguinte mensagem de erro:

Infelizmente, a sua palavra-passe contém uma palavra, frase ou padrão que faz com que a palavra-passe guessable facilmente. Tente novamente com uma palavra-passe diferente.

## <a name="next-steps"></a>Passos Seguintes

* [Configurar a lista de palavras-passe banidas personalizado](howto-password-ban-bad.md)
* [Ativar o Azure AD palavra-passe proteção agentes no local](howto-password-ban-bad-on-premises.md)
