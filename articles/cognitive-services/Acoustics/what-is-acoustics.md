---
title: O que é o Projeto Acoustics?
titlesuffix: Azure Cognitive Services
description: O plug-in Projeto Acoustics fornece oclusão, reverberação e espacialização para os projetos direcionados para RV e ecrãs tradicionais.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.component: acoustics
ms.topic: overview
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 6a3c97ead40cfb24604edac0624e38a9b0713fc0
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901029"
---
# <a name="what-is-project-acoustics"></a>O que é o Projeto Acoustics?
O plug-in Projeto Acoustics fornece oclusão, reverberação e espacialização para os projetos direcionados para RV e ecrãs tradicionais. Fornece uma forma de criar acústica para jogos que cria camadas para as intenções do designer através de uma simulação de ondas baseada em física.

## <a name="why-use-acoustics-in-virtual-environments"></a>Por quê utilizar acústica em ambientes virtuais?
Os seres humanos utilizam indicações audiovisuais para compreender o mundo à sua volta. Nos mundos virtuais, a combinação de áudio espacial com a acústica aumenta a imersão do utilizador. A ferramenta de acústica descrita aqui analisa os mundos virtuais para criar uma simulação acústica realista e suporta um processo de design de pós-simulação. A análise inclui a geometria e os materiais para cada superfície do mundo. A simulação inclui parâmetros como a direção de chegada (criação de portais), poder de reverberação, tempos de decadência e efeitos de oclusão e obstrução.

## <a name="how-does-this-approach-to-acoustics-work"></a>Como funciona esta abordagem relativamente à acústica?
O sistema baseia-se numa computação offline do mundo virtual, que permite uma simulação mais complexa do que se a análise fosse feita durante o tempo de execução. A computação offline produz uma tabela de referência de parâmetros acústicos. Um designer especifica as regras aplicadas aos parâmetros durante o tempo de execução. A otimização destas regras permite alcançar efeitos hiper-realistas para cenas de grande intensidade emocional ou hipo-realistas para mais sons de áudio de fundo.

## <a name="design-process-comparison"></a>Comparação do processo de design
O plug-in Projeto Acoustics suporta um novo processo de design para acústica em cenas do Unity. Para explicar este novo processo de design, vamos compará-lo a uma das abordagens populares atuais referente à acústica.

### <a name="typical-approach-to-acoustics-today"></a>Abordagem típica atual para acústica
Numa abordagem típica para acústica hoje em dia, desenha volumes de reverberação:

![Vista de design](media/reverbZonesAltSPace2.png)

Em seguida, otimiza os parâmetros para cada zona:

![Vista de design](media/TooManyReverbParameters.png)

Por fim, adicione lógica de traçado de raios, para obter os filtros de oclusão e obstrução adequados em toda a cena, e lógica de pesquisa de caminhos para criação de portais. Este código pode adicionar um custo de tempo de execução. Tem também problemas com a suavidade dos cantos e casos extremos em cenas com formas irregulares.

### <a name="an-alternative-approach-with-physics-based-design"></a>Abordagem alternativa com design baseado em física
Com a abordagem fornecida pelo plug-in Projeto Acoustics do Unity, fornece formas e materiais estáticos à cena. Uma vez que a cena é voxelizada e o processo não utiliza o traçado de raios, não tem de fornecer uma malha acústica simplificada ou estanque. Também não é necessário marcar a cena com volumes de reverberação. O plug-in carrega a cena para a cloud, onde utiliza a simulação de ondas baseada em física. O resultado é integrado no seu projeto como uma tabela de referência e pode ser modificado para efeitos de estética ou jogabilidade.

![Vista de design](media/GearsWithVoxels.jpg)

## <a name="requirements"></a>Requisitos
* Unity 2018.2+ para acústica e Unity 5.2+ para design e implementação de som
* Unity Editor para Windows de 64 bits
* Subscrição do Azure Batch para acústica
* O tempo de execução de scripts do Unity tem de ser definido como "equivalente a .NET 4.x"

## <a name="platform-support"></a>Suporte da plataforma
* Ambiente de trabalho do Windows (x86 e AMD64)
* Windows UWP (x86, AMD64 e ARM)
* Android (x86 e ARM64)

## <a name="download"></a>Transferência
Se tiver interesse em avaliar o plug-in de acústica, registe-se [aqui](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) para aderir à Pré-visualização do Designer.

## <a name="next-steps"></a>Passos seguintes
* Saiba mais sobre o [processo de design](design-process.md)
* Introdução à [integração de acústica no projeto do Unity](getting-started.md)

