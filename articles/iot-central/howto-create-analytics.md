---
title: Criar análise personalizada para a sua aplicação do Azure IoT Central | Microsoft Docs
description: Como um operador, como criar análise personalizada para a sua aplicação do Azure IoT Central.
services: iot-central
author: tanmaybhagwat
ms.author: tanmayb
ms.date: 04/16/2018
ms.topic: article
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: 2f19998429daab9000e35b520673c417b4bc828a
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="how-to-use-analytics-to-analyze-your-device-data"></a>Como utilizar a análise para analisar os dados de dispositivo

Microsoft Azure IoT Central fornece capacidades avançadas de análise para fazer sentido de grandes quantidades de dados dos seus dispositivos. Pode utilizar a análise para ver e analisar dados para um [conjunto dispositivo](howto-use-device-sets.md) na sua aplicação. Um conjunto de dispositivos é um grupo definido pelo utilizador dos seus dispositivos. Pode restringir a análise para um pequeno conjunto de dispositivos ou para um único dispositivo.

Como um operador, escolher **análise** no menu de navegação esquerdo, escolha um conjunto de dispositivos e, em seguida, escolha os valores para apresentar no gráfico. Seguem-se algumas ferramentas que pode utilizar para os dados do setor mais:

* **Valores:** escolher medidas para apresentar, tais como os relativos à temperatura e humidade.
* **Agregação:** escolha a função de agregação para os valores. Por exemplo, **mínimo** ou **médio**.
* **Divisão pela:** desagregar dividindo os dados por propriedades do dispositivo ou nome de dispositivo. Por exemplo, a dividir pela localização do dispositivo:

     ![Página principal da análise](media\howto-create-analytics\analytics-main.png)

* **Intervalo de tempo:** pode escolher o intervalo de tempo a partir de um dos intervalos de tempo predefinido ou criar um intervalo de tempo personalizada: ![intervalo de tempo de análise](media\howto-create-analytics\analytics-time-range.png)

## <a name="change-the-visualizations"></a>Altere as visualizações

Pode alterar os gráficos para satisfazer os seus requisitos de visualização, seleccionando um de três modos:

* **Empilhadas:** um gráfico para cada medida é empilhado e cada um dos gráficos ter os seus próprios eixo y. Gráficos empilhados são úteis quando tem várias medidas selecionadas e pretende ter vista distinta uma destas medidas.
* **Unstacked:** um gráfico para cada medida é representada contra um eixo y, mas os valores do eixo y são alterados com base na medida realçada. Gráficos unstacked são úteis quando pretende sobreposição várias medidas e pretende ver padrões entre estas medidas para o mesmo intervalo de tempo.
* **Eixo y partilhado:** todos os gráficos partilham o mesmo eixo y e não alteram os valores para o eixo. Gráficos de eixo y partilhados são úteis quando se pretende observar uma medida único ao repartir os dados com por divisão.

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como criar análise personalizada para a sua aplicação do Azure IoT Central, eis o passo seguinte sugerido:

> [!div class="nextstepaction"]
> [Preparar e ligue-se uma aplicação Node.js](howto-connect-nodejs.md)