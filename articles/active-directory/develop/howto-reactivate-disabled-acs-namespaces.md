---
title: Como reativar desativada espaços de nomes do serviço de controle de acesso (ACS) do Azure
description: Saiba como localizar e ativar os espaços de nomes do serviço de controlo de acesso (ACS) do Azure e solicitar uma extensão para mantê-los ativada até 4 de Fevereiro de 2019.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: celested
ms.reviewer: jlu
ms.custom: aaddev
ms.openlocfilehash: 298afda7549690a9ea0314bff63a714be50a33b9
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019268"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Como: reativar desativada espaços de nomes do serviço de controlo de acesso

Em Novembro de 2017, anunciámos que o Microsoft Azure controlo de acesso Service (ACS), um serviço do Azure Active Directory (Azure AD), vai ser reformada no dia 7 de Novembro de 2018.

Uma vez que, em seguida, enviámos várias mensagens de correio eletrónico ao e-mail do administrador das subscrições ACS sobre a desativação de ACS 12 meses, 9 meses, 6 meses, três meses, 1, 2 semanas, 1 semana, e 1 dia do mês antes da data de extinção de 7 de Novembro de 2018.

3 de Outubro de 2018, anunciámos (através do e-mail do e [uma mensagem de blogue](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)) uma oferta de extensão para os clientes que não é possível concluir a respetiva migração antes de 7 de Novembro de 2018. O anúncio também continha instruções para pedir a extensão.

## <a name="why-your-namespace-is-disabled"></a>Por que o seu espaço de nomes está desativado

Se ainda não selecionada para a extensão, começaremos a desativar espaços de nomes do ACS a partir de 7 de Novembro de 2018. Se perdeu as comunicações e ainda quiser escolher para a extensão para 4 de Fevereiro de 2019, siga as instruções nas seções a seguir.

> [!NOTE]
> Tem de ser um administrador da subscrição para executar os comandos do PowerShell e pedir uma extensão.

## <a name="find-and-enable-your-acs-namespaces"></a>Localizar e ativar os espaços de nomes do ACS

Pode utilizar o PowerShell de ACS para listar todos os seus espaços de nomes do ACS e reativar as que foram desativados.

1. Transferir e instalar o PowerShell do ACS:
    1. Aceda à galeria do PowerShell e transfira [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2).
    1. Instale o módulo:

        ```powershell
        Install-Module -Name Acs.Namespaces
        ```

    1. Obter uma lista de todos os comandos de possíveis:

        ```powershell
        Get-Command -Module Acs.Namespaces
        ```

        Para obter ajuda sobre um comando específico, execute:

        ```
        Get-Help [Command-Name] -Full
        ```
    
        onde `[Command-Name]` é o nome do comando ACS.
1. Listar as subscrições do Azure disponíveis com o **Get-AcsSubscription** cmdlet.
1. Listar a sua espaços de nomes do ACS com o **Get-AcsNamespace** cmdlet.
1. Confirme que os espaços de nomes estão desativados por confirmar que `State` é `Disabled`.

    [![Confirme que os espaços de nomes estão desativados](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png)](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png#lightbox)

    Também pode utilizar `nslookup {your-namespace}.accesscontrol.windows.net` para confirmar se o domínio ainda está ativo.

1. Ativar a sua namespace(s) de ACS com o **Enable-AcsNamespace** cmdlet.

    Assim que tive ativado seu namespace(s), pode pedir uma extensão, para que o namespace(s) não ser desativado novamente antes de 4 de Fevereiro de 2019. Após essa data, todos os pedidos para ACS irão falhar.

## <a name="request-an-extension"></a>Pedir uma extensão

1. Navegue até ao portal de gestão do seu espaço de nomes ACS acedendo a `https://{your-namespace}.accesscontrol.windows.net`.
1. Selecione o **termos de leitura** botão para ler o [atualizada os termos de utilização](https://azure.microsoft.com/support/legal/access-control/), que irá direcioná-lo para uma página com os termos de utilização atualizados.

    [![Selecione o botão de termos de leitura](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png#lightbox)

1. Selecione **pedido de extensão** na faixa na parte superior da página. O botão só será ativado depois de ler a [atualizada os termos de utilização](https://azure.microsoft.com/support/legal/access-control/).

    [![Selecione o botão de pedido de extensão](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png#lightbox)

1. Depois do pedido de extensão é registado, a página irá atualizar com uma nova faixa na parte superior da página.

    [![Página atualizada com faixa atualizada](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png#lightbox)

## <a name="help-and-support"></a>Ajuda e suporte

- Caso se depare com quaisquer problemas depois de seguir este procedimentos, contacte [suporte do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
- Se tiver questões ou comentários sobre a extinção de ACS, contacte-nos em acsfeedback@microsoft.com.

## <a name="next-steps"></a>Passos Seguintes

- Reveja as informações sobre a extinção de ACS no [como: migrar a partir do serviço de controle de acesso do Azure](active-directory-acs-migration.md).
