---
title: Importar um ficheiro do Power BI Desktop para o Azure Analysis Services | Documentos da Microsoft
description: Descreve como importar um ficheiro do Power BI Desktop (pbix) com o portal do Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 3dd90fc862e64812c0ba17bef74818d18788f4b5
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37440993"
---
# <a name="import-a-power-bi-desktop-file"></a>Importar um ficheiro do Power BI Desktop

Pode importar um modelo de dados no ficheiro do Power BI Desktop (pbix) para o Azure Analysis Services. Metadados de modelo, dados em cache e ligações de origem de dados são importadas. Relatórios e visualizações não são importadas. Importar dados de modelos do Power BI Desktop são no nível de compatibilidade 1400.

**Restrições**   
- O modelo de pbix pode ligar à **base de dados do Azure SQL** e **Azure SQL Data Warehouse** apenas origens de dados. 
- O modelo de pbix não pode ter ao vivo ou ligações do DirectQuery. 
- Importação poderá falhar se o seu modelo de dados do pbix contém metadados não suportado no Analysis Services.

## <a name="to-import-from-pbix"></a>Para importar a partir do pbix

1. No seu servidor **descrição geral** > **Web designer**, clique em **aberto**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. Na **Web designer** > **modelos**, clique em **+ adicionar**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. Na **novo modelo**, escreva um nome de modelo e, em seguida, selecione o ficheiro do Power BI Desktop.

    ![Caixa de diálogo novo modelo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. Na **importação**, localize e selecione o seu ficheiro.

     ![Ligue-se a caixa de diálogo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Consulte também

[Criar um modelo no portal do Azure](analysis-services-create-model-portal.md)   
[Ligar ao Azure Analysis Services](analysis-services-connect.md)  
