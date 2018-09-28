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
ms.date: 11/15/2017
ms.openlocfilehash: c9bddf8e20524433b31793e277efd954a5d1320e
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423379"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Conceder acesso a dados a um ambiente do Time Series Insights com o portal do Azure

Este artigo aborda os dois tipos de políticas de acesso do Time Series Insights.

## <a name="video"></a>Vídeo: 

### <a name="in-this-video-we-cover-creating-and-managing-access-policies-within-time-series-insights-br"></a>Neste vídeo, vamos abordar criar e gerir as políticas de acesso no Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/5zTIdyHMJW8]

Os ambientes do Time Series Insights têm dois tipos de políticas de acesso independentes:

* Políticas de acesso de gestão
* Políticas de acesso a dados

Ambas as políticas concedem aos principais (utilizadores e aplicações) do Azure Active Directory várias permissões num determinado ambiente. Principais (utilizadores e aplicações) têm de pertencer ao active directory (conhecido como o inquilino do Azure) associado à subscrição que contém o ambiente.

As políticas de acesso de gestão concedem permissões relacionadas com a configuração do ambiente, como a:
*   Criação e eliminação do ambiente, origens de eventos, conjuntos de dados de referência, e
*   Gestão das políticas de acesso a dados.

As políticas de acesso a dados concedem permissões para emitir consultas de dados, manipular dados de referência no ambiente e partilhar consultas guardadas e perspetivas associadas ao ambiente.

Ambos os tipos de políticas permitem uma clara separação entre o acesso à gestão do ambiente e o acesso aos dados dentro do ambiente. Por exemplo, é possível configurar um ambiente de forma a que o proprietário/criador do ambiente é removido do acesso a dados. Além disso, os utilizadores e serviços que têm permissão para ler dados a partir do ambiente podem ser concedidos sem acesso à configuração do ambiente.

## <a name="grant-data-access"></a>Conceder acesso a dados
Siga estes passos para conceder acesso a dados para um principal de utilizador:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Localize o seu ambiente do Time Series Insights. Tipo **séries de tempo** no **pesquisa** caixa. Selecione **ambiente de Time Series** nos resultados da pesquisa. 

3. Selecione o seu ambiente do Time Series Insights na lista.
   
4. Selecione **políticas de acesso de dados**, em seguida, selecione **+ adicionar**.
  ![Gerir a origem do Time Series Insights - ambiente](media/data-access/getstarted-grant-data-access1.png)

5. Selecione **selecionar utilizador**.  Procure o endereço de e-mail ou nome de utilizador localizar o utilizador que pretende adicionar. Clique em **selecione** para confirmar a seleção. 

   ![Gerir a origem do Time Series Insights - adicionar](media/data-access/getstarted-grant-data-access2.png)

6. Selecione **selecionar função**. Escolha a função de acesso apropriados para o utilizador:
   - Selecione **contribuinte** se pretender permitir que o utilizador alterar dados de referência e consultas de partilha guardada e perspetivas com outros utilizadores do ambiente. 
   - Caso contrário, selecione **leitor** para permitir que os dados de consulta de utilizador no ambiente e guardar consultas pessoais de (não partilhadas) no ambiente.

   Selecione **Ok** para confirmar a escolha de função.

   ![Gerir a origem do Time Series Insights - selecionar utilizador](media/data-access/getstarted-grant-data-access3.png)

8. Selecione **Ok** no **selecionar função de utilizador** página.

   ![Gerir a origem do Time Series Insights - selecionar função](media/data-access/getstarted-grant-data-access4.png)

9. O **políticas de acesso de dados** página lista os utilizadores e funções para cada utilizador.

   ![Gerir a origem do Time Series Insights - resultados](media/data-access/getstarted-grant-data-access5.png)

## <a name="provide-guest-access-to-a-user-from-another-aad-tenant"></a>Fornecer acesso de convidado para um utilizador de outro inquilino do AAD

