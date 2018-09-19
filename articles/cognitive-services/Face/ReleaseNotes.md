---
title: Notas de versão – serviço de API de rostos
titleSuffix: Azure Cognitive Services
description: Notas de versão para o serviço de API de rostos incluem um histórico das alterações de versão para várias versões.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: conceptual
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 6fd3d33d40b0ed142127e46dd7c9173de39947c7
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121996"
---
# <a name="face-api-release-notes"></a>Notas de versão de API de rostos

Este artigo diz respeito à versão do serviço de API de rostos de 1.0.

### <a name="release-changes-in-may-2018"></a>Alterações de lançamento em Maio de 2018

* Melhorada `gender` atributo significativamente e também aprimorado `age`, `glasses`, `facialHair`, `hair`, `makeup` atributos. Usá-los por meio [enfrentam – detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parâmetro. 

* Limite de tamanho de ficheiro do entrada de imagem maior de 4 MB para 6 MB [enfrentam – detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup pessoa - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) e [LargePersonGroup pessoa - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Lance as alterações de Março de 2018

* Contentor de escala dos milhões adicionada: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) e [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Obter mais detalhes no [como utilizar a funcionalidade em grande escala](Face-API-How-to-Topics/how-to-use-large-scale.md).

* Aumentou [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` parâmetro de [1, 5] para [1, 100] e predefinido para o 10.

### <a name="release-changes-in-may-2017"></a>Alterações de lançamento em Maio de 2017

* Adicionado `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure`, e `noise` atributos no [enfrentam – detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parâmetro.

* Suportado de 10 mil pessoas num PersonGroup e [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Suportada paginação no [PersonGroup pessoa - listar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) com parâmetros opcionais: `start` e `top`.

* Simultaneidade suportada em Adicionar/eliminar rostos contra FaceLists diferentes e diferentes pessoas em PersonGroup.

### <a name="release-changes-in-march-2017"></a>Alterações de versão em Março de 2017
* Adicionado `emotion` atributo [enfrentam – detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parâmetro.

* Fixo não poderia ser redetected o mostrador com retângulo retornado da [enfrentam – detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como `targetFace` no [FaceList - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) e [PersonGroup pessoa - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Foi corrigido o tamanho de face detetável para se certificar de que é estritamente entre pixels de 36 x 36 para 4096 x 4096.

### <a name="release-changes-in-november-2016"></a>Alterações de versão em Novembro de 2016
* Adicionado o padrão de armazenamento de rostos subscrição armazene rostos persistentes adicionais quando utiliza [PersonGroup pessoa - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ou [FaceList - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) para identificação ou correspondência de semelhanças. As imagens armazenadas são cobradas por US $0,5 por 1000 rostos e esta tarifa é calculada diariamente. As assinaturas do escalão gratuito continuam a ser limitadas a 1000 pessoas total.

### <a name="release-changes-in-october-2016"></a>Alterações de lançamento em Outubro de 2016
* Mudou a mensagem de erro de mais de um rosto na targetFace de "Existem mais de um rosto na imagem" para "Há mais de um rosto na imagem" no [FaceList - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) e [PersonGroup pessoa - adicionar Face](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Alterações de versão em Julho de 2016
* Suportado Face à autenticação do objeto Person na [enfrentam - verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* Adicionado opcional `mode` ativar a seleção dos dois modos de trabalho de parâmetro: `matchPerson` e `matchFace` no [Face - encontrar semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) e a predefinição é `matchPerson`.

* Adicionado opcional `confidenceThreshold` parâmetro para o utilizador definir o limiar de se um rosto pertence a um objeto Person no [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Adicionado opcional `start` e `top` parâmetros [PersonGroup - lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) para permitir que o usuário especificar o ponto inicial e o número de PersonGroups total à lista.

### <a name="v10-changes-from-v0"></a>V1.0 é alterado de V0
* Atualizado a partir do ponto final de raiz do serviço ```https://westus.api.cognitive.microsoft.com/face/v0/``` para ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Alterações aplicadas aos: [enfrentam – detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [Face - encontrar semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) e [enfrentam - grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* Atualizado o tamanho mínimo face detetável para 36 x 36 pixels. Menores do que os pixels de 36 x 36 de rostos não serão detetados.

* Preterido os dados PersonGroup e pessoa na Face V0. Não não possível aceder esses dados com o serviço de Face V1.0.

* Preterido o ponto de extremidade V0 da API de rostos em 30 de Junho de 2016.
