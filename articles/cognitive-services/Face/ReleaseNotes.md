---
title: Notas de versão para o serviço de API de rostos em | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Notas de versão para o serviço de API de rostos em incluem um histórico de alterações de versão para várias versões.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 918b3ea5dbaaa44e4eee1a679354c7becffd4686
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355081"
---
# <a name="face-api-release-notes"></a>Notas de versão de API de rostos em

Este artigo diz respeito à versão do serviço de API de rostos em Microsoft 1.0.

### <a name="release-changes-in-may-2018"></a>Alterações de versão em Maio de 2018

* Melhorado `gender` atributo melhorado significativamente e também `age`, `glasses`, `facialHair`, `hair`, `makeup` atributos. Utilizá-los através de [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parâmetro. 

* Limite de tamanho de ficheiro do imagem entrada maior de 4 MB para 6 MB [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [FaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargeFaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [PersonGroup pessoa - adicionar Enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) e [LargePersonGroup pessoa - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42).

### <a name="release-changes-in-march-2018"></a>Alterações de versão em Março de 2018

* Contentor de milhões de hiper escala adicionado: [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc) e [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d). Obter mais detalhes no [como utilizar a funcionalidade em grande escala](Face-API-How-to-Topics/how-to-use-large-scale.md).

* Aumentado [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) `maxNumOfCandidatesReturned` parâmetro de [1, 5] para [1, 100] e predefinida para 10.

### <a name="release-changes-in-may-2017"></a>Alterações de versão em Maio de 2017

* Adicionar `hair`, `makeup`, `accessory`, `occlusion`, `blur`, `exposure`, e `noise` atributos no [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parâmetro.

* Suportado 10 mil pessoas um PersonGroup e [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Suportada paginação na [PersonGroup pessoa - lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241) com parâmetros opcionais: `start` e `top`.

* Simultaneidade suportada na adição/eliminar faces contra FaceLists diferentes e pessoas diferentes em PersonGroup.

### <a name="release-changes-in-march-2017"></a>Alterações de versão em Março de 2017
* Adicionar `emotion` atributo em [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) `returnFaceAttributes` parâmetro.

* Fixo de rostos não foi possível redetected com retângulo devolvido de [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) como `targetFace` no [FaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) e [PersonGroup pessoa - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

* Corrigido o tamanho de rostos em detetável para se certificar de que é estritamente entre 36 x 36 para 4096 x 4096 pixéis.

### <a name="release-changes-in-november-2016"></a>Alterações de versão de Novembro de 2016
* Adicionar subscrição enfrentam armazenamento Standard para armazenar faces persistentes adicionais ao utilizar [PersonGroup pessoa - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b) ou [FaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) de identificação ou semelhança correspondente. As imagens armazenadas são cobradas às $0,5 por 1000 faces e esta taxa é proporcional numa base diária. Subscrições do escalão gratuito continuem a estar limitado a 1000 pessoas totais.

### <a name="release-changes-in-october-2016"></a>Alterações de versão em Outubro de 2016
* Alterar a mensagem de erro de mais de um rosto em targetFace de 'Existem mais de um rosto na imagem' para 'Há mais de um rosto na imagem' no [FaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250) e [PersonGroup pessoa - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

### <a name="release-changes-in-july-2016"></a>Alterações à versão de Julho de 2016
* Suportado enfrentam para autenticação de objeto de pessoa em [enfrentam - verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

* Adicionar opcional `mode` parâmetro ativar a seleção dos dois modos de trabalho: `matchPerson` e `matchFace` no [enfrentam - Localizar semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) e a predefinição é `matchPerson`.

* Adicionar opcional `confidenceThreshold` parâmetro para o utilizador definir o limiar de se um enfrentam pertence a um objeto de pessoa em [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

* Adicionar opcional `start` e `top` parâmetros [PersonGroup - lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395248) para ativar o utilizador especificar o ponto de início e o número total de PersonGroups à lista.

### <a name="v10-changes-from-v0"></a>Alterações de v 1.0 do V0
* Atualizar o ponto final de raiz do serviço de ```https://westus.api.cognitive.microsoft.com/face/v0/``` para ```https://westus.api.cognitive.microsoft.com/face/v1.0/```. Alterações aplicadas a: [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [enfrentam - Localizar semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) e [enfrentam - grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

* O tamanho mínimo enfrentam detetável foi atualizado para 36 x 36 pixéis. Não serão detetados faces inferior 36 x 36 pixéis.

* Preterido os dados PersonGroup e pessoa V0 de letra. Não não possível aceder esses dados com o serviço de v 1.0 de letra.

* Preterido o ponto final de V0 da API de rostos em 30 de Junho de 2016.
