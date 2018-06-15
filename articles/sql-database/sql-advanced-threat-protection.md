---
title: Proteção avançada contra ameaças - base de dados SQL do Azure | Microsoft Docs
description: Saiba mais sobre a funcionalidade para detetar e classificar os dados confidenciais, gerir as vulnerabilidades de base de dados e detetar atividades anómalas que pode indicar uma ameaça à sua base de dados SQL do Azure.
services: sql-database
author: ronitr
manager: craigg
ms.service: sql-database
ms.topic: conceptual
ms.date: 5/17/2018
ms.author: ronitr
ms.reviewer: carlrab
ms.openlocfilehash: da21a0b66d86b4cc3e2dc59bdd972d4e24d7e5ec
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34305508"
---
# <a name="advanced-threat-protection-for-azure-sql-database"></a>Proteção avançada contra ameaças para a base de dados SQL do Azure

Proteção contra ameaças avançadas do SQL Server é um pacote unificado para capacidades avançadas de segurança do SQL Server. Inclui uma funcionalidade para detetar e classificar os dados confidenciais, gerir as vulnerabilidades de base de dados e detetar atividades anómalas que pode indicar uma ameaça à sua base de dados. Fornece uma localização única aceda a para ativar e gerir estas capacidades. 

## <a name="overview"></a>Descrição geral

SQL Server avançadas Threat Protection (ATP) fornece um conjunto de capacidades avançadas de segurança SQL, incluindo dados de deteção & classificação, avaliação de vulnerabilidade e a deteção de ameaças. 

- [Deteção de dados & classificação](sql-database-data-discovery-and-classification.md) (atualmente em pré-visualização) fornece capacidades incorporadas no SQL Database do Azure para detetar, classificar, Etiquetar e proteger os dados confidenciais nas suas bases de dados. Pode ser utilizado para dar visibilidade sobre o estado de classificação de base de dados e para controlar o acesso aos dados sensíveis na base de dados e os respetivos limites.
- [Avaliação da vulnerabilidade](sql-vulnerability-assessment.md) é uma fácil de configurar o serviço que pode detetar, controlar e ajudar a remediar potenciais vulnerabilidades de base de dados. -Lhe visibilidade para o estado de segurança e inclui passos acionáveis para resolver problemas de segurança e para melhorar a sua fortifications de base de dados.
- [Deteção de ameaças](sql-database-threat-detection.md) Deteta atividades anómalas, indicando invulgar e potencialmente prejudicial tenta aceder ou explorar menores, a base de dados. Continuamente monitoriza a base de dados relativos a atividades suspeitas e fornece alertas de segurança imediata em potenciais vulnerabilidades, ataques de injeção de SQL e padrões de acesso de base de dados anómalas. Alertas de deteção de ameaças fornecem detalhes da atividade suspeita e recomendamos ação sobre a investigar e mitigar a ameaça.

Ative SQL ATP depois de ativar todos estes incluídos funcionalidades. Com um clique, pode ativar ATP no seu servidor de base de dados completa, aplicar a todas as bases de dados no servidor. 

