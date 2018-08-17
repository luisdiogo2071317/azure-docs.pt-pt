---
title: Visão geral do processo de design de acoustics - serviços cognitivos
description: Este documento descreve como expressar sua intenção de design em todas as três fases do fluxo de trabalho Acoustics do projeto.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 3b35f78d66a6ae66b0a56818f5d4be455ce00de5
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181693"
---
# <a name="design-process-overview"></a>Descrição geral do processo de design
Pode expressar sua intenção de design em todas as três fases do fluxo de trabalho do projeto Acoustics: Inserir previamente o design, o posicionamento de som de origem e o design de pós-criar. O processo exige menos marcação associada com a colocação de reverberação volumes, mantendo o controle de designer sobre como pode parecer uma cena.

## <a name="pre-bake-design"></a>Design de pré-criar
O processo de design de pré-criar produz a cena e os metadados que são utilizados para a simulação de som wave, que inclui a seleção que elementos de cena irão participar na simulação para fornecer occlusions, reflexo e reverberation. Os metadados para a cena são a seleção de materiais acústicos para cada elemento de cena. Os materiais acústicos controlam a quantidade de energia som refletida novamente a partir de cada superfície.

O coeficiente de absorption padrão para todas as superfícies é 0.04, que é altamente reflexiva. Pode conseguir efeitos estética e jogabilidade ajustando os coeficientes absorption de materiais diferentes em toda a cena, que são especialmente importantes para serviços de escuta quando elas ouvem as transições de uma área da cena para outro. Por exemplo, fazer a transição de uma sala reverberant escura para um brilhante, que não reverberant cena de equipamentos esportivos aprimora o impacto da transição. Para obter este efeito, Otimize os coeficientes absorption nos materiais a cena de equipamentos esportivos superior.

O tempo de reverberation de um determinado material numa sala inversamente está relacionado ao seu coeficiente de absorption, com a maioria dos materiais ter valores absorption no intervalo 0,01 para 0,20. Materiais com coeficientes absorption fora deste intervalo são muito absorbent.

![Gráfico de tempos de reverberação](media/ReverbTimeGraph.png)

O [criar a interface do Usuário percorrer](bake-ui-walkthrough.md) descreve os controles de pré-criar em detalhes.

## <a name="sound-source-placement"></a>Colocação de origem de som
Visualizar voxels e sonda pontos no tempo de execução pode ajudar a depurar problemas com as origens de som a ser bloqueadas dentro da geometria voxelized. Para ativar/desativar a grade de voxel e pesquisa de pontos de apresentação, clique na caixa de verificação correspondente no menu Gizmos, no canto superior direito da vista de cena. Se a origem de som é dentro de um voxel de parede, mova-o para um voxel de ar.

![Menu de gizmos](media/GizmosMenu.png)  

A exibição de voxel pode ajudar a determinar se os componentes visuais no jogo tem uma transformação aplicada às mesmas. Se assim for, aplicam-se a mesma transformação para a hospedagem de GameObject a **Acoustics Manager**.

## <a name="post-bake-design"></a>Design de pós-criar
Criar resultados são armazenados no ficheiro de ACE como parâmetros oclusão e reverberation para todos os pares de localização de serviço de escuta de origem em toda a cena. Este resultado fisicamente preciso pode ser utilizado para o seu projeto como-é, e é um ótimo ponto de partida para o design. O processo de design de pós-criar especifica regras para transformar os parâmetros de resultado de criar no tempo de execução.

### <a name="distance-based-attenuation"></a>Com base na distância atenuação
O áudio DSP fornecida pelos **Microsoft Acoustics** Plug-in do Unity spatializer respeita a atenuação de com base na distância do código-fonte criada no Editor do Unity. Controlos de atenuação com base na distância estão no **origem de áudio** componente encontrado no **Inspetor** origens de painel de som, em **definições de som 3D**:

![Atenuação de distância](media/distanceattenuation.png)

### <a name="tuning-scene-parameters"></a>Ajuste os parâmetros de cenas
Para ajustar os parâmetros para todas as origens, clique na faixa do canal do Unity **Mixer de áudio**e ajustar os parâmetros na **Acoustics Mixer** efeito.

![Personalização do Mixer](media/MixerParameters.png)

### <a name="tuning-source-parameters"></a>Ajuste os parâmetros de origem
A anexar o **AcousticsSourceCustomization** script para uma origem permite parâmetros de ajuste para essa origem. Para anexar o script, clique em **Add Component** na parte inferior dos **Inspetor** painel e navegue para **Scripts > Acoustics origem personalização**. O script tem três parâmetros:

![Personalização de origem](media/SourceCustomization.png)

* **Reverberação Power ajustar** -ajusta o poder de reverberação no dB. Valores positivos emitir um som mais reverberant, enquanto valores negativos emitir um som mais dry.
* **Win32/decay escala de tempo** -ajusta um multiplicador para a hora de Win32/decay. Por exemplo, se o resultado de criar Especifica um tempo de Win32/decay de 750 milissegundos, mas este valor é definido como 1,5, o tempo de Win32/decay aplicado para a origem é 1.125 milissegundos.
* **Ativar Acoustics** -controla se acoustics se aplica a esta origem. Quando desmarcado, a origem será spatialized com HRTFs, mas sem acoustics, ou seja, sem obstrução, oclusão e parâmetros de reverberation dinâmica, como o tempo de nível e Win32/decay. Reverberation ainda está a ser aplicada com um nível fixo e tempo de Win32/decay.

Origens diferentes podem requerer diferentes configurações alcançar certos efeitos estética ou jogo. Caixa de diálogo é um exemplo de possíveis. O ouvido humano é mais attuned para reverberation na conversão de voz, enquanto a caixa de diálogo, muitas vezes, tem de ser inteligível para o jogo. Pode justificar isso sem ter que fazer a caixa de diálogo não-diegetic ao ajustar o poder de reverberação baixo.
