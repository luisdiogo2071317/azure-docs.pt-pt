---
title: Ativar aplicações multi-inquilino duplos Digital do Azure | Documentos da Microsoft
description: Noções básicas sobre como registar os inquilinos do Azure Active Directory dos seus clientes duplos Digital do Azure
author: mavoge
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: mavoge
ms.openlocfilehash: a2d9ece119003c341f49ee03d735d5636b179a32
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51259892"
---
# <a name="enable-multitenant-applications-with-azure-digital-twins"></a>Ativar aplicações multi-inquilino duplos Digital do Azure

Os desenvolvedores que usam duplos Digital do Azure normalmente desejam criar aplicações multi-inquilino. R *aplicação multi-inquilino* é uma única instância de aprovisionamento que oferece suporte a vários clientes. Cada cliente tem seus próprios dados independentes e privilégios.

Este documento fornece detalhes sobre como criar uma aplicação multi-inquilino de duplos Digital do Azure que suporta vários inquilinos do Azure Active Directory (Azure AD) e os clientes.

## <a name="scenario-summary"></a>Resumo do cenário

Neste cenário, considere D de desenvolvedor e c do cliente:

- Programador D tem uma subscrição do Azure com um inquilino do Azure AD.
- O desenvolvedor D implementa uma instância de duplos Digital do Azure na sua subscrição do Azure.
- Os utilizadores no inquilino do Azure AD do desenvolvedor D podem obter tokens contra o serviço de duplos Digital do Azure, porque o Azure AD um principal de serviço criado no inquilino do Azure AD do desenvolvedor D.
- O desenvolvedor D agora criar uma aplicação móvel que é integrado diretamente com a API de gestão de duplos Digital do Azure.
- Desenvolvedor D permite a utilização da aplicação móvel de cliente C.
- Cliente C tem de ser autorizado a utilizar a API Management da duplos Digital do Azure no aplicativo do desenvolvedor D.

  > [!IMPORTANT]
  > - Quando C de cliente inicia sessão na aplicação do desenvolvedor D, a aplicação não pode adquirir tokens para os utilizadores do cliente C comunicar com a API de gestão.
  > - O Azure AD emite um erro, que indica que duplos Digital do Azure não é reconhecido dentro do diretório do cliente C.

## <a name="solution"></a>Solução

Para resolver o cenário anterior, as seguintes ações são necessárias para criar um duplos Digital do Azure principal de serviço no inquilino do Azure AD do cliente C:

- Se o cliente C ainda não tiver uma subscrição do Azure com um inquilino do Azure AD:

  - O administrador de inquilino do cliente do C do Azure AD tem de adquirir um [pay as you go de subscrição do Azure](https://azure.microsoft.com/offers/ms-azr-0003p/).
  - O administrador de inquilino do cliente do C do Azure AD, em seguida, deve [ligar o respetivo inquilino com a nova subscrição](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

- Sobre o [portal do Azure](https://portal.azure.com), administrador de inquilino do Azure AD do cliente C realiza as seguintes etapas:

  1. Open **subscrições**.
  1. Selecione a subscrição que tem o inquilino do Azure AD para ser usado em aplicativo do D de desenvolvedor.
  1. Selecione **fornecedores de recursos**.
  1. Procure **Microsoft.IoTSpaces**.
  1. Selecione **Registar**.
  
## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre como utilizar funções definidas pelo utilizador duplos Digital do Azure, leia [Azure Digital duplos UDFs](how-to-user-defined-functions.md).

Para saber como utilizar o controlo de acesso baseado em funções para proteger ainda mais o aplicativo com atribuições de funções, leia [duplos Digital do Azure baseada em funções de controlo de acesso](security-create-manage-role-assignments.md).
