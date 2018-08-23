---
title: Tutorial de fala do Microsoft Translator (c#) | Documentos da Microsoft
titleSuffix: Cognitive Services
description: Saiba como utilizar o serviço de voz do Translator para traduzir texto em tempo real.
services: cognitive-services
author: v-jerkin
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-speech
ms.devlang: csharp
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jerkin
ms.openlocfilehash: 010ad8b5ceeaf046c8d361ff352e6058154a482d
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/10/2018
ms.locfileid: "41988475"
---
# <a name="tutorial-microsoft-translator-wpf-application-in-c"></a>Tutorial: Aplicativo do WPF de Microsoft Translator em c#

Este tutorial é uma apresentação de uma ferramenta de tradução de voz interativo que utiliza o serviço de tradução de voz do Microsoft Translator, uma parte dos serviços cognitivos da Microsoft no Azure. Saiba como:

> [!div class="checklist"]
> * Solicitar uma lista dos idiomas suportados pelo serviço
> * Captura de áudio e transmiti-los para o serviço
> * Receber e apresentar as traduções da voz como texto
> * Opcionalmente, reproduzir uma versão falada (texto para discurso) da tradução

Um ficheiro de solução do Visual Studio para esta aplicação está [está disponível no GitHub](https://github.com/MicrosoftTranslator/SpeechTranslator).

## <a name="prerequisites"></a>Pré-requisitos

Para este tutorial, terá de qualquer edição do Visual Studio 2017, incluindo a edição de Comunidade. 

A solução do Visual Studio também cria um instalador da aplicação. Terá do [conjunto de ferramentas WiX](http://wixtoolset.org/) e o [WiX conjunto de ferramentas do Visual Studio Extension](https://marketplace.visualstudio.com/items?itemName=RobMensching.WixToolsetVisualStudio2017Extension) para suportar esta funcionalidade.

Também precisa de uma chave de subscrição para o serviço de voz do Translator, que pode obter no dashboard do Microsoft Azure. Um escalão de preço gratuito está disponível que permite-lhe para efetuar a conversão de voz por mês sem encargos, até 10 horas. Esta camada é suficiente para este tutorial.

De terceiros [JSON.Net biblioteca](https://www.newtonsoft.com/json) (a partir de Newtonsoft) também é necessário. Esse assembly é instalado automaticamente pelo NuGet se ambas as caixas de seleção de restauro do pacote estão ativadas nas opções do Visual Studio.

## <a name="trying-the-translation-app"></a>Experimentar a aplicação de tradução

Depois de abrir a solução de voz de Microsoft Translator (`SpeechTranslator.sln`) no Visual STudio, prima F5 para compilar e iniciar o aplicativo.  É apresentada a janela principal do programa.

![[Janela da principal de tradutor de voz]](media/speech-translator-main-window.png)

Na primeira execução, escolha **definições de conta** partir a **definições** menu para abrir a janela mostrada aqui.

![[Janela da principal de tradutor de voz]](media/speech-translator-settings-window.png)

Cole a chave de subscrição de voz do Microsoft Translator nesta janela, em seguida, clique em **guardar.** A chave é guardada entre as execuções.

Na janela principal, selecione a entrada de áudio e quiser que a utilização e as From e linguagens de dispositivos de saída. Se desejar ouvir o som da tradução, certifique-se de que o **TTS** (texto para discurso) opção seja marcada. Se quiser ver especulativas parciais traduções à medida que fala, ative o **resultados parciais** opção.

Por fim, clique em **iniciar** para começar a tradução. Digamos que algo que queira ter convertido e ver o texto reconhecido e a tradução são apresentados na janela. Se ativou a opção de TTS, também ouvir a tradução.

## <a name="obtaining-supported-languages"></a>Obter idiomas suportados

No momento desta edição, o serviço Microsoft Translator suporta várias linguagens de mais de cinco dezenas de tradução de texto. Um número menor de idiomas é suportado para tradução de voz. Nesses idiomas necessitam de suporte para ambos os transcrição (reconhecimento de fala) e, para a saída de texto para voz, síntese.

Em outras palavras, para a tradução de voz, o idioma de origem tem de ser um suportados para transcrição. O idioma de saída pode ser qualquer uma das linguagens suportadas para a tradução de texto, partindo do princípio de que pretende que um resultado de texto. Se pretender que a saída de voz, só é possível converter num idioma suportado para voz.

Microsoft pode adicionar suporte para novos idiomas de tempos em tempos. Por esse motivo, deve não embutir qualquer conhecimento dos idiomas com suporte em seu aplicativo. Em vez disso, a API de voz do Translator fornece um ponto de extremidade de linguagens que permite-lhe obter os idiomas com suporte em tempo de execução. Pode optar por receber uma ou mais listas de idiomas: 

| | |
|-|-|
|`speech`|Os idiomas suportados para transcrição de voz. Pode ser idiomas de origem para tradução de voz.|
|`text`|Os idiomas suportados para a tradução de texto em texto. Pode ser idiomas de destino para a tradução de voz quando é utilizada a saída de texto.|
|`tts`|As vozes suportadas para síntese de fala, cada um associado com um idioma específico. Pode ser idiomas de destino para a tradução de voz quando é utilizada a voz. Um determinado idioma pode ser suportado por mais do que uma voz.|

O ponto de extremidade de idiomas não requerem uma chave de subscrição e sua utilização não contam para a sua quota. O URI é `https://dev.microsofttranslator.com/languages` e retorna os resultados no formato JSON.

O método `UpdateLanguageSettingsAsync()` em `MainWindow.xaml.cs`, conforme apresentado aqui, chama o ponto de final de idiomas para obter a lista de idiomas suportados. 

```csharp
private async Task UpdateLanguageSettingsAsync()
{
    Uri baseUri = new Uri("https://" + baseUrl);
    string fullUriString = "/Languages?api-version=1.0&scope=text,speech,tts";
    if (MenuItem_Experimental.IsChecked) fullUriString += "&flight=experimental";            
    Uri fullUri = new Uri(baseUri, fullUriString);

    using (HttpClient client = new HttpClient()) //'client' is the var - using statement ensures the dispose method is used even after an exception.
    {
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, fullUri);

        // get language names for current UI culture:
        request.Headers.Add("Accept-Language", CultureInfo.CurrentUICulture.TwoLetterISOLanguageName);

        // add a client-side trace Id. In case of issues, one can contact support and provide this:
        //string traceId = "SpeechTranslator" + Guid.NewGuid().ToString();
        //request.Headers.Add("X-ClientTraceId", traceId);
        //Debug.Print("TraceId: {0}", traceId);

        client.Timeout = TimeSpan.FromMilliseconds(10000);
        HttpResponseMessage response = await client.SendAsync(request); //make the async call to the web using the client var and passing the built up URI
        response.EnsureSuccessStatusCode(); //causes exception if the return is false

        Debug.Print("Request Id returned: {0}", GetRequestId(response));

        //create dictionaries to hold the language specific data
        spokenLanguages = new Dictionary<string, string>();
        fromLanguages = new Dictionary<string, string>();
        textLanguages = new Dictionary<string, string>();
        isLTR = new Dictionary<string, bool>();
        voices = new Dictionary<string, List<TTsDetail>>();

        JObject jResponse = JObject.Parse(await response.Content.ReadAsStringAsync()); //get the json from the async call with the response var created above, parse it and put it in a var called jResponse - JObject is a newton class

        //Gather the set of TTS voices
        foreach (JProperty jTts in jResponse["tts"])
        {
            JObject ttsDetails = (JObject)jTts.Value;

            string code = jTts.Name;
            string language = ttsDetails["language"].ToString();
            string displayName = ttsDetails["displayName"].ToString();
            string gender = ttsDetails["gender"].ToString();

            if (!voices.ContainsKey(language)) //check dictionary for a specific key value
            {
                voices.Add(language, new List<TTsDetail>()); //add to the dictionary the locale key and a ttsDetail object
            }

            voices[language].Add(new TTsDetail() { Code = code, DisplayName = string.Format("{0} ({1})", displayName, gender) });
        }

        // Gather the set of speech translation languages
        foreach (JProperty jSpeech in jResponse["speech"])
        {
            JObject languageDetails = (JObject)jSpeech.Value;
            string code = jSpeech.Name;
            string simplecode = languageDetails["language"].ToString();
            string displayName = languageDetails["name"].ToString();
            spokenLanguages.Add(code, displayName);
            fromLanguages.Add(code,simplecode);
        }

        spokenLanguages = spokenLanguages.OrderBy(x => x.Value).ToDictionary(x => x.Key, x => x.Value);
        FromLanguage.Items.Clear();
        foreach (var language in spokenLanguages)
        {
            FromLanguage.Items.Add(new ComboBoxItem() { Content = language.Value, Tag = language.Key});
        }

        // Gather the set of text translation languages
        foreach (JProperty jText in jResponse["text"])
        {
            JObject languageDetails = (JObject)jText.Value;
            string code = jText.Name;
            string displayName = languageDetails["name"].ToString();
            textLanguages.Add(code, displayName);

            string direction = languageDetails["dir"].ToString().ToLowerInvariant();
            bool LTR = true;
            if (direction.ToLowerInvariant() == "rtl") LTR = false;
            isLTR.Add(code, LTR);
        }

        textLanguages = textLanguages.OrderBy(x => x.Value).ToDictionary(x => x.Key, x => x.Value);
        ToLanguage.Items.Clear();
        foreach (var language in textLanguages)
        {
            ToLanguage.Items.Add(new ComboBoxItem() { Content = language.Value, Tag = language.Key });
        }

        if (Properties.Settings.Default.FromLanguageIndex >= 0) FromLanguage.SelectedIndex = Properties.Settings.Default.FromLanguageIndex;
        else
        {
            for(int i=0; i < FromLanguage.Items.Count; ++i)
            {
                ComboBoxItem item = (ComboBoxItem)FromLanguage.Items[i];
                if(CultureInfo.CurrentUICulture.Name.Equals((string)item.Tag, StringComparison.OrdinalIgnoreCase))
                {
                    FromLanguage.SelectedIndex = i;
                }
            }
        }
        if (Properties.Settings.Default.ToLanguageIndex >= 0) ToLanguage.SelectedIndex = Properties.Settings.Default.ToLanguageIndex;
        else
        {
            Random rnd = new Random(DateTime.Now.Millisecond);
            ToLanguage.SelectedIndex = (rnd.Next() % textLanguages.Count);
        }
    }
}
```

Esse método primeiro constrói um pedido HTTP para o ponto final de idiomas, solicitando todas as três listas de idiomas (`text`, `speech`, e `tts`).

O ponto de extremidade de idiomas usa o pedido `Accept-Languages` cabeçalho para determinar o idioma em que os nomes dos idiomas são representados. Por exemplo, os idiomas conhecidos falantes de inglês como "Alemanha" se chama "Deutsch" em alemão e "Alemán" em espanhol e a lista de idiomas refletem essas diferenças. Idioma padrão do sistema é utilizado para este cabeçalho.

Depois do pedido foi enviado e a resposta JSON recebida, a resposta é analisada em estruturas de dados internas. Estas estruturas, em seguida, são utilizadas para construir os menus de idioma e a linguagem. 

Uma vez que as vozes disponíveis dependem do idioma para escolhido pelo utilizador, não é possível definir o menu de voz ainda. Em vez disso, as vozes disponíveis para cada idioma são armazenadas para uso posterior. O `ToLanguage_SelectionChanged` manipulador (no mesmo ficheiro de origem) mais tarde atualiza o menu de voz chamando `UpdateVoiceComboBox()` quando o usuário escolhe uma linguagem para. 

Só por diversão, uma linguagem para aleatoriamente está selecionada, se o utilizador não tiver executado o aplicativo antes. (As configurações de menu são armazenadas entre sessões.)

## <a name="authenticating-requests"></a>Autenticar pedidos

Para autenticar para o serviço de voz do Microsoft Translator tiver de enviar a chave de subscrição do Azure no cabeçalho, como o valor de `Ocp-Apim-Subscription-Key` no pedido de ligação.

## <a name="translation-overview"></a>Descrição geral da tradução

A API de tradução (ponto final de WebSockets `wss://dev.microsofttranslator.com/speech/translate`) aceita áudio para ser convertido em monophonic, kHz 16, 16 bits assinado formato WAVE. O serviço retorna um ou mais respostas JSON que contém tanto o texto reconhecido e traduzido. Se tiver sido solicitada a voz, um arquivo de áudio é enviado.

O utilizador escolhe a origem de áudio utilizando o menu de entrada do microfone/ficheiro. O áudio pode vir de um dispositivo de áudio (por exemplo, um microfone) ou de um `.WAV` ficheiro.

O método `StartListening_Click` é invocado quando o usuário clica no botão Iniciar. Esse manipulador de eventos, por sua vez, chama `Connect()` para iniciar o processo de envio de áudio para o ponto final de API de serviço. O `Connect()` método realiza as seguintes tarefas:


> [!div class="checklist"]
> * Obter as definições de utilizador da janela principal e validá-los
> * A inicializar a entrada de áudio e fluxos de saída
> * Chamar `ConnectAsync()` para lidar com o restante do trabalho

`ConnectAsync()`, por sua vez, lida com as seguintes tarefas:

> [!div class="checklist"]
> * Autenticação com a chave de subscrição do Azure no cabeçalho `Ocp-Apim-Subscription-Key`
> * Criar uma `SpeechClient` instância (encontrada no `SpeechClient.cs`) para comunicar com o serviço
> * A inicializar `TextMessageDecoder` e `BinaryMessageDecoder` instâncias (consulte `SpeechResponseDecoder.cs`) para processar as respostas
> * Enviar o áudio através do `SpeechClient` instância para o serviço de voz do tradutor
> * Receber e processar os resultados da tradução

As responsabilidades de `SpeechClient` são menos:

> [!div class="checklist"]
> * Estabelecer uma conexão WebSocket para o serviço de voz do tradutor
> * Enviar dados de áudio e receber respostas através de socket

## <a name="a-closer-look"></a>Uma análise detalhada

Deve ser mais claro agora como partes do aplicativo funcionam em conjunto para efetuar o pedido de tradução. Vamos dar uma olhada em alguns códigos, focalizando as partes relevantes.

Aqui está uma versão parcial do `Connect()` definição que aparece as sequências de áudio em:

```csharp
private void Connect()
{
    if (this.currentState != UiState.ReadyToConnect) return;

    Stopwatch watch = Stopwatch.StartNew();
    UpdateUiState(UiState.Connecting);

    // Omitted: code to validate UI settings

    string tag = ((ComboBoxItem)Mic.SelectedItem).Tag as string;
    string audioFileInputPath = null;
    if (tag == "File")
    {
        audioFileInputPath = this.AudioFileInput.Text;
        foreach (string currFile in audioFileInputPath.Split('|'))
        {
            if (!File.Exists(currFile))
            {
                SetMessage(String.Format($"Invalid audio source: selected file {currFile} does not exist."), "", MessageKind.Error);
                UpdateUiState(UiState.ReadyToConnect);
                return;
            }
        }
    }
    bool shouldSuspendInputAudioDuringTTS = this.CutInputAudioCheckBox.IsChecked.HasValue ? this.CutInputAudioCheckBox.IsChecked.Value : false;

    correlationId = Guid.NewGuid().ToString("D").Split('-')[0].ToUpperInvariant();

    // Setup speech translation client options
    SpeechClientOptions options;

    string voicename = "";
    if (this.Voice.SelectedItem != null)
    {
        voicename = ((ComboBoxItem)this.Voice.SelectedItem).Tag.ToString();
    }
    options = new SpeechTranslateClientOptions()
    {
        TranslateFrom = ((ComboBoxItem)this.FromLanguage.SelectedItem).Tag.ToString(),
        TranslateTo = ((ComboBoxItem)this.ToLanguage.SelectedItem).Tag.ToString(),
        Voice = voicename,
    };
    
    options.Hostname = baseUrl;
    options.AuthHeaderKey = "Authorization";
    options.AuthHeaderValue = ""; // set later in ConnectAsync.
    options.ClientAppId = new Guid("EA66703D-90A8-436B-9BD6-7A2707A2AD99");
    options.CorrelationId = this.correlationId;
    options.Features = GetFeatures().ToString().Replace(" ", "");
    options.Profanity = ((SpeechClient.ProfanityFilter)Enum.Parse(typeof(SpeechClient.ProfanityFilter), GetProfanityLevel(), true)).ToString();
    options.Experimental = MenuItem_Experimental.IsChecked;

    // Setup player and recorder but don't start them yet.
    WaveFormat waveFormat = new WaveFormat(16000, 16, 1);

    // WaveProvider for incoming TTS
    // We use a rather large BufferDuration because we need to be able to hold an entire utterance.
    // TTS audio is received in bursts (faster than real-time).
    textToSpeechBytes = 0;
    playerTextToSpeechWaveProvider = new BufferedWaveProvider(waveFormat);
    playerTextToSpeechWaveProvider.BufferDuration = TimeSpan.FromMinutes(5);

    ISampleProvider sampleProvider = null;
    if (audioFileInputPath != null)
    {
        // Setup mixing of audio from input file and from TTS
        playerAudioInputWaveProvider = new BufferedWaveProvider(waveFormat);
        var srce1 = new Pcm16BitToSampleProvider(playerTextToSpeechWaveProvider);
        var srce2 = new Pcm16BitToSampleProvider(playerAudioInputWaveProvider);
        var mixer = new MixingSampleProvider(srce1.WaveFormat);
        mixer.AddMixerInput(srce1);
        mixer.AddMixerInput(srce2);
        sampleProvider = mixer;
    }
    else
    {
        recorder = new WaveIn();
        recorder.DeviceNumber = (int)((ComboBoxItem)Mic.SelectedItem).Tag;
        recorder.WaveFormat = waveFormat;
        recorder.DataAvailable += OnRecorderDataAvailable;
        sampleProvider = playerTextToSpeechWaveProvider.ToSampleProvider();
    }

    if (!batchMode)
    {
        player = new WaveOut();
        player.DeviceNumber = (int)((ComboBoxItem)Speaker.SelectedItem).Tag;
        player.Init(sampleProvider);
    }

    this.audioBytesSent = 0;

    string logAudioFileName = null;
    if (LogSentAudio.IsChecked|| LogReceivedAudio.IsChecked)
    {
        string logAudioPath = System.IO.Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData), Properties.Settings.Default.OutputDirectory);
        try
        {
            Directory.CreateDirectory(logAudioPath);
        }
        catch
        {
            this.AddItemToLog(string.Format("Could not create folder {0}", logAudioPath));
        }

        if (LogSentAudio.IsChecked)
        {
            logAudioFileName = System.IO.Path.Combine(logAudioPath, string.Format("audiosent_{0}.wav", this.correlationId));
        }

        if (LogReceivedAudio.IsChecked)
        {
            string fmt = System.IO.Path.Combine(logAudioPath, string.Format("audiotts_{0}_{{0}}.wav", this.correlationId));
            this.audioReceived = new BinaryMessageDecoder(fmt);
        }
    }
}
```

Uma parte substancial `Connect()` envolve a criação de um `SpeechClientOptions` instância (consulte `SpeechClientOptions.cs`) para conter as opções para a tradução. As opções incluem as informações necessárias para ligar ao serviço (por exemplo, a chave de autenticação e nome de anfitrião) e os recursos utilizados para a tradução. Aqui os campos do mapa para os campos de cabeçalho e os parâmetros HTTP expostos pela [a API de voz do Translator](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference).

`Connect()` também cria e inicializa o dispositivo de entrada de áudio (variável `sampleProvider`) que serve como a fonte de voz para ser convertido. Este dispositivo é um dispositivo de entrada de hardware, como um microfone ou um ficheiro que contém dados de áudio WAVE.

Aqui está o `ConnectAsync()` método que instancie o `speechClient` classe e se conecta a funções anônimas para lidar com texto e binárias respostas do serviço.

```csharp
private async Task ConnectAsync(SpeechClientOptions options, bool suspendInputAudioDuringTTS)
{
    await ADMAuthenticate(options);
    
    TextMessageDecoder textDecoder;
    
    s2smtClient = new SpeechClient((SpeechTranslateClientOptions)options, CancellationToken.None);
    
    s2smtClient.OnBinaryData += (c, a) => { AddSamplesToPlay(a, suspendInputAudioDuringTTS); };
    s2smtClient.OnEndOfBinaryData += (c, a) => { AddSamplesToPlay(a, suspendInputAudioDuringTTS); };
    s2smtClient.OnTextData += (c, a) => { textDecoder.AppendData(a); lastReceivedPacketTick = DateTime.Now.Ticks; };
    s2smtClient.OnEndOfTextData += (c, a) =>
    {
        textDecoder.AppendData(a);
        lastReceivedPacketTick = DateTime.Now.Ticks;
        textDecoder
            .Decode()
            .ContinueWith(t =>
            {
                if (t.IsFaulted)
                {
                    Log(t.Exception, "E: Failed to decode incoming text message.");
                }
                else
                {
                    object msg = t.Result;
                    if (msg.GetType() == typeof(FinalResultMessage))
                    {
                        // omitted: code to process final binary result
                    }
                    if (msg.GetType() == typeof(PartialResultMessage))
                    {
                        // omitted: code to process partial binary result
                    }
                }
            });
    };
    s2smtClient.Failed += (c, ex) =>
    {
        Log(ex, "E: SpeechTranslation client reported an error.");
    };
    s2smtClient.Disconnected += (c, ea) =>
    {
        SafeInvoke(() =>
        {
            // We only care to react to server disconnect when our state is Connected. 
            if (currentState == UiState.Connected)
            {
                Log("E: Connection has been lost.");
                Log($"E: Errors (if any): \n{string.Join("\n", s2smtClient.Errors)}");
                Disconnect();
            }
        });
    };
    await s2smtClient.Connect();
}
```

Depois de autenticar, o método cria o `SpeechClient` instância. O `SpeechClient` classe (no `SpeechClient.cs`) invoca os manipuladores de eventos nos dados binários e de texto de receção. Manipuladores de adicionais são chamados quando a ligação falha ou se desliga.

Dados binários são áudio (saída de texto para discurso) enviado pelo serviço quando TTS está ativada. Dados de texto são um parcial ou de uma tradução inteira do texto falado. Portanto, depois de instanciar, o método conecta as funções para lidar com essas mensagens: áudio armazenando-os para reprodução posterior e o texto ao exibi-la na janela.

## <a name="next-steps"></a>Passos Seguintes

Este exemplo de código é um aplicativo de rico em recursos a fim de demonstrar o uso da API de voz do Translator. Assim, há um bom número de partes móveis entender. Percorremos os bits mais importantes. Para o restante, ele pode ser instrutivo para definir alguns pontos de interrupção no Visual Studio e percorrer o processo de tradução. Quando compreender o aplicativo de exemplo, está equipado para utilizar o serviço de voz do Translator em seus próprios aplicativos.

> [!div class="nextstepaction"]
> [Referência da API de voz do Microsoft Translator](https://docs.microsoft.com/azure/cognitive-services/translator-speech/reference)
