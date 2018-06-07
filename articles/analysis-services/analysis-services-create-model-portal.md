---
title: Criar um modelo em tabela utilizando o estruturador de Web do Azure Analysis Services | Microsoft Docs
description: Saiba como criar um modelo em tabela do Analysis Services do Azure utilizando o estruturador de Web do portal do Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 443549b9456d8e7d29e807fb00a9f25be00e6faa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34597846"
---
# <a name="create-a-model-in-azure-portal"></a>Criar um modelo no portal do Azure

A funcionalidade de designer (pré-visualização) de web do Azure Analysis Services no portal do Azure fornece uma forma rápida e fácil de criar e editar modelos em tabela e consultar o direito de dados modelo no seu browser. 

Tenha em mente, o estruturador de web está **pré-visualização**. A funcionalidade é limitada. Para mais avançadas modelo desenvolvimento e testes, é melhor utilizar o Visual Studio (SSDT) e SQL Server Management Studio (SSMS).

## <a name="before-you-begin"></a>Antes de começar

- Um servidor de Analysis Services do Azure, o escalão Standard ou programador. Novos modelos criados utilizando o estruturador de Web são DirectQuery, só suportada por estas camadas.
- Uma SQL Database do Azure, o Azure SQL Data Warehouse ou o ficheiro do Power BI Desktop (. pbix) como uma origem de dados. Novos modelos criados a partir de suporte de ficheiros do Power BI Desktop SQL Database do Azure e Azure SQL Data Warehouse.
- Uma conta do SQL Server e a palavra-passe para ligar a origens de dados SQL Database do Azure ou do Azure SQL Data Warehouse.

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="to-create-a-new-tabular-model"></a>Para criar um novo modelo em tabela

1. No seu servidor **descrição geral** > **Web designer**, clique em **abra**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. No **Web designer** > **modelos**, clique em **+ adicionar**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. No **novo modelo**, escreva um nome de modelo e, em seguida, selecione uma origem de dados.

    ![Caixa de diálogo novo modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. No **Connect**, introduza as propriedades de ligação. Nome de utilizador e palavra-passe tem de ser uma conta do SQL Server.

     ![Ligar a caixa de diálogo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. No **tabelas e vistas**, selecione as tabelas para incluir no seu modelo e, em seguida, clique em **criar**. São criadas automaticamente relações entre tabelas com um par de chaves.

     ![Selecione tabelas e vistas](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

O novo modelo que é apresentado no seu browser. Aqui, pode:   

- Dados do modelo de consulta arrastar campos para o estruturador de consultas a e adicionando filtros.
- Crie novas medidas nas tabelas.
- Edite metadados do modelo utilizando o editor de json.
- Abra o modelo no Visual Studio (SSDT), o ambiente de trabalho do Power BI ou no Excel.

![Selecione tabelas e vistas](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Ao editar os metadados do modelo ou criar novas medidas no seu browser, está a guardar estas alterações para o seu modelo no Azure. Se também estiver a trabalhar no seu modelo no SSDT, o Power BI Desktop ou o Excel, o seu modelo pode obter fora de sincronização.


## <a name="next-steps"></a>Passos Seguintes 
[Gerir utilizadores e funções de base de dados](analysis-services-database-users.md)  
[Ligar com o Excel](analysis-services-connect-excel.md)  


