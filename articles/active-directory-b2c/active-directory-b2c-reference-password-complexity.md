---
title: Complexidade de palavra-passe no Azure Active Directory B2C | Documentos da Microsoft
description: Como configurar requisitos de complexidade de palavras-passe fornecidos pelos consumidores no Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e728fb037895908882bbe6dc7e8414457ccf273f
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845559"
---
# <a name="azure-ad-b2c-configure-complexity-requirements-for-passwords"></a>Azure AD B2C: Configurar os requisitos de complexidade de palavras-passe

> [!NOTE]
> **Esta funcionalidade está em pré-visualização pública.**

O Azure Active Directory B2C (Azure AD B2C) oferece suporte a alterar os requisitos de complexidade de palavras-passe fornecidas por um usuário final quando criar uma conta.  Por predefinição, o Azure AD B2C utiliza `Strong` palavras-passe.  O Azure AD B2C também oferece suporte a opções de configuração para controlar a complexidade de palavras-passe que os clientes podem utilizar.

## <a name="when-password-rules-are-enforced"></a>Quando são impostas regras de palavra-passe

Durante a inscrição ou de reposição de palavra-passe, um utilizador final tem de fornecer uma palavra-passe que cumpra as regras de complexidade.  Regras de complexidade de palavra-passe são impostas por fluxo de utilizador.  É possível ter um fluxo de utilizador exigir um pin de quatro dígitos durante a inscrição enquanto outro fluxo de utilizador necessita de uma cadeia de caracteres de oito durante a inscrição.  Por exemplo, pode usar um fluxo de utilizador com a complexidade de palavra-passe diferente para os adultos que para crianças.

Complexidade de palavra-passe nunca é aplicada durante o início de sessão.  É pedido aos utilizadores nunca durante o início de sessão para alterar a palavra-passe, porque não cumpre o requisito de complexidade atual.

Aqui estão os tipos de fluxos de utilizador em que a complexidade de palavra-passe pode ser configurada:

* Fluxo de utilizador de inscrição ou início de sessão
* Fluxo de utilizador de reposição de palavra-passe
* Política personalizada ([configurar a complexidade de palavra-passe numa política personalizada do](active-directory-b2c-reference-password-complexity-custom.md))

## <a name="how-to-configure-password-complexity"></a>Como configurar a complexidade de palavra-passe

1. Open **fluxos de utilizador**.
2. Selecione um fluxo de utilizador e clique em **propriedades**.
3. Sob **complexidade de palavra-passe**, altere a complexidade de palavra-passe para que este fluxo de utilizador **simples**, **forte**, ou **personalizado**.

### <a name="comparison-chart"></a>Gráfico de comparação

| Complexidade | Descrição |
| --- | --- |
| Simples | Uma palavra-passe que é, pelo menos, 8 a 64 carateres. |
| Forte | Uma palavra-passe que é, pelo menos, 8 a 64 carateres. Ela exige 3 de 4 em minúsculas, letras maiúsculos, números ou símbolos. |
| Personalizado | Esta opção fornece mais controle sobre regras de complexidade de palavra-passe.  Permite configurar um comprimento personalizado.  Ela também permite aceitar apenas de número de palavras-passe (pins). |

## <a name="options-available-under-custom"></a>As opções disponíveis sob personalizado

### <a name="character-set"></a>Conjunto de carateres

Permite-lhe aceitar dígitos única (pins) ou todo o conjunto de carateres.

* **Apenas os números** permite dígitos única (0-9) ao introduzir uma palavra-passe.
* **Todos os** permite que qualquer letra, número ou símbolo.

### <a name="length"></a>Comprimento

Permite-lhe controlar os requisitos de comprimento da senha.

* **Comprimento mínimo** tem de ser, pelo menos, 4.
* **Comprimento máximo** tem de ser maior ou igual ao comprimento mínimo e máximo que pode ser 64 carateres.

### <a name="character-classes"></a>Classes de caractere

Permite-lhe controlar os tipos de carateres diferentes usados na palavra-passe.

* **2 de 4: Caráter em minúsculas, carateres em maiúsculas, número (0-9), símbolo** garante que a palavra-passe contém, pelo menos, dois tipos de caráter. Por exemplo, um número e um caráter em minúsculas.
* **3 de 4: Caráter em minúsculas, carateres em maiúsculas, número (0-9), símbolo** garante que a palavra-passe contém, pelo menos, dois tipos de caráter. Por exemplo, um número, um caráter em minúsculas e um caráter em maiúsculas.
* **4 de 4: Caráter em minúsculas, carateres em maiúsculas, número (0-9), símbolo** garante que a palavra-passe contém tudo para tipos de carateres.

    > [!NOTE]
    > Exigir **4 de 4** pode resultar em frustração do usuário final. Alguns estudos demonstraram que este requisito não melhora entropia de palavra-passe. Consulte [NIST diretrizes de palavra-passe](https://pages.nist.gov/800-63-3/sp800-63b.html#appA)
