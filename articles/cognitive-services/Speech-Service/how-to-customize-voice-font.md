---
title: O que é voz personalizadas? -Serviços cognitivos as do azure | Microsoft Docs
description: Este artigo descrições gerais Microsoft texto conversão em voz voz personalização, que permite-lhe criar uma voz reconhecível, uma-de-a-tipo marca.
services: cognitive-services
author: noellelacharite
manager: nolach
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2018
ms.author: nolach
ms.openlocfilehash: ad5af799fd46dc51b85432999f986de8cdb056ec
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355094"
---
# <a name="creating-custom-voice-fonts"></a>Criar tipos de letra de voz personalizadas

Personalização de voz Microsoft texto conversão em voz (TTS) permite-lhe criar uma voz reconhecível, uma-de-a-tipo para a sua marca: um *tipo de letra de voz.* 

Para criar o tipo de letra de voz, efetuar uma gravação studio e carregar os scripts associados, como os dados de preparação. O serviço, em seguida, cria um modelo de voz exclusivo otimizado para a gravação da. Em seguida, pode utilizar este tipo de letra de voz para sintetizar o reconhecimento de voz. 

Pode começar com uma pequena quantidade de dados para uma prova de conceito. Mas os dados mais fornecer, mais natural e professional sua voz SOA.

Personalização de voz está disponível para inglês (en-US) e mainland chinês (zh-CN).

## <a name="prerequisites"></a>Pré-requisitos

A funcionalidade de personalização de voz de reconhecimento de voz do texto para está atualmente em pré-visualização privada. [Preencha o formulário de aplicação](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR0N8Vcdi8MZBllkZb70o6KdURjRaUzhBVkhUNklCUEMxU0tQMEFPMjVHVi4u) ser considerados para acesso.

Também tem de ter:

