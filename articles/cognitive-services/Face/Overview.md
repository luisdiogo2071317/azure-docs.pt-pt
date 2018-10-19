---
title: O que é o Serviço da API Face?
titleSuffix: Azure Cognitive Services
description: O glossário explica os termos que poderá encontrar ao trabalhar com o Serviço da API Face.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: overview
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 15de899be5ab85e9fe84ba1b6284bc9419fcf8a1
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123475"
---
# <a name="what-is-the-face-api-service"></a>O que é o Serviço da API Face?

Bem-vindo ao Serviço da API Face, um serviço baseado na cloud que fornece os algoritmos de rosto mais avançados. A API Face tem duas funções principais: deteção de rostos com atributos e reconhecimento facial.

## <a name="face-detection"></a>Deteção de Rostos

A API Face deteta até 64 rostos humanos através da localização de alta precisão de rostos numa imagem. Além disso, a imagem pode ser especificada por um ficheiro em bytes ou por um URL válido.

![Descrição Geral – Deteção de Rostos](./Images/Face.detection.jpg)

O retângulo de rostos (esquerda, cima, largura e altura) que indica a localização do rosto na imagem é devolvido com cada um dos rostos detetados. Opcionalmente, a deteção de rostos extrai vários atributos relacionados com o rosto, tais como a pose, o género, a idade, a posição da cabeça, pelos faciais e óculos. Para obter mais informações, veja [Rosto – Detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Reconhecimento Facial

O reconhecimento facial é bastante utilizado em muitos cenários, incluindo em contexto de segurança, interface de utilizador natural, análise e gestão de conteúdos de imagem, aplicações móveis e robótica. São fornecidas quatro funções de reconhecimento facial: verificação de rostos, localização de rostos semelhantes, agrupamento de rostos e identificação de pessoas.

### <a name="face-verification"></a>Verificação de Rostos

A verificação da API Face faz a autenticação com a deteção de dois rostos ou a autenticação com a deteção de um rosto para um objeto de pessoa. Para obter informações mais detalhadas, veja [Rosto – Verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-face"></a>Localizar Rostos Semelhantes

Tendo em conta a deteção de um rosto visado e um conjunto de rostos candidatos com os quais se vai procurar, o serviço localiza um pequeno conjunto de rostos com o aspeto mais semelhante ao rosto visado. São suportados dois modos de trabalho: `matchFace` e `matchPerson`. O modo `matchPerson` devolve rostos semelhantes após a aplicação de um limiar de mesma pessoa resultante de [Rosto – Verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). O modo `matchFace` ignora o limiar de mesma pessoa e devolve os principais rostos candidatos semelhantes. Veja o seguinte exemplo, em que são listados os rostos candidatos.
![Descrição Geral – Localizar Rosto Semelhante](./Images/FaceFindSimilar.Candidates.jpg) O rosto de consulta é ![Descrição Geral – Localizar Rosto Semelhante](./Images/FaceFindSimilar.QueryFace.jpg)

Para localizar quatro rostos semelhantes, o modo `matchPerson` devolve (a) e (b), que pertencem à mesma pessoa com o rosto de consulta. O modo `matchFace` devolve (a), (b), (c) e (d), ou seja, exatamente quatro candidatos mesmo que sejam pouco semelhantes entre si. Para obter mais informações, veja [Rosto – Localizar Semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Agrupamento de Rostos

Tendo em conta um conjunto de rostos desconhecidos, a API de agrupamento de rostos divide-os automaticamente em vários grupos com base na semelhança. Cada grupo é um subconjunto devidamente desajustado do conjunto de rostos desconhecidos originais e contém rostos semelhantes. Além disso, todos os rostos no mesmo grupo podem ser considerados como pertencendo ao mesmo objeto de pessoa. Para obter mais informações, veja [Rosto – Agrupar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="face-identification"></a>Identificação de Rostos

A API Face pode ser utilizada para identificar pessoas com base na deteção de um rosto e numa base de dados de pessoas (definida como um LargePersonGroup/PersonGroup). Crie previamente esta base de dados, que pode ser editada ao longo do tempo.

A figura abaixo é um exemplo de um LargePersonGroup/PersonGroup com o nome "myfriends". Cada grupo pode conter 1 000 000/10 000 objetos de pessoas. Além disso, cada objeto de pessoa pode ter 248 rostos registados.

![Descrição Geral – LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Depois de se criar e preparar um LargePersonGroup/PersonGroup, é possível dar início à identificação em relação ao grupo e à deteção de um novo rosto. Se o rosto for identificado como um objeto de pessoa no grupo, esse objeto será devolvido.

Para obter mais informações sobre a identificação de pessoas, veja os seguintes guias de API:

[Rosto – Identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)
[PersonGroup - Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)
[Pessoa PersonGroup – Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)
[PersonGroup – Preparar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)
[LargePersonGroup – Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)
[Pessoa LargePersonGroup – Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)
[LargePersonGroup – Preparar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)

### <a name="face-storage"></a>Armazenamento de Rostos

O Armazenamento de Rostos permite que uma subscrição padrão armazene rostos persistentes adicionais quando se utilizam objetos de Pessoa LargePersonGroup/PersonGroup ([Pessoa PersonGroup – Adicionar Rosto](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[Pessoa LargePersonGroup – Adicionar Rosto](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) ou LargeFaceLists/FaceLists ([FaceList – Adicionar Rosto](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList – Adicionar Rosto](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) para a identificação ou correspondência de semelhanças com a API Face. As imagens armazenadas são cobradas a 0,5 $ por 1000 rostos e esta tarifa é calculada diariamente. As subscrições do escalão gratuito são gratuitas, mas limitadas a 1000 pessoas no total.

Os preços do Armazenamento de Rostos são calculados diariamente. Por exemplo, se a sua conta tiver utilizado 10 000 rostos persistentes todos os dias durante a primeira metade do mês, mas nenhum na segunda metade, só serão faturados os 10 000 rostos correspondentes aos dias armazenados. Em alternativa, se todos os dias do mês mantiver a persistência de 1000 rostos durante algumas horas e, em seguida, os eliminar todas as noites, continuarão a ser-lhe cobrados 1000 rostos persistentes todos os dias.

## <a name="getting-started-tutorials"></a>Tutoriais de Introdução

Os seguintes tutoriais demonstram as funcionalidades básicas e os processos de subscrição da API Face:

- [Tutorial de Introdução à API Face no CSharp](Tutorials/FaceAPIinCSharpTutorial.md)
- [Tutorial de Introdução à API Face em Java para Android](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Tutorial de Introdução à API Face no Python](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="sample-apps"></a>Exemplos de Aplicações

Veja estes exemplos de aplicações que utilizam a API Face.

- [API Face da Microsoft: Biblioteca de Cliente do Windows e Exemplo](https://github.com/Microsoft/Cognitive-Face-Windows)
  - Exemplo de aplicação do WPF que demonstra vários cenários de deteção, análise e identificação de rostos.
- [Aplicação FamilyNotes da UWP](https://github.com/Microsoft/Windows-appsample-familynotes)
  - Exemplo de aplicação da Plataforma Universal do Windows (UWP) que mostra a utilização de voz, Cortana, tinta digital e câmara através de um cenário de partilha de notas de família.
- [Exemplo de Análise de Fotograma](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Exemplo de aplicação da Plataforma Universal do Windows (UWP) que mostra a análise de transmissões de vídeo em direto quase em tempo real com as APIs Face, de Imagem Digitalizada e de Emoções.

## <a name="related-topics"></a>Tópicos Relacionados

- [Notas de Versão da API Face Versão 1.0](ReleaseNotes.md)
- [Como Detetar Rostos na Imagem](Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
- [Como Identificar Rostos na Imagem](Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)
