---
title: Descrição geral do serviço de API de enfrentam | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: O glossário explica certos termos que podem surgir à medida que trabalha com o serviço de API de letra.
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: bb3ee9431f24a51dfd3877f1c4d621f7ba633b96
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352472"
---
# <a name="what-is-face-api"></a>O que é a letra API?

Bem-vindo ao Microsoft enfrentam API, um serviço baseado na nuvem que fornece os algoritmos de rostos em mais avançados. Enfrentam API tem duas funções principais: enfrentam deteção com atributos e enfrentam reconhecimento.

## <a name="face-detection"></a>Deteção de Rostos

Enfrentam API Deteta até 64 faces humanos com a localização de rostos em precisão elevada numa imagem. E a imagem pode ser especificada pelo ficheiro em bytes ou URL válido.

![Descrição geral - deteção de rostos em](./Images/Face.detection.jpg)

O retângulo de rostos (à esquerda, superior, largura e altura) que indica a localização de enfrentam reside na imagem é devolvida juntamente com cada detetado enfrentam reside. Opcionalmente, a deteção de rostos em extrai uma série de atributos relacionados com a letra como implicar, sexo, idade, apresentam head, facial hair e glasses. Para obter mais informações, consulte [enfrentam - detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Reconhecimento de rostos em

Reconhecimento de rostos em é amplamente utilizado em vários cenários, incluindo a segurança, a interface de utilizador natural, análise de conteúdo de imagem e gestão, aplicações móveis e robotics. São fornecidas quatro funções de reconhecimento de rostos em: enfrentam verificação, faces semelhante a localizar, agrupamento de rostos em e identificação de pessoa.

### <a name="face-verification"></a>Verificação Facial

Verificação de API de rostos em executa uma autenticação contra duas faces detetados ou a autenticação de um rosto detetado a um objeto de pessoa. Para obter mais informações, consulte [enfrentam - verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-face"></a>Localizar enfrentam semelhante

Tendo em conta enfrentam um destino detetado e um conjunto de faces candidato para procurar com, o serviço encontra um pequeno conjunto de faces mais semelhante para a letra de destino. Dois modos de trabalho, `matchFace` e `matchPerson` são suportados. `matchPerson` modo devolve faces semelhantes depois de aplicar um limiar da mesma pessoa derivado [enfrentam - verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). `matchFace` o modo ignora o mesma pessoa limiar e devolve superior faces candidato semelhantes. Executar o exemplo seguinte, faces candidatos são listados.
![Descrição geral - enfrentam Localizar semelhante](./Images/FaceFindSimilar.Candidates.jpg) e é a letra de consulta ![descrição geral - enfrentam localizar semelhantes](./Images/FaceFindSimilar.QueryFace.jpg)

Para localizar quatro faces semelhantes, `matchPerson` devolve o modo (a) e (b), que pertence a mesma pessoa com letra de consulta. `matchFace` modo devolve (a), (b), (c) e (d), exatamente quatro candidatos semelhança mesmo se baixa. Para obter mais informações, consulte [enfrentam - Localizar semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Agrupamento Facial

Determinado um conjunto de faces desconhecidos, enfrentam agrupamento API automaticamente divide-las em vários grupos com base no semelhança. Cada grupo é um subconjunto não contíguo adequado de rosto desconhecido original definido e contém faces semelhantes. E podem ser consideradas todas as faces no mesmo grupo de pertencer ao mesmo objeto da pessoa. Para obter mais informações, consulte [enfrentam - grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="face-identification"></a>Identificação Facial

API de letra pode ser utilizada para identificar as pessoas com base num rosto detetado e uma base de dados de pessoas (definido como um LargePersonGroup/PersonGroup). Crie esta base de dados seguinte com antecedência, que pode ser editada ao longo do tempo.

A figura seguinte é um exemplo de um LargePersonGroup/PersonGroup com o nome "myfriends". Cada grupo pode conter objetos de pessoa até 1,000,000/10 000. Entretanto, cada objeto de pessoa pode ter até 248 faces registados.

![Descrição geral - LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Depois de um LargePersonGroup/PersonGroup foi criado e preparado, identificação pode executar o grupo e de um rosto detetado novo. Se a letra for identificada como um objeto de pessoa no grupo, é devolvido o objeto da pessoa.

Para obter mais informações sobre a identificação de pessoa, consulte os seguintes guias de API:

[Enfrentam – identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)
[PersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
[PersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
[PersonGroup - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) 
 [LargePersonGroup - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)
[LargePersonGroup pessoa - criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)
[LargePersonGroup - formação](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)

### <a name="face-storage"></a>Armazenamento de Rostos

Armazenamento de rostos em permite uma subscrição padrão armazenar faces persistentes adicionais ao utilizar objetos de pessoa LargePersonGroup/PersonGroup ([PersonGroup pessoa - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[LargePersonGroup pessoa - Adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) ou LargeFaceLists/FaceLists ([FaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList - adicionar enfrentam](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) para identificação ou semelhança correspondência com a API de rostos em. As imagens armazenadas são cobradas às $0,5 por 1000 faces e esta taxa é proporcional numa base diária. Subscrições do escalão gratuito estão livre, mas limitado a 1000 pessoas totais.

Preços para armazenamento de rostos em é proporcional diariamente. Por exemplo, se estiver a sua conta 10 000 faces persistentes por dia para a metade primeiro do mês e nenhum meio segundo, deverá ser-lhe faturado apenas para os 10 000 faces dias armazenados. Em alternativa, se todos os dias durante o mês manter 1.000 faces para algumas horas e, em seguida, elimine-os todas as noites, que lhe seria ainda ser cobrados 1.000 faces persistentes por dia.

## <a name="getting-started-tutorials"></a>Tutoriais de introdução

Os tutoriais seguintes demonstram as funcionalidades básicas da API de rostos em e processos de subscrições:

- [Introdução à API de letra CSharp tutorial](Tutorials/FaceAPIinCSharpTutorial.md)
- [Introdução Android Tutorial de rostos em API em Java](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Introdução à API de letra tutorial do Python](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="sample-apps"></a>Exemplos de Aplicações

Observe estas aplicações de exemplo que utilizar a API de letra.

- [Aplicação FamilyNotes UWP](https://github.com/Microsoft/Windows-appsample-familynotes)
  - Aplicação de exemplo de plataforma do Windows (UWP) universal, que mostra a utilização de reconhecimento de voz, Cortana, tinta digital e câmara através de uma família tenha em atenção o cenário de partilha.
- [Exemplo de análise de moldura de vídeo](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Aplicação de exemplo de plataforma do Windows (UWP) universal mostra analisar fluxos de vídeos em direto em tempo real com o enfrentam, visão do computador e APIs de emoções.

## <a name="related-topics"></a>Tópicos relacionados

- [Notas de versão de API de rostos em versão 1.0](ReleaseNotes.md)
- [Como detetar enfrenta na imagem](Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
- [Como identificar enfrenta na imagem](Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)