"Convidado" não é uma função de gestão; é um termo utilizado para uma conta que é foi convidada a partir de um inquilino para outro. Depois da conta de convidado foi convidada no diretório do inquilino, ele pode ter o mesmo controle de acesso aplicado à mesma, como qualquer outra conta, para conceder acesso de gestão para um ambiente de TSI usando o painel de controlo de acesso (IAM) ou para conceder acesso aos dados do o ambiente por meio do painel de políticas de acesso de dados. Para obter mais informações sobre o acesso de convidado de inquilino do AAD, consulte esta [documento](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Siga estes passos para conceder acesso de convidado para um ambiente do Time Series Insights para um utilizador do AAD de outro inquilino:

1. Inicie sessão no [portal do Azure](https://portal.azure.com).

2. Localize o seu ambiente do Time Series Insights. Tipo **séries de tempo** no **pesquisa** caixa. Selecione **ambiente de Time Series** nos resultados da pesquisa.

3. Selecione o seu ambiente do Time Series Insights na lista.

4. Selecione **políticas de acesso de dados**, em seguida, selecione + **convidar**.

    ![Gerir a origem do Time Series Insights - convidar utilizador](media/data-access/getstarted-grant-data-access6.png)

5. Fornece e-mail do utilizador que pretende convidar. Tenha em atenção de que deve ser uma mensagem de e-mail associada com o AAD. Opcionalmente, pode incluir uma mensagem pessoal com o convite.

    ![Gerir a origem do Time Series Insights - selecionar utilizador](media/data-access/getstarted-grant-data-access7.png)

6. Deverá ver uma bolha de confirmação que apareçam no ecrã.

    ![Gerir a origem do Time Series Insights - confirmar o utilizador](media/data-access/getstarted-grant-data-access8.png)

7. Selecione **selecionar utilizador**. Procure o endereço de e-mail do utilizador convidado que acabou de convidado para localizar o utilizador que pretende adicionar. Clique em **selecione** para confirmar a seleção.
  
    ![Gerir a origem do Time Series Insights - confirmar o utilizador](media/data-access/getstarted-grant-data-access9.png)

8. Selecione **selecionar função**. Escolha a função de acesso apropriados para o utilizador convidado:

    * Selecione **contribuinte** se pretender permitir que o utilizador alterar dados de referência e consultas de partilha guardada e perspetivas com outros utilizadores do ambiente.

    * Caso contrário, selecione **leitor** para permitir que os dados de consulta de utilizador no ambiente e guardar consultas pessoais de (não partilhadas) no ambiente.

    Selecione **Ok** para confirmar a escolha de função.

    ![Gerir a origem do Time Series Insights - selecionar função](media/data-access/getstarted-grant-data-access10.png)

9. Selecione **Ok** no **selecionar função de utilizador** página.

10. O **políticas de acesso de dados** página lista agora o utilizador convidado e as funções para cada utilizador convidado.

    ![Gerir a origem do Time Series Insights - confirmar função](media/data-access/getstarted-grant-data-access11.png)

11. Agora, o utilizador convidado terá de efetuar alguns passos para aceder ao ambiente localizado no Azure do inquilino apenas convidado-lhes. Em primeiro lugar, eles precisam de aceitar o convite que acabou de enviá-los. Este convite é enviado por e-mail para o endereço de e-mail convidado no passo 5. Eles devem clicar em 'Get Started,' para aceitar.

    ![Gerir a origem do Time Series Insights - convidar utilizador](media/data-access/getstarted-grant-data-access12.png)

12. Em seguida, o utilizador convidado terá de aceitar as permissões associadas a organização do administrador.

    ![Gerir a origem do Time Series Insights - aceitar as permissões](media/data-access/getstarted-grant-data-access13.png)

13. Quando o utilizador convidado tem sessão iniciado para o endereço de e-mail convidou e que aceite o convite, irão avançar para insights.azure.com. Uma vez, terão de clicar no avatar junto ao respetivo e-mail no canto superior direito da tela. 

    ![Gerir a origem do Time Series Insights - aceitar as permissões](media/data-access/getstarted-grant-data-access14.png)

14. Em seguida, o utilizador convidado irá selecionar o seu Azure inquilino no menu de lista pendente de diretório. Este é o inquilino que convidou-los para. 

    ![Gerir a origem do Time Series Insights - aceitar as permissões](media/data-access/getstarted-grant-data-access15.png)

15. Por fim, quando o utilizador convidado seleciona o seu inquilino, verão o ambiente de Time Series Insights que acabou de fornecer-lhes acesso a. Agora deve ter todos os recursos associados à função que indicou no passo 8.

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais [como adicionar uma origem de evento do Hub de eventos para o seu ambiente do Azure Time Series Insights](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Enviar eventos](time-series-insights-send-events.md) para a origem do evento.
* Ver o seu ambiente no [Explorador do Time Series Insights](https://insights.timeseries.azure.com).
