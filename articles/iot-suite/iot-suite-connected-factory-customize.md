---
title: "Personalizar a solução de fábrica ligado - Azure | Microsoft Docs"
description: "Uma descrição de como personalizar o comportamento da fábrica de ligado a solução pré-configurada."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2017
ms.author: dobett
ms.openlocfilehash: 48c8036d0bc9534ce94529b96d32b004769246c1
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Personalizar a forma como a solução de fábrica ligado apresenta dados dos seus servidores de OPC UA

A solução de fábrica ligado agrega e apresenta os dados dos servidores de OPC UA ligados à solução. Pode procurar e enviar comandos para os servidores de OPC UA na sua solução. Para obter mais informações sobre OPC UA, veja as [FAQ sobre a fábrica ligada](iot-suite-faq-cf.md).

Exemplos de dados agregados na solução incluem a eficiência global de equipamento (OEE) e os indicadores de desempenho de chave (KPIs) que pode ver no dashboard de fábrica, linha e níveis de estação. A seguinte captura de ecrã mostra os valores OEE e KPI para a **assemblagem** estação no **produção linha 1**, no **Munich** factory:

![Exemplo de valores OEE e KPI na solução][img-oee-kpi]

A solução permite-lhe ver informações detalhadas de itens de dados específico de servidores OPC UA, denominados *estações*. A seguinte captura de ecrã mostra rastreia do número de itens fabricados de uma estação específica:

![Plots do número de itens fabricados][img-manufactured-items]

Se clicar em um dos gráficos, pode explorar os dados, utilizando as informações de séries de tempo (TSI):

![Explore os dados através das informações de séries de tempo][img-tsi]

Este artigo descreve:

- Como os dados são disponibilizados para as várias vistas na solução.
- Como pode personalizar a forma como a solução mostra os dados.

## <a name="data-sources"></a>Origens de dados

A solução de fábrica ligado apresenta os dados dos servidores de OPC UA ligados à solução. A instalação predefinida inclui várias OPC UA servidores que executam uma simulação de fábrica. Pode adicionar os seus próprios servidores OPC UA que [ligar através de um gateway] [ lnk-connect-cf] à sua solução.

Pode procurar os itens de dados que pode enviar um servidor de OPC UA ligado à sua solução no dashboard:

1. Escolha **Browser** para navegar para o **selecione um servidor de OPC UA** vista:

    ![Navegue para selecionar uma vista do servidor de OPC UA][img-select-server]

1. Selecione um servidor e clique em **Connect**. Clique em **continuar** quando aparece o aviso de segurança.

    > [!NOTE]
    > Este aviso só é apresentada uma vez para cada servidor e estabelece uma relação de confiança entre o dashboard de solução e o servidor.

1. Pode procurar os itens de dados que o servidor pode enviar para a solução. Os itens que estão a ser enviados para a solução têm uma marca de verificação:

    ![Itens publicados][img-published]

1. Se for um *administrador* na solução, pode optar por publicar um item de dados para o disponibilizar na solução de fábrica ligado. Como administrador, também pode alterar o valor dos itens de dados e chamar os métodos no servidor de OPC UA.

## <a name="map-the-data"></a>Os dados do mapa

A solução de fábrica ligado mapeia e agrega os itens de dados publicados do servidor de OPC UA para as várias vistas na solução. Implementa a solução de fábrica ligado à sua conta do Azure, quando Aprovisiona a solução. Um ficheiro JSON na solução de fábrica ligado do Visual Studio armazena estas informações de mapeamento. Pode ver e modificar este ficheiro de configuração JSON na fábrica de ligado solução do Visual Studio. Pode implementar a solução pode ser novamente depois de efetuar uma alteração.

Pode utilizar o ficheiro de configuração para:

- Edite as fábricas de simulada existentes, linhas de produção e estações.
- Mapear dados dos servidores de OPC UA reais que ligam à solução.

Para obter mais informações sobre o mapeamento e agregar os dados para satisfazer os seus requisitos específicos, consulte [solução pré-configurada de como configurar a fábrica ligada ](iot-suite-connected-factory-configure.md).

## <a name="deploy-the-changes"></a>Implementar as alterações

Quando terminar de efetuar as alterações para o **ContosoTopologyDescription.json** ficheiro, é necessário voltar a implementar a solução de fábrica ligado à sua conta do Azure.

O **do azure-iot-ligado-factory** repositório inclui um **build.ps1** script do PowerShell que pode utilizar para reconstruir e implementar a solução.

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre a solução de fábrica ligado pré-configurada ao ler os artigos seguintes:

* [Connected factory preconfigured solution walkthrough][lnk-rm-walkthrough] (Instruções da solução pré-configurada de fábrica de dados)
* [Implementar um gateway para a fábrica ligado][lnk-connect-cf]
* [Permissões no site azureiotsuite.com][lnk-permissions]
* [FAQ de fábrica ligada](iot-suite-faq-cf.md)
* [FAQ][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-v1-permissions.md
[lnk-faq]: iot-suite-v1-faq.md