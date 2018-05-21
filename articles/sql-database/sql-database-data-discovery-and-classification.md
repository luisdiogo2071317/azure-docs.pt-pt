---
title: Base de dados SQL do Azure deteção & classificação | Microsoft Docs
description: Base de dados SQL do Azure deteção & classificação
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 05/18/2018
ms.author: giladm
ms.openlocfilehash: b43b010a88f313930217289549448de30a82a070
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="azure-sql-database-data-discovery-and-classification"></a>Base de dados SQL do Azure deteção e a classificação
Deteção de dados & classificação (atualmente em pré-visualização) fornece capacidades avançadas de incorporadas no Azure SQL Database para **detetar**, **classificar**, **etiquetas**  &  **proteger** os dados confidenciais nas suas bases de dados.
Detetar e classificar os dados mais confidenciais (empresas, financeiras, cuidados de saúde, PII, etc.) pode desempenham um papel no seu stature de proteção de informações organizacionais. Que possa servir como a infraestrutura para:
* Ajudar a satisfazer as normas de privacidade de dados e requisitos de conformidade de regulamentação, tais como GDPR.
* Vários cenários de segurança, tais como (auditoria) de monitorização e alertas anómalos acesso a dados confidenciais.
* Controlar o acesso ao e proteger a segurança das bases de dados que contém dados altamente confidenciais.

Deteção de dados & classificação faz parte o [proteção contra ameaças avançadas do SQL Server](sql-advanced-threat-protection.md) oferta (ATP), que é um pacote unificado para capacidades avançadas de segurança do SQL Server. Deteção de dados & classificação podem ser acedido e gerido através do portal do central ATP do SQL Server.

