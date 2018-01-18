---
title: "Monitorizar com a análise de registos do Azure de Surface Hubs | Microsoft Docs"
description: "Utilize a solução de Surface Hub para controlar o estado de funcionamento do seu Surface Hubs e compreender como estão a ser utilizados."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4f56369e412bdd285d3c370f5153fee4f539dfcf
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="monitor-surface-hubs-with-log-analytics-to-track-their-health"></a>Monitorizar Surface Hubs com a análise de registos para controlar o respetivo estado de funcionamento

![Símbolo de Hub de superfície](./media/log-analytics-surface-hubs/surface-hub-symbol.png)

Este artigo descreve como pode utilizar a solução de Surface Hub na análise de registos para monitorizar dispositivos Microsoft Surface Hub. Análise de registos ajuda-o a controlar o estado de funcionamento do seu Surface Hubs, bem como a compreender como estão a ser utilizados.

Cada Surface Hub tem o Microsoft Monitoring Agent instalada. O através do agente que pode enviar dados a partir do seu Surface Hub à análise de registos. Ficheiros de registo são lidos a partir do seu Surface Hubs e estão, em seguida, são enviados para a análise de registos. Problemas, como servidores estar offline, o calendário a sincronização não, ou se a conta de dispositivo não é possível iniciar sessão no Skype são apresentados no dashboard do Surface Hub na análise de registos. Ao utilizar os dados no dashboard, pode identificar dispositivos que não estão em execução, ou que estão a ter outros problemas e potencialmente aplicar correções para os problemas detetados.

## <a name="install-and-configure-the-solution"></a>Instalar e configurar a solução
Utilize as seguintes informações para instalar e configurar a solução. Para gerir o Surface Hubs na análise de registos, irá precisar do seguinte:

* A [subscrição de análise de registos](https://azure.microsoft.com/pricing/details/log-analytics/) nível que irão suportar o número de dispositivos que pretende monitorizar. Preços de análise do registo variam consoante o número de dispositivos inscritos e a quantidade de dados-processos. Poderá ser útil tem isto em consideração ao planear a sua implementação Surface Hub.

Em seguida, irá adicionar uma área de trabalho de análise de registos existente ou crie um novo. Instruções detalhadas para utilizando um dos métodos é [introdução à análise de registos](log-analytics-get-started.md). Depois de ter configurada a área de trabalho de análise de registos, existem duas formas de inscrever os dispositivos Surface Hub:

* Automaticamente através do Intune
* Manualmente através de **definições** no seu dispositivo Surface Hub.

## <a name="set-up-monitoring"></a>Configurar a monitorização
Pode monitorizar o estado de funcionamento e a atividade do seu Hub a superfície de através da análise do registo. Pode inscrever o Surface Hub, utilizando o Intune ou localmente ao utilizar **definições** no Surface Hub.

## <a name="connect-surface-hubs-to-log-analytics-through-intune"></a>Ligar de Surface Hubs ao Log Analytics através do Intune
Terá do ID da área de trabalho e a chave de área de trabalho para a área de trabalho de análise de registos que irão gerir a sua Surface Hubs. Pode obter os das definições da área de trabalho no portal do Azure.

O Intune é um produto Microsoft que lhe permite gerir centralmente as definições de configuração de análise de registos que são aplicadas a um ou mais dos seus dispositivos. Siga estes passos para configurar os seus dispositivos através do Intune:

1. Iniciar sessão no Intune.
2. Navegue para **definições** > **ligado origens**.
3. Criar ou editar uma política baseada no modelo de Surface Hub.
4. Navegue para a secção do OMS (informações operacionais do Azure) da política e adicionar a análise de registos *ID da área de trabalho* e *chave da área de trabalho* para a política.
5. Guarde a política.
6. Associe a política de grupo adequado de dispositivos.

   ![Política do Intune](./media/log-analytics-surface-hubs/intune.png)

Intune, em seguida, sincroniza-se as definições de análise de registos com os dispositivos no grupo de destino, inscrevendo-os na sua área de trabalho de análise de registos.

## <a name="connect-surface-hubs-to-log-analytics-using-the-settings-app"></a>Ligar Surface Hubs ao utilizar a aplicação de definições de análise de registos
Terá do ID da área de trabalho e a chave de área de trabalho para a área de trabalho de análise de registos que irão gerir a sua Surface Hubs. Pode obter os das definições para a área de trabalho de análise de registos no portal do Azure.

Se não utilizar o Intune para gerir o ambiente, pode inscrever dispositivos manualmente através de **definições** em cada Surface Hub:

1. A partir do seu Surface Hub, abra **definições**.
2. Introduza as credenciais de administrador do dispositivo quando lhe for pedido.
3. Clique em **este dispositivo**e em **monitorização**, clique em **configurar definições do OMS**.
4. Selecione **Ativar monitorização**.
5. Na caixa de diálogo de definições de OMS, escreva a análise de registos **ID da área de trabalho** e escreva o **chave da área de trabalho**.  
   ![settings](./media/log-analytics-surface-hubs/settings.png)
6. Clique em **OK** para concluir a configuração.

Uma confirmação é apresentado a informar se pretende ou não a configuração foi aplicada com êxito para o dispositivo. Se tiver sido, é apresentada uma mensagem a indicar que o agente ligado com êxito à análise de registos. O dispositivo, em seguida, começa a enviar dados para análise de registos, onde pode ver e atuar no mesmo.

## <a name="monitor-surface-hubs"></a>Monitor de Surface Hubs
Monitorização do seu Surface Hubs através da análise do registo são idêntica à monitorização de todos os outros dispositivos inscritos.

1. Inicie sessão no Portal do Azure.
2. Navegue até à sua área de trabalho de análise de registos e selecione **descrição geral**.
2. Clique no mosaico Surface Hub.
3. É apresentado o estado de funcionamento do seu dispositivo.

   ![Dashboard de Hub de superfície](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Pode criar [alertas](log-analytics-alerts.md) com base na procura de registo existentes ou personalizados. Utilizar os dados que recolhe de análise de registos do seu Surface Hubs, pode procurar problemas e alerta nas condições que definem para os seus dispositivos.

## <a name="next-steps"></a>Passos Seguintes
* Utilize [pesquisas de registo na análise de registos](log-analytics-log-searches.md) para ver os dados detalhados de Surface Hub.
* Criar [alertas](log-analytics-alerts.md) para ser notificado quando ocorrem problemas com o Surface Hubs.