* Uma conta do Azure ([inscrever gratuitamente](https://azure.microsoft.com/free/ai/) se ainda não tiver uma).

* Uma subscrição do serviço de reconhecimento de voz. [Criar um](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices) se ainda não o fez.

    ![Criar Painel](media/custom-voice/create-panel.png)

Depois de criar a sua subscrição, pode encontrar duas chaves de subscrição no painel de início rápido ou o painel de descrição geral da nova subscrição. Pode utilizar a chave.

Por fim, ligar-se a subscrição para o portal de voz personalizadas da seguinte forma.

1. Inicie sessão no [portal de voz personalizadas](https://customvoice.ai) com a mesma conta Microsoft que utilizou para aplicar para acesso.

2. Aceda às subscrições em seu nome de conta no canto superior direito.

    ![Subscrições](media/custom-voice/subscriptions.png)

3. Na página "Subscrições", escolha 'Subscrição existente do Connect'.

     ![Ligar a subscrição existente](media/custom-voice/connect-existing-sub.png)

4. Cole a chave de subscrição na tabela, conforme mostrado abaixo.

     ![Adicionar Subscrição](media/custom-voice/add-subscription.png)

Está pronto para começar!

## <a name="prepare-recordings-and-transcripts"></a>Preparar as gravações e transcrições

Um conjunto de dados de formação de voz é composta por um conjunto de ficheiros de áudio, juntamente com um ficheiro de texto que contém as transcrições de todos estes ficheiros de áudio.

Pode preparar estes ficheiros em qualquer direção:: escrever um script e tiver lidos pelo talent de voz, ou utilize áudio publicamente disponíveis e transcribe-los para texto. No caso desta última opção, edite disfluencies dos ficheiros do áudio, tais como "ompatibilidade" e outros sons filler, stutters, palavras mumbled ou mispronunciations.

Para produzir um tipo de letra voz boa, é importante que as gravações são efetuadas numa sala de silenciosos com um microfone de alta qualidade. Volume consistente, falando taxa, falando rápida e expressivas mannerisms de reconhecimento de voz são essenciais para a criação de uma excelente voz digital. Para criar uma voz para utilização em produção, recomendamos que utilize um talent studio e tom de gravação profissionais.

### <a name="audio-files"></a>Ficheiros de áudio

Cada ficheiro de áudio deve conter um único utterance (por exemplo, uma frase único ou uma única vez de um sistema de caixa de diálogo). Todos os ficheiros têm de estar no mesmo idioma (multilanguage voices personalizados não são suportadas). Os ficheiros de áudio tem também cada uma tem um nome de ficheiro exclusivo numérico efetuada com a extensão de nome de ficheiro `.wav`.

Ficheiros de áudio devem estar preparados da seguinte forma. Outros formatos que não são suportados e serão rejeitados.

| **Propriedade** | **Valor** |
| ------------ | --------- |
| Formato de ficheiro  | RIFF (WAV)|
| Frequência de amostragem| 16,000 Hz |
| Canais     | 1 (monophonic)  |
| Formato de exemplo| PCM, 16 bits |
| Nome de Ficheiro    | Um valor numérico, com `.wav` extensão |
| Formato de arquivo| Zip      |
| Tamanho máximo de arquivo|200 MB|

Coloque o conjunto de ficheiros de áudio para uma única pasta sem subdiretórios e todo o conjunto como um único arquivo de ficheiro ZIP do pacote.

> [!NOTE]
> O portal atualmente importa ZIP arquiva até 200 MB. No entanto, vários arquivos podem ser carregados. O número máximo de conjuntos de dados permitidos é que 10 ZIP gratuitamente ficheiros subscrição utilizadores e 50 para utilizadores de subscrição padrão.

### <a name="transcripts"></a>Transcrições

O ficheiro de transcription é um ficheiro de texto de Unicode simples (UTF-16 pouca-endian). Cada linha do ficheiro transcription tem de ter o nome de um ficheiro de áudio, seguido por um caráter do separador (ponto de código 9) e, finalmente, o respetivo transcript. Não existem linhas em branco são permitidas.

Por exemplo:

```
0000000001  This is the waistline, and it's falling.
0000000002  We have trouble scoring.
0000000003  It was Janet Maslin.
```

O sistema de voz personalizadas normaliza transcrições pelo texto a converter para minúsculas e remoção de pontuação supérfluas. É importante que o transcrições são precisos para gravações de áudio correspondentes a 100%.

> [!TIP]
> Quando a criação de produção Text-to-Speech voices, selecionadas utterances (ou scripts de escrita) considerar cobertura fonético e eficiência.

## <a name="upload-your-datasets"></a>Carregar os conjuntos de dados

Depois de preparar o seu arquivo de ficheiro de áudio e transcrições, carregá-los através do [Portal de serviço de voz personalizadas](https://customvoice.ai).

> [!NOTE]
> Conjuntos de dados não podem ser editados depois de se tiverem sido carregados. Se a se esqueceu de incluir transcrições de alguns dos ficheiros de áudio, por exemplo, ou acidentalmente, escolha o sexo errado, tem de carregar novamente todo o conjunto de dados. Verifique as definições e o conjunto de dados cuidadosamente antes de iniciar o carregamento.

1. Inicie sessão no portal.

2. Escolha **dados** em voz personalizado na página principal. 

    ![A minha projetos](media/custom-voice/my-projects.png)

    A tabela dos meus dados de voz é apresentada. Trata-se vazia se ainda não carregou quaisquer conjuntos de dados de voz.

3. Clique em **importar dados** para abrir a página para carregar um novo conjunto de dados.

    ![Importar dados de voz](media/custom-voice/import-voice-data.png)

4. Introduza um nome e descrição nos campos fornecidos. 

5. Selecione a região para os tipos de letra de voz. Certifique-se de que as informações de região corresponde ao idioma dos dados de gravação e os scripts. 

6. Selecione o género de altifalante cujo voz estiver a utilizar.

7. Escolha o script e os ficheiros de áudio para carregar. 

8. Clique em **importação** para carregar os dados. Para conjuntos de dados maiores, a importação poderá demorar vários minutos.

> [!NOTE]
> Utilizadores de subscrição gratuita podem carregar dois conjuntos de dados num momento. Utilizadores padrão de subscrição podem carregar cinco conjuntos de dados em simultâneo. Se atingir o limite, aguarde pelo menos um dos seus conjuntos de dados termina a importação, em seguida, tente novamente.

Quando o carregamento estiver concluído, a tabela de dados de voz My aparece novamente. Deverá ver uma entrada que corresponde ao conjunto de dados carregados apenas. 

Conjuntos de dados são validados automaticamente após o carregamento. Validação de dados inclui uma série de verificações os ficheiros de áudio para verificar o respetivo formato de ficheiro, tamanho e frequência de amostragem. Verificações nos ficheiros transcription Verifique o formato de ficheiro e efetuar algumas normalização de texto. Os utterances são transcribed com reconhecimento de voz e o texto resultante é comparado com o transcript fornecida.

![Os meus dados de voz](media/custom-voice/my-voice-data.png)

A tabela seguinte mostra os Estados de processamento de conjuntos de dados importados. 

| Estado | Significado
| ----- | -------
| `NotStarted` | O conjunto de dados foi recebido e é colocada na fila para processamento
| `Running` | O conjunto de dados está a ser validado
| `Succeeded` | O conjunto de dados foi validado e agora pode ser utilizado para criar um tipo de letra de voz

Depois de concluída a validação, pode ver o número total de utterances correspondentes para cada um dos seus conjuntos de dados na coluna Utterance.

Pode transferir um relatório para verificar as pontuações de pronunciação e o nível de ruído para cada um dos seus gravações. Os intervalos de pontuação de pronunciação de 0 a 100; uma pontuação abaixo 70 normalmente indica um erro de correspondência do erro ou de script de reconhecimento de voz. Um pesada acentos podem reduzir o modelo de pontuação de pronunciação e afetar a voz digital gerada.

Um rácio de sinal para ruído superior (SNR) indica ruído inferior na sua áudio. Normalmente, pode aceder SNR 50 + por gravação através de estúdios profissionais. Áudio com um SNR abaixo 20 pode resultar em ruído óbvios na sua voz gerado.

Considere qualquer utterances com pontuações de pronunciação baixa ou fracas rácios de sinal para ruído de gravação novamente. Se não for possível gravar novamente, pode excluir esses utterances do seu conjunto de dados.

## <a name="build-your-voice-font"></a>Criar o tipo de letra de voz

Depois do conjunto de dados foi validado, pode utilizá-lo para criar o tipo de letra de voz personalizadas. 

1. Escolha **modelos** no menu pendente "Voz personalizado". 
 
    A tabela de tipos de letra de voz My é apresentada, listagem quaisquer tipos de letra de voz personalizadas, que já criou.

1. Clique em **criar voices** sob o título de tabela. 

    É apresentada a página para a criação de um tipo de letra de voz. A região atual é apresentada na primeira linha da tabela. Altere a região para criar uma voz noutro idioma. A região tem de ser o mesmo que os conjuntos de dados que está a ser utilizados para criar a voz.

1. Tal como fez quando carregou o conjunto de dados, introduza um nome e descrição para o ajudar a identificar este modelo. 

    O nome que introduzir aqui será o nome utilizado para especificar a voz do seu pedido para synthesis de reconhecimento de voz como parte da entrada SSML, por isso, escolha com atenção. Apenas letras, números e carateres de pontuação alguns, como '-', '_' '(',')' são permitidos.

    Uma utilização comum do campo de descrição é registar os nomes dos conjuntos de dados que foram utilizados para criar o modelo.

1. Escolha o sexo do seu tipo de letra de voz. Tem de corresponder o sexo do conjunto de dados.

1. Selecione dos DataSets que pretende utilizar para o tipo de letra de voz de preparação. Todos os conjuntos de dados utilizados tem de ser do mesmo altifalante.

1. Clique em **criar** para começar a criar o tipo de letra de voz.

    ![Criar modelo](media/custom-voice/create-model.png)

O novo modelo aparece na tabela de tipos de letra de voz My. 

![A minha tipos de letra de voz](media/custom-voice/my-voice-fonts.png)

O estado apresentado reflete o processo de conversão do conjunto de dados para um tipo de letra de voz, conforme mostrado aqui.

| Estado | Significado
| ----- | -------
| `NotStarted` | O pedido para a criação do tipo de letra de voz é colocada na fila para processamento
| `Running` | O tipo de letra de voz está a ser criado
| `Succeeded` | O tipo de letra de voz foi criado e pode ser implementado

Tempo de preparação varia consoante o volume do processamento de dados de áudio. Vezes típicos no intervalo de sobre 30 minutos para centenas de utterances 40 horas para 20.000 utterances.

> [!NOTE]
> Utilizadores de subscrição gratuita podem preparar dois tipos de letra de voz cada vez. Utilizadores padrão de subscrição podem dar formação sobre três voices em simultâneo. Se atingir o limite, aguarde pela conclusão, pelo menos, um dos tipos de letra voz formação e tente novamente.

## <a name="test-your-voice-font"></a>Testar o seu tipo de letra de voz

Uma vez que o tipo de letra de voz é criado com êxito, pode testá-lo antes de o implementar para utilização. Clique em **teste** na coluna de operações. É apresentada a página de teste para o tipo de letra voz seleccionada. A tabela estiver vazia se ainda não tiver submetido quaisquer pedidos de teste para a voz.

![Tipos de letra meu voz, parte 2](media/custom-voice/my-voice-fonts2.png)

Clique em **teste com texto** botão sob o título de tabela para apresentar um menu de pop-up para submeter pedidos de texto. Pode submeter o pedido de teste em texto simples ou SSML. O tamanho máximo de entrada é 1024 caracteres, incluindo todas as etiquetas para o pedido SSML. O idioma do seu texto tem de ser o mesmo que o idioma do seu tipo de letra de voz.

![Tipo de letra de teste de voz](media/custom-voice/voice-font-testing.png)

Depois de preencher a caixa de texto e confirmar o modo de entrada, clique em **Sim** para submeter o pedido de teste e voltar para a página de teste. A tabela inclui agora uma entrada que corresponde ao seu novo pedido e a coluna Estado agora familiar. Pode demorar alguns minutos para sintetizar o reconhecimento de voz. Quando a coluna Estado lê com êxito, pode transferir a entrada de texto (um `.txt` ficheiro) e saída de áudio (um `.wav` ficheiro) e audition a última opção da qualidade.

![Tipo de letra de teste, parte 2 de voz](media/custom-voice/voice-font-testing2.png)

## <a name="create-and-use-a-custom-endpoint"></a>Criar e utilizar um ponto de final personalizado

Depois de ter criado com êxito e testar o modelo de voz, implementá-lo num ponto final personalizado texto conversão em voz. Em seguida, utilizar este ponto final em vez do ponto final habitual quando são efetuados pedidos de reconhecimento de voz do texto para através da API REST. O ponto final personalizado pode ser chamado apenas pela subscrição que utilizou para implementar o tipo de letra.

Para criar um novo ponto de final personalizado, escolha **pontos finais** no menu de voz personalizadas, na parte superior da página. Implementação é apresentada a página, com a respetiva tabela de pontos finais de voz personalizadas atual, se aplicável.

Clique em de **implementar voices** botão para criar um novo ponto final. Página no ponto final criar", a região atual é refletida na primeira linha da tabela. Para criar uma implementação para um idioma diferente, altere a região apresentada. (Tem de corresponder a voz que está a implementar). Introduza o nome e descrição do seu ponto final personalizado.

No menu de subscrição, escolha a subscrição que pretende utilizar. Utilizadores de subscrição gratuita podem ter apenas um modelo que implementou de cada vez. Utilizadores de subscrição padrão, podem criar até 20 pontos finais, cada um com o seu próprio voz personalizadas.

![Criar o ponto final](media/custom-voice/create-endpoint.png)

Depois de selecionar o modelo de implementação, clique em **criar**. A página de implementação reappears agora com uma entrada para o ponto final de novo. Pode demorar alguns minutos para instanciar um novo ponto final. Quando o estado da implementação é concluída com êxito, o ponto final está pronto a utilizar.

![A minha Voices implementados](media/custom-voice/my-deployed-voices.png)

Quando o estado de implementação é concluída com êxito, o ponto final do seu tipo de letra voz implementado é apresentado no meu tabela voices implementado. Pode utilizar este URI diretamente num pedido de HTTP.

Testar online do ponto final está também disponível através do portal de voz personalizadas. Para testar o ponto final, escolha **pontos finais testar** no menu de lista pendente de voz personalizadas. O ponto final de testar a página é apresentada. Escolha uma voz que tenha implementado e o texto a ser ditas (texto simples ou formato SSML) de entrada na caixa de texto.

> [!NOTE] 
> Quando utilizar SSML, o `<voice>` etiquetas tem de especificar o nome que deu a sua voz personalizada quando o criou.

Clique em **reproduzir** ouvi texto ditas o tipo de letra de voz personalizadas.

![Teste de ponto final](media/custom-voice/endpoint-testing.png)

O ponto final personalizado é funcionalmente idêntico ao ponto final padrão utilizado para pedidos de Text-to-Speech. Consulte [REST API](rest-apis.md) para obter mais informações.

## <a name="next-steps"></a>Passos Seguintes

- [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconhecer o reconhecimento de voz em c#](quickstart-csharp-windows.md)
