---
title: Como banir palavras-passe no Azure AD
description: Banir palavras-passe fracas na sua envirionment com o Azure AD dinamicamente banned passwrords
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: rogoya
ms.openlocfilehash: 0c1517f94d4a6d59077b62614eec8fef665b1529
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296142"
---
# <a name="configuring-the-custom-banned-password-list"></a>Configurar a lista de palavras-passe banned personalizadas

|     |
| --- |
| Proteção de palavra-passe do Azure AD e a lista de palavras-passe banned personalizadas são funcionalidades de pré-visualização pública do Azure Active Directory. Para mais informações sobre pré-visualizações, consulte [suplementares os termos de utilização para a pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Muitas organizações localizar que os seus utilizadores criarem palavras-passe palavras locais comuns, tais como um profissional, desporto equipa ou famosa que, deixando-as fácil de adivinhar a utilizar. Lista de palavra-passe banned personalizada da Microsoft permite às organizações adicione cadeias para avaliar e bloquear, para além o global banned a lista de palavra-passe, quando os utilizadores e administradores de tentam de alterar ou repor uma palavra-passe.

## <a name="add-to-the-custom-list"></a>Adicionar à lista personalizada

Configurar a lista de palavras-passe banned personalizadas necessita de uma licença do Azure Active Directory Premium P1 ou P2. Para obter mais informações sobre o licenciamento do Azure Active Directory, consulte o [Azure Active Directory página de preços](https://azure.microsoft.com/pricing/details/active-directory/). |

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com) e navegue para **do Azure Active Directory**, **métodos de autenticação**, em seguida, **proteção de palavra-passe (pré-visualização)**.
1. Defina a opção **lista personalizada de impor**ao **Sim**.
1. Adicione cadeias para o **personalizada banned lista de palavra-passe**, uma cadeia de carateres por linha
   * A lista de palavra-passe de banned personalizado pode conter até 1000 palavras.
   * A lista de palavra-passe banned personalizada é sensível.
   * A lista de palavras-passe banned personalizadas considera comuns substituição de carateres.
      * "Exemplo: o" e "0" ou "a" e "@"
   * O comprimento da cadeia mínimo é de quatro caracteres e o máximo é de 16 carateres.
1. Quando tiver adicionado todas as cadeias, clique em **guardar**.

> [!NOTE]
> Pode demorar várias horas para as atualizações à lista de palavras-passe banned personalizadas sejam aplicadas.

![Modifique a lista de palavras-passe banned personalizadas em métodos de autenticação no portal do Azure](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Como funciona

Sempre que um utilizador ou um administrador repõe alterações e uma palavra-passe do Azure AD, fluem através de listas banned palavra-passe para confirmar que não é uma lista. Esta verificação é incluída nas palavras-passe definir ou alterar a utilizar o Azure AD.

## <a name="what-do-users-see"></a>O que os utilizadores veem

Quando um utilizador tenta repor uma palavra-passe para algo que deverá ser banned, verá a seguinte mensagem de erro:

Infelizmente, a palavra-passe contém uma palavra, expressão ou padrão que torna guessable facilmente a sua palavra-passe. Tente novamente com uma palavra-passe diferente.

## <a name="next-steps"></a>Passos Seguintes

[Descrição geral conceptual das listas de palavra-passe banned](concept-password-ban-bad.md)

[Descrição geral conceptual de proteção de palavra-passe do Azure AD](concept-password-ban-bad-on-premises.md)

[Ativar a integração no local com as listas de palavra-passe banned](howto-password-ban-bad-on-premises.md)
