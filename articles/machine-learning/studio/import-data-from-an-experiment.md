---
title: Importar dados para Machine Learning Studio a partir de outra experimentação - Azure | Documentos da Microsoft
description: Como guardar dados de formação no Azure Machine Learning Studio e utilizá-lo em outra experimentação.
keywords: Importar dados, dados, origens de dados, dados de treinamento
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: previous-ms.author=deguhath, previous-author=deguhath
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 7da9dcec-5693-4bb6-8166-15904e7f75c3
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: ea6e813d78cafa4e31ea37afa2675f30bccc5c4f
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53258084"
---
# <a name="import-your-data-into-azure-machine-learning-studio-from-another-experiment"></a>Importar os seus dados para o Azure Machine Learning Studio a partir de outra experimentação

Haverá situações quando vai querer tirar um resultado intermediário a partir de uma experiência e utilizá-lo como parte de outra experimentação. Para fazer isso, salvar o módulo como um conjunto de dados:

1. Clique a saída do módulo que pretende guardar como um conjunto de dados.
2. Clique em **guardar como conjunto de dados**.
3. Quando lhe for pedido, introduza um nome e uma descrição que permitiria que identifique o conjunto de dados facilmente.
4. Clique nas **OK** marca de verificação.

Quando o salvamento for concluído, o conjunto de dados estarão disponível para uso dentro de qualquer experimentação na sua área de trabalho. Pode encontrá-lo na **conjuntos de dados guardado** lista na paleta do módulo.

