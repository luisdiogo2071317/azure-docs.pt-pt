---
title: Perguntas mais frequentes sobre o projeto Acoustics
titlesuffix: Azure Cognitive Services
description: Esta página fornece respostas às perguntas mais frequentes com frequência sobre Acoustics de projeto, incluindo instruções para download e inserir o processo.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 07e65e6ff544d6372197010a2b9d7f3f647eeb0f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55166565"
---
# <a name="frequently-asked-questions"></a>Perguntas mais frequentes

## <a name="what-is-project-acoustics"></a>O que é o Projeto Acoustics?

O plug-in do projeto Acoustics Unity é um sistema de acoustics que calcula o comportamento de som wave antes do tempo de execução, semelhante a iluminação estático. A cloud faz o trabalho pesado de onda computações de física, para que o custo de tempo de execução da CPU é baixo.  

## <a name="where-can-i-download-the-plugin"></a>Onde posso transferir o plug-in?

Se tiver interesse em avaliar o plug-in de acústica, registe-se [aqui](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) para aderir à Pré-visualização do Designer.

## <a name="is-azure-used-at-runtime"></a>O Azure é utilizado no tempo de execução?

Não, integração na cloud é utilizada apenas durante a fase de precompute como parte da configuração da cena.
 
## <a name="what-is-simulation-input"></a>O que é a entrada de simulação? 

A entrada de simulação é sua cena 3D, o ambiente virtual ou o nível de jogo. Projeto Acoustics executa simulações de onda volumetric 3D que modelam a parte física do som com atenção, incluindo oclusão uniforme e scattering.
 
## <a name="what-is-the-runtime-cost"></a>O que é o custo de tempo de execução?

Acoustics demora aproximadamente 0,01% de CPU por origem por quadro. Utilização de RAM depende do tamanho da cena e pode variar entre 10 e 100 MB.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>É necessário simplificar a geometria de nível? Contagem de triângulo do controle? Tornar as malhas watertight?

Não. O sistema será ingerir geometry nível detalhada diretamente. Será voxelized para processamento interno.
 
## <a name="whats-in-the-runtime-lookup-table"></a>O que inclui a tabela de pesquisa de tempo de execução?

O ficheiro ACE é uma tabela de parâmetros acústicos entre vários pares de localização de origem e o serviço de escuta.
 
## <a name="can-it-handle-moving-sources"></a>Pode processar mover origens?

Sim, o **Microsoft Acoustics** Plug-in do Unity spatializer consulta a tabela de pesquisa em cada marca de processamento de áudio com as localizações de origem e o serviço de escuta atuais. O spatializer DSP suavemente atualiza os parâmetros de processamento acústico em todas as marcas.
 
## <a name="can-it-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>Ele processa geometria dinâmica? Fechar portas? Paredes hoje mesmo?

Não. Os parâmetros acústicos são calculados com base no estado estático de um nível de jogo. Podemos sugerir deixando a geometria de porta fora acoustics e, em seguida, aplicar oclusão adicional com base no estado de objetos do jogo destructible e movível usando técnicas estabelecidas.
 
## <a name="does-it-handle-materials"></a>Processar materiais?

Sim. Materiais são escolhidos dos físicos materiais nomes no seu nível, orientando absorptivity.
 
## <a name="what-do-the-probes-represent"></a>O que representam as "sondas"?

As pesquisas são uma amostragem dos locais de player possíveis. Cada pesquisa representa uma simulação de onda separado da cena com origem no local sonda. No tempo de execução, acústicos parâmetros para a localização de serviço de escuta são interpolados a partir próximos localizações da sonda.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>Por que gastar muito computação na cloud? O que isso compre-me?

Projeto Acoustics fornece parâmetros acústicos precisos e confiáveis mesmo para ambientes virtuais complexos Ultra, tendo cada aspecto da arquitetura em conta. Ele fornece smooth oclusão/obstrução sem todo o trabalho manual e variação de reverberação dinâmica sem volumes de desenho. Enquanto o restante uma luz sobre CPU durante o tempo de execução.

## <a name="what-exactly-happens-during-baking"></a>Exatamente o que acontece durante a "implantando"?

O sistema considera potenciais localizações de player para gerar um conjunto de posições de exemplo "pesquisa" espaçadas uniformemente. Um criar para um nível de consiste em tarefas independentes para cada sonda: O sistema considera um cuboid "Simulação região" centralizado na sonda e faz uma simulação de onda de detalhado dentro dessa região em até 25 resolução do cm.

## <a name="next-steps"></a>Próximos Passos
* Explorar o [cena de exemplo](sample-walkthrough.md)

