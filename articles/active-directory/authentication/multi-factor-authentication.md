---
title: Saiba mais sobre a verificação de dois passos na MFA do Azure | Microsoft Docs
description: O que é o Azure multi-factor Authentication, porquê utilizar o MFA e os diferentes métodos e as versões disponíveis.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/03/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 2bf2cbefb2b60ad5278bdee7bd97aff1ed6212a4
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
ms.locfileid: "33867163"
---
# <a name="what-is-azure-multi-factor-authentication"></a>O que é a Multi-Factor Authentication do Azure?

Verificação de dois passos é um método de autenticação que requer mais do que um método de verificação e adiciona uma segunda camada crítica de segurança aos inícios de sessão de utilizador e de transações. Funciona exigindo quaisquer dois ou mais dos seguintes métodos de verificação:

* Algo sabe (normalmente uma palavra-passe)
* Algo que tem (um dispositivo fidedigno não for facilmente duplicado, como um telefone)
* Algo que são (biometria)

<center>![Nome de utilizador e palavra-passe](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certificados](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![de Smart Card](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![nome de utilizador e palavra-passe](./media/multi-factor-authentication/cert.png)</center>

A Azure Multi-Factor Authentication (MFA) é uma solução de verificação em dois passos da Microsoft. A Azure MFA ajuda a salvaguardar o acesso a dados e a aplicações, satisfazendo, em simultâneo, a necessidade dos utilizadores de terem um processo de início de sessão simples. Oferece autenticação forte através de vários métodos de verificação, incluindo chamada telefónica, mensagem de texto ou verificação de aplicação móvel.

## <a name="why-use-azure-multi-factor-authentication"></a>Porquê utilizar a Autenticação Multifator do Azure?
Hoje, mais do que nunca, pessoas cada vez mais estão ligadas. Com inteligentes telemóveis, tablets, portáteis e PCs, o que as pessoas têm várias opções para aceder as respetivas contas e as aplicações a partir de qualquer lugar e permaneça ligado a qualquer momento.

Multi-factor Authentication do Azure é uma solução fiável, fácil de utilizar e escalável que fornece um segundo método de autenticação para proteger os seus utilizadores.

| ![Fácil de Utilizar](./media/multi-factor-authentication/simple.png) | ![Escalável](./media/multi-factor-authentication/scalable.png) | ![Sempre protegido](./media/multi-factor-authentication/protected.png) | ![Fiável](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Fácil de utilizar** |**Dimensionável** |**Sempre protegido** |**Fiável** |

* **Fácil de utilizar** -Azure multi-factor Authentication é simple de configurar e utilizar. A proteção adicional que vem com o Azure multi-factor Authentication permite aos utilizadores gerir os seus próprios dispositivos. Melhor de tudo, em muitos casos, pode ser configurado com apenas alguns cliques.
* **Dimensionável** -Azure multi-factor Authentication utiliza a capacidade da nuvem e integra-se com o ambiente AD e aplicações personalizadas. Esta proteção é expandida, mesmo para os cenários de volume elevado, fundamentais.
* **Sempre protegido** -Azure multi-factor Authentication fornece autenticação forte, utilizando as normas da indústria mais elevadas.
* **Fiável** -Microsoft garante a disponibilidade de 99,9% do Azure multi-factor Authentication. O serviço é considerado indisponível quando não o consegue receber ou processar pedidos de verificação para a verificação de dois passos.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [como funciona o Azure multi-factor Authentication](concept-mfa-howitworks.md)

- Leia sobre os diferentes [versões e métodos de consumo de multi-factor Authentication do Azure](concept-mfa-licensing.md)
