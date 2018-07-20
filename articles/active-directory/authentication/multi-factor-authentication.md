---
title: Saiba mais sobre a verificação de dois passos na MFA do Azure | Documentos da Microsoft
description: O que é o Azure multi-factor Authentication, por que utilizar MFA e os diferentes métodos e as versões disponíveis.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: d58d81d85dac7e5cd520b8d8a3fb5d91650e0cbe
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39161610"
---
# <a name="what-is-azure-multi-factor-authentication"></a>O que é a Multi-Factor Authentication do Azure?

Verificação de dois passos é um método de autenticação que requer mais do que um método de verificação e adiciona uma segunda camada crítica de segurança aos inícios de sessão de utilizador e de transações. Funciona ao exige dois ou mais dos seguintes métodos de verificação:

* Algo que saiba (normalmente uma palavra-passe)
* Algo que tenha (um dispositivo fidedigno que não seja duplicado facilmente, como um telemóvel)
* Algo que seja (biometria)

<center>![Nome de utilizador e palavra-passe](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certificados](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![smart cards](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![nome de utilizador e palavra-passe](./media/multi-factor-authentication/cert.png)</center>

A Azure Multi-Factor Authentication (MFA) é uma solução de verificação em dois passos da Microsoft. A Azure MFA ajuda a salvaguardar o acesso a dados e a aplicações, satisfazendo, em simultâneo, a necessidade dos utilizadores de terem um processo de início de sessão simples. Oferece autenticação forte através de vários métodos de verificação, incluindo chamada telefónica, mensagem de texto ou verificação de aplicação móvel.

## <a name="why-use-azure-multi-factor-authentication"></a>Porquê utilizar a Autenticação Multifator do Azure?
Hoje em dia, mais do que nunca, as pessoas estão cada vez mais ligadas. Com a smartphones, tablets, laptops e PCs, as pessoas têm várias opções para acessar seus aplicativos e contas de qualquer lugar e se manter conectado em qualquer altura.

O Azure multi-factor Authentication é uma solução fácil de usar, escalável e fiável que fornece um segundo método de autenticação para proteger os seus utilizadores.

| ![Fácil de Utilizar](./media/multi-factor-authentication/simple.png) | ![Escalável](./media/multi-factor-authentication/scalable.png) | ![Sempre protegido](./media/multi-factor-authentication/protected.png) | ![Fiável](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Fácil de usar** |**Dimensionável** |**Sempre protegido** |**Fiável** |

* **Fácil de usar** -multi-factor Authentication do Azure é simples de configurar e utilizar. A proteção extra que vem com o Azure multi-factor Authentication permite aos utilizadores gerir os seus dispositivos. Melhor de tudo, em muitos casos ele pode ser configurado com apenas alguns cliques simples.
* **Dimensionável** -multi-factor Authentication do Azure utiliza o poder da cloud e integra-se com o ambiente AD e aplicações personalizadas. Esta proteção é estendida até mesmo aos seus cenários de missão crítica, de grande volume.
* **Sempre protegido** -multi-factor Authentication fornece autenticação forte usando os mais altos padrões do setor.
* **Fiável** -Microsoft garante a disponibilidade de 99,9% do multi-factor Authentication. O serviço é considerado indisponível quando não conseguir receber ou processar pedidos de verificação para a verificação de dois passos.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [como funciona o Azure multi-factor Authentication](concept-mfa-howitworks.md)

- Leia sobre os diferentes [versões e métodos de consumo para a Azure multi-factor Authentication](concept-mfa-licensing.md)
