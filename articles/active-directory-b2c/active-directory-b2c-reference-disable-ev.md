---
title: Desativar a verificação de e-mail durante consumidor Inscreva-se no Azure Active Directory B2C | Documentos da Microsoft
description: Um tópico que demonstram como desativar a verificação de e-mail durante a inscrição no Azure Active Directory B2C do consumidor.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 2/06/2017
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e008fb87b57b92f8f7e914e6b4344b52d42f9ef8
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/26/2018
ms.locfileid: "39263934"
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a>O Azure Active Directory B2C: Verificação de e-mail de desativar durante a inscrição do consumidor
Quando ativada, o Azure Active Directory (Azure AD) B2C dá um consumidor a capacidade para se inscrever para aplicações, fornecendo um endereço de e-mail e criar uma conta local. O Azure AD B2C garante que os endereços de e-mail válidos, exigindo que os consumidores verificá-los durante o processo de inscrição. Ele também evita que um processo automatizado malicioso gerar falsas contas para os aplicativos.

Alguns desenvolvedores de aplicativo preferem ignorar a verificação de e-mail durante o processo de inscrição e em vez disso, ter consumidores, verifique se o endereço de e-mail mais tarde. Para suportar isto, o Azure AD B2C pode ser configurado para desativar a verificação de e-mail. Se o fizer, cria um processo de inscrição mais suave e oferece aos desenvolvedores a flexibilidade para diferenciar os consumidores que tem verificado o respetivo endereço de e-mail desses consumidores que não o tiver feito.

Por predefinição, as políticas de inscrição têm a verificação de e-mail ativada. Utilize os seguintes passos para desativá-la:

1. [Siga estes passos para navegar para o painel de funcionalidades do B2C no portal do Azure](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Clique em **políticas de inscrição** ou **políticas de inscrição ou início de sessão** dependendo da configuração para se inscrever.
3. Clique em sua política (por exemplo, "B2C_1_SiUp") para abri-lo. 
4. Clique em **editar** na parte superior do painel.
5. Clique em **personalização da IU da página**.
6. Clique em **página de inscrição de conta Local**.
7. Clique em **endereço de E-Mail** no **nome** coluna com o **atributos de inscrição** secção.
8. Ativar/desativar a **necessitam de verificação** a opção de **não**.
9. Clique em **OK** na parte inferior, até chegar a **Editar política** painel.
10. Clique em **guardar** na parte superior do painel. Já está!

> [!NOTE]
> Desativar a verificação de e-mail no processo de inscrição pode levar para lhe enviar spam. Se desativar o predefinido, recomendamos que adicione o seu próprio sistema de verificação.
> 
> 

Estamos sempre abertas para comentários e sugestões! Se têm dificuldades com este tópico ou tiver recomendações para melhorar este conteúdo, Agradecemos os seus comentários na parte inferior da página. Para pedidos de funcionalidades, adicioná-los ao [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
