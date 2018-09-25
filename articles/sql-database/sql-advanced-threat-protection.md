---
title: Proteção avançada contra ameaças - base de dados SQL do Azure | Documentos da Microsoft
description: Saiba mais sobre a funcionalidade para detetar e classificar os dados confidenciais, gerenciar sua vulnerabilidades de base de dados e detetar atividades anómalas que poderá indicar uma ameaça à base de dados SQL do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: threat-protection
ms.custom: security
ms.devlang: ''
ms.topic: conceptual
author: ronitr
ms.author: ronitr
ms.reviewer: vanto
manager: craigg
ms.date: 05/17/2018
ms.openlocfilehash: 90d0784a33b3b80a5b2c78bf4b22dac5654b1450
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063919"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Proteção avançada contra ameaças para a base de dados SQL do Azure

O SQL Advanced Threat Protection é um pacote unificado para capacidades avançadas de segurança do SQL. Ele inclui a funcionalidade para detetar e classificar dados confidenciais, analisar e atenuante vulnerabilidades potenciais de base de dados e detetar atividades anómalas que poderá indicar uma ameaça à sua base de dados. Oferece uma localização única para ativar e gerir estas capacidades. 

## <a name="overview"></a>Descrição geral

Da proteção contra ameaças avançadas SQL (ATP) fornece um conjunto de funções de segurança SQL avançadas, incluindo dados de deteção e classificação, avaliação de vulnerabilidade e a deteção de ameaças. 

- A [Deteção de Dados e Classificação](sql-database-data-discovery-and-classification.md) (atualmente em pré-visualização) oferece capacidades incorporadas na Base de Dados SQL do Azure para detetar, classificar, etiquetar e proteger os dados confidenciais nas suas bases de dados. Pode ser utilizada para oferecer visibilidade sobre o estado de classificação da base de dados e para controlar o acesso aos dados confidenciais na base de dados e além dos respetivos limites.
- A [Avaliação da Vulnerabilidade](sql-vulnerability-assessment.md) é um serviço fácil de configurar que pode detetar, controlar e ajudar a corrigir potenciais vulnerabilidades da base de dados. Oferece visibilidade sobre o estado de segurança e inclui passos acionáveis para resolver problemas de segurança e melhorar as fortificações da sua base de dados.
- A [Deteção de Ameaças](sql-database-threat-detection.md) deteta ameaças anómalas que indicam tentativas potencialmente perigosas e invulgares para aceder ou explorar a sua base de dados. Monitoriza continuamente a sua base de dados quanto a atividades suspeitas e apresenta alertas de segurança imediata em potenciais vulnerabilidades, ataques de injeção SQL e padrões de acesso de base de dados anómalos. Os alertas da Deteção de Ameaças mostram detalhes da atividade suspeita e a ação recomendada para investigar e mitigar essa ameaça.

Ative o ATP de SQL uma vez permitir que todos esses incluído funcionalidades. Com um clique, pode ativar o ATP em todo o servidor da sua base de dados, aplicando-se a todas as bases de dados no servidor. 

Preços do ATP se alinha com o escalão standard do Centro de segurança do Azure us $15/nó/mês, em que cada servidor de base de dados SQL protegido é contabilizado como um nó. Os primeiros 60 dias após a ativação são considerados um período de avaliação gratuito e não são cobrados. Para obter mais informações, consulte a [Centro de segurança do Azure página de preços](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="getting-started-with-atp"></a>Guia de introdução do ATP 
Os seguintes passos ajudá-lo a começar a ATP. 

## <a name="1-enable-atp"></a>1. Ativar o ATP

Ativar o ATP ao navegar até **proteção avançada contra ameaças** sob a **segurança** cabeçalho no seu painel de base de dados do Azure SQL. Para ativar o ATP para todas as bases de dados no servidor, clique em **ativar a proteção avançada contra ameaças no servidor**.

![Ativar o ATP](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> O custo da ATP é US $15/nó/mês, em que um nó é todo servidor lógico SQL. É, portanto, pagando apenas uma vez para proteger todas as bases de dados com o ATP no servidor. Os primeiros 60 dias são considerados uma avaliação gratuita.

## <a name="2-configure-vulnerability-assessment"></a>2. Configurar a avaliação de vulnerabilidade

Para começar a utilizar a avaliação de vulnerabilidades, terá de configurar uma conta de armazenamento onde são guardados os resultados da análise. Para tal, clique no cartão de avaliação de vulnerabilidades.

![Configurar a avaliação de vulnerabilidades](./media/sql-advanced-protection/configure_va.png) 

Selecione ou crie uma conta de armazenamento para guardar os resultados da análise. Pode também ativar as análises periódicas recorrentes para configurar a avaliação de vulnerabilidade para executar a análise automática de uma vez por semana. Um resumo de resultados de análise é enviado para os endereços de e-mail que indicar.

![Definições de avaliação de vulnerabilidades](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Começar a classificar dados, ao controlar a vulnerabilidades e investigar alertas de ameaça

Clique nas **dados de deteção e classificação** recomendado de cartão para ver colunas confidenciais para classificar e classificar os dados com etiquetas de sensibilidade persistente. Clique nas **avaliação de vulnerabilidade** cartão para ver e gerir relatórios e análises de vulnerabilidade e para controlar o escritor de segurança. Se receber os alertas de segurança, clique nas **deteção de ameaças** cartão para ver detalhes dos alertas e para ver um relatório consolidado em todos os alertas na sua subscrição do Azure através da página de alertas de segurança do Centro de segurança do Azure.

## <a name="4-manage-atp-settings-on-your-sql-server"></a>4. Gerir as definições do ATP no SQL server

Para ver e gerir definições de proteção avançada contra ameaças, navegue para **proteção avançada contra ameaças** sob a **segurança** cabeçalho no seu painel de servidor SQL. Nesta página, pode ativar ou desativar o ATP e modificar as definições de deteção de ameaças para o servidor SQL completo.

![Definições do servidor](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-atp-settings-for-a-sql-database"></a>5. Gerir as definições do ATP para uma base de dados SQL

Para substituir definições de deteção de ameaças do ATP para uma base de dados específica, consulte a **ativar a proteção avançada contra ameaças ao nível da base de dados** caixa de verificação. Utilize esta opção apenas se tiver uma necessidade específica para receber alertas de deteção de ameaças separados para a base de dados individual, em vez de ou em conjunto com os alertas recebidos para todas as bases de dados no servidor. 

Assim que a caixa de verificação está selecionada, clique em **definições de deteção de ameaças para esta base de dados** e, em seguida, configure as definições relevantes para esta base de dados.

![Definições de deteção de ameaças e base de dados](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Também podem ser contatadas definições de proteção contra ameaças avançadas para o servidor a partir do painel da base de dados de ATP. Clique em **configurações** no painel principal do ATP e, em seguida, clique **definições do servidor de proteção do vista avançada contra ameaças**. 

![Definições da base de dados](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Passos Seguintes 

- Saiba mais sobre [dados de deteção e classificação](sql-database-data-discovery-and-classification.md) 
- Saiba mais sobre [avaliação de vulnerabilidade](sql-vulnerability-assessment.md) 
- Saiba mais sobre [a deteção de ameaças](sql-database-threat-detection.md)
- Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
