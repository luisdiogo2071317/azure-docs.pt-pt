---
title: Visão geral do processo de design de projeto Acoustics
titlesuffix: Azure Cognitive Services
description: Este documento descreve como expressar sua intenção de design em todas as três fases do fluxo de trabalho Acoustics do projeto.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 7c0a48c22e6fe1d1904771915aef401ffa58d588
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55152542"
---
# <a name="design-process-overview"></a>Descrição geral do processo de design
Pode expressar sua intenção de design em todas as três fases do fluxo de trabalho do projeto Acoustics: pré-inserir instalação de cena, Colocação de origem de som e design de pós-criar. O processo exige menos marcação associada com a colocação de reverberação volumes, mantendo o controle de designer sobre como pode parecer uma cena.

## <a name="pre-bake-design"></a>Design de pré-criar
O processo de configuração de pré-criar cena produz a cena e os metadados que são utilizados para a simulação de som wave, que inclui a seleção que elementos de cena irão participar na simulação para fornecer occlusions, reflexo e reverberation. Os metadados para a cena são a seleção de materiais acústicos para cada elemento de cena. Os materiais acústicos controlam a quantidade de energia som refletida novamente a partir de cada superfície.

O coeficiente de absorption padrão para todas as superfícies é 0.04, que é altamente reflexiva. Pode conseguir efeitos estética e jogabilidade ajustando os coeficientes absorption de materiais diferentes em toda a cena, que são especialmente importantes para serviços de escuta quando elas ouvem as transições de uma área da cena para outro. Por exemplo, fazer a transição de uma sala reverberant escura para um brilhante, que não reverberant cena de equipamentos esportivos aprimora o impacto da transição. Para obter este efeito, Otimize os coeficientes absorption nos materiais a cena de equipamentos esportivos superior.

O tempo de reverberation de um determinado material numa sala inversamente está relacionado ao seu coeficiente de absorption, com a maioria dos materiais ter valores absorption no intervalo 0,01 para 0,20. Materiais com coeficientes absorption fora deste intervalo são muito absorbent.

![Gráfico de tempos de reverberação](media/ReverbTimeGraph.png)

O [criar a interface do Usuário percorrer](bake-ui-walkthrough.md) descreve os controles de pré-criar em detalhes.

## <a name="sound-source-placement"></a>Colocação de origem de som
Visualizar voxels e sonda pontos no tempo de execução pode ajudar a depurar problemas com as origens de som a ser bloqueadas dentro da geometria voxelized. Para ativar/desativar a grade de voxel e pesquisa de pontos de apresentação, clique na caixa de verificação correspondente no menu Gizmos, no canto superior direito da vista de cena. Se a origem de som é dentro de um voxel de parede, mova-o para um voxel de ar.

![Menu de gizmos](media/GizmosMenu.png)  

A exibição de voxel pode ajudar a determinar se os componentes visuais no jogo tem uma transformação aplicada às mesmas. Se assim for, aplicam-se a mesma transformação para a hospedagem de GameObject a **Acoustics Manager**.

### <a name="voxel-size-discrepancies"></a>Voxel discrepâncias de tamanho
Poderá reparar que o tamanho do voxels usados para ilustrar o que, da cena, as malhas participam na criar acoustics é diferente nas vistas de tempo de execução e de tempo de design. Essa diferença não afeta a qualidade/granularidade de sua frequência de simulação selecionado, mas é bastante um biproduct do uso de tempo de execução da cena voxelized. No tempo de execução as voxels de simulação são "refinados" para suportar a interpolação entre localizações de origem. Isto também permite design tempo posicionamento de som origens mais próximo para as malhas de cena de que o tamanho de voxel simulação permite – uma vez que as origens dentro de um voxel que contêm uma malha acoustically treated não fazem qualquer som.

Seguem-se duas imagens que mostra a diferença entre voxels de design (pré-criar) e voxels de tempo de execução (pós-criar) como visualizado pelo plug-in do Unity:

Voxels de tempo de design:

![VoxelsDesignTime](media/VoxelsDesignTime.png)

Voxels de tempo de execução:

![VoxelsRuntime](media/VoxelsRuntime.png)

A decisão sobre se é ou não a malha de voxel representa com precisão as malhas de cena de arquitetura/estrutural deve ser feita usando o voxels de modo de design, não a visualização de tempo de execução do voxels refinados.

## <a name="post-bake-design"></a>Design de pós-criar
Criar resultados são armazenados no ficheiro de ACE como parâmetros oclusão e reverberation para todos os pares de localização de serviço de escuta de origem em toda a cena. Este resultado fisicamente preciso pode ser utilizado para o seu projeto como-é, e é um ótimo ponto de partida para o design. O processo de design de pós-criar especifica regras para transformar os parâmetros de resultado de criar no tempo de execução.

### <a name="distance-based-attenuation"></a>Com base na distância atenuação
O áudio DSP fornecida pelos **Microsoft Acoustics** Plug-in do Unity spatializer respeita a atenuação de com base na distância do código-fonte criada no Editor do Unity. Controlos de atenuação com base na distância estão no **origem de áudio** componente encontrado no **Inspetor** origens de painel de som, em **definições de som 3D**:

![Atenuação de distância](media/distanceattenuation.png)