> [!NOTE]
> Este documento está relacionada com apenas a SQL Database do Azure. Para o SQL Server (no local), consulte [deteção de dados do SQL Server e a classificação](https://go.microsoft.com/fwlink/?linkid=866999).

## <a id="subheading-1"></a>O que é a deteção de dados e a classificação?
Deteção de dados & classificação apresenta um conjunto de serviços avançados e novas funcionalidades do SQL Server, que formam uma nova paradigma de proteção de informações do SQL Server diversificada proteger os dados, não apenas a base de dados:
* **Deteção & recomendações** – o motor de classificação analisa a base de dados e identifica as colunas que contêm dados potencialmente confidenciais. -Lo, em seguida, fornece uma forma fácil de rever e aplicar as recomendações de classificação apropriado através do portal do Azure.
* **Etiquetagem** – etiquetas de classificação de sensibilidade podem ser etiquetadas forma permanente em colunas com novos atributos de metadados de classificação introduzidos no motor de SQL Server. Estes metadados, em seguida, podem ser utilizados para com base na sensibilidade de auditoria e proteção cenários avançados.
* **Resultado da consulta definir sensibilidade** – é calculada a sensibilidade de conjunto de resultados de consulta em tempo real para fins de auditoria.
* **Visibilidade** -o estado de classificação de base de dados pode ser visualizado no dashboard detalhado no portal. Além disso, pode transferir um relatório (no formato de Excel) a serem utilizadas para compatibilidade & fins de auditorias, bem como outras necessidades.

## <a id="subheading-2"></a>Detetar, classificar e etiquetar colunas confidenciais
A secção seguinte descreve os passos para detetar, classificar e etiquetar colunas que contêm dados sensíveis na sua base de dados, bem como visualizar o estado de classificação atual da base de dados e exportar relatórios.

A classificação inclui dois atributos de metadados:
* Etiquetas – os atributos de classificação principal utilizadas para definir o nível de sensibilidade dos dados armazenados na coluna.  
* Tipos de informações – proporcionam granularidade adicional para o tipo de dados armazenados na coluna.

## <a name="classify-your-sql-database"></a>Classificar a sua base de dados do SQL Server

1. Aceda ao [Portal do Azure](https://portal.azure.com).

2. Navegue para **Advanced Threat Protection** sob o cabeçalho de segurança no seu painel SQL Database do Azure. Clique para ativar a proteção avançada contra ameaças e, em seguida, clique em de **deteção de dados & classificação (pré-visualização)** cartão.

   ![Uma base de dados de análise](./media/sql-data-discovery-and-classification/data_classification.png) 

3. O **descrição geral** separador inclui um resumo do Estado de classificação atual da base de dados, incluindo uma lista detalhada de todas as colunas classificadas, também pode filtrar para ver apenas as partes do esquema específico, tipos de informações e as etiquetas. Se ainda não ainda classificado quaisquer colunas [avance para o passo 5](#step-5).

   ![Resumo do estado atual de classificação](./media/sql-data-discovery-and-classification/2_data_classification_overview_dashboard.png) 

4. Para transferir um relatório no formato de Excel, clique em de **exportar** opção no menu superior da janela.

   ![Exportar para Excel](./media/sql-data-discovery-and-classification/3_data_classification_export_report.png) 

5.  <a id="step-5"></a>Para começar a classificar os dados, clique em de **separador classificação** na parte superior da janela.

    ![Classificar a dados](./media/sql-data-discovery-and-classification/4_data_classification_classification_tab_click.png) 

6. O motor de classificação analisa a base de dados em colunas que contém dados potencialmente sensíveis e fornece uma lista de **recomendado classificações de coluna**. Para visualizar e aplicar classificação recomendações:

    * Para ver a lista de classificações de coluna recomendados, clique no painel recomendações na parte inferior da janela:
    
      ![Classificar os dados](./media/sql-data-discovery-and-classification/5_data_classification_recommendations_panel.png) 

    * Reveja a lista de recomendações – para aceitar uma recomendação para uma coluna específica, selecione a caixa de verificação na coluna esquerda da linha relevante. Também pode marcar *todas as recomendações* como aceite marcando a caixa de verificação no cabeçalho de tabela de recomendações.

       ![Lista de recomendação de revisão](./media/sql-data-discovery-and-classification/6_data_classification_recommendations_list.png) 

    * Para aplicar as recomendações selecionadas, clique na azul **aceitar recomendações selecionadas** botão.

      ![Aplicar recomendações](./media/sql-data-discovery-and-classification/7_data_classification_accept_selected_recommendations.png) 

7. Também pode **classificar manualmente** colunas como alternativa, ou além disso, a classificação com base na Recomendação:

    * Clique em **Adicionar classificação** no menu superior da janela.
  
      ![Adicione manualmente a classificação](./media/sql-data-discovery-and-classification/8_data_classification_add_classification_button.png) 

    * Na janela do contexto que se abre, selecione o esquema > tabela > coluna que pretende classificar e a etiqueta de tipo e sensibilidade de informações. Em seguida, clique na azul **Adicionar classificação** na parte inferior da janela de contexto.

      ![Selecionar coluna a classificar](./media/sql-data-discovery-and-classification/9_data_classification_manual_classification.png) 

8. Para concluir a classificação e forma permanente etiqueta colunas (etiquetas) a base de dados com novos metadados de classificação, clique em **guardar** no menu superior da janela.

   ![Guardar](./media/sql-data-discovery-and-classification/10_data_classification_save.png) 

## <a id="subheading-3"></a>Auditoria de acesso a dados confidenciais

Um aspeto importante do paradigma de proteção de informações é a capacidade para monitorizar o acesso a dados confidenciais. [Auditoria de base de dados SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) foi melhorado para incluir um novo campo no registo de auditoria chamado *data_sensitivity_information*, que regista as classificações de sensibilidade (etiquetas) dos dados reais que foi devolvidos por a consulta.

![Registo de auditoria](./media/sql-data-discovery-and-classification/11_data_classification_audit_log.png) 

## <a id="subheading-4"></a>Passos seguintes

- Saiba mais sobre [SQL Advanced Threat Protection](sql-advanced-threat-protection.md).
- Considere configurar [auditoria de base de dados SQL do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) para monitorização e auditoria de acesso a dados confidenciais classificados.

<!--Anchors-->
[SQL Data Discovery & Classification overview]: #subheading-1
[Discovering, classifying & labeling sensitive columns]: #subheading-2
[Auditing access to sensitive data]: #subheading-3
[Next Steps]: #subheading-4