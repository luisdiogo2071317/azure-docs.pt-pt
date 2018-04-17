---
title: Importar um ficheiro do Power BI Desktop para Azure Analysis Services | Microsoft Docs
description: Descreve como importar um ficheiro do Power BI Desktop (pbix) utilizando o portal do Azure.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 35bf2ba85017de43788f802b6244d61ed2bb62df
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Importar um ficheiro do Power BI Desktop

Pode criar um novo modelo no Azure como ao importar um ficheiro de ficheiro (. pbix) do Power BI Desktop. Metadados do modelo, dados em cache e ligações de origem de dados são importadas. Relatórios e visualizações não foram importadas.

**Restrições**   
- O modelo de pbix pode ligar à SQL Database do Azure e Azure SQL Data Warehouse origens de dados apenas. 
- O modelo de pbix não pode ter direta ou ligações de DirectQuery. 
- Importação poderá falhar se o modelo de dados pbix contém metadados que não é suportado no Analysis Services.

## <a name="to-import-from-pbix"></a>Para importar a partir de pbix

1. No seu servidor **descrição geral** > **Web designer**, clique em **abra**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. No **Web designer** > **modelos**, clique em **+ adicionar**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. No **novo modelo**, escreva um nome de modelo e, em seguida, selecione o ficheiro do Power BI Desktop.

    ![Caixa de diálogo novo modelo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. No **importação**, localize e selecione o ficheiro.

     ![Ligar a caixa de diálogo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Ver também

[Criar um modelo no portal do Azure](analysis-services-create-model-portal.md)   
[Ligar ao Azure Analysis Services](analysis-services-connect.md)  