Acoustics executa o cálculo numa caixa "região de simulação" centrada a localização de player. Se uma origem de som é distante do jogador, localizado fora desta região de simulação, apenas a geometria na caixa de afetará a propagação de som (por exemplo, fazendo com que oclusão) que funciona razoavelmente bem quando occluders estão perto o jogador. No entanto, em casos quando o jogador está no espaço aberto, mas os occluders estão próximos da origem de som distante, o som pode se tornar inacreditavelmente disoccluded. Nossa solução sugerida é Certifique-se nestes casos que a atenuação de som recai como 0 em cerca de 45m, a distância de horizontal padrão do player na periferia da caixa.

### <a name="tuning-scene-parameters"></a>Ajuste os parâmetros de cenas
Para ajustar os parâmetros para todas as origens, clique na faixa do canal do Unity **Mixer de áudio**e ajustar os parâmetros na **Acoustics Mixer** efeito.

![Personalização do Mixer](media/MixerParameters.png)

### <a name="tuning-source-parameters"></a>Ajuste os parâmetros de origem
A anexar o **AcousticsAdjust** script para uma origem permite parâmetros de ajuste para essa origem. Para anexar o script, clique em **Add Component** na parte inferior dos **Inspetor** painel e navegue para **Scripts > Acoustics ajustar**. O script tem seis controles:

![AcousticsAdjust](media/AcousticsAdjust.png)

* **Ativar Acoustics** -controla se acoustics se aplica a esta origem. Quando desmarcado, a origem será spatialized com HRTFs, mas sem acoustics, ou seja, sem obstrução, oclusão e parâmetros de reverberation dinâmica, como o tempo de nível e Win32/decay. Reverberation ainda está a ser aplicada com um nível fixo e tempo de Win32/decay.
* **Oclusão** -aplicar um multiplicador para o nível de dB oclusão calculado pelo sistema acoustics. Se este multiplicador for superior a 1, irá ser exagerada oclusão, enquanto valores inferior a 1 fazer o efeito de oclusão mais sutil e um valor de 0 desativa oclusão.
* **Transmissão (dB)** -definir a atenuação (no dB) causada por transmissão por meio de geometria. Defina este controlo de deslize para o nível mais baixo para desativar a transmissão. Acoustics spatializes o áudio de dry inicial como que chegam em torno de geometria da cena (portaling). Transmissão fornece uma chegada de dry adicional que é spatialized na direção de linha de visão. Tenha em atenção que a curva de atenuação de distância para a origem também é aplicada.
* **Umidade (dB)** -ajusta o poder de reverberação no dB, de acordo com a distância da origem. Valores positivos emitir um som mais reverberant, enquanto valores negativos emitir um som mais dry. Clique no controle de curva (linha verde) para abrir o editor de curva. Modificar a curva left-clicking para adicionar pontos e arrastando esses pontos para formar a função que pretende. O eixo x é a distância de origem e o eixo y é o ajuste de reverberação no dB. Ver isso [Manual do Unity](https://docs.unity3d.com/Manual/EditingCurves.html) para obter mais detalhes sobre a edição de curvas. Repor a curva predefinido, clique em **Umidade** e selecione **repor**.
* **Win32/decay escala de tempo** -ajusta um multiplicador para a hora de Win32/decay. Por exemplo, se o resultado de criar Especifica um tempo de Win32/decay de 750 milissegundos, mas este valor é definido como 1,5, o tempo de Win32/decay aplicado para a origem é 1.125 milissegundos.
* **Outdoorness** -um ajuste aditiva na estimativa do sistema acoustics do como "pessoas" reverberation numa origem deve parecer. Definir este para 1 fará com que uma origem sempre som completamente pessoas, durante a definição-como -1 fará com que uma origem de som indoors.

Origens diferentes podem requerer diferentes configurações alcançar certos efeitos estética ou jogo. Caixa de diálogo é um exemplo de possíveis. O ouvido humano é mais attuned para reverberation na conversão de voz, enquanto a caixa de diálogo, muitas vezes, tem de ser inteligível para o jogo. Pode justificar isso sem ter que fazer a caixa de diálogo não-diegetic ao mover o **Umidade** baixo, ajustar a **Warp de distância Percetual** parâmetro descrito a seguir, adicionando alguns  **Transmissão** para alguns boost áudio dry se propaguem por intermédio de paredes de e/ou reduzir a **oclusão** de 1 para ter mais som chegam através de portais.

A anexar o **AcousticsAdjustExperimental** script para uma origem permite parâmetros de ajuste experimentais adicionais para essa origem. Para anexar o script, clique em **Add Component** na parte inferior dos **Inspetor** painel e navegue para **Scripts > Acoustics ajustar Experimental**. Atualmente, existe um controlo experimental:

![AcousticsAdjustExperimental](media/AcousticsAdjustExperimental.png)

* **Warp de distância percetual** -aplicar um distorcendo exponencial para a distância usada para calcular o rácio de dry wet. O sistema de acoustics calcula wet níveis por todo o espaço, que variam sem problemas com a distância e proporcionam indicações de percepção de distância. Distorcendo valores maiores que 1 exaggerate esse efeito, aumentando os níveis de reverberation relacionados com a distância, tornando o som "distantes", enquanto distorcendo valores marca menos de 1 a alteração com base na distância reverberation mais sutil, tornando o som mais "apresentam".

