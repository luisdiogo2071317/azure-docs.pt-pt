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
ms.openlocfilehash: 97c9480d6f2b75d83252bfb6410d7b5f946757ef
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39630658"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Conceder acesso a dados a um ambiente do Time Series Insights com o portal do Azure

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

## <a name="next-steps"></a>Passos Seguintes
* Saiba mais [como adicionar uma origem de evento do Hub de eventos para o seu ambiente do Azure Time Series Insights](time-series-insights-how-to-add-an-event-source-eventhub.md).
* [Enviar eventos](time-series-insights-send-events.md) para a origem do evento.
* Ver o seu ambiente no [Explorador do Time Series Insights](https://insights.timeseries.azure.com).
