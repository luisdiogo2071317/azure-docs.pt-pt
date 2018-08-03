---
title: Como criar um modelo de linguagem com o Serviço de Voz - Serviços Cognitivos da Microsoft
description: Saiba como criar um modelo de linguagem com o Serviço de Voz nos Serviços Cognitivos da Microsoft.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 0161a691cbec45a87ade218d1707a2784d7f1cfc
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283889"
---
# <a name="tutorial-create-a-custom-language-model"></a>Tutorial: Criar um modelo de linguagem personalizado

Neste documento, pode criar um modelo de linguagem personalizado, que pode utilizar em seguida em conjunto com os modelos de voz de última geração existentes da Microsoft para adicionar interação de voz à sua aplicação.

O documento aborda como:
> [!div class="checklist"]
> * Preparar os dados
> * Importar o conjunto de dados de linguagem
> * Criar o modelo de linguagem personalizado

Se não tiver uma conta dos Serviços Cognitivos, crie uma [conta gratuita](https://azure.microsoft.com/try/cognitive-services/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que a conta dos Serviços Cognitivos está associada a uma subscrição, abrindo a página [Subscrições de Serviços Cognitivos](https://customspeech.ai/Subscriptions).

Pode ligar a uma subscrição do Serviço de Voz criada no portal do Azure ao clicar no botão **Ligar subscrição existente**.

Para obter informações sobre como criar uma subscrição do Serviço de Voz no portal do Azure, veja a página [get-started](get-started.md).

## <a name="prepare-the-data"></a>Preparar os dados

Para criar um modelo de linguagem personalizado para a sua aplicação, terá de fornecer uma lista de expressões de exemplo ao sistema, por exemplo:

*   "o paciente teve urticária na semana passada."
*   "O paciente teve uma cicatriz de uma herniorrafia que cicatrizou bem."

As frases não têm de ser frases completas ou gramaticamente corretas e devem refletir com precisão a entrada de voz que espera que o sistema deve encontrar na implementação. Estes exemplos devem refletir o estilo e o conteúdo da tarefa que os utilizadores irão executar com a sua aplicação.

Os dados do modelo de linguagem devem ser gravados no UTF-8 BOM. O ficheiro de texto deve conter um exemplo (frase, expressão ou consulta) por linha.

Se desejar que alguns termos tenham uma ponderação mais elevada (importância), pode adicionar várias expressões que incluem esse termo aos seus dados. 

Os requisitos principais para os dados de linguagem estão resumidos na tabela seguinte.

| Propriedade | Valor |
|----------|-------|
| Codificação de Texto | UTF-8 BOM|
| n.º de Expressões por linha | 1 |
| Tamanho Máximo do Ficheiro | 1,5 GB |
| Observações | evitar repetir os carateres mais de quatro vezes, por exemplo «aaaaa»|
| Observações | não existem carateres especiais, como «\t» ou qualquer outro caráter UTF-8 acima de U+00A1 na [tabela de carateres Unicode](http://www.utf8-chartable.de/)|
| Observações | os URIs também serão rejeitados porque não existe uma forma única de pronunciar um URI|

Quando o texto é importado, é texto normalizado para que possa ser processado pelo sistema. No entanto, existem algumas normalizações importantes que devem ser feitas pelo utilizador _antes_ de carregar os dados. Veja as [Diretrizes de transcrição](prepare-transcription.md) para determinar a linguagem apropriada quando preparar os dados de linguagem.

## <a name="language-support"></a>Suporte de idiomas

São suportados os seguintes idiomas para os modelos de idioma **Conversão de Voz em Texto** personalizados.

Clique para obter uma lista completa dos [Idiomas suportados](supported-languages.md)

## <a name="import-the-language-data-set"></a>Importar o conjunto de dados de linguagem

Clique no botão Importar na linha Conjuntos de Dados da Linguagem e o site apresenta uma página para carregar um novo conjunto de dados.

Quando estiver pronto para importar o seu conjunto de dados de linguagem, inicie sessão no [Portal do Serviço de Voz](https://customspeech.ai).  Em seguida, clique no menu pendente "Custom Speech" (Voz Personalizada) no friso superior e selecione "Adaptation Data" (Dados de Adaptação). Quando alguém tentar pela primeira vez carregar dados para o Serviço de Voz, verá uma tabela vazia chamada Conjuntos de dados.

Para importar um novo conjunto de dados, clique no botão "Import" (Importar) na linha "Language Datasets" (Conjuntos de dados de linguagem) e o site apresenta uma página para carregar um novo conjunto de dados. Introduza um Nome e uma Descrição para ajudar a identificar o conjunto de dados no futuro e escolha o local. Em seguida, utilize o botão "Choose File" (Escolher ficheiro) para localizar o ficheiro de texto dos dados de linguagem. Depois disso, clique em "Import" (Importar) e o conjunto de dados será carregado. Dependendo do tamanho do conjunto de dados, a importação pode demorar vários minutos.

![experimente](media/stt/speech-language-datasets-import.png)

Quando a importação estiver concluída, voltará à tabela de dados de linguagem e verá uma entrada que corresponde ao seu conjunto de dados de linguagem. Tenha em atenção que foi atribuído um ID exclusivo (GUID) ao conjunto de dados. Os dados também terão um estado que reflete o estado atual dos mesmos. O estado será "Waiting" (A aguardar) enquanto estiverem a ser colocados em fila para processamento, "Processing" (A processar) enquanto estiverem a ser submetidos a validação e "Complete" (Concluído) quando os dados estiverem prontos para utilização. A validação de dados realiza uma série de verificações no texto no ficheiro e alguma normalização de texto dos dados.

Quando o estado for "Complete" (Concluído), pode clicar em "View Report" (Ver Relatório) para ver o relatório de verificação de dados de linguagem. O número de expressões que passaram e falharam na verificação é apresentado, juntamente com os detalhes sobre as expressões que falharam. No exemplo abaixo, dois exemplos falharam a verificação devido a carateres incorretos (neste conjunto de dados, a primeira linha tinha dois carateres de tabulação, a segunda tinha vários carateres fora do conjunto de carateres imprimíveis ASCII e a terceira linha estava em branco).

![experimente](media/stt/speech-language-datasets-report.png)

Quando o estado do conjunto de dados de linguagem for "Complete" (Concluído), pode ser utilizado para criar um modelo de linguagem personalizado.

![try](media/stt/speech-language-datasets.png)

## <a name="create-a-custom-language-model"></a>Criar um modelo de linguagem personalizado

Quando os dados de linguagem estiverem prontos, clique em "Language Models" (Modelos de Linguagem) no menu pendente "Menu" para iniciar o processo de criação do modelo de linguagem personalizado. Esta página contém uma tabela chamada "Language Models" (Modelos de Linguagem) com os seus atuais modelos de linguagem personalizados. Se ainda não tiver criado modelos de linguagem personalizados, a tabela estará vazia. A região atual é mostrada na tabela junto à entrada de dados relevantes.

Tem de selecionar a região apropriada antes de realizar qualquer ação. A região atual é indicada no título da tabela em todas as páginas de dados, modelo e implementação. Para alterar a região, clique no botão "Alterar região" que se encontra sob o título da tabela que o irá encaminhar para uma página de confirmação de região. Clique em "OK" para regressar à tabela.

Na página "Create Language Model" (Criar Modelo de Linguagem), introduza um nome em "Name" e uma descrição em "Description" para ajudá-lo a manter-se a par de informações pertinentes sobre este modelo, tal como o conjunto de dados utilizado. Em seguida, selecione o “Base Language Model” (Modelo de Linguagem Base) no menu pendente. Este modelo será o ponto de partida para a sua personalização. Pode escolher entre dois modelos de linguagem base. O modelo Pesquisa e Ditado é adequado para voz direcionada para uma aplicação, como comandos, consultas de pesquisa ou ditado. O modelo Conversação é adequado para reconhecer voz falada num estilo de conversação. Este tipo de voz é, normalmente, direcionado para outra pessoa e ocorre em centros de atendimento telefónico ou reuniões. Um novo modelo chamado "Universal" também está publicamente disponível. O Universal tem como objetivo lidar com todos os cenários e, eventualmente, substituir os modelos de Pesquisa e Ditado e da Conversação.

5.  No exemplo abaixo, depois de especificar o modelo de linguagem base, selecione o conjunto de dados de linguagem que pretende utilizar para a personalização com o menu pendente "Dados de Linguagem"

![try](media/stt/speech-language-models-create2.png)

Tal como acontece com a criação de modelos acústicos, pode optar por realizar testes offline do novo modelo quando o processamento estiver concluído. As avaliações de modelo precisam de um conjunto de dados acústico.

Para realizar testes offline do seu modelo de linguagem, selecione a caixa de verificação junto a "Testes Offline". Em seguida, selecione um modelo acústico no menu pendente. Se ainda não tiver criado modelos acústicos personalizados, o modelo acústico base da Microsoft será o único modelo no menu. No caso de ter selecionado um modelo base de LM de conversação, terá de utilizar um modelo AM de conversação aqui. Caso utilize um modelo LM de pesquisa e ditado, terá de selecionar um modelo AM de pesquisa e ditado.

Por fim, selecione o conjunto de dados acústicos que pretende utilizar para realizar a avaliação.

Quando estiver pronto para começar a processar, prima "Criar", que o irá encaminhar para a tabela de modelos de linguagem. Haverá uma nova entrada na tabela correspondente a este modelo. O estado reflete o estado do modelo e irá passar por vários estados, incluindo "Waiting" (A aguardar), "Processing" (A processar) e "Complete" (Concluído).

Quando o modelo chegar ao estado "Complete", pode ser implementado num ponto final. Clicar em "View Result" (Ver Resultado) irá mostrar os resultados dos testes offline, caso sejam realizados.

Se, em determinado momento, quiser alterar o Nome e a Descrição do modelo, pode utilizar a hiperligação "Edit" (Editar) na linha adequada da tabela de modelos de linguagem.

## <a name="next-steps"></a>Passos seguintes

- [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Como reconhecer voz em C#](quickstart-csharp-dotnet-windows.md)
- [Dados de Exemplo do Git](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
