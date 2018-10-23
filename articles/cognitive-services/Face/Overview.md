---
title: O que é o serviço da API Face?
titleSuffix: Azure Cognitive Services
description: Este tópico explica o serviço da API Face e os termos relacionados.
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: overview
ms.date: 10/11/2018
ms.author: sbowles
ms.openlocfilehash: 6c1e0d0a51bc01c581c05e7ce3215f5501b4be76
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/13/2018
ms.locfileid: "49310374"
---
# <a name="what-is-the-face-api-service"></a>O que é o serviço da API Face?

O serviço da API Face é um serviço com base na cloud que apresenta algoritmos de análise de rostos humanos em imagens e vídeo. A API Face tem duas funções principais: deteção de rostos com atributos e reconhecimento facial.

## <a name="face-detection"></a>Deteção de rostos

A API Face pode detetar até 64 rostos humanos através da localização de alta precisão de rostos numa imagem. A imagem pode ser especificada por um ficheiro (um fluxo de bytes) ou com um URL válido.

![Descrição Geral – Deteção de Rostos](./Images/Face.detection.jpg)

O retângulo de rostos (esquerda, cima, largura e altura) que indica a localização do rosto na imagem é devolvido com cada um dos rostos detetados. Opcionalmente, a deteção de rostos extrai vários atributos relacionados com o rosto, tais como a pose, o género, a idade, a posição da cabeça, pelos faciais e óculos. Para obter mais informações, veja [Rosto – Detetar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-recognition"></a>Reconhecimento facial

A capacidade de identificar rostos humanos é importante em muitos cenários, incluindo em contexto de segurança, interface de utilizador natural, análise e gestão de conteúdos de imagem, aplicações móveis e robótica. O serviço de API Face oferece quatro funções de reconhecimento facial: verificação de rostos, localização de rostos semelhantes, agrupamento de rostos e identificação de pessoas.

### <a name="face-verification"></a>Verificação de rostos

A verificação do rosto faz a autenticação com a deteção de dois rostos ou com a deteção de um rosto para um objeto de pessoa. Para obter informações mais detalhadas, veja [Rosto – Verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

### <a name="finding-similar-faces"></a>Encontrar rostos semelhantes

Tendo em conta a deteção de um rosto visado e um conjunto de rostos candidatos com os quais se vai procurar, o serviço localiza um pequeno conjunto de rostos com o aspeto mais semelhante ao rosto visado. São suportados dois modos de trabalho, **matchFace** e **matchPerson**. O modo **matchPerson** devolve rostos semelhantes após a aplicação de um limiar de mesma pessoa resultante de [Rosto – Verificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a). O modo **matchFace** ignora o limiar de mesma pessoa e devolve os principais rostos candidatos semelhantes. Os rostos candidate estão listados no seguinte exemplo.
![Descrição geral - Encontrar Rostos Semelhantes](./Images/FaceFindSimilar.Candidates.jpg) Este é o rosto de consulta.
![Descrição geral - Encontrar Rostos Semelhantes](./Images/FaceFindSimilar.QueryFace.jpg)

Para encontrar quatro rostos semelhantes, o modo **matchPerson** iria devolver (a) e (b), que representam a mesma pessoa que o rosto de consulta. O modo **matchFace** devolve (a), (b), (c) e (d), ou seja, exatamente quatro candidatos mesmo que alguns sejam pouco semelhantes entre si. Para obter mais informações, veja [Rosto – Localizar Semelhante](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

### <a name="face-grouping"></a>Agrupamento de rostos

Tendo em conta um conjunto de rostos desconhecidos, a API de agrupamento de rostos divide-os automaticamente em vários grupos com base na semelhança. Cada grupo é um subconjunto devidamente desajustado do conjunto de rostos desconhecidos originais e contém rostos semelhantes. Todos os rostos no mesmo grupo podem ser considerados como pertencendo à mesma pessoa. Para obter mais informações, veja [Rosto – Agrupar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

### <a name="person-identification"></a>Identificação de pessoas

A API Face pode ser utilizada para identificar pessoas com base na deteção de um rosto e numa base de dados de pessoas. Crie previamente esta base de dados, e pode ser editada ao longo do tempo.

A figura abaixo é um exemplo de uma base de dados com o nome "myfriends." Cada grupo pode conter 1 000 000/10 000 objetos de pessoas diferentes. Cada objeto de pessoa pode ter 248 rostos registados.

![Descrição Geral – LargePersonGroup/PersonGroup](./Images/person.group.clare.jpg)

Depois de se criar e preparar uma base de dados, é possível dar início à identificação em relação ao grupo com a deteção de um novo rosto. Se o rosto for identificado como uma pessoa no grupo, esse objeto será devolvido.

Para obter mais informações sobre a identificação de pessoas, veja os seguintes guias de API:

[Rosto – Identificar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239)  
[PersonGroup – Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244)  
[PersonGroup Person – Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)  
[PersonGroup – Preparar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249)  
[LargePersonGroup – Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)  
[LargePersonGroup Person – Criar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adcba3a7b9412a4d53f40)  
[LargePersonGroup – Preparar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599ae2d16ac60f11b48b5aa4)  

#### <a name="face-storage-and-pricing"></a>Armazenamento de rostos e preços

O Armazenamento de Rostos permite que uma subscrição padrão armazene rostos persistentes adicionais quando se utilizam objetos de Pessoa LargePersonGroup/PersonGroup ([Pessoa PersonGroup – Adicionar Rosto](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523b)/[Pessoa LargePersonGroup – Adicionar Rosto](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599adf2a3a7b9412a4d53f42)) ou LargeFaceLists/FaceLists ([FaceList – Adicionar Rosto](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395250)/[LargeFaceList – Adicionar Rosto](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a158c10d2de3616c086f2d3)) para a identificação ou correspondência de semelhanças com a API Face. As imagens armazenadas são cobradas a 0,50 $ por 1000 rostos e esta tarifa é calculada diariamente. As subscrições do escalão gratuito são limitadas a 1000 pessoas no total.

Por exemplo, se a sua conta tiver utilizado 10 000 rostos persistentes todos os dias durante a primeira metade do mês, mas nenhum na segunda metade, só serão faturados os 10 000 rostos correspondentes aos dias armazenados. Em alternativa, se todos os dias do mês mantiver a persistência de 1000 rostos durante algumas horas e, em seguida, os eliminar todas as noites, continuarão a ser-lhe cobrados 1000 rostos persistentes todos os dias.

## <a name="sample-apps"></a>Exemplos de aplicações

Veja estes exemplos de aplicações que utilizam a API Face.

- [API Face da Microsoft: Biblioteca de Cliente do Windows e Exemplo](https://github.com/Microsoft/Cognitive-Face-Windows)
  - Exemplo de aplicação do WPF que demonstra vários cenários de deteção, análise e identificação de rostos.
- [Aplicação FamilyNotes da UWP](https://github.com/Microsoft/Windows-appsample-familynotes)
  - Exemplo de aplicação da Plataforma Universal do Windows (UWP) que mostra a utilização de voz, Cortana, tinta digital e câmara através de um cenário de partilha de notas de família.
- [Exemplo de Análise de Fotograma](https://github.com/microsoft/cognitive-samples-videoframeanalysis)
  - Exemplo de aplicação do Win32 que mostra a análise de transmissões de vídeo em direto quase em tempo real com as APIs Face, de Imagem Digitalizada e de Emoções.

## <a name="tutorials"></a>Tutoriais
Os seguintes tutoriais demonstram as funcionalidades básicas e os processos de subscrição da API Face:
- [Tutorial de Introdução à API Face no CSharp](Tutorials/FaceAPIinCSharpTutorial.md)
- [Tutorial de Introdução à API Face em Java para Android](Tutorials/FaceAPIinJavaForAndroidTutorial.md)
- [Tutorial de Introdução à API Face no Python](Tutorials/FaceAPIinPythonTutorial.md)

## <a name="next-steps"></a>Passos seguintes

Tente um início rápido para implementar um cenário simples da Face API.
- [Guia de Início Rápido: Detetar rostos numa imagem com C#](quickstarts/csharp.md) (estão disponíveis outras linguagens)
