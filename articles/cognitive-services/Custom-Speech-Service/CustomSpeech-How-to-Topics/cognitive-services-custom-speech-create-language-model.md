---
title: 'Tutorial: criar um modelo de idioma - Serviço de Voz Personalizada'
titlesuffix: Azure Cognitive Services
description: Neste tutorial, vai aprender a criar um modelo de linguagem com o Serviço de Voz Personalizada.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: b7216688efb6a2124748cd08111d57b122085143
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219322"
---
# <a name="tutorial-create-a-custom-language-model"></a>Tutorial: Criar um modelo de linguagem personalizado

Neste tutorial, vai criar um modelo de linguagem personalizado para consultas de texto ou expressões que espera que os utilizadores digam ou escrevam numa aplicação. Em seguida, pode utilizar este modelo de linguagem personalizado em conjunto com os modelos de voz de última geração existentes da Microsoft para adicionar interação de voz à sua aplicação.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Preparar os dados
> * Importar o conjunto de dados de linguagem
> * Criar o modelo de linguagem personalizado

Se não tiver uma conta dos Serviços Cognitivos, crie uma [conta gratuita](https://cris.ai) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que a conta dos Serviços Cognitivos está associada a uma subscrição, abrindo a página [Subscrições de Serviços Cognitivos](https://cris.ai/Subscriptions).

Se não existirem subscrições listadas, pode configurar os Serviços Cognitivos para lhe criarem uma conta, clicando no botão **Get free subscription** (Obter subscrição gratuita). Em alternativa, pode ligar a uma subscrição do Custom Search Service criada no portal do Azure, clicando no botão **Connect existing subscription** (Ligar subscrição existente).

Para obter informações sobre como criar uma subscrição do Custom Search Service no portal do Azure, veja [Criar uma conta de APIs Serviços Cognitivos no portal do Azure](../../cognitive-services-apis-create-account.md).

## <a name="prepare-the-data"></a>Preparar os dados

Para criar um modelo de linguagem personalizado para a sua aplicação, terá de fornecer uma lista de expressões de exemplo ao sistema, por exemplo:

*   "Ele teve urticária na semana passada."
*   "O paciente teve uma cicatriz de uma herniorrafia que cicatrizou bem."

As frases não têm de ser frases completas ou gramaticamente corretas e devem refletir com precisão a entrada de voz que espera que o sistema encontre na implementação. Estes exemplos devem refletir o estilo e o conteúdo da tarefa que os utilizadores irão executar com a sua aplicação.

Os dados do modelo de linguagem devem ser escritos num ficheiro de texto simples com a codificação US-ASCII ou UTF-8, consoante a região. Para a região en-US, ambas as codificações são suportadas. Para a região zh-CN, apenas é suportada a codificação UTF-8 (a marca de ordem de bytes é opcional). O ficheiro de texto deve conter um exemplo (frase, expressão ou consulta) por linha.

Se pretender que algumas frases tenham uma ponderação superior (importância), pode adicioná-las várias vezes aos seus dados. Um número satisfatório de repetições está compreendido entre 10 e 100. Se o normalizar para 100, pode determinar facilmente a importância da frase.

Os requisitos principais para os dados de linguagem estão resumidos na tabela seguinte.

| Propriedade | Valor |
|----------|-------|
| Codificação de Texto | en-US: US-ACSII ou UTF-8 ou zh-CN: UTF-8|
| n.º de Expressões por linha | 1 |
| Tamanho Máximo do Ficheiro | 200 MB |
| Observações | evitar repetir os carateres mais de 4 vezes, por exemplo «aaaaa»|
| Observações | não existem carateres especiais, como «\t» ou qualquer outro caráter UTF-8 acima de U+00A1 na [tabela de carateres Unicode](http://www.utf8-chartable.de/)|
| Observações | os URIs também serão rejeitados porque não existe uma forma única de pronunciar um URI|

Quando o texto é importado, é texto normalizado para que possa ser processado pelo sistema. No entanto, existem algumas normalizações importantes que devem ser feitas pelo utilizador _antes_ de carregar os dados. Veja as [Diretrizes de transcrição](cognitive-services-custom-speech-transcription-guidelines.md) para determinar a linguagem apropriada quando preparar os dados de linguagem.

## <a name="import-the-language-data-set"></a>Importar o conjunto de dados de linguagem

Clique no botão "Import" (Importar) na linha "Acoustic Datasets" (Conjuntos de dados acústicos) e o site apresenta uma página para carregar um novo conjunto de dados.

Quando estiver pronto para importar o seu conjunto de dados de linguagem, inicie sessão no [Portal do Serviço de Voz Personalizada](https://cris.ai).  Em seguida, clique no menu pendente "Custom Speech" (Voz Personalizada) no friso superior e selecione "Adaptation Data" (Dados de Adaptação). Se for a primeira vez que está a carregar dados para o Serviço de Voz Personalizada, verá uma tabela vazia chamada "Datasets" (Conjuntos de dados).

Para importar um novo conjunto de dados, clique no botão "Import" (Importar) na linha "Language Datasets" (Conjuntos de dados de linguagem) e o site apresenta uma página para carregar um novo conjunto de dados. Introduza um Nome e uma Descrição para ajudar a identificar o conjunto de dados no futuro. Em seguida, utilize o botão "Choose File" (Escolher ficheiro) para localizar o ficheiro de texto dos dados de linguagem. Depois disso, clique em "Import" (Importar) e o conjunto de dados será carregado. Dependendo do tamanho do conjunto de dados, este processo pode demorar vários minutos.

![try](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-import.png)

Quando a importação estiver concluída, voltará à tabela de dados de linguagem e verá uma entrada que corresponde ao seu conjunto de dados de linguagem. Tenha em atenção que foi atribuído um ID exclusivo (GUID) ao conjunto de dados. Os dados também terão um estado que reflete o estado atual dos mesmos. O estado será "Waiting" (A aguardar) enquanto estiverem a ser colocados em fila para processamento, "Processing" (A processar) enquanto estiverem a ser submetidos a validação e "Complete" (Concluído) quando os dados estiverem prontos para utilização. A validação de dados realiza uma série de verificações no texto no ficheiro e alguma normalização de texto dos dados.

Quando o estado for "Complete" (Concluído), pode clicar em "View Report" (Ver Relatório) para ver o relatório de verificação de dados de linguagem. O número de expressões que passaram e falharam na verificação é apresentado, juntamente com os detalhes sobre as expressões que falharam. No exemplo abaixo, dois exemplos falharam a verificação devido a carateres incorretos (neste conjunto de dados, o primeiro tinha dois ícones expressivos e o segundo tinha vários carateres fora do conjunto de carateres imprimíveis ASCII).

![try](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets-report.png)

Quando o estado do conjunto de dados de linguagem for "Complete" (Concluído), pode ser utilizado para criar um modelo de linguagem personalizado.

![try](../../../media/cognitive-services/custom-speech-service/custom-speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Criar um modelo de linguagem personalizado

Quando os dados de linguagem estiverem prontos, clique em "Language Models" (Modelos de Linguagem) no menu pendente "Menu" para iniciar o processo de criação do modelo de linguagem personalizado. Esta página contém uma tabela chamada "Language Models" (Modelos de Linguagem) com os seus atuais modelos de linguagem personalizados. Se ainda não tiver criado modelos de linguagem personalizados, a tabela estará vazia. A região atual está refletida no título da tabela. Se pretender criar um modelo de linguagem para um idioma diferente, clique em "Change Locale" (Alterar Região). Pode encontrar informações adicionais sobre os idiomas suportados na secção sobre como [Alterar a Região](cognitive-services-custom-speech-change-locale.md). Para criar um novo modelo, clique na ligação "Create New" (Criar Novo), abaixo do título da tabela.

Na página "Create Language Model" (Criar Modelo de Linguagem), introduza um nome em "Name" e uma descrição em "Description" para ajudá-lo a manter-se a par de informações pertinentes sobre este modelo, tal como o conjunto de dados utilizado. Em seguida, selecione o “Base Language Model” (Modelo de Linguagem Base) no menu pendente. Este modelo será o ponto de partida para a sua personalização. Pode escolher entre dois modelos de linguagem base. O modelo _Microsoft Search and Dictation LM_ é adequado para voz direcionada para uma aplicação, como comandos, consultas de pesquisa ou ditado. O modelo _Microsoft Conversational LM_ é adequado para reconhecer voz falada num estilo de conversação. Este tipo de voz é, normalmente, direcionado para outra pessoa e ocorre em centros de atendimento telefónico ou reuniões.

Depois de especificar o modelo de linguagem base, selecione o conjunto de dados de linguagem que pretende utilizar para a personalização com o menu pendente de "Language Data" (Dados de Linguagem)

![try](../../../media/cognitive-services/custom-speech-service/custom-speech-language-models-create2.png)

Tal como acontece com a criação de modelos acústicos, pode optar por realizar testes offline do novo modelo quando o processamento estiver concluído. Uma vez que se trata de uma avaliação do desempenho da conversão de voz em texto, os testes offline precisam de um conjunto de dados acústicos.

Para realizar testes offline do seu modelo de linguagem, selecione a caixa de verificação junto a "Testes Offline". Em seguida, selecione um modelo acústico no menu pendente. Se ainda não tiver criado modelos acústicos personalizados, o modelo acústico base da Microsoft será o único modelo no menu. No caso de ter selecionado um modelo base de LM de conversação, terá de utilizar um modelo AM de conversação aqui. Caso utilize um modelo LM de pesquisa e ditado, terá de selecionar um modelo AM de pesquisa e ditado.

Por fim, selecione o conjunto de dados acústicos que pretende utilizar para realizar a avaliação.

Quando estiver pronto para começar a processar, prima "Create" (Criar). Desta forma, regressa à tabela de modelos de linguagem. Haverá uma nova entrada na tabela correspondente a este modelo. O estado reflete o estado do modelo e irá passar por vários estados, incluindo "Waiting" (A aguardar), "Processing" (A processar) e "Complete" (Concluído).

Quando o modelo chegar ao estado "Complete", pode ser implementado num ponto final. Clicar em "View Result" (Ver Resultado) irá mostrar os resultados dos testes offline, caso sejam realizados.

Se, em determinado momento, quiser alterar o Nome e a Descrição do modelo, pode utilizar a hiperligação "Edit" (Editar) na linha adequada da tabela de modelos de linguagem.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, desenvolveu um modelo de linguagem personalizado para utilizar com texto. Para criar um modelo acústico personalizado para utilizar com ficheiros de áudio e transcrições, avance para o tutorial sobre como criar um modelo acústico.

> [!div class="nextstepaction"]
> [Criar um modelo acústico personalizado](cognitive-services-custom-speech-create-acoustic-model.md)
