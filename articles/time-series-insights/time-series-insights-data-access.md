---
title: Configurar a segurança para aceder e gerir o Azure Time Series Insights | Documentos da Microsoft
description: Este artigo descreve como configurar permissões e segurança que o acesso de gestão políticas e acesso a dados políticas para proteger o Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: 3694ff0db63d685cb63f586062158b4f8acac5cf
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847614"
---
# <a name="grant-data-access-to-an-environment"></a>Conceder acesso a dados a um ambiente

Este artigo aborda os dois tipos de políticas de acesso do Azure Time Series Insights (pré-visualização).

## <a name="grant-data-access"></a>Conceder acesso a dados

Siga estes passos para conceder acesso a dados para um principal de utilizador:

1. Inicie sessão no [Portal do Azure](https://portal.azure.com/).
1. Localize o seu ambiente do Time Series Insights (TSI). Tipo `Time Series` no **pesquisa** caixa. Selecione **ambiente de Time Series** nos resultados da pesquisa.
1. Selecione o seu ambiente do TSI da lista.
1. Selecione **políticas de acesso de dados**, em seguida, selecione **+ adicionar**.

    ![um de acesso de dados][1]

1. Selecione **selecionar utilizador**. Procure o endereço de e-mail ou nome de utilizador localizar o utilizador que pretende adicionar. Clique em **selecione** para confirmar a seleção.

    ![dois de acesso de dados][2]

1. Selecione **selecionar função**. Escolha a função de acesso apropriados para o utilizador:

    * Selecione **contribuinte** se pretender permitir que o utilizador alterar dados de referência e consultas de partilha guardada e perspetivas com outros utilizadores do ambiente.

    * Caso contrário, selecione **leitor** para permitir que os dados de consulta de utilizador no ambiente e guardar consultas pessoais de (não partilhadas) no ambiente.

    Selecione **Ok** para confirmar a escolha de função.

    ![acesso a dados-três][3]

1. Selecione **Ok** no **selecionar função de utilizador** página.

    ![quarto de acesso de dados][4]

1. O **políticas de acesso de dados** página lista os utilizadores e funções para cada utilizador.

    ! [acesso de dados-cinco[5]

## <a name="provide-guest-access-to-a-user-from-another-azure-active-directory-tenant"></a>Fornecer acesso de convidado para um utilizador de outro inquilino do Azure Active Directory

`Guest` Não é uma função de gestão; é um termo utilizado para uma conta que é foi convidada a partir de um inquilino para outro. Depois da conta de convidado foi convidada no diretório do inquilino, ele pode ter o mesmo controle de acesso aplicado à mesma, como qualquer outra conta, para conceder acesso de gestão para um ambiente de TSI usando o painel de controlo de acesso (IAM) ou para conceder acesso aos dados do o ambiente por meio do painel de políticas de acesso de dados. Para obter mais informações sobre o acesso de convidado de inquilino do Azure Active Directory (AAD), leia [utilizadores de colaboração de adicionar Azure Active Directory B2B no portal do Azure](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Siga estes passos para conceder acesso de convidado para um ambiente de TSI para um utilizador do AAD de outro inquilino:

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
1. Localize o seu ambiente do TSI. Tipo **séries de tempo** na caixa de pesquisa. Selecione **ambiente de Time Series** nos resultados da pesquisa.
1. Selecione o seu ambiente do TSI da lista.
1. Selecione **políticas de acesso de dados**, em seguida, selecione **+ convidar**.

    ![acesso de dados-seis][6]

1. Fornece e-mail do utilizador que pretende convidar. Tenha em atenção de que deve ser uma mensagem de e-mail associada com o AAD. Opcionalmente, pode incluir uma mensagem pessoal com o convite.

    ![acesso de dados-sete][7]

1. Deverá ver uma bolha de confirmação que apareçam no ecrã.

    ![acesso de dados-oito][8]

1. Selecione **selecionar utilizador**. Procure o endereço de e-mail do utilizador convidado que acabou de convidado para localizar o utilizador que pretende adicionar. Clique em **selecione** para confirmar a seleção.

    ![dados e acesso-nove][9]

1. Selecione **selecione** função. Escolha a função de acesso apropriados para o utilizador convidado:

    1. Selecione **contribuinte** se pretender permitir que o utilizador alterar dados de referência e consultas de partilha guardada e perspetivas com outros utilizadores do ambiente.
    1. Caso contrário, selecione **leitor** para permitir que os dados de consulta de utilizador no ambiente e guardar consultas pessoais de (não partilhadas) no ambiente.
    1. Selecione **Ok** para confirmar a escolha de função.

    ![acesso de dados-dez][10]

1. Selecione **Ok** no **selecionar função de utilizador** página.

1. O **políticas de acesso de dados** página lista agora o utilizador convidado e as funções para cada utilizador convidado.

    ![acesso de dados-11][11]

1. Agora, o utilizador convidado terá de efetuar alguns passos para aceder ao ambiente localizado no Azure do inquilino apenas convidado-lhes. Em primeiro lugar, eles precisam de aceitar o convite que acabou de enviá-los. Este convite é enviado por e-mail para o endereço de e-mail convidado no passo 5. Eles devem clicar em **começar**, para aceitar.

    ![acesso de dados-doze][12]

1. Em seguida, o utilizador convidado terá de aceitar as permissões associadas a organização do administrador.

    ![treze dados-acesso][13]

1. Quando o utilizador convidado tem sessão iniciado para o endereço de e-mail convidou e que aceite o convite, irão avançar para insights.azure.com. Uma vez, terão de clicar no avatar junto ao respetivo e-mail no canto superior direito da tela.

    ![data-access-quatorze][14]

1. Em seguida, o utilizador convidado irá selecionar o seu Azure inquilino no menu de lista pendente de diretório. Este é o inquilino que convidou-los para.

    ![acesso de dados-quinze][15]

1. Por fim, quando o utilizador convidado seleciona o seu inquilino, verão o ambiente de TSI que acabou de fornecer-lhes acesso a. Agora deve ter todos os recursos associados à função que indicou no passo 8.

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais [como adicionar uma origem de evento do Hub de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md) ao seu ambiente do Azure TSI.
* Envie [eventos para a origem do evento](./time-series-insights-send-events.md).
* Modo de exibição [seu ambiente no Explorador do Time Series Insights](./time-series-insights-update-explorer.md).

<!-- Images -->
[1]: media/data-access/data-access-one.png
[2]: media/data-access/data-access-two.png
[3]: media/data-access/data-access-three.png
[4]: media/data-access/data-access-four.png
[5]: media/data-access/data-access-five.png
[6]: media/data-access/data-access-six.png
[7]: media/data-access/data-access-seven.png
[8]: media/data-access/data-access-eight.png
[9]: media/data-access/data-access-nine.png
[10]: media/data-access/data-access-ten.png
[11]: media/data-access/data-access-eleven.png
[12]: media/data-access/data-access-twelve.png
[13]: media/data-access/data-access-thirteen.png
[14]: media/data-access/data-access-fourteen.png
[15]: media/data-access/data-access-fifteen.png