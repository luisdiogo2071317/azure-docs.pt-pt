---
title: Como ativar aplicações multi-inquilino duplos Digital do Azure | Documentos da Microsoft
description: Noções básicas sobre como registar os inquilinos do Azure Active Directory dos seus clientes duplos Digital do Azure
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: mavoge
ms.openlocfilehash: e465ab95b670268f8ef31472259783fce8f24b36
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49324272"
---
# <a name="how-to-enable-multitenant-applications-with-azure-digital-twins"></a>Como ativar aplicações multi-inquilino duplos Digital do Azure

Os desenvolvedores que usam a plataforma de duplos Digital do Azure provavelmente desejará criar aplicações multi-inquilino. R *aplicação multi-inquilino* é uma única instância aprovisionada usada para oferecer suporte a vários clientes com seus próprios dados independentes e privilégios.

Este documento fornece detalhes sobre como criar uma aplicação multi-inquilino de duplos Digital do Azure que suporta vários inquilinos do Azure Active Directory (AD) e os clientes.

## <a name="scenario-summary"></a>Resumo do cenário

Neste cenário, considere D de desenvolvedor e c do cliente:

- Programador D tem uma subscrição do Azure com um inquilino do Azure AD.
- Desenvolvedor D tem implementada uma instância de duplos Digital na respetiva subscrição do Azure.
- Os utilizadores no inquilino do Azure AD do desenvolvedor D podem obter tokens com o serviço do Digital duplos, uma vez que o Azure AD tem um principal de serviço criado no inquilino do Azure AD do desenvolvedor D.
- Desenvolvedor D agora criar uma aplicação móvel que é integrado diretamente com a API de gestão de Digital duplos.
- O desenvolvedor D permite, em seguida, C de cliente, a utilização da aplicação móvel.
- Agora, C de cliente tem de estar autorizado a utilizar a API de gestão dos gémeos Digital no aplicativo do desenvolvedor D.

  > [!IMPORTANT]
  > - Quando C de cliente inicia sessão na aplicação do desenvolvedor D, a aplicação não será possível adquirir tokens para os utilizadores do cliente C comunicar com a API de gestão.
  > - O Azure AD, em seguida, irá gerar um erro que indica que duplos Digital não é reconhecido dentro do diretório do cliente C.

## <a name="solution"></a>Solução

Para resolver o cenário acima, as seguintes ações são necessárias para criar um serviço de duplos Digital principal no inquilino do Azure AD do cliente C:

- Se o cliente C ainda não tiver uma subscrição do Azure com o inquilino do Azure AD:

  - Administrador de inquilino do Azure AD do cliente do C será necessário adquirir uma [pay as you go de subscrição do Azure](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - Administrador de inquilino do Azure AD do cliente do C, em seguida, será necessário [ligar o respetivo inquilino com a nova subscrição](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- Partir do [Portal do Azure](https://portal.azure.com), administrador de inquilino do Azure AD do cliente C, em seguida, deve:
  1. Open **subscrições**.
  1. Selecione a subscrição que tem o inquilino do Azure AD para ser usado em aplicativo do D de desenvolvedor.
  1. Selecione **fornecedores de recursos**.
  1. Procure **Microsoft.IoTSpaces**.
  1. Clique em **registar**.
  
## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre como utilizar funções definidas pelo utilizador duplos Digital do Azure, leia [Azure Digital duplos UDFs](how-to-user-defined-functions.md).

Para saber como utilizar o controlo de acesso baseado em funções para proteger ainda mais o aplicativo com atribuições de funções, leia [duplos Digital baseada em funções de controlo de acesso](security-create-manage-role-assignments.md).
