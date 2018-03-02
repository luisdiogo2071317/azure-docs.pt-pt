---
title: Importar um ficheiro do Power BI Desktop para Azure Analysis Services | Microsoft Docs
description: Descreve como importar um ficheiro do Power BI Desktop (pbix) utilizando o portal do Azure.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/26/2018
ms.author: owend
ms.openlocfilehash: 43eab587a1e5209069a248f1e2e1f57af158a2b8
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/27/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Importar um ficheiro do Power BI Desktop

Pode criar um novo modelo no Azure como ao importar um ficheiro de ficheiro (. pbix) do Power BI Desktop. Metadados do modelo, dados em cache e ligações de origem de dados são importadas. Relatórios e visualizações não foram importadas. Uma vez no seu servidor, é possível efetuar alterações de modelo, atualizar e voltar a importar o pbix através da funcionalidade de designer (pré-visualização) web no portal ou utilizando o SQL Server Management Studio (SSMS). Modelos importados não podem ser abertos ou exportados para o Visual Studio.

> [!NOTE]
> Se o seu modelo pbix liga-se a origens de dados no local, um [gateway no local](analysis-services-gateway.md) tem de ser configurado para o servidor.

## <a name="to-import-from-pbix"></a>Para importar a partir de pbix

1. No seu servidor **descrição geral** > **Web designer**, clique em **abra**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. No **Web designer** > **modelos**, clique em **+ adicionar**.

    ![Criar um modelo no portal do Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. No **novo modelo**, escreva um nome de modelo e, em seguida, selecione o ficheiro do Power BI Desktop.

    ![Caixa de diálogo novo modelo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. No **importação**, localize e selecione o ficheiro.

     ![Ligar a caixa de diálogo no portal do Azure](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Consulte também

[Criar um modelo no portal do Azure](analysis-services-create-model-portal.md)   
[Ligar ao Azure Analysis Services](analysis-services-connect.md)  
