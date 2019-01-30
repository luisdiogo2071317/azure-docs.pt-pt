---
title: Tutorial para criar um modelo acústico com o Serviço de Voz Personalizada - Serviços Cognitivos da Microsoft | Microsoft Docs
description: Neste tutorial, vai aprender a criar um modelo acústico com o Serviço de Voz Personalizada nos Serviços Cognitivos da Microsoft.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: panosper
ms.openlocfilehash: 0e4c21a064cdb0a60aef49482eee4b768112b899
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55216426"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Tutorial: Criar um modelo acústico personalizado

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Neste tutorial, vai criar um modelo acústico personalizado para os dados de voz que espera que a sua aplicação reconheça. A criação de um modelo acústico personalizado é útil se a sua aplicação for concebida para ser utilizada num ambiente específico, como uma fábrica barulhenta, bem como por uma população de utilizadores em particular.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Preparar os dados
> * Importar o conjunto de dados acústico
> * Criar o modelo acústico personalizado

Se não tiver uma conta dos Serviços Cognitivos, crie uma [conta gratuita](https://cris.ai) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que a conta dos Serviços Cognitivos está associada a uma subscrição, abrindo a página [Subscrições de Serviços Cognitivos](https://cris.ai/Subscriptions).

Se não existirem subscrições listadas, pode configurar os Serviços Cognitivos para lhe criarem uma conta, clicando no botão **Get free subscription** (Obter subscrição gratuita). Em alternativa, pode ligar a uma subscrição do Custom Search Service criada no portal do Azure, clicando no botão **Connect existing subscription** (Ligar subscrição existente).

Para obter informações sobre como criar uma subscrição do Custom Search Service no portal do Azure, veja [Criar uma conta de APIs Serviços Cognitivos no portal do Azure](../../cognitive-services-apis-create-account.md).

## <a name="prepare-the-data"></a>Preparar os dados

Para personalizar o modelo acústico para um determinado domínio, é precisa uma coleção de dados de voz. Esta coleção é composta por um conjunto de ficheiros de áudio de dados de voz e um ficheiro de texto de transcrições de cada ficheiro de áudio. Os dados de áudio devem ser representativos do cenário em que gostaria de utilizar o reconhecedor.

Por exemplo:

*   Se quiser reconhecer melhor a voz num ambiente de fábrica ruidoso, os ficheiros de áudio devem ser constituídos por pessoas a falar numa fábrica ruidosa.
<a name="Preparing data to customize the acoustic model"></a>
*   Se estiver interessado na otimização do desempenho para um único orador, por exemplo, se quiser transcrever todas as Conversas Informais de FDR, os ficheiros de áudio devem ter muitos exemplos apenas desse orador.

Um conjunto de dados acústico para personalizar o modelo acústico é constituído por duas partes: (1) um conjunto de arquivos de áudio que contém os dados de voz e (2) um ficheiro que contenha as transcrições de todos os arquivos de áudio.

### <a name="audio-data-recommendations"></a>Recomendações de dados de áudio

*   Todos os ficheiros de áudio no conjunto de dados devem ser armazenados no formato de áudio WAV (RIFF).
*   O áudio tem de ter uma taxa de amostragem de 8 kHz ou 16 kHz e os valores de exemplo devem ser armazenados como números inteiros com sinal de 16 bits PCM descomprimidos.
*   Apenas são suportados ficheiros de áudio de canal único (mono).
*   Os ficheiros de áudio têm de ter entre 100 ms e 1 minuto de duração. Idealmente, cada ficheiro de áudio deve começar e terminar com, pelo menos, 100 ms de silêncio e é comum um valor entre 500 ms e 1 segundo.
*   Se tiver ruído de fundo nos seus dados, é recomendado ter também alguns exemplos com segmentos de silêncio mais longos, por exemplo, alguns segundos, nos dados, antes e/ou após o conteúdo de voz.
*   Cada ficheiro de áudio deve consistir numa única expressão, por exemplo, uma frase para ditado, uma consulta ou um elemento de um sistema de diálogo.
*   Cada ficheiro de áudio no conjunto de dados deve ter um nome de ficheiro exclusivo e a extensão "wav".
*   O conjunto de ficheiros de áudio deve ser colocado numa única pasta sem subdiretórios e empacotado como um único arquivo de ficheiros ZIP.

> [!NOTE]
> As importações de dados através do portal Web estão atualmente limitadas a 2 GB, pelo que este é o tamanho máximo de um conjunto de dados acústico. Isto corresponde a cerca de 17 horas de áudio gravadas a 16 kHz ou 34 horas de áudio gravadas a 8 kHz. Os requisitos principais para os dados de áudio estão resumidos na tabela seguinte.
>

| Propriedade | Valor |
|---------- |----------|
| Formato do Ficheiro | RIFF (WAV) |
| Taxa de Amostragem | 8000 Hz ou 16.000 Hz |
| Canais | 1 (mono) |
| Formato de Exemplo | PCM, números inteiros de 16 bits |
| Duração do Ficheiro | 0,1 segundos < duração < 60 segundos |
| Colar de Silêncio | > 0,1 segundos |
| Formato do Arquivo | Zip |
| Tamanho Máximo do Arquivo | 2 GB |

### <a name="transcriptions"></a>Transcrições

As transcrições para todos os ficheiros WAV devem estar contidas num único ficheiro de texto simples. Cada linha do ficheiro de transcrição deve ter o nome de um dos ficheiros de áudio, seguido da transcrição correspondente. O nome de ficheiro e a transcrição devem estar separados por uma tabulação (\t).

  Por exemplo:
```
  speech01.wav  speech recognition is awesome

  speech02.wav  the quick brown fox jumped all over the place

  speech03.wav  the lazy dog was not amused
```

As transcrições terão texto normalizado para que possam ser processadas pelo sistema. No entanto, existem algumas normalizações importantes que devem ser feitas pelo utilizador _antes_ de carregar os dados para o Serviço de Voz Personalizada. Consulte a secção de [diretrizes de transcrição](cognitive-services-custom-speech-transcription-guidelines.md) para o idioma apropriado quando preparar as suas transcrições.

Os passos seguintes são realizados através do [Portal do Serviço de Voz Personalizada](https://cris.ai). 

## <a name="import-the-acoustic-data-set"></a>Importar o conjunto de dados acústico

Assim que os ficheiros de áudio e as transcrições tiverem sido preparados, estão prontos para serem importados para o portal Web do serviço.

Para tal, certifique-se primeiro de que tem sessão iniciada no [Portal do Serviço de Voz Personalizada](https://cris.ai). Em seguida, clique no menu pendente "Voz Personalizada" no friso superior e selecione "Dados de Adaptação". Se for a primeira vez que está a carregar dados para o Serviço de Voz Personalizada, verá uma tabela vazia chamada "Conjuntos de Dados". 

Clique no botão "Importar" na linha "Conjuntos de Dados Acústicos" e o site apresenta uma página para carregar um novo conjunto de dados.

![experimente](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-import.png)

Introduza um _Nome_ e uma _Descrição_ nas caixas de texto apropriadas. Estas são úteis para manter o controlo dos vários conjuntos de dados carregados. Em seguida, clique em "Escolher Ficheiro" para "Ficheiro de Transcrição" e "Ficheiros WAV", e selecione o ficheiro de transcrição de texto simples e o arquivo zip dos ficheiros WAV, respetivamente. Quando estiver concluída, clique em "Importar" para carregar os dados. Em seguida, os dados serão carregados. Para conjuntos de dados maiores, este processo pode demorar vários minutos.

Quando o carregamento estiver concluído, voltará à tabela "Conjuntos de Dados Acústicos" e verá uma entrada que corresponde ao seu conjunto de dados acústicos. Tenha em atenção que foi atribuído um ID exclusivo (GUID) ao conjunto de dados. Os dados também terão um estado que reflete o estado atual dos mesmos. O estado será "Waiting" (A aguardar) enquanto estiverem a ser colocados em fila para processamento, "Processing" (A processar) enquanto estiverem a ser submetidos a validação e "Complete" (Concluído) quando os dados estiverem prontos para utilização.

A validação de dados inclui uma série de verificações nos ficheiros de áudio para verificar o formato de ficheiro, a duração e a taxa de amostragem, e nos ficheiros de transcrição para verificar o formato de ficheiro e efetuar a normalização de texto.

Quando o estado for "Complete" (Concluído), pode clicar em "Detalhes" para ver o relatório de verificação de dados acústicos. O número de expressões que passaram e falharam na verificação será apresentado, juntamente com os detalhes sobre as expressões que falharam. No exemplo abaixo, dois ficheiros WAV falharam a verificação devido a formato de áudio impróprio (neste conjunto de dados, um tinha uma taxa de amostragem incorreta e o outro tinha o formato de ficheiro incorreto).

![experimente](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-datasets-report.png)

Em algum momento, se quiser alterar o Nome ou a Descrição do conjunto de dados, pode clicar na ligação "Editar" e alterar estas entradas. Note que não pode modificar os ficheiros de áudio ou as transcrições.

## <a name="create-a-custom-acoustic-model"></a>Criar um modelo acústico personalizado

Quando o estado do conjunto de dados acústicos for "Complete" (Concluído), pode ser utilizado para criar um modelo acústico personalizado. Para tal, clique em "Modelos Acústicos" no menu pendente "Voz Personalizada". Verá uma tabela chamada "Os seus modelos" com todos os modelos acústicos personalizados. Esta tabela estará vazia se for a sua primeira utilização. A região atual é mostrada no título da tabela. Atualmente, os modelos acústicos podem ser criados apenas para inglês dos EUA.

Para criar um novo modelo, clique me "Criar Novo", abaixo do título da tabela. Tal como anteriormente, introduza um nome e uma descrição para ajudar a identificar este modelo. Por exemplo, o campo "Descrição" pode servir para registar o modelo inicial e o conjunto de dados acústicos que servem para criar o modelo. Em seguida, selecione um "Modelo Acústico Base" no menu pendente. O modelo base é o modelo que é o ponto de partida para a sua personalização. Pode escolher entre dois modelos acústicos base. O modelo _Microsoft Search and Dictation AM_ é adequado para voz direcionada para uma aplicação, como comandos, consultas de pesquisa ou ditado. O modelo _Microsoft Conversational_ é adequado para reconhecer voz falada num estilo de conversação. Este tipo de voz é, normalmente, direcionado para outra pessoa e ocorre em centros de atendimento telefónico ou reuniões. Note que a latência de resultados parciais em modelos de Conversação é maior do que em modelos de Pesquisa e Ditado.

Em seguida, selecione os dados acústicos que quer utilizar para efetuar a personalização através do menu pendente.

![experimente](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-create2.png)

Opcionalmente, pode optar por realizar testes offline do novo modelo quando o processamento estiver concluído. Será executada uma avaliação de voz em texto num conjunto de dados acústicos especificado com o modelo acústico personalizado e os resultados reportados. Para executar este teste, selecione a caixa de verificação "Testes de Precisão". Em seguida, selecione um modelo de idioma no menu pendente. Se não criou quaisquer modelos de idioma personalizados, apenas os modelos de idioma base estarão na lista pendente. Veja a [descrição](cognitive-services-custom-speech-create-language-model.md) dos modelos de idioma base no guia e selecione o que for mais adequado.

Por fim, selecione o conjunto de dados acústicos que quer utilizar para avaliar o modelo personalizado. Se efetuar testes de precisão, é importante selecionar um conjunto de dados acústicos diferente do utilizado na criação do modelo para obter uma noção realista do desempenho do modelo. Tenha também em atenção que os testes offline estão limitados a 1000 expressões. Se o conjunto de dados acústicos para teste for maior, apenas serão avaliadas as primeiras 1000 expressões.

Quando estiver pronto para começar a executar o processo de personalização, prima "Criar".

Verá agora uma nova entrada na tabela de modelos acústicos correspondente a este novo modelo. O estado do processo está refletido na tabela. Os estados são "Waiting" (A Aguardar), "Processing" (Em Processamento) e "Complete" (Concluído).

![experimente](../../../media/cognitive-services/custom-speech-service/custom-speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, desenvolveu um modelo acústico personalizado para utilizar com ficheiros de áudio e transcrições. Para criar um ficheiro de idioma personalizado para utilizar com ficheiros de texto, avance para o tutorial sobre como criar um modelo de linguagem personalizado.

> [!div class="nextstepaction"]
> [Criar um modelo de linguagem personalizado](cognitive-services-custom-speech-create-language-model.md)
