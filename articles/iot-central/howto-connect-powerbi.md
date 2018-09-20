---
title: Visualize os seus dados do Azure IoT Central no dashboard do Power BI | Documentos da Microsoft
description: Utilize o modelo de solução do Azure IoT Central Analytics Power BI para visualizar e analisar os seus dados de IoT Central.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: 6f8bed8aba43d77cdfad49f4fe62a4c7aa2c5ce3
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368074"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Visualize e analise os dados do Azure IoT Central no dashboard do Power BI

*Este tópico aplica-se aos administradores.*

![Pipeline de modelo de solução do Power BI](media/howto-connect-powerbi/iot-continuous-data-export.png)

Utilize o modelo de solução do Azure IoT Central Analytics Power BI para criar um dashboard do Power BI poderoso para monitorizar o desempenho dos seus dispositivos IoT. No dashboard do Power BI, pode:
- Controlar a quantidade de dados estão a enviar seus dispositivos ao longo do tempo
- Comparar o volume de dados entre Estados, quantidades de telemetria e eventos
- Identificar dispositivos que estão a enviar relatórios inúmeras medições
- Observe as tendências históricas derivadas de medidas de dispositivo
- Identificar dispositivos problemáticos que enviar muitos eventos críticos

Este modelo de solução, configura o pipeline que utiliza os dados na sua conta de armazenamento de Blobs do Azure partir [exportação de dados contínua](howto-export-data.md). Estes dados fluem através das funções do Azure, Azure Data Factory e SQL Database do Azure para processar e transformar os dados. A saída pode ser visualizada e analisada no relatório do Power BI que pode baixar como um ficheiro PBIX. Todos esses recursos são criados na sua subscrição do Azure, pelo que pode personalizar cada componente de acordo com as suas necessidades. Este modelo de solução é totalmente aberto, para que possa saber mais sobre a arquitetura e expandir a solução ao visitar a [repositório do GitHub](https://aka.ms/iotcentralgithubpowerbisolutiontemplate).

## <a name="get-the-azure-iot-central-analytics-solution-templatehttpsakamsiotcentralpowerbisolutiontemplate-from-microsoft-appsource"></a>Obter o [modelo de solução do Azure IoT Central Analytics](https://aka.ms/iotcentralpowerbisolutiontemplate) do Microsoft AppSource

## <a name="prerequisites"></a>Pré-requisitos
Configurar o modelo necessita do seguinte:
- Acesso a uma subscrição do Azure
- Exportar dados com [exportação de dados contínua](howto-export-data.md) da sua aplicação IoT Central. Recomendamos que ative medidas, dispositivos e fluxos de modelo do dispositivo para aproveitar ao máximo o dashboard do Power BI.
- Power BI Desktop (versão mais recente)
- Power BI Pro (se quiser partilhar o dashboard com outras pessoas)

## <a name="reports"></a>Relatórios

Dois relatórios são gerados automaticamente. 

O primeiro relatório mostra uma vista histórica das medidas de reportados pelos dispositivos e divide os diferentes tipos de medidas e dispositivos que enviaram o maior número de medidas.

![Página de relatório do Power BI 1](media/howto-connect-powerbi/template-page1-hasdata.PNG)

O segundo relatório se aprofunda-se nos eventos e mostra uma vista histórica dos erros e avisos relatados. Ela também mostra quais os dispositivos que comunicam o maior número de eventos todos juntos, bem como especificamente os eventos de erro e eventos de aviso.

![Página de relatório do Power BI 2](media/howto-connect-powerbi/template-page2-hasdata.PNG)

## <a name="resources"></a>Recursos

Visite o AppSource para obter o [modelo de solução do Azure IoT Central Analytics](https://aka.ms/iotcentralpowerbisolutiontemplate).

Visite o [repositório do GitHub](https://aka.ms/iotcentralgithubpowerbisolutiontemplate) para saber mais sobre a arquitetura e expandir a solução.

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como visualizar os seus dados no Power BI, este é o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Como gerir dispositivos](howto-manage-devices.md)
