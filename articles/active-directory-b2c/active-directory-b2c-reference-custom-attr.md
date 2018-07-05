---
title: Os atributos personalizados do Azure Active Directory B2C | Documentos da Microsoft
description: Como utilizar atributos personalizados no Azure Active Directory B2C para recolher informações sobre os consumidores.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 893dfbae96d2cfea01b1f281f888e9281bf582f9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441921"
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>O Azure Active Directory B2C: Utilizar atributos personalizados para recolher informações sobre os consumidores
O diretório do Azure Active Directory (Azure AD) B2C vem com um conjunto interno de informações (atributos): nome próprio, apelido, cidade, Código Postal e outros atributos. No entanto, todas as aplicações direcionadas para o consumidor tem requisitos exclusivos nos quais atributos para recolher dos consumidores. Com o Azure AD B2C, pode estender o conjunto de atributos armazenados em cada conta de consumidor. Pode criar atributos personalizados sobre o [portal do Azure](https://portal.azure.com/) e utilizá-lo em suas políticas de inscrição, conforme mostrado abaixo. Também pode ler e gravar esses atributos utilizando o [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Utilização de atributos personalizados [do Azure AD Graph API Extensões de esquema do](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).
> 
> 

## <a name="create-a-custom-attribute"></a>Criar um atributo personalizado
1. [Siga estes passos para navegar para o painel de funcionalidades do B2C no portal do Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Clique em **atributos de utilizador**.
3. Clique em **+ Adicionar** na parte superior do painel.
4. Fornecer um **Name** para o atributo personalizado (por exemplo, "ShoeSize") e, opcionalmente, uma **Descrição**. Clique em **Criar**.
   
   > [!NOTE]
   > Apenas o "String", "Booleano" e "Int" **tipos de dados** estão atualmente disponíveis.
   > 
   > 

O atributo personalizado está agora disponível na lista de **atributos de utilizador**para uso em suas políticas de inscrição.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Utilizar um atributo personalizado na política de inscrição
1. [Siga estes passos para navegar para o painel de funcionalidades do B2C no portal do Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Clique em **Políticas de inscrição**.
3. Clique em política de inscrição (por exemplo, "B2C_1_SiUp") para abri-lo. Clique em **editar** na parte superior do painel.
4. Clique em **atributos de inscrição** e selecione o atributo personalizado (por exemplo, "ShoeSize"). Clique em **OK**.
5. Clique em **afirmações de aplicação** e selecione o vlastní atribut. Clique em **OK**.
6. Clique em **guardar** na parte superior do painel.

Pode utilizar a funcionalidade de "Executar agora" na política para verificar a experiência de consumidor. Agora deve ver "ShoeSize" na lista de atributos recolhidos durante a inscrição do consumidor e vê-lo no token enviado para a sua aplicação.

## <a name="notes"></a>Notas
* Juntamente com as políticas de inscrição, os atributos personalizados também podem ser utilizados nas políticas de inscrição ou início de sessão e as políticas de edição de perfil.
* Há uma limitação conhecida de atributos personalizados. É só criado na primeira vez que é utilizado em qualquer política e não quando o adicionar à lista de **atributos de utilizador**.

