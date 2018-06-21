---
title: Dinamicamente banned palavras-passe no Azure AD
description: Banir fracas palavras-passe do seu ambiente com palavras-passe do Azure AD dinamicamente banned
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 89cbe386d87c6ccb81df7fabd86b197bb69e41e1
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295611"
---
# <a name="eliminate-bad-passwords-in-your-organization"></a>Eliminar palavras-passe incorretas na sua organização

|     |
| --- |
| Proteção de palavra-passe do Azure AD e a lista de palavras-passe banned personalizadas são funcionalidades de pré-visualização pública do Azure Active Directory. Para mais informações sobre pré-visualizações, consulte [suplementares os termos de utilização para a pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Líderes do setor indicam para não utilizar a mesma palavra-passe em vários locais, certifique-se complexa e não tornam mais fácil como Password123. Como podem organizações garantir que os seus utilizadores estão a seguir orientações? Como podem, certifique-se os utilizadores não estiverem a utilizar palavras-passe ou palavras-passe que são conhecidas por ser incluído em falhas recentes de dados comuns?

## <a name="global-banned-password-list"></a>Lista de palavra-passe de banned global

Microsoft sempre está a trabalhar para se manter um passo à frente das criminals informático. Assim a equipa do Azure AD Identity Protection continuamente procura palavras-passe normalmente utilizadas e comprometidas. Em seguida, bloqueiam as palavras-passe que são consideradas demasiado comum, em que chama-se a lista de palavra-passe de banned global. Informático criminals também utilizam estratégias semelhantes no respetivos ataques, por conseguinte Microsoft não publicar o conteúdo desta lista publicamente. Estas palavras-passe vulnerável está bloqueadas antes de ficarem uma ameaça real para os clientes da Microsoft. Para obter mais informações sobre os esforços de segurança atual, consulte o [Microsoft segurança Intelligence relatório](https://www.microsoft.com/security/intelligence-report).

## <a name="preview-custom-banned-password-list"></a>Pré-visualização: Personalizada banned lista de palavra-passe

Algumas organizações poderão querer passo de segurança um ainda mais ao adicionar as seus próprios personalizações sobre a lista de palavra-passe de banned global que Microsoft chama a lista de palavras-passe banned personalizadas. Clientes empresariais, como Contoso, em seguida, pode optar por bloquear variantes dos respetivos nomes de marca, termos de licenciamento específico da empresa ou outros itens.

Personalizado banned lista de palavra-passe e a capacidade de permitir do Active Directory no local integração é gerida através do portal do Azure.

![Modifique a lista de palavras-passe banned personalizadas em métodos de autenticação no portal do Azure](./media/concept-password-ban-bad/authentication-methods-password-protection.png)

## <a name="on-premises-hybrid-scenarios"></a>Cenários de híbrida no local

Proteger contas apenas na nuvem é útil mas, muitas organizações mantêm cenários híbridos, incluindo local no Windows Server Active Directory. É possível instalar a proteção de palavra-passe do Azure AD para o Windows Server Active Directory (pré-visualização) agentes no local expandir as listas de palavra-passe banned à sua infraestrutura existente. Agora, os utilizadores e administradores que alterar, definir ou repor palavras-passe no local têm de estar em conformidade com a mesma política de palavra-passe como os utilizadores apenas na nuvem.

## <a name="how-does-the-banned-password-list-work"></a>Como funciona a lista de palavra-passe banned

A lista de palavra-passe banned corresponde palavras-passe na lista, a cadeia a converter em minúsculas e compara para as palavras-passe banned conhecidas dentro de uma distância de edição de 1 com correspondência por semelhantes.

Exemplo: A palavra-passe do word está bloqueada para uma organização
   - Um utilizador tentar definir a palavra-passe "P@ssword" que é convertido "password" e está bloqueada, porque é uma variante de palavra-passe.
   - Um administrador tenta definir uma palavra-passe de utilizadores para "Password123!" que convertido em "password123!" e porque é uma variante de palavra-passe está bloqueada.

Sempre que um utilizador repõe ou alterações de palavra-passe do Azure AD fluem através deste processo para confirmar que não está na lista banned palavra-passe. Esta verificação é incluída no híbrida repor cenários utilizando a palavra-passe self-service, a sincronização de hash de palavra-passe e a autenticação pass-through.

## <a name="license-requirements"></a>Requisitos de licença

As vantagens da lista de palavra-passe de banned global aplicam-se a todos os utilizadores do Azure Active Directory (Azure AD).

A lista de palavras-passe banned personalizadas requer licenças básico do Azure AD.

Proteção de palavra-passe do Azure AD para o Windows Server Active Directory requer licenças do Azure AD Premium. 

Informações de licenciamento adicionais, incluindo os custos, podem ser encontradas no [Azure Active Directory preços site](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="what-do-users-see"></a>O que os utilizadores veem

Quando um utilizador tenta repor uma palavra-passe para algo que deverá ser banned, verá a seguinte mensagem de erro:

Infelizmente, a palavra-passe contém uma palavra, expressão ou padrão que torna guessable facilmente a sua palavra-passe. Tente novamente com uma palavra-passe diferente.

## <a name="next-steps"></a>Passos Seguintes

* [Configurar a lista de palavras-passe banned personalizadas](howto-password-ban-bad.md)
* [Ativar o Azure AD palavra-passe proteção agentes no local](howto-password-ban-bad-on-premises.md)
