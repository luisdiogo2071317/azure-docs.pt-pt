---
title: Personalização de reconhecimento de voz para modelos de texto | Microsoft Docs
description: Melhore o reconhecimento de voz ao personalizar o reconhecimento de voz para modelos de texto.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 380c585f57737c0aa4ec99303c52d4567500b5f4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354152"
---
# <a name="customize-speech-to-text-models-using-speech-service"></a>Personalizar os modelos de "Reconhecimento de voz para texto" com o serviço de reconhecimento de voz

Para ajudar a alcançar os melhores resultados de reconhecimento de voz, o serviço de reconhecimento de voz permite personalizar a três modelos utilizados pelo **reconhecimento de voz para texto** API. Os modelos são preparados automaticamente a partir de dados de exemplo que fornecer.

| Modelo | Dados de exemplo | Objetivo |
|-------|---------------|---------|
| Modelo acústica      | Voz, texto | Personalize os sons (fonemas) associados palavras específicas. Preparar uma nova acentos ou dialeto, utilizando um ambiente, etc. |
| Modelo de idioma      | Texto | Personalize as palavras que são conhecidas para o serviço e como são utilizados no utterances. Adicione termos técnicos, os nomes de local de locais, etc. |
| Pronunciação | Texto | Melhore o reconhecimento de palavras troublesome, compounds e abreviaturas. Por exemplo, mapear "consulte threepio" para ser reconhecido como "C3PO" |

Depois de criar novos modelos, pode criar um ponto de final personalizado que utiliza o seu modelo para uma ou mais efeitos acima. Também pode escolher que um modelo de base fornecidos pelo serviço de reconhecimento de voz se de que pretende utilizar, por exemplo, um modelo personalizado acústica e não um modelo de linguagem padrão. Em seguida, utilizar o ponto final personalizado em vez do ponto final padrão para pedidos REST. Cada ponto final tem um associados *ID de implementação* para que possa ser utilizado com o SDK de reconhecimento de voz.

