---
title: Criar um modelo de tabela com o designer da Web do Azure Analysis Services | Documentos da Microsoft
description: Saiba como criar um modelo de tabela do Azure Analysis Services com o Web designer no portal do Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: dcfcfb24d2b47a8272c576856fc3accc547f354a
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "42054402"
---
# <a name="create-a-model-in-azure-portal"></a>Criar um modelo no portal do Azure

A funcionalidade de designer (pré-visualização) de web do Azure Analysis Services no portal do Azure fornece uma forma rápida e fácil criar e editar modelos de tabela e consultar o direito de dados de modelo no seu browser. 

Tenha em mente, é o web designer **pré-visualização**. A funcionalidade é limitada. Para mais avançados modelo desenvolvimento e teste, é melhor usar o Visual Studio (SSDT) e o SQL Server Management Studio (SSMS).

## <a name="before-you-begin"></a>Antes de começar

- Seu servidor Azure Analysis Services tem de estar no escalão Standard ou o desenvolvedor. Novos modelos criados com o Web designer são DirectQuery, só suportada por estas camadas.
- Uma base de dados do Azure SQL, o Azure SQL Data Warehouse ou o ficheiro do Power BI Desktop (. pbix) como uma origem de dados. Novos modelos criados a partir de suporte de ficheiros do Power BI Desktop SQL Database do Azure e Azure SQL Data Warehouse.
- Uma conta do SQL Server e a palavra-passe para ligar a origens de dados de base de dados do Azure SQL ou armazém de dados SQL do Azure.
- Tem de ter privilégios de administrador de servidor para criar um novo modelo. São necessários privilégios de administrador de base de dados para editar e consultar um modelo utilizando o estruturador.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="to-create-a-new-tabular-model"></a>Para criar um novo modelo em tabela

1. No seu servidor **descrição geral** > **Web designer**, clique em **aberto**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. Na **Web designer** > **modelos**, clique em **+ adicionar**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. Na **novo modelo**, escreva um nome de modelo e, em seguida, selecione uma origem de dados.

    ![Caixa de diálogo novo modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. Na **Connect**, introduza as propriedades de ligação. Nome de utilizador e palavra-passe tem de ser uma conta do SQL Server.

     ![Ligue-se a caixa de diálogo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. Na **tabelas e vistas**, selecione as tabelas a incluir no seu modelo e, em seguida, clique em **criar**. As relações são criadas automaticamente entre tabelas com um par de chaves.

     ![Selecionar tabelas e vistas](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Seu novo modelo é apresentada no seu browser. A partir daqui, pode:   

- Consultar dados de modelo ao arrastar campos para o estruturador de consulta e adicionar filtros.
- Crie novas medidas em tabelas.
- Edite metadados do modelo com o editor de json.
- Abra o modelo no Visual Studio (SSDT), o Power BI Desktop ou o Excel.

![Selecionar tabelas e vistas](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Quando editar metadados do modelo ou crie novas medidas no seu browser, está a guardar essas alterações ao seu modelo no Azure. Se também estiver a trabalhar no seu modelo no SSDT, o Power BI Desktop ou o Excel, o seu modelo pode ficar fora de sincronizado.


## <a name="next-steps"></a>Passos Seguintes 
[Gerir funções de base de dados e utilizadores](analysis-services-database-users.md)  
[Ligar com o Excel](analysis-services-connect-excel.md)  


