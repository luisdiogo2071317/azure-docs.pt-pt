---
title: Tutorial de reconhecimento de voz tradutor (c#) | Microsoft Docs
titleSuffix: Cognitive Services
description: Saiba como utilizar o serviço de reconhecimento de voz tradutor para traduzir texto em tempo real.
services: cognitive-services
author: v-jerkin
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-speech
ms.devlang: csharp
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jerkin
ms.openlocfilehash: e82c5c5ccfa6b7de8a9ec111140dad1a40ad44f6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352286"
---
# <a name="tutorial-microsoft-translator-wpf-application-in-c"></a>Tutorial: Microsoft Translator WPF aplicação c#

Este tutorial é a introdução de uma ferramenta de conversão de reconhecimento de voz interativos que utiliza o serviço de tradução de reconhecimento de voz do Microsoft tradutor, uma parte dos serviços cognitivos da Microsoft no Azure. Saiba como:

> [!div class="checklist"]
> * Pediu uma lista dos idiomas suportados pelo serviço
> * Captura de áudio e transmite-lo para o serviço
> * Receber e apresentar traduções do reconhecimento de voz como texto
> * Opcionalmente, reproduzir uma versão (text-to-speech) ditas da tradução

Um ficheiro de solução do Visual Studio para esta aplicação é [está disponível no GitHub](https://github.com/MicrosoftTranslator/SpeechTranslator).

## <a name="prerequisites"></a>Pré-requisitos

Para este tutorial, é necessário qualquer edição do Visual Studio 2017, incluindo a edição de Comunidade. 

A solução do Visual Studio também cria um instalador para a aplicação. É necessário o [conjunto de ferramentas WiX](http://wixtoolset.org/) e [WiX conjunto de ferramentas Visual Studio extensão](https://marketplace.visualstudio.com/items?itemName=RobMensching.WixToolsetVisualStudio2017Extension) para suportar esta funcionalidade.

Também precisa de uma chave de subscrição para o serviço de reconhecimento de voz tradutor, que pode obter no dashboard do Microsoft Azure. Um escalão de preço gratuito está disponível que lhe permite traduzir até 10 horas de reconhecimento de voz por mês, sem encargos. Esta camada é suficiente para este tutorial.

A terceiros [JSON.Net biblioteca](https://www.newtonsoft.com/json) (a partir de Newtonsoft) também for necessária. Esta assemblagem é instalada automaticamente pelo NuGet se ambas as caixas de verificação de restauro do pacote estão ativadas nas opções do Visual Studio.

## <a name="trying-the-translation-app"></a>A tentar a aplicação de conversão

Depois de abrir a solução Microsoft voz tradutor (`SpeechTranslator.sln`) no Visual STudio, prima F5 para compilar e executar a aplicação.  É apresentada a janela principal do programa.

![[Janela de principal tradutor do reconhecimento de voz]](media/speech-translator-main-window.png)

Na primeira execução, escolha **as definições da conta** do **definições** menu para abrir a janela mostrada aqui.

![[Janela de principal tradutor do reconhecimento de voz]](media/speech-translator-settings-window.png)

Colar a chave de subscrição do Microsoft tradutor voz nesta janela, em seguida, clique em **guardar.** A chave é guardada entre é executado.

Na janela principal, selecione a entrada de áudio e dispositivos de saída que pretende para utilização e de e para idiomas. Se pretender ouvir áudio da tradução, certifique-se a **TTS** (text-to-speech) opção está selecionada. Se quiser ver speculative parciais traduções como enunciar, ative o **resultados parciais** opção.

Por fim, clique em **iniciar** para iniciar a conversão. Diga algo que pretende ter traduzidos e ver o texto reconhecido e a tradução são apresentados na janela. Se tiver ativado a opção de TTS, também ouvir a tradução.

## <a name="obtaining-supported-languages"></a>Obter os idiomas suportados

Desta redação, o serviço de Microsoft Translator suporta idiomas de mais de cinco dúzia de conversão de texto. Um número mais pequeno idiomas é suportado para tradução de reconhecimento de voz. Essas idiomas precisam de suporte para ambos os transcription (reconhecimento de voz) e, para o resultado text-to-speech, synthesis.

Por outras palavras, para tradução de reconhecimento de voz, o idioma de origem tem de ser suportado para transcription um. O idioma de saída pode ser qualquer um dos idiomas suportados para a conversão de texto, partindo do princípio de que pretende um resultado de texto. Se pretender que o resultado de reconhecimento de voz, apenas pode implica um idioma suportado para text-to-speech.

Microsoft pode adicionar suporte para novos idiomas ocasionalmente. Por este motivo, deve não codificar qualquer conhecimento de idiomas suportados na sua aplicação. Em vez disso, a API de reconhecimento de voz tradutor fornece um ponto final de idiomas que permite-lhe obter os idiomas suportados no tempo de execução. Pode optar por receber um ou mais apresenta uma lista de idiomas: 

| | |
|-|-|
|`speech`|Os idiomas suportados para transcription de reconhecimento de voz. Pode ser idiomas de origem para tradução de reconhecimento de voz.|
|`text`|Os idiomas suportados para a conversão de texto-texto. Pode ser idiomas de destino para tradução de reconhecimento de voz quando é utilizada a saída de texto.|
|`tts`|Os voices suportados para synthesis de reconhecimento de voz, cada um associado com um idioma específico. Pode ser o destino de idiomas para tradução de reconhecimento de voz quando text-to-speech é utilizado. Um determinado idioma pode ser suportado por mais do que uma voz.|

O ponto final de idiomas não necessita de uma chave de subscrição e respetiva utilização não count contra a sua quota. O URI é `https://dev.microsofttranslator.com/languages` e devolve os resultados no formato JSON.

O método `UpdateLanguageSettingsAsync()` no `MainWindow.xaml.cs`, mostrados aqui, chama o ponto final de idiomas para obter a lista de idiomas suportados. 

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

Este método primeiro constrói um pedido HTTP para o ponto final de idiomas, pedir todas as listas de três de idiomas (`text`, `speech`, e `tts`).

O ponto final de idiomas utiliza o pedido `Accept-Languages` cabeçalho para determinar o idioma em que os nomes dos idiomas são representados. Por exemplo, o idioma inglês speakers conhecido como "Alemão" é chamado "Deutsch" em alemão e "Alemán" espanhol e a lista de idiomas reflete estas diferenças. Idioma predefinido do sistema é utilizado para este cabeçalho.

Depois do pedido foi enviado e a resposta JSON recebida, a resposta é analisada para estruturas de dados internos. Estas estruturas, em seguida, são utilizadas para construir os menus de idioma e para o idioma. 

Uma vez que o voices disponíveis dependem o idioma para escolhido pelo utilizador,-não é possível configurar o menu de voz ainda. Em vez disso, os voices disponíveis para cada idioma são armazenadas para utilização posterior. O `ToLanguage_SelectionChanged` processador (no mesmo ficheiro de origem) mais tarde atualiza o menu de voz chamando `UpdateVoiceComboBox()` quando o utilizador escolhe um idioma para. 

Apenas para experimentar um idioma para aleatoriamente está selecionado se o utilizador não tem a executar a aplicação antes. (As definições de menu são armazenadas entre sessões.)

## <a name="authenticating-requests"></a>Autenticar pedidos

Para se autenticar o serviço de reconhecimento de voz do Microsoft tradutor terá de enviar a sua chave de subscrição do Azure no cabeçalho de como o valor de `Ocp-Apim-Subscription-Key` no pedido de ligação.

## <a name="translation-overview"></a>Descrição geral de conversão

A API traduzir (ponto final de WebSockets `wss://dev.microsofttranslator.com/speech/translate`) aceita áudio para ser convertido em monophonic, 16 kHz, 16 bits assinado formato WAVE. O serviço devolve um ou mais respostas JSON que contém o texto reconhecido e traduzido. Se tiver sido solicitada text-to-speech, é enviado um ficheiro de áudio.

O utilizador escolhe a origem de áudio utilizando o menu de entrada do microfone/ficheiro. Áudio pode ter a partir de um dispositivo de áudio (por exemplo, um microfone) ou de um `.WAV` ficheiro.

O método `StartListening_Click` é invocada quando o utilizador clica no botão de início. Este processador de eventos, por sua vez, chama `Connect()` para iniciar o processo de envio de áudio para o ponto final de API do serviço. O `Connect()` método executa as seguintes tarefas:


> [!div class="checklist"]
> * Obter as definições de utilizador da janela principal e a validá-los
> * A inicializar a entrada de áudio e fluxos de saída
> * Chamar `ConnectAsync()` para lidar com o resto do trabalho

`ConnectAsync()`, por sua vez, processa as chores seguintes:

> [!div class="checklist"]
> * Autenticação com a chave de subscrição do Azure no cabeçalho `Ocp-Apim-Subscription-Key`
> * Criar um `SpeechClient` instância (localizado no `SpeechClient.cs`) para comunicar com o serviço
> * A inicializar `TextMessageDecoder` e `BinaryMessageDecoder` instâncias (consulte `SpeechResponseDecoder.cs`) para processar as respostas
> * Enviar áudio através de `SpeechClient` instância para o serviço de reconhecimento de voz tradutor
> * Receber e a processar os resultados da tradução

Responsabilidades do `SpeechClient` são menos:

> [!div class="checklist"]
> * Estabelecer uma ligação de WebSocket para o serviço de reconhecimento de voz tradutor
> * Enviar dados de áudio e receber respostas através do socket

## <a name="a-closer-look"></a>Observá

Deve ser mais clara agora como partes da aplicação funcionam em conjunto para efetuar o pedido de conversão. Vamos ver alguns códigos, concentrar-se nas partes relevantes.

Eis uma versão parcial do `Connect()` definição que aparece os fluxos de áudio:

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

Uma parte considerável do `Connect()` envolve a criação de um `SpeechClientOptions` instância (consulte `SpeechClientOptions.cs`) para conter as opções de conversão. As opções incluem as informações necessárias para ligar ao serviço (como chave de autenticação e nome de anfitrião) e as funcionalidades utilizadas para a conversão. Os campos aqui mapeiam para os parâmetros HTTP expostos pelo e campos de cabeçalho [a API de reconhecimento de voz tradutor](http://docs.microsofttranslator.com/speech-translate.html).

`Connect()` também cria e inicia o dispositivo de entrada de áudio (variável `sampleProvider`) que funciona como origem de reconhecimento de voz para ser convertido. Este dispositivo é o um dispositivo de entrada de hardware, tais como um microfone ou um ficheiro que contém os dados de áudio WAVE.

Eis o `ConnectAsync()` método que cria uma instância de `speechClient` classe e hooks anónimas funções para processar texto e as respostas de binárias do serviço de cópia de segurança.

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

Após a autenticação, o método cria o `SpeechClient` instância. O `SpeechClient` classe (no `SpeechClient.cs`) invoca processadores de eventos após dados binários e de texto de receção. Processadores adicionais são invocados quando a ligação falha ou desliga.

Dados binários são áudio (saída text-to-speech) enviado pelo serviço quando TTS está ativada. Dados de texto são parcial ou uma tradução completa do texto ditas. Para depois instanciar, o método hooks configurar funções para processar estas mensagens: áudio armazenando-o para posterior reprodução e texto, apresentando-na janela.

## <a name="next-steps"></a>Passos Seguintes

Este exemplo de código é uma aplicação avançada em termos de funcionalidade que demonstra a utilização da API de reconhecimento de voz tradutor. Como tal, existem justa diversas partes mover compreender. Tiver walked utilizando os bits mais importantes. Para os restantes, pode ser instructive para definir alguns pontos de interrupção no Visual Studio e guiá-lo durante o processo de conversão. Quando compreender a aplicação de exemplo, estiver equipado para utilizar o serviço de reconhecimento de voz tradutor nas suas próprias aplicações.

> [!div class="nextstepaction"]
> [Referência da API de reconhecimento de voz do Microsoft tradutor](http://docs.microsofttranslator.com/speech-translate.html)