Preços ATP está alinhada com escalão standard do Centro de segurança do Azure em 15 $/ nó/mês, onde cada servidor de base de dados do SQL Server protegido é contabilizado como um nó. Os primeiro 60 dias após a ativação da são considerados um período de avaliação gratuita e não serem cobrados. Para obter mais informações, consulte o [página de preços do Centro de segurança do Azure](https://azure.microsoft.com/pricing/details/security-center/).


## <a name="getting-started-with-atp"></a>Introdução ao ATP 
Os passos seguintes ajudá-lo a começar a utilizar ATP. 

## <a name="1-enable-atp"></a>1. Ativar ATP

Ativar ATP navegando para **Advanced Threat Protection** sob o **segurança** cabeçalho no seu painel SQL Database do Azure. Para ativar ATP para todas as bases de dados no servidor, clique em **ativar proteção avançada contra ameaças no servidor**.

![Ativar ATP](./media/sql-advanced-protection/enable_atp.png) 

> [!NOTE]
> O custo de ATP é $15/nó/mês em que um nó é todo do SQL Server lógico. Deste modo, pagar com apenas uma vez para todas as bases de dados no servidor com ATP a proteger. Os primeiro 60 dias são considerados uma avaliação gratuita.

## <a name="2-configure-vulnerability-assessment"></a>2. Configurar a avaliação da vulnerabilidade

Para começar a utilizar a avaliação da vulnerabilidade, terá de configurar uma conta do storage onde os resultados da análise são guardados. Para tal, clique no cartão avaliação da vulnerabilidade.

![Configurar VA](./media/sql-advanced-protection/configure_va.png) 

Selecione ou crie uma conta de armazenamento para guardar os resultados da análise. Também pode ativar o periódicas análises recorrentes para configurar a avaliação da vulnerabilidade executar análises automáticas, uma vez por semana. Um resumo de resultados de análise é enviado para os endereços de e-mail que fornecer.

![Definições de VA](./media/sql-advanced-protection/va_settings.png) 

## <a name="3-start-classifying-data-tracking-vulnerabilities-and-investigating-threat-alerts"></a>3. Iniciar classificar dados, ao controlar a vulnerabilidades e investigar alertas de ameaça

Clique em de **deteção de dados e a classificação** cartão Consulte recomendado colunas confidenciais para classificar e classificar os dados com etiquetas de sensibilidade persistente. Clique em de **avaliação da vulnerabilidade** cartão para ver e gerir relatórios e análises de vulnerabilidade e para controlar o stature de segurança. Se receber os alertas de segurança, clique em de **a deteção de ameaças** cartão para ver os detalhes dos alertas e para ver um relatório consolidado em todos os alertas na sua subscrição do Azure através da página de alertas de segurança do Centro de segurança do Azure.

## <a name="4-manage-atp-settings-on-your-sql-server"></a>4. Gerir definições de ATP no seu SQL server

Para ver e gerir as definições da proteção avançada contra ameaças, navegue até à **Advanced Threat Protection** sob o **segurança** cabeçalho no painel de servidor do SQL Server. Nesta página, pode ativar ou desativar ATP e modificar as definições de deteção de ameaças para o servidor SQL completo.

![Definições do servidor](./media/sql-advanced-protection/server_settings.png) 

## <a name="5-manage-atp-settings-for-a-sql-database"></a>5. Gerir definições de ATP para uma base de dados do SQL Server

Para substituir definições de deteção de ameaças ATP para uma determinada base de dados, verifique o **ativar proteção avançada contra ameaças ao nível da base de dados** caixa de verificação. Utilize esta opção apenas se tiver um requisito específico para receber alertas de deteção de ameaças separado para a base de dados individuais, em vez de ou para além dos alertas recebidos para todas as bases de dados no servidor. 

Assim que a caixa de verificação está selecionada, clique em **definições de deteção de ameaças para esta base de dados** e, em seguida, configure as definições relevantes para esta base de dados.

![Definições de deteção da base de dados e de ameaça](./media/sql-advanced-protection/database_threat_detection_settings.png) 

Definições de proteção contra ameaças avançadas para o servidor também podem ser contactadas a partir do painel de base de dados de ATP. Clique em **definições** no painel de ATP principal e, em seguida, clique em **as definições do servidor de vista Advanced Threat Protection**. 

![Definições da base de dados](./media/sql-advanced-protection/database_settings.png) 

## <a name="next-steps"></a>Passos Seguintes 

- Saiba mais sobre [dados de deteção & classificação](sql-database-data-discovery-and-classification.md) 
- Saiba mais sobre [avaliação da vulnerabilidade](sql-vulnerability-assessment.md) 
- Saiba mais sobre [a deteção de ameaças](sql-database-threat-detection.md)
- Saiba mais sobre [Centro de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)
