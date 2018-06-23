---
title: Glossário para o serviço de API de rostos em | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: O glossário explica certos termos que podem surgir à medida que trabalha com o serviço de API de letra.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: ec3068c013be9f2fa4ff34b1c24596e46e7c5d05
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351926"
---
# <a name="glossary"></a>Glossário

## <a name="a"></a>A

#### <a name="Attributes"></a>atributos

Atributos são opcionais no [deteção](#Detection-Face-Detection) resulta, tais como [idade](#Age-Attribute), [sexo](#Gender-Attribute), [implicar head](#Head-Pose-Attribute), [facial hair](#Facial-Hair-Attribute), [smiling](#Smile-Attribute).
Podem ser obtidos a partir de [deteção](#Detection-Face-Detection) API, especificando os parâmetros de consulta: returnFaceAttributes. Atributos fornecer informações adicionais sobre selecionado [faces](#Face); além o [enfrentam ID](#Face-ID) e o [retângulo](#Face-Rectangle).

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Age-Attribute"></a>Duração (atributo)

Idade é uma do [atributos](#Attributes) que descreve a duração de um rosto específico. O atributo de idade é opcional no [deteção](#Detection-Face-Detection) resultados e pode ser controlado com um [deteção](#Detection-Face-Detection) pedido especificando o parâmetro returnFaceAttributes.

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="b"></a>B

## <a name="c"></a>C

#### <a name="Candidate"></a>Candidatos

Candidatos são essencialmente [identificação](#Identification) resultados (por exemplo, pessoas identificadas e nível de confiança no deteções). Um candidato é representado pelo [PersonID](#Person-ID) e [confiança](#Confidence), que indica que a pessoa que é identificada com um elevado nível de confiança.

Para obter mais detalhes, consulte o guia [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

#### <a name="Confidence"></a>Confiança

Confiança é uma medida revelando semelhança entre [faces](#Face) ou [pessoa](#Person) numérico valores – que é utilizada no [identificação](#Identification), e [ verificação](#Verification) para indicar as semelhanças dos resultados de procurar, identificados e verificados.

Para obter mais detalhes, consulte os guias seguintes: [enfrentam - Localizar semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [enfrentam - verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="d"></a>D

#### <a name="Detection-Face-Detection"></a>Deteção de rostos/deteção

Deteção de rostos em é a ação de localizar faces nas imagens. Os utilizadores podem carregar uma imagem ou especificar um URL de imagem no pedido. Os faces detetados são devolvidos com [enfrentam os IDs de](#Face-ID) que indica uma identidade exclusiva na API de letra. Os retângulos indicam as localizações de enfrentam reside na imagem em pixels, bem como o opcional [atributos](#Attributes) para cada enfrentam como [idade](#Age-Attribute), [sexo](#Gender-Attribute), [implicar principal ](#Head-Pose-Attribute), [facial hair](#Facial-Hair-Attribute) e [smiling](#Smile-Attribute).

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="e"></a>E

## <a name="f"></a>F

#### <a name="Face"></a>Letra

Enfrentam é um termo unificado para os resultados derivado de API de rostos em relacionado com faces detetados. Em última análise, enfrentam é representada por uma identidade unificada ([ID de rostos em](#Face-ID)), uma região especificada nas imagens ([retângulo de rostos em](#Face-Rectangle)) e enfrentam adicional relacionados com [atributos](#Face-Attributes-Facial-Attributes), tais como [idade](#Age-Attribute), [sexo](#Gender-Attribute), [landmarks](#Face-Landmarks-Facial-Landmarks) e [implicar head](#Head-Pose-Attribute). Além disso, faces podem ser devolvidos a partir de [deteção](#Detection-Face-Detection).

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Face-API"></a>Enfrentam API

API de letra é uma API baseada na nuvem que fornece os algoritmos mais avançados para a deteção de rostos em e de reconhecimento. A funcionalidade principal de API de letra pode ser dividida em duas categorias: enfrentam [deteção](#Detection-Face-Detection) com [atributos](#Face-Attributes-Facial-Attributes)e enfrentam [reconhecimento](#Recognition).

Para obter mais detalhes, consulte os guias seguintes: [descrição geral da API de rostos em](./Overview.md), [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236), [enfrentam - Localizar semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [enfrentam - grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [Enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [enfrentam - verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="Face-Attributes-Facial-Attributes"></a>Atributos de atributos Facial enfrentam

Consulte [atributos](#Attributes).

#### <a name="Face-ID"></a>ID de rostos em

ID de rostos em é derivado do [deteção](#Detection-Face-Detection) resultados, em que representa uma cadeia de um [enfrentam](#Face) no [API de rostos em](#Face-API).

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Face-Landmarks-Facial-Landmarks"></a>Landmarks Landmarks Facial enfrentam

Landmarks são opcionais no [deteção](#Detection-Face-Detection) resultados; que são pontos facial semânticos, tais como o continuará ser, nose e boca (ilustrado na figura seguinte). Podem ser controlados landmarks com um [deteção](#Detection-Face-Detection) pedido efetuado pelo returnFaceLandmarks número booleano. Se returnFaceLandmarks estiver definido como VERDADEIRO, os faces devolvidos tem atributos landmark.

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![HowToDetectFace](./Images/landmarks.1.jpg)

#### <a name="Face-Rectangle"></a>Retângulo de rostos em

O retângulo de rostos em é derivado do [deteção](#Detection-Face-Detection) resultados, o que é um retângulo upright (à esquerda, superior, a largura, altura) nas imagens em pixéis. O canto superior esquerdo de um [enfrentam](#Face) (à esquerda, principais), além da largura e altura, indica os tamanhos de rostos em x e y eixos, respetivamente.

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Facial-Hair-Attribute"></a>Hair facial (atributo)

Facial hair é uma do [atributos](#Attributes) utilizados para descrever o comprimento facial hair os faces disponíveis. O atributo facial hair é opcional no [deteção](#Detection-Face-Detection) resultados e pode ser controlado com um [deteção](#Detection-Face-Detection) pedido efetuado pelo returnFaceAttributes. Se returnFaceAttributes contém 'facialHair', os faces devolvidos tem atributos facial hair.

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="FaceList"></a>FaceList

FaceList é uma coleção de [PersistedFace](#PersistedFace) e é a unidade de [Localizar semelhante](#Find-Similar). Um FaceList vem com uma [FaceList ID](#FaceList-ID), assim como outros atributos como [nome](#Name) e [dados de utilizador](#UserData-User-Data).

Para obter mais detalhes, consulte os guias seguintes: [FaceList - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="FaceList-ID"></a>ID de FaceList

FaceList ID é uma cadeia de fornecido pelo utilizador utilizada como um identificador de um [FaceList](#FaceList). O ID de FaceList têm de ser exclusivo dentro da subscrição.

Para obter mais detalhes, consulte os guias seguintes: [FaceList - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b), [FaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c).

#### <a name="Find-Similar"></a>Encontrar semelhantes

Esta API é utilizada para/consulta de pesquisa faces semelhantes com base numa coleção de faces. Consulta faces e coleções de rostos em são representadas como [enfrentam os IDs de](#Face-ID) ou [FceList ID](#FaceList-ID)/[LargeFaceList ID](#LargeFaceList-ID) no pedido. Devolvidos os resultados são procurar faces semelhantes, representados pelo [enfrentam os IDs de](#Face-ID) ou [PersistedFace IDs](#PersistedFace-ID).

Para obter mais detalhes, consulte os guias seguintes: [enfrentam - Localizar semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [LargeFaceList - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [FaceList - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b).

## <a name="g"></a>G

#### <a name="Gender-Attribute"></a>Sexo (atributo)

Sexo é uma do [atributos](#Attributes) utilizados para descrever os genders dos faces disponíveis. O atributo sexo é opcional no [deteção](#Detection-Face-Detection) resultados e pode ser controlado com um [deteção](#Detection-Face-Detection) pedido efetuado pelo returnFaceAttributes. Se returnfaceAttributes contém 'sexo', os faces devolvidos tem atributos sexo.

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="Grouping"></a>Agrupamento

Agrupamento de rostos em é o agrupamento de uma coleção de faces, de acordo com semelhanças facial. Coleções de rostos em são indicadas pelas coleções de ID de rostos no pedido. Como resultado de agrupamento, faces semelhantes são agrupados em conjunto como [grupos](#Groups), e faces que não são semelhantes a qualquer outro tipo de letra são intercaladas em conjunto como um grupo messy. Há no máximo, um [grupo messy](#Messy-Group) no resultado de agrupamento.

Para obter mais detalhes, consulte o guia [enfrentam - grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

#### <a name="Groups"></a>Grupos

Grupos são derivados do [agrupamento](#Grouping) resultados. Cada grupo contém uma coleção de faces semelhantes, onde faces são indicados por [enfrentam os IDs de](#Face-ID).

Para obter mais detalhes, consulte o guia [enfrentam - grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="h"></a>H

#### <a name="Head-Pose-Attribute"></a>HEAD implicar (atributo)

HEAD implicar é uma do [atributos](#Attributes) que representa enfrentam orientação no espaço 3D, de acordo com os ângulos de agregação, rápida e yaw, conforme mostrado na figura seguinte. Os intervalos de valor de agregação e yaw são [-180 e, 180] e [-90, 90] em graus. Na versão atual, o valor de tipos devolvido de deteção é sempre 0. O atributo implicar head é opcional no [deteção](#Detection-Face-Detection) resultados e pode ser controlado com um [deteção](#Detection-Face-Detection) pedido pelo parâmetro returnFaceAttributes. Se o parâmetro de returnFaceAttributes contém 'headPose', os faces devolvidos terão head implicar atributos.

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

![GlossaryHeadPose](./Images/headpose.1.jpg)

## <a name="i"></a>I

#### <a name="Identification"></a>Identificação

Identificação consiste em identificar faces um ou mais de um LargePersonGroup/PersonGroup.
A [PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup) é uma coleção de [pessoas](#Person).
Faces e LargePersonGroup/PersonGroup são representados por [enfrentam os IDs de](#Face-ID) e [LargePersonGroup IDs](#LargePersonGroup-ID)/[PersonGroup IDs](#PersonGroup-ID) respetivamente no pedido.
São identificados resultados [candidatos](#Candidate), representado pelo [pessoas](#Person) com confiança.
Vários faces na entrada são considerados em separado e terá de cada enfrentam resultado identificado.

> [!NOTE]
> LargePersonGroup/PersonGroup deve ser preparado com êxito antes de identificação. Se não está preparada LargePersonGroup/PersonGroup, ou a formação [estado](#Status-Train) não é apresentada como 'foi concluída com êxito' (ou seja, ' Executar', 'falhada' ou 'timeout'), a resposta do pedido é 400.
> 

Para obter mais detalhes, consulte os guias seguintes: [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [LargePersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [ LargePersonGroup - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [PersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="Is-Identical"></a>IsIdentical

IsIdentical é um campo Booleano de [verificação](#Verification) resultados que indica se duas faces pertencem a mesma pessoa.

Para obter mais detalhes, consulte o guia [enfrentam - verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="j"></a>D

## <a name="k"></a>K

## <a name="l"></a>L

#### <a name="landmarks"></a>Landmarks

Consulte [enfrentam landmarks](#Face-Landmarks-Facial-Landmarks).

#### <a name="LargeFaceList"></a>LargeFaceList

LargeFaceList é uma coleção de [PersistedFace](#PersistedFace) e é a unidade de [Localizar semelhante](#Find-Similar). Um LargeFaceList vem com uma [LargeFaceList ID](#LargeFaceList-ID), assim como outros atributos como [nome](#Name) e [dados de utilizador](#UserData-User-Data).

Para obter mais detalhes, consulte os guias seguintes: [LargeFaceList - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce), [LargeFaceList - lista enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158db4d2de3616c086f2d6).

#### <a name="LargeFaceList-ID"></a>ID de LargeFaceList

LargeFaceList ID é uma cadeia de fornecido pelo utilizador utilizada como um identificador de um [LargeFaceList](#LargeFaceList). O ID de LargeFaceList têm de ser exclusivo dentro da subscrição.

Para obter mais detalhes, consulte os guias seguintes: [LargeFaceList - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc), [LargeFaceList - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a15827cd2de3616c086f2ce).

#### <a name="LargePersonGroup"></a>LargePersonGroup

LargePersonGroup é uma coleção de [pessoas](#Person) e é a unidade de [identificação](#Identification). Um LargePersonGroup vem com uma [LargePersonGroup ID](#LargePersonGroup-ID), assim como outros atributos como [nome](#Name) e [dados de utilizador](#UserData-User-Data).

Para obter mais detalhes, consulte os guias seguintes: [LargePersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [LargePersonGroup pessoa - lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adda06ac60f11b48b5aa1).

#### <a name="LargePersonGroup-ID"></a>ID de LargePersonGroup

LargePersonGroup ID é uma cadeia de fornecido pelo utilizador utilizada como um identificador de um [LargePersonGroup](#LargePersonGroup). O ID de LargePersonGroup têm de ser exclusivo dentro da subscrição.

Para obter mais detalhes, consulte os guias seguintes: [LargePersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e).

## <a name="m"></a>M

#### <a name="Messy-Group"></a>Grupo messy

Grupo messy é derivado de [agrupamento](#Grouping) resultados; que contém faces não semelhantes a qualquer outro tipo de letra. Cada enfrentam reside num grupo messy é indicada pelo [enfrentam ID](#Face-ID).

Para obter mais detalhes, consulte o guia [enfrentam - grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="n"></a>N

#### <a name="name-person"></a>Nome (pessoa)

O nome é uma cadeia descritivo amigável de utilizador para [pessoa](#Person). Ao contrário do [pessoa ID](#Person-ID), o nome das pessoas que pode ser duplicado dentro de um grupo.

Para obter mais detalhes, consulte os guias seguintes: [LargePersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup pessoa - obter](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ Obter PersonGroup pessoa -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="Name"></a>Nome (LargePersonGroup/PersonGroup)

Nome também é uma cadeia descritivo amigável de utilizador para [LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup). Ao contrário do [LargePersonGroup ID](#LargePersonGroup-ID)/[PersonGroup ID](#PersonGroup-ID), o nome do LargePersonGroups/PersonGroups pode ser duplicado numa subscrição.

Para obter mais detalhes, consulte os guias seguintes: [LargePersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acebb6ac60f11b48b5a9e), [PersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [ PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

## <a name="o"></a>O

## <a name="p"></a>P

#### <a name="PersistedFace"></a>PersistedFace

PersistedFace é uma estrutura de dados na API de letra. PersistedFace vem com uma [PersistedFace ID](#PersistedFace-ID), assim como outros atributos como [nome](#Name), e [dados de utilizador](#UserData-User-Data).

Para obter mais detalhes, consulte os guias seguintes: [LargeFaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup pessoa - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [ Pessoa PersonGroup - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="Person-ID"></a>ID de pessoa

ID da pessoa que é gerado quando um [PersistedFace](#PersistedFace) é criado com êxito. É criada uma cadeia para representar este enfrentam reside na [enfrentam API](#Face-API).

Para obter mais detalhes, consulte os guias seguintes: [LargeFaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3), [FaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250), [LargePersonGroup pessoa - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42), [ Pessoa PersonGroup - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b).

#### <a name="Person"></a>Pessoa

Pessoa é uma estrutura de dados gerida na API de letra. Pessoa vem com uma [pessoa ID](#Person-ID), assim como outros atributos como [nome](#Name), uma coleção de [PersistedFace](#PersistedFace), e [dados de utilizador](#UserData-User-Data).

Para obter mais detalhes, consulte os guias seguintes: [LargePersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup pessoa - obter](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ Obter PersonGroup pessoa -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="Person-ID"></a>ID de pessoa

ID da pessoa que é gerado quando um [pessoa](#Person) é criado com êxito. É criada uma cadeia para representar esta pessoa em [enfrentam API](#Face-API).

Para obter mais detalhes, consulte os guias seguintes: [LargePersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup pessoa - obter](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599add376ac60f11b48b5aa0), [PersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ Obter PersonGroup pessoa -](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523f).

#### <a name="PersonGroup"></a>PersonGroup

PersonGroup é uma coleção de [pessoas](#Person) e é a unidade de [identificação](#Identification). Um PersonGroup vem com uma [PersonGroup ID](#PersonGroup-ID), assim como outros atributos como [nome](#Name) e [dados de utilizador](#UserData-User-Data).

Para obter mais detalhes, consulte os guias seguintes: [PersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246), [PersonGroup pessoa - lista](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395241).

#### <a name="PersonGroup-ID"></a>ID de PersonGroup

PersonGroup ID é uma cadeia de fornecido pelo utilizador utilizada como um identificador de um [PersonGroup](#PersonGroup). O ID do grupo tem de ser exclusivo dentro da subscrição.

Para obter mais detalhes, consulte os guias seguintes: [PersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - Get](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395246).

#### <a name="pose-attribute"></a>Apresentam (atributo)

Consulte [Head implicar](#Head-Pose-Attribute).

## <a name="q"></a>Q

## <a name="r"></a>R

#### <a name="Recognition"></a>Reconhecimento

O reconhecimento é uma área de aplicações populares para tecnologias de letra, tal como [Localizar semelhante](#Find-Similar), [agrupamento](#Grouping), [identificar](#Identification),[verificar dois enfrenta mesmo ou não ](#Verification).

Para obter mais detalhes, consulte os guias seguintes: [enfrentam - Localizar semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237), [enfrentam - grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238), [enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239), [enfrentam - verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

#### <a name="rectangle-face"></a>Retângulo (enfrentam)

Consulte [retângulo de rostos em](#Face-Rectangle).

## <a name="s"></a>S

#### <a name="Smile-Attribute"></a>Smile (atributo)

Smile é uma do [atributos](#Attributes) utilizados para descrever a expressão smile os faces disponíveis. O atributo smile é opcional no [deteção](#Detection-Face-Detection) resultados e pode ser controlado com um [deteção](#Detection-Face-Detection) pedido efetuado pelo returnFaceAttributes. Se returnFaceAttributes contém 'smile', os faces devolvidos terá smile atributos.

Para obter mais detalhes, consulte o guia [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

#### <a name="similar-face-searching"></a>Pesquisa de Faces Semelhantes

Consulte [Localizar semelhante](#Find-Similar).

#### <a name="Status-Train"></a>Estado (formação)

Estado é uma cadeia utilizada para descrever o procedimento para [formação LargeFaceList/LargePersonGroups/PersonGroups](#Train), incluindo 'notstarted', 'Executar', 'foi concluída com êxito","Falha".

Para obter mais detalhes, consulte o guia [LargeFaceList - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249).

#### <a name="subscription-key"></a>Chave de subscrição

Chave de subscrição é uma cadeia que tem de especificar como um parâmetro de cadeia de consulta para invocar qualquer API de letra. A chave de subscrição pode ser encontrada na página das minhas subscrições depois de iniciar sessão no portal de serviços cognitivos da Microsoft. Existirão duas chaves associadas com cada subscrição: uma chave primária e uma chave secundária. Podem ser utilizados para invocar a API forma idêntica. Tem de manter as chaves de subscrição segura e pode voltar a gerar chaves de subscrição em qualquer altura da página das minhas subscrições bem.

## <a name="t"></a>T

#### <a name="Train"></a>Preparar (LargePersonGroup/LargeFaceList/PersonGroup)

Esta API é utilizada para processar previamente os [LargeFaceList](#LargeFaceList)/[LargePersonGroup](#LargePersonGroup)/[PersonGroup](#PersonGroup) para garantir a [localizar Semelhante](#Find-Similar)/[identificação](#Identification) desempenho. Se a formação não manipula, ou o [estado de preparação](#Status-Train) não é apresentada como concluída com êxito, a identificação para este PersonGroup resultará na falha.

Para obter mais detalhes, consulte os guias seguintes: [LargeFaceList - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158422d2de3616c086f2d1), [LargePersonGroup - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4), [PersonGroup - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249), [enfrentam – identificar ](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="u"></a>U

#### <a name="UserData-User-Data"></a>UserData/dados de utilizador

Dados de utilizador são informações adicionais associadas [pessoa](#Person) e [PersonGroup](#PersonGroup)/[LargePersonGroup](#LargePersonGroup). Para facilitar a dados a utilizar, compreender e lembre-se de dados de utilizador está definido pelos utilizadores.

Para obter mais detalhes, consulte os guias seguintes: [LargePersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d), [LargePersonGroup - atualização](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acfc83a7b9412a4d53f3f), [LargePersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40), [LargePersonGroup pessoa - atualizar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ade043a7b9412a4d53f41), [PersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244), [PersonGroup - atualização](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524a), [PersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c), [ Pessoa PersonGroup - atualizar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395242).

## <a name="v"></a>V

#### <a name="Verification"></a>Verificação

Esta API é utilizada para verificar se dois faces são os mesmos ou não. Ambos os faces são representados como enfrentam os IDs no pedido. Verificar os resultados contêm um campo Booleano ([isIdentical](#Is-Identical)) que indica mesmo se for true e um campo do número ([confiança](#Confidence)) que indica o nível de confiança.

Para obter mais detalhes, consulte o guia [enfrentam - verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="w"></a>W

## <a name="x"></a>X

## <a name="y"></a>S

## <a name="z"></a>Z
