---
title: Instalar as Ferramentas do Azure Data Lake para Visual Studio
description: Este artigo descreve como instalar as ferramentas do Azure Data Lake para Visual Studio.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 05/03/2018
ms.openlocfilehash: c520c437212c23cc9dc8327c95b9f2a77b08e1ac
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34622875"
---
# <a name="install-data-lake-tools-for-visual-studio"></a>Instalar as Ferramentas do Data Lake para Visual Studio

Saiba como utilizar o Visual Studio para criar contas do Azure Data Lake Analytics, definir tarefas em [U-SQL](data-lake-analytics-u-sql-get-started.md) e submeter tarefas para o serviço Data Lake Analytics. Para mais informações sobre a Data Lake Analytics, consulte [Descrição geral da Análise do Azure Data Lake](data-lake-analytics-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

* **Visual Studio**: são suportadas todas as edições exceto a Express.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* Versão 2.7.1 ou superior do **Microsoft Azure SDK para .NET**.  Instale-a através do [instalador de plataforma Web](http://www.microsoft.com/web/downloads/platform.aspx).
* A conta do **Data Lake Analytics**. Para criar uma conta, consulte [Introdução ao Azure Data Lake Analytics com o portal do Azure](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio-2017"></a>Instalar as Ferramentas do Azure Data Lake para Visual Studio 2017

As ferramentas do Azure Data Lake para Visual Studio são suportadas no Visual Studio 2017 15.3 ou superior. Esta ferramenta faz parte das cargas de trabalho de **Armazenamento e processamento de dados** e de **Desenvolvimento do Azure** no Instalador do Visual Studio. Ative uma destas duas cargas de trabalho como parte da sua instalação do Visual Studio.  

Ative a carga de trabalho de **Armazenamento e processamento de dados** da seguinte forma: ![Ativar a carga de trabalho de armazenamento e processamento de dados](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-01.png)

Ative a carga de trabalho de **Desenvolvimento do Azure** da seguinte forma: ![Ativar a carga de trabalho de desenvolvimento do Azure](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-tools-for-vs-2017-install-02.png)

## <a name="install-azure-data-lake-tools-for-visual-studio-2013-and-2015"></a>Instalar as Ferramentas do Azure Data Lake para Visual Studio 2013 e 2015

Transfira e instale as Ferramentas do Azure Data Lake para Visual Studio [a partir do centro de transferências](http://aka.ms/adltoolsvs). Após a instalação, tenha em atenção que:
* O nó do **Server Explorer** > **Azure** contém um nó do **Data Lake Analytics**. 
* O menu **Ferramentas** inclui um item do **Data Lake**.


## <a name="next-steps"></a>Próximos Passos
* Para obter informações de diagnóstico de registo, veja [Accessing diagnostics logs for Azure Data Lake Analytics (Aceder aos registos de diagnóstico do Azure Data Lake Analytics)](data-lake-analytics-diagnostic-logs.md)
* Para ver uma consulta mais complexa, consulte [Analisar registos de site através da Análise do Azure Data Lake](data-lake-analytics-analyze-weblogs.md).
* Para utilizar a vista de execução de vértice, consulte [utilize a vista de execução de vértice ferramentas do Data Lake para Visual Studio](data-lake-analytics-data-lake-tools-use-vertex-execution-view.md)