Personalização de todos os modelos é feita através de [portal de reconhecimento de voz personalizadas](https://www.cris.ai/).

## <a name="language-support"></a>Suporte de idiomas

Os seguintes idiomas são suportados para personalizada **reconhecimento de voz para texto** modelos de idioma.

| Código | Idioma |
|-|-|
| pt-PT | Inglês (Estados Unidos) 
| zh-CN | Chinês 
| SP SP | Espanhol (Espanha) 
| FR-FR | Francês (França) 
| it-IT | Italiano 
| Alemanha da Alemanha | Alemão
| pt-BR | Português (Brasil)
| ru-RU | Russo
| JP-JP | Japonês
| ar ex | Árabe (Egito)

Modelos acústica personalizados suportam apenas inglês (en-US). No entanto, chinês acústica conjuntos de dados podem ser importados para os modelos de idioma chinês de teste.

Pronunciação personalizada suporta apenas inglês (en-US) e alemão (Alemanha-) neste momento.

## <a name="prepare-data-sets"></a>Preparar os conjuntos de dados

Cada tipo de modelo requer dados ligeiramente diferentes e formatação, conforme descrito aqui.

| Modelo | O que fornece      |
|-------|-----------------------|
| Acústica | Um ficheiro ZIP que contém ficheiros de áudio do utterances completas e um ficheiro de texto que contenha transcriptions destes ficheiros. Cada linha do ficheiro de pode conter o nome do ficheiro, um separador (ASCII 9) e o texto.|
| Idioma | Um ficheiro de texto que contenha um utterance por linha. |
| Pronunciação | Um ficheiro de texto que contém uma sugestão de pronunciação em cada linha. Cada sugestão é um formulário de apresentação (word ou abreviatura), seguido de um separador (ASCII 9) e o formulário ditas (o pronunciação pretendido).  |

Ficheiros de texto devem seguir o [diretrizes de transcription texto](prepare-transcription.md) para o idioma do modelo.

## <a name="prepare-audio-files"></a>Preparar os ficheiros de áudio

Ficheiros de áudio para modelos acústica devem registados numa localização representativa, por diversos utilizadores representativos (a menos que o seu objetivo é para otimizar o reconhecimento de um orador), utilizando um microfone semelhante para os utilizadores têm. O formato exigido de todas as amostras de áudio é descrito nesta tabela.

| Propriedade | Valor necessário |
|----------|------|
Formato de ficheiro | RIFF (WAV)
Frequência de amostragem | 8000 Hz ou 16,000 Hz
Canais | 1 (mono)
Formato de exemplo | PCM, número inteiro de 16 bits
Duração do ficheiro | Entre 0.1 e 60 segundos
Collar silêncio | segundo 0.1
Formato de arquivo | Zip
Tamanho máximo de arquivo | 2GB

Se estiver a formação um modelo para funcionar no inúteis dos jogadores, tal como uma fábrica ou de um carro incluir alguns segundos do ruído de fundo típico no início ou fim do alguns exemplos. Não inclua apenas de ruído amostras.

## <a name="upload-data-sets"></a>Carregar conjuntos de dados

Para criar um modelo personalizado, o primeiro carregar os dados e iniciar o processo de preparação.

1.  Inicie sessão no [portal de reconhecimento de voz personalizadas](https://www.cris.ai/).

1.  Escolha o tipo de conjunto de dados que pretende criar a partir de **reconhecimento de voz personalizadas** menu - Adaptation dados para modelos acústica, dados de idioma para modelos de idioma ou pronunciação. É apresentada uma lista existentes de conjuntos de dados desse tipo (se aplicável).

1. Escolha o idioma clicando **alteração região**.

1.  Clique em **importar novo** e especifique um nome e descrição para o novo conjunto de dados.

1. Escolha os ficheiros de dados que está preparado.

1. Clique em **importação** para carregar os dados e iniciar a validação. Validação garante que todos os ficheiros estão no formato correto. Validação pode demorar algum tempo.

## <a name="create-a-model"></a>Criar um modelo

 Depois do conjunto de dados foi validado, pode preparar o modelo da seguinte forma.

> [!NOTE]
> Dados de pronunciação têm de ser preparado.

1. Escolha o tipo de modelo que está a criar o **reconhecimento de voz personalizadas** menu, em seguida, clique em **criar novo.**

1. Escolha a região do modelo.

1. Escolha um modelo de base para o novo modelo. À sua escolha de modelo base determina os modos de reconhecimento, para que o seu modelo pode ser utilizado, bem como a funcionar como contingência para todos os dados não no seu conjunto de dados.

1.  Escolha o conjunto de dados a partir do qual o modelo está a ser criado. Um conjunto de dados pode ser utilizado por qualquer número de modelos.

1. Clique em **criar** para iniciar o novo modelo de preparação.

## <a name="test-a-model"></a>Testar um modelo

Como parte do processo de criação de modelo, pode testar o seu modelo em relação a um conjunto de dados acústica. O novo modelo é utilizado para reconhecer o reconhecimento de voz nos ficheiros de áudio o conjunto de dados e os resultados testados relativamente o texto correspondente. Para obter os melhores resultados, utilize um conjunto de dados acústica diferente daquela que utilizou para criar o modelo.

## <a name="custom-endpoint"></a>Ponto final personalizado

Depois de criar modelos acústica personalizados ou modelos de idioma, pode implementá-las para um personalizado **reconhecimento de voz para texto** ponto final. Apenas a conta que criou um ponto final tem permissão para efetuar chamadas ao mesmo.

Para criar um ponto final, escolha **implementações** do **reconhecimento de voz personalizadas** menu na parte superior da página. O **implementações** página contém uma tabela de pontos finais personalizados atuais, se tiver criado quaisquer. Clique em **criar novo** para criar um novo ponto final.

Escolha os modelos que pretende utilizar no **modelo acústica** e **idioma modelo** apresenta uma lista. As escolhas disponíveis incluem sempre os modelos base do Microsoft. Não pode combinar conversational modelos com a pesquisa e modelos da sua organização ditarem, por isso, escolher um modelo acústica limita os modelos de idioma disponível e vice-versa. Pode utilizar os mesmos modelos em qualquer número de pontos finais.

Clique em **criar** depois de escolher os modelos. O novo ponto final poderá demorar até 30 minutos para aprovisionar.

Quando o ponto final estiver pronto, selecione-o no **implementações** tabela para ver o ID de URI e a implementação. Pode utilizar os pontos finais personalizados com o [Rest API](rest-apis.md#speech-to-text) e [SDK de reconhecimento de voz](speech-sdk.md). O [exemplos de código](samples.md) incluem um exemplo de utilização de uma voz personalizada para o ponto final de texto.

## <a name="next-steps"></a>Passos Seguintes

- [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
- [Como reconhecer voz em c#](quickstart-csharp-windows.md)
