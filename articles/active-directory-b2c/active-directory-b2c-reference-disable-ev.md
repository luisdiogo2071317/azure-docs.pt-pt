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
ms.openlocfilehash: e36dd19aa020b8cb2a623cda904cf7fa8a0b26da
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51004604"
---
# <a name="disable-email-verification-during-consumer-sign-up-in-azure-active-directory-b2c"></a>Desativar a verificação de e-mail durante a inscrição no Azure Active Directory B2C do consumidor 
Quando ativada, o Azure Active Directory (Azure AD) B2C dá um consumidor a capacidade para se inscrever para aplicações, fornecendo um endereço de e-mail e criar uma conta local. O Azure AD B2C garante que os endereços de e-mail válidos, exigindo que os consumidores verificá-los durante o processo de inscrição. Ele também evita que um processo automatizado malicioso gerar falsas contas para os aplicativos.

Alguns desenvolvedores de aplicativo preferem ignorar a verificação de e-mail durante o processo de inscrição e em vez disso, ter consumidores, verifique se o endereço de e-mail mais tarde. Para suportar isto, o Azure AD B2C pode ser configurado para desativar a verificação de e-mail. Se o fizer, cria um processo de inscrição mais suave e oferece aos desenvolvedores a flexibilidade para diferenciar os consumidores que tem verificado o respetivo endereço de e-mail desses consumidores que não o tiver feito.

Por predefinição, as políticas de inscrição têm a verificação de e-mail ativada. Utilize os seguintes passos para desativá-la:

1. Clique em **políticas de inscrição** ou **políticas de inscrição ou início de sessão** dependendo da configuração para se inscrever.
2. Clique em sua política (por exemplo, "B2C_1_SiUp") para abri-lo. 
3. Clique em **editar** na parte superior do painel.
4. Clique em **personalização da IU da página**.
5. Clique em **página de inscrição de conta Local**.
6. Clique em **endereço de E-Mail** no **nome** coluna com o **atributos de inscrição** secção.
7. Ativar/desativar a **necessitam de verificação** a opção de **não**.
8. Clique em **OK** na parte inferior, até chegar a **Editar política** painel.
9. Clique em **Guardar**, na parte superior do painel. Já está!

> [!NOTE]
> Desativar a verificação de e-mail no processo de inscrição pode levar para lhe enviar spam. Se desativar o predefinido, recomendamos que adicione o seu próprio sistema de verificação.
> 
>