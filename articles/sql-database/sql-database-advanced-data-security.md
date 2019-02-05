---
title: Avançadas de segurança de dados - base de dados SQL do Azure | Documentos da Microsoft
description: Saiba mais sobre a funcionalidade para detetar e classificar os dados confidenciais, gerenciar sua vulnerabilidades de base de dados e detetar atividades anómalas que poderá indicar uma ameaça à base de dados SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: 388a1a2ff70f4d00e66511811bffdb76806ed713
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735684"
---
# <a name="advanced-data-security-for-azure-sql-database"></a>Segurança de dados avançados para a base de dados do Azure SQL

Segurança avançada de dados é um pacote unificado para funções de segurança avançadas do SQL. Ele inclui a funcionalidade para detetar e classificar dados confidenciais, analisar e atenuante vulnerabilidades potenciais de base de dados e detetar atividades anómalas que poderá indicar uma ameaça à sua base de dados. Oferece uma localização única para ativar e gerir estas capacidades.

## <a name="overview"></a>Descrição geral

Segurança de dados avançados (ADS) fornece um conjunto de funções de segurança SQL avançadas, incluindo deteção de dados e classificação, avaliação de vulnerabilidade e a deteção de ameaças.

- [Dados de deteção e classificação](sql-database-data-discovery-and-classification.md) (atualmente em pré-visualização) fornece recursos incorporados ao banco de dados do Azure SQL para detetar, classificar, Etiquetar e proteger os dados confidenciais em seus bancos de dados. Pode ser utilizada para oferecer visibilidade sobre o estado de classificação da base de dados e para controlar o acesso aos dados confidenciais na base de dados e além dos respetivos limites.
- [Avaliação de vulnerabilidade](sql-vulnerability-assessment.md) é fácil de configurar o serviço que pode detetar, controlar e ajudá-lo a remediar potenciais vulnerabilidades das bases de dados. Oferece visibilidade sobre o estado de segurança e inclui passos acionáveis para resolver problemas de segurança e melhorar as fortificações da sua base de dados.
- [Deteção de ameaças](sql-database-threat-detection-overview.md) Deteta atividades anómalas que indiquem tentativas invulgares e potencialmente prejudiciais de acesso ou exploração de sua base de dados. Monitoriza continuamente a sua base de dados quanto a atividades suspeitas e apresenta alertas de segurança imediata em potenciais vulnerabilidades, ataques de injeção SQL e padrões de acesso de base de dados anómalos. Alertas de deteção de ameaças fornecem detalhes da atividade suspeita e a ação sobre como investigar e mitigar a ameaça recomendada.

Ative SQL ADS uma vez permitir que todos esses incluído funcionalidades. Com um clique, pode ativar o ADS para todas as bases de dados no seu servidor de base de dados SQL ou a instância gerida. Ativar ou gerir as definições de anúncios requer pertencentes à [Gestor de segurança SQL](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#sql-security-manager) função, a função de administrador de base de dados SQL ou a função de administrador do SQL server. 

ANÚNCIOS de preços que se alinha com o escalão standard do Centro de segurança do Azure, onde cada um protegido que o servidor de base de dados SQL ou a instância gerida é contabilizada como um nó. Recursos recentemente protegidos se qualificam para uma avaliação gratuita do escalão standard do Centro de segurança. Para obter mais informações, consulte a [Centro de segurança do Azure página de preços](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="getting-started-with-ads"></a>Guia de introdução de anúncios

Os seguintes passos ajudar a começar com anúncios.

## <a name="1-enable-ads"></a>1. Ativar o ADS

Ativar anúncios ao navegar para o **segurança de dados avançada** sob a **segurança** do cabeçalho para o seu servidor de base de dados SQL ou a instância gerida. Para ativar o ADS para todas as bases de dados no servidor de base de dados ou a instância gerida, clique em **ativar a segurança de dados avançada no servidor**.

![Ativar o ADS](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> O custo de anúncios é alinhado com o Centro de segurança do Azure preços do escalão standard por nó, em que um nó é o servidor de base de dados SQL completo ou a instância gerida. É, portanto, pagando apenas uma vez para proteger todas as bases de dados no servidor de base de dados ou a instância gerida com anúncios. Pode experimentar ADS inicialmente com uma avaliação gratuita.

## <a name="2-configure-vulnerability-assessment"></a>2. Configurar a avaliação de vulnerabilidade

Para começar a utilizar a avaliação de vulnerabilidades, terá de configurar uma conta de armazenamento onde são guardados os resultados da análise. Para tal, clique no cartão de avaliação de vulnerabilidade.

![Configurar a avaliação de vulnerabilidades](./media/sql-advanced-protection/configure_va.png) 

Selecione ou crie uma conta de armazenamento para guardar os resultados da análise. Pode também ativar as análises periódicas recorrentes para configurar a avaliação de vulnerabilidade para executar a análise automática de uma vez por semana. Um resumo de resultados de análise é enviado para os endereços de e-mail que indicar.

![Definições de avaliação de vulnerabilidades](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Começar a classificar dados, ao controlar a vulnerabilidades e investigar alertas de ameaça

Clique nas **dados de deteção e classificação** recomendado de cartão para ver colunas confidenciais para classificar e classificar os dados com etiquetas de sensibilidade persistente. Clique nas **avaliação de vulnerabilidade** cartão para ver e gerir relatórios e análises de vulnerabilidade e para controlar o escritor de segurança. Se receber os alertas de segurança, clique nas **deteção de ameaças** cartão para ver detalhes dos alertas e para ver um relatório consolidado em todos os alertas na sua subscrição do Azure através da página de alertas de segurança do Centro de segurança do Azure.

## <a name="4-manage-ads-settings-on-your-sql-database-server-or-managed-instance"></a>4. Gerir definições de anúncios em seu servidor de base de dados SQL ou a instância gerida

Para ver e gerir definições de anúncios, navegue para **segurança de dados avançada** sob a **segurança** do cabeçalho para o seu servidor de base de dados SQL ou a instância gerida. Nesta página, pode ativar ou desativar anúncios e ameaças de modificar as definições de deteção para o seu servidor de base de dados SQL completo ou a instância gerida.

![Definições do servidor](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-ads-settings-for-a-sql-database"></a>5. Gerir definições de anúncios de uma base de dados SQL

Para substituir definições de anúncios para uma base de dados específica, consulte a **ativar a segurança de dados avançada ao nível da base de dados** caixa de verificação. Utilize esta opção apenas se tiver uma necessidade específica para receber ameaças separadas alertas de deteção para a base de dados individual, em vez de ou em conjunto com os alertas receberam para todas as bases de dados no servidor de base de dados ou a instância gerida. 

Assim que a caixa de verificação está selecionada, clique em **definições de deteção de ameaças para esta base de dados** e, em seguida, configure as definições relevantes para esta base de dados.

![Definições de deteção de ameaças e base de dados](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Também podem ser contatadas configurações de segurança avançada de dados para o seu servidor de base de dados ou a instância gerida a partir do painel da base de dados de anúncios. Clique em **configurações** no painel de anúncios principal e, em seguida, clique **definições do servidor de segurança de dados avançada da vista**. 

![Definições da base de dados](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Passos Seguintes 

- Saiba mais sobre [dados de deteção e classificação](sql-database-data-discovery-and-classification.md) 
- Saiba mais sobre [avaliação de vulnerabilidade](sql-vulnerability-assessment.md) 
- Saiba mais sobre [deteção de ameaças](sql-database-threat-detection.md)
- Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
