---
title: Como criar um modelo acústico com o Serviço de Voz - Serviços Cognitivos da Microsoft | Microsoft Docs
description: Saiba como criar um modelo acústico com o Serviço de Voz nos Serviços Cognitivos da Microsoft.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: speech-service
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: panosper
ms.openlocfilehash: 3e247b6f81a9306cbad630d42c3a41d8ce3045a5
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37345129"
---
# <a name="tutorial-create-a-custom-acoustic-model"></a>Tutorial: Criar um modelo acústico personalizado

A criação de um modelo acústico personalizado é útil se a sua aplicação for concebida para ser utilizada num ambiente específico, como um automóvel, com condições ou dispositivos de gravação específicos, bem como por uma população de utilizadores em particular. Os exemplos envolvem voz acentuada, ruídos de fundo específicos ou utilizar um microfone específico para gravação.

Nesta página, ficará a saber como:
> [!div class="checklist"]
> * Preparar os dados
> * Importar o conjunto de dados acústico
> * Criar o modelo acústico personalizado

Se não tiver uma conta dos Serviços Cognitivos, crie uma [conta gratuita](https://azure.microsoft.com/try/cognitive-services) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Certifique-se de que a conta dos Serviços Cognitivos está associada a uma subscrição ao abrir a página [Subscrições de Serviços Cognitivos](https://customspeech.ai/Subscriptions).

Pode ligar a uma subscrição do Serviço de Voz criada no portal do Azure ao clicar no botão **Ligar subscrição existente**.

Para obter informações sobre como criar uma subscrição do Serviço de Voz no portal do Azure, veja a página [get-started](get-started.md).

## <a name="prepare-the-data"></a>Preparar os dados

Para personalizar um modelo acústico para um determinado domínio, é necessária uma coleção de dados de voz. Isto pode variar entre duas expressões e várias centenas de horas de voz. Esta coleção é composta por um conjunto de ficheiros de áudio de dados de voz e um ficheiro de texto de transcrições de cada ficheiro de áudio. Os dados de áudio devem ser representativos do cenário em que gostaria de utilizar o reconhecedor.

Por exemplo:

*   Se quiser reconhecer melhor a voz num ambiente de fábrica ruidoso, os ficheiros de áudio devem ser constituídos por pessoas a falar numa fábrica ruidosa.
*   Se estiver interessado na otimização do desempenho para um único orador, por exemplo, se quiser transcrever todas as Conversas Informais de FDR, os ficheiros de áudio devem ter muitos exemplos apenas desse orador.

Um conjunto de dados acústico para personalizar o modelo acústico é constituído por duas partes: (1) um conjunto de ficheiros de áudio que contém os dados de voz e (2) um ficheiro com as transcrições de todos os ficheiros de áudio.

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
| Taxa de Amostragem | 8000 Hz ou 16 000 Hz |
| Canais | 1 (mono) |
| Formato de Exemplo | PCM, números inteiros de 16 bits |
| Duração do Ficheiro | 0,1 segundos < duração < 60 segundos |
| Colar de Silêncio | > 0,1 segundos |
| Formato do Arquivo | Zip |
| Tamanho Máximo do Arquivo | 2 GB |

## <a name="language-support"></a>Suporte de idiomas

São suportados os seguintes idiomas para os modelos de idioma **Conversão de Voz em Texto** personalizados.

Clique para obter uma lista completa dos [Idiomas suportados](supported-languages.md)

### <a name="transcriptions-for-the-audio-dataset"></a>Transcrições do conjunto de dados de áudio

As transcrições para todos os ficheiros WAV devem estar contidas num único ficheiro de texto simples. Cada linha do ficheiro de transcrição deve ter o nome de um dos ficheiros de áudio, seguido da transcrição correspondente. O nome de ficheiro e a transcrição devem estar separados por uma tabulação (\t).

  Por exemplo:
```
  speech01.wav  speech recognition is awesome
  speech02.wav  the quick brown fox jumped all over the place
  speech03.wav  the lazy dog was not amused
```
> [!NOTE]
> A transcrição deve ser codificada como UTF-8 Bom

As transcrições terão texto normalizado para que possam ser processadas pelo sistema. No entanto, existem algumas normalizações importantes que devem ser feitas pelo utilizador _antes_ de carregar os dados para o Serviço de Voz Personalizada. Consulte a secção de [diretrizes de transcrição](prepare-transcription.md) para o idioma apropriado quando preparar as suas transcrições.

Os passos seguintes são efetuados através do [Portal do Serviço de Voz](https://customspeech.ai).

## <a name="import-the-acoustic-data-set"></a>Importar o conjunto de dados acústico

Assim que os ficheiros de áudio e as transcrições tiverem sido preparados, estão prontos para serem importados para o portal Web do serviço.

Para tal, certifique-se primeiro de que tem sessão iniciada no [Portal do Serviço de Voz](https://customspeech.ai). Em seguida, clique no menu pendente "Voz Personalizada" no friso superior e selecione "Dados de Adaptação". Se for a primeira vez que está a carregar dados para o Serviço de Voz Personalizada, verá uma tabela vazia chamada "Conjuntos de Dados". 

Clique no botão "Importar" na linha "Conjuntos de Dados Acústicos" e o site apresenta uma página para carregar um novo conjunto de dados.

![experimente](media/stt/speech-acoustic-datasets-import.png)

Introduza um _Nome_ e uma _Descrição_ nas caixas de texto apropriadas. As descrições amigáveis são úteis para manter o controlo dos vários conjuntos de dados carregados. Em seguida, clique em "Escolher Ficheiro" para "Ficheiro de Transcrição" e "Ficheiros WAV", e selecione o ficheiro de transcrição de texto simples e o arquivo zip dos ficheiros WAV, respetivamente. Quando a preparação estiver concluída, clique em "Importar" para carregar os dados. Em seguida, os dados serão carregados. Para conjuntos de dados maiores, este processo pode demorar vários minutos.

Quando o carregamento estiver concluído, voltará à tabela "Conjuntos de Dados Acústicos" e verá uma entrada que corresponde ao seu conjunto de dados acústicos. Tenha em atenção que foi atribuído um ID exclusivo (GUID) ao conjunto de dados. Os dados também terão um estado que reflete o estado atual dos mesmos. O estado será "NotStarted" (Não Iniciado) enquanto estiverem a ser colocados em fila para processamento, "Running" (Em Execução) enquanto estiverem a ser submetidos a validação e "Complete" (Concluído) quando os dados estiverem prontos para utilização.

A validação de dados inclui uma série de verificações nos ficheiros de áudio para verificar o formato de ficheiro, a duração e a taxa de amostragem, e nos ficheiros de transcrição para verificar o formato de ficheiro e efetuar a normalização de texto.

Quando o estado for "Succeeded" (Com Êxito), pode clicar em "Detalhes" para ver o relatório de verificação de dados acústicos. O número de expressões que passaram e falharam na verificação será apresentado, juntamente com os detalhes sobre as expressões que falharam. No exemplo abaixo, dois ficheiros WAV falharam a verificação devido a formato de áudio impróprio (neste conjunto de dados, um tinha uma taxa de amostragem incorreta e o outro tinha o formato de ficheiro incorreto).

![experimente](media/stt/speech-acoustic-datasets-report.png)

Em algum momento, se quiser alterar o Nome ou a Descrição do conjunto de dados, pode clicar na ligação "Editar" e alterar estas entradas. Não é possível modificar os ficheiros de áudio ou as transcrições.

## <a name="create-a-custom-acoustic-model"></a>Criar um modelo acústico personalizado

Quando o estado do conjunto de dados acústicos for "Complete" (Concluído), pode ser utilizado para criar um modelo acústico personalizado. Para tal, clique em "Modelos Acústicos" no menu pendente "Voz Personalizada". Verá uma tabela chamada "Os seus modelos" com todos os modelos acústicos personalizados. Esta tabela estará vazia se for a sua primeira utilização. A região atual é mostrada no título da tabela. Atualmente, os modelos acústicos podem ser criados apenas para inglês dos EUA.

Para criar um novo modelo, clique me "Criar Novo", abaixo do título da tabela. Tal como anteriormente, introduza um nome e uma descrição para ajudar a identificar este modelo. Por exemplo, o campo "Descrição" pode ser utilizado para registar o modelo inicial e o conjunto de dados acústicos utilizados para criar o modelo. Em seguida, selecione um "Modelo Acústico Base" no menu pendente. O modelo base é o modelo que é o ponto de partida para a sua personalização. Pode escolher entre dois modelos acústicos base. O modelo _Microsoft Search and Dictation AM_ é adequado para voz direcionada para uma aplicação, como comandos, consultas de pesquisa ou ditado. O modelo _Microsoft Conversational_ é adequado para reconhecer voz falada num estilo de conversação. Este tipo de voz é, normalmente, direcionado para outra pessoa e ocorre em centros de atendimento telefónico ou reuniões. A latência de resultados parciais em modelos de Conversação é maior do que em modelos de Pesquisa e Ditado.

> [!NOTE]
> Estamos atualmente a implementar o nosso novo modelo Universal destinado a todos os cenários. Os modelos mencionados anteriormente também permanecerão disponíveis publicamente.

Em seguida, selecione os dados acústicos que quer utilizar para efetuar a personalização através do menu pendente.

![experimente](media/stt/speech-acoustic-models-create2.png)

Opcionalmente, pode optar por realizar testes de precisão do novo modelo quando o processamento estiver concluído. Será executada uma avaliação de voz em texto num conjunto de dados acústicos especificado com o modelo acústico personalizado e os resultados reportados. Para executar este teste, selecione a caixa de verificação "Testes de Precisão". Em seguida, selecione um modelo de idioma no menu pendente. Se não criou quaisquer modelos de idioma personalizados, apenas os modelos de idioma base estarão na lista pendente. Veja a [descrição](how-to-customize-language-model.md) dos modelos de idioma base no guia e selecione o que for mais adequado.

Por fim, selecione o conjunto de dados acústicos que quer utilizar para avaliar o modelo personalizado. Se efetuar testes de precisão, é importante selecionar um conjunto de dados acústicos diferente do utilizado na criação do modelo para obter uma noção realista do desempenho do modelo. Testar a precisão dos dados de preparação não permitirá avaliar o desempenho do modelo adaptado em condições reais. O resultado será demasiado otimista. Tenha também em atenção que os testes de precisão estão limitados a 1000 expressões. Se o conjunto de dados acústicos para teste for maior, apenas serão avaliadas as primeiras 1000 expressões.

Quando estiver pronto para começar a executar o processo de personalização, prima "Criar".

Verá agora uma nova entrada na tabela de modelos acústicos correspondente a este novo modelo. O estado do processo está refletido na tabela. Os estados são "Waiting" (A Aguardar), "Processing" (Em Processamento) e "Complete" (Concluído).

![experimente](media/stt/speech-acoustic-models-creating.png)

## <a name="next-steps"></a>Passos seguintes

- [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Como reconhecer voz em C#](quickstart-csharp-windows.md)
- [Dados de Exemplo do Git](https://github.com/Microsoft/Cognitive-Custom-Speech-Service)
