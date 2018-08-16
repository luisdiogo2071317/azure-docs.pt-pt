---
title: 'Tutorial: Escreva uma aplicação de WPF para a Tradução de Texto com C# | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: Neste tutorial, irá aprender como utilizar a API de Tradução de Texto para traduzir texto, obter uma lista localizada dos idiomas suportados e muito mais, ao criar uma aplicação do WPF com o C#.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: tutorial
ms.date: 07/20/2018
ms.author: nolachar
ms.openlocfilehash: 353c1d91b7925a84667ef1bb7c38ab87c6a89cc1
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39716377"
---
# <a name="tutorial-write-a-wpf-application-for-translator-text-using-c35"></a>Tutorial: Escreva uma aplicação de WPF para a Tradução de Texto com C&#35;

Neste tutorial, irá criar uma ferramenta de tradução de texto interativa com a API de Tradução de Texto (V3), uma peça dos Serviços Cognitivos da Microsoft no Azure. Vai aprender a:

> [!div class="checklist"]
> * Obter uma lista dos idiomas suportados pelo serviço
> * Executar uma tradução de texto inserida pelo utilizador de um idioma para outro

Esta aplicação também apresenta a integração com dois outros Serviços Cognitivos da Microsoft.

|||
|-|-|
|[Análise de Texto](https://azure.microsoft.com/services/cognitive-services/text-analytics/)|Utilizada para detetar automaticamente, opcionalmente, o idioma de origem do texto a ser traduzido|
|[Verificação de Ortografia do Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/)|Para texto de origem em inglês, utilizado para corrigir erros ortográficos, para que a tradução seja mais precisa

![[O tutorial do programa em execução]](media/translator-text-csharp-session.png)

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [Visual Studio 2017](https://www.visualstudio.com/downloads/) para executar este código no Windows. (A Edição de Comunidade gratuita irá funcionar.)

Também precisa de chaves de subscrição para os três serviços do Azure utilizados no programa. Pode obter uma chave para o serviço de Tradução de Texto a partir do dashboard do Azure. Está disponível um escalão de preço gratuito que lhe permite traduzir até dois milhões de carateres por mês sem custos.

Os serviços de Análise de Texto e Verificação de Ortografia do Bing oferecem avaliações gratuitas, nos quais se pode inscrever em [Experimentar Serviços Cognitivos](https://azure.microsoft.com/try/cognitive-services/). Também pode criar uma subscrição para qualquer um dos serviços através do dashboard do Azure. A Análise de Texto tem um escalão gratuito.

O código de origem para este tutorial está disponível abaixo. As chaves de subscrição devem ser copiadas para o código de origem, como as variáveis `TEXT_TRANSLATION_API_SUBSCRIPTION_KEY`, e assim por diante, em `MainWindow.xaml.cs`.

> [!IMPORTANT]
> O serviço de Análise de Texto está disponível em várias regiões. O URI neste código de origem do tutorial está na região `westus`, que é a região utilizada nas avaliações gratuitas. Se tiver uma subscrição noutra região, atualize este URI em conformidade.

## <a name="source-code"></a>Código de origem

Este é o código de origem para a API de texto do Microsoft Translator. Para executar a aplicação, copie o código de origem para o ficheiro adequado num novo projeto do WPF no Visual Studio.

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Este é o ficheiro code-behind que oferece a funcionalidade da aplicação.

```csharp
using System;
using System.Windows;
using System.Net;
using System.Net.Http;
using System.IO;
using System.Collections.Generic;
using System.Linq;
using System.Text;

// NOTE: Add assembly references to System.Runtime.Serialization, System.Web, and System.Web.Extensions.

// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace MSTranslatorTextDemo
{
    /// <summary>
    /// This WPF application demonstrates the use of the Microsoft Translator Text API to translate a brief text string from
    /// one language to another. The languages are selected from a drop-down menu. The text of the translation is displayed.
    /// The source language may optionally be automatically detected. English text is spell-checked.
    /// </summary>
    public partial class MainWindow : Window
    {
        // Translator text subscription key from Microsoft Azure dashboard
        const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
        const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

        public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
        const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
        const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

        private string[] languageCodes;     // array of language codes

        // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
        private SortedDictionary<string, string> languageCodesAndTitles =
            new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

        public MainWindow()
        {
            // at least show an error dialog if there's an unexpected error
            AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

            if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
                || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
                || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
            {
                MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                    "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                    "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
                System.Windows.Application.Current.Shutdown();
            }
            else
            {
                InitializeComponent();          // start the GUI
                GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
                PopulateLanguageMenus();        // fill the drop-down language lists
            }
        }

        // Global exception handler to display error message and exit
        private static void HandleExceptions(object sender, UnhandledExceptionEventArgs args)
        {
            Exception e = (Exception)args.ExceptionObject;
            MessageBox.Show("Caught " + e.Message, "Error", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }

        // ***** POPULATE LANGUAGE MENUS
        private void PopulateLanguageMenus()
        {
            // Add option to automatically detect the source language
            FromLanguageComboBox.Items.Add("Detect");

            int count = languageCodesAndTitles.Count;
            foreach (string menuItem in languageCodesAndTitles.Keys)
            {
                FromLanguageComboBox.Items.Add(menuItem);
                ToLanguageComboBox.Items.Add(menuItem);
            }

            // set default languages
            FromLanguageComboBox.SelectedItem = "Detect";
            ToLanguageComboBox.SelectedItem = "English";
        }

        // ***** DETECT LANGUAGE OF TEXT TO BE TRANSLATED
        private string DetectLanguage(string text)
        {
            string uri = TEXT_ANALYTICS_API_ENDPOINT + "languages?numberOfLanguagesToDetect=1";

            // create request to Text Analytics API
            HttpWebRequest detectLanguageWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_ANALYTICS_API_SUBSCRIPTION_KEY);
            detectLanguageWebRequest.Method = "POST";

            // create and send body of request
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            string jsonText = serializer.Serialize(text);

            string body = "{ \"documents\": [ { \"id\": \"0\", \"text\": " + jsonText + "} ] }";
            byte[] data = Encoding.UTF8.GetBytes(body);
            detectLanguageWebRequest.ContentLength = data.Length;

            using (var requestStream = detectLanguageWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);

            HttpWebResponse response = (HttpWebResponse)detectLanguageWebRequest.GetResponse();

            // read and parse JSON response
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);

            // fish out the detected language code
            var languageInfo = jsonResponse["documents"][0]["detectedLanguages"][0];
            if (languageInfo["score"] > (decimal)0.5)
                return languageInfo["iso6391Name"];
            else
                return "";
        }

        // ***** CORRECT SPELLING OF TEXT TO BE TRANSLATED
        private string CorrectSpelling(string text)
        {
            string uri = BING_SPELL_CHECK_API_ENDPOINT + "?mode=spell&mkt=en-US";

            // create request to Bing Spell Check API
            HttpWebRequest spellCheckWebRequest = (HttpWebRequest)WebRequest.Create(uri);
            spellCheckWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", BING_SPELL_CHECK_API_SUBSCRIPTION_KEY);
            spellCheckWebRequest.Method = "POST";
            spellCheckWebRequest.ContentType = "application/x-www-form-urlencoded"; // doesn't work without this

            // create and send body of request
            string body = "text=" + System.Web.HttpUtility.UrlEncode(text);
            byte[] data = Encoding.UTF8.GetBytes(body);
            spellCheckWebRequest.ContentLength = data.Length;
            using (var requestStream = spellCheckWebRequest.GetRequestStream())
                requestStream.Write(data, 0, data.Length);
            HttpWebResponse response = (HttpWebResponse)spellCheckWebRequest.GetResponse();

            // read and parse JSON response and get spelling corrections
            var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
            var responseStream = response.GetResponseStream();
            var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
            dynamic jsonResponse = serializer.DeserializeObject(jsonString);
            var flaggedTokens = jsonResponse["flaggedTokens"];

            // construct sorted dictionary of corrections in reverse order in string (right to left)
            // so that making a correction can't affect later indexes
            var corrections = new SortedDictionary<int, string[]>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
            for (int i = 0; i < flaggedTokens.Length; i++)
            {
                var correction = flaggedTokens[i];
                var suggestion = correction["suggestions"][0];  // consider only first suggestion
                if (suggestion["score"] > (decimal)0.7)         // take it only if highly confident
                    corrections[(int)correction["offset"]] = new string[]   // dict key   = offset
                        { correction["token"], suggestion["suggestion"] };  // dict value = {error, correction}
            }

            // apply the corrections in order from right to left
            foreach (int i in corrections.Keys)
            {
                var oldtext = corrections[i][0];
                var newtext = corrections[i][1];

                // apply capitalization from original text to correction - all caps or initial caps
                if (text.Substring(i, oldtext.Length).All(char.IsUpper)) newtext = newtext.ToUpper();
                else if (char.IsUpper(text[i])) newtext = newtext[0].ToString().ToUpper() + newtext.Substring(1);

                text = text.Substring(0, i) + newtext + text.Substring(i + oldtext.Length);
            }

            return text;
        }

        // ***** GET TRANSLATABLE LANGUAGE CODES
        private void GetLanguagesForTranslate()
        {
            // send request to get supported language codes
            string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
            WebRequest WebRequest = WebRequest.Create(uri);
            WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
            WebRequest.Headers.Add("Accept-Language", "en");
            WebResponse response = null;
            // read and parse the JSON response
            response = WebRequest.GetResponse();
            using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
            {
                var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
                var languages = result["translation"];

                languageCodes = languages.Keys.ToArray();
                foreach (var kv in languages)
                {
                    languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
                }
            }
        }

        // ***** PERFORM TRANSLATION ON BUTTON CLICK
        private async void TranslateButton_Click(object sender, EventArgs e)
        {
            string textToTranslate = TextToTranslate.Text.Trim();

            string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
            string fromLanguageCode;

            // auto-detect source language if requested
            if (fromLanguage == "Detect")
            {
                fromLanguageCode = DetectLanguage(textToTranslate);
                if (!languageCodes.Contains(fromLanguageCode))
                {
                    MessageBox.Show("The source language could not be detected automatically " +
                        "or is not supported for translation.", "Language detection failed",
                        MessageBoxButton.OK, MessageBoxImage.Error);
                    return;
                }
            }
            else
                fromLanguageCode = languageCodesAndTitles[fromLanguage];

            string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

            // spell-check the source text if the source language is English
            if (fromLanguageCode == "en")
            {
                if (textToTranslate.StartsWith("-"))    // don't spell check in this case
                    textToTranslate = textToTranslate.Substring(1);
                else
                {
                    textToTranslate = CorrectSpelling(textToTranslate);
                    TextToTranslate.Text = textToTranslate;     // put corrected text into input field
                }
            }

            // handle null operations: no text or same source/target languages
            if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
            {
                TranslatedTextLabel.Content = textToTranslate;
                return;
            }

            // send HTTP request to perform the translation
            string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
            string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

            System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
            var requestBody = JsonConvert.SerializeObject(body);

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
                request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();

                var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
                var translations = result[0]["translations"];
                var translation = translations[0]["text"];

                // Update the translation field
                TranslatedTextLabel.Content = translation;
            }
        }
    }
}
```

### <a name="mainwindowxaml"></a>MainWindow.xaml

Este ficheiro define a interface do utilizador da aplicação, um formulário do WPF. Se quiser criar a sua própria versão do formulário, não precisa deste XAML.

```xml
<Window x:Class="MSTranslatorTextDemo.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:MSTranslatorTextDemo"
        mc:Ignorable="d"
        Title="Microsoft Translator" Height="400" Width="700" BorderThickness="0">
    <Grid>
        <Label x:Name="label" Content="Microsoft Translator" HorizontalAlignment="Left" Margin="39,6,0,0" VerticalAlignment="Top" Height="49" FontSize="26.667"/>
        <TextBox x:Name="TextToTranslate" HorizontalAlignment="Left" Height="23" Margin="42,160,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="600" FontSize="14" TabIndex="3"/>
        <Label x:Name="EnterTextLabel" Content="Text to translate:" HorizontalAlignment="Left" Margin="40,129,0,0" VerticalAlignment="Top" FontSize="14"/>
        <Label x:Name="toLabel" Content="Translate to:" HorizontalAlignment="Left" Margin="304,58,0,0" VerticalAlignment="Top" FontSize="14"/>

        <Button x:Name="TranslateButton" Content="Translate" HorizontalAlignment="Left" Margin="39,206,0,0" VerticalAlignment="Top" Width="114" Height="31" Click="TranslateButton_Click" FontSize="14" TabIndex="4" IsDefault="True"/>
        <ComboBox x:Name="ToLanguageComboBox"
                HorizontalAlignment="Left"
                Margin="306,88,0,0"
                VerticalAlignment="Top"
                Width="175" FontSize="14" TabIndex="2">

        </ComboBox>
        <Label x:Name="fromLabel" Content="Translate from:" HorizontalAlignment="Left" Margin="40,58,0,0" VerticalAlignment="Top" FontSize="14"/>
        <ComboBox x:Name="FromLanguageComboBox"
            HorizontalAlignment="Left"
            Margin="42,88,0,0"
            VerticalAlignment="Top"
            Width="175" FontSize="14" TabIndex="1"/>
        <Label x:Name="TranslatedTextLabel" Content="Translation appears here" HorizontalAlignment="Left" Margin="39,255,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="85" BorderThickness="0"/>
    </Grid>
</Window>
```

## <a name="service-endpoints"></a>Pontos finais de serviço

O serviço Microsoft Translator tem vários pontos finais que oferecem várias peças da funcionalidade de tradução. As que são utilizadas neste tutorial são:

|||
|-|-|
|`Languages`|Devolve o conjunto de idiomas atualmente suportados por outras operações da API de Tradução de Texto.|
|`Translate`|Tendo em conta o texto de origem, um código do idioma de origem e um código do idioma de destino, devolve uma tradução do texto de origem para o idioma de destino.|

## <a name="the-translation-app"></a>A aplicação de tradução

A interface do utilizador da aplicação é criada com o Windows Presentation Foundation (WPF). Crie um novo projeto do WPF no Visual Studio ao seguir estes passos.

* No menu **Ficheiro**, escolha **Novo > Projeto**.
* Na janela Novo Projeto, abra **Instalados > Modelos > Visual C#**. É apresentada uma lista dos modelos de projeto disponíveis no centro da caixa de diálogo.
* Certifique-se de que **.NET Framework 4.5.2** é escolhido no menu pendente acima da lista de modelos do projeto.
* Clique em **Aplicação WPF (.NET Framework)** na lista de modelos de projeto.
* Com os campos na parte inferior da caixa de diálogo, nomeie o novo projeto e a solução que o contém.
* Clique em **OK** para criar o novo projeto e a solução.

![[Criar uma nova aplicação do WPF no Visual Studio]](media/translator-text-csharp-new-project.png)

Adicione referências aos seguintes conjuntos de .NET framework no seu projeto.

* System.Runtime.Serialization
* System.Web
* System.Web.Extensions

Além disso, instale o pacote NuGet `Newtonsoft.Json` no seu projeto.

Agora encontre o ficheiro `MainWindow.xaml` no Explorador de Soluções e abra-o. Inicialmente está em branco. A interface do utilizador concluída deve ser semelhante ao seguinte, anotada com os nomes dos controlos em azul. Utilize os mesmos nomes para os controlos na interface do utilizador, uma vez que eles também aparecem no código.

![[Vista anotada da janela principal no designer do Visual Studio]](media/translator-text-csharp-xaml.png)

> [!NOTE]
> O código de origem deste tutorial inclui a origem XAML para este formulário. Pode colá-lo ao seu projeto, em vez de criar o formulário no Visual Studio.

* `FromLanguageComboBox` *(ComboBox)* - Apresenta uma lista dos idiomas suportados pelo Microsoft Translator para tradução de texto. O utilizador seleciona o idioma de origem que está a traduzir.
* `ToLanguageComboBox` *(ComboBox)* - Apresenta a mesma lista de idiomas que `FromComboBox`, mas é utilizado para selecionar o idioma para o qual o utilizador está a traduzir.
* `TextToTranslate` *(TextBox)* - O utilizador insere o texto a ser traduzido aqui.
* `TranslateButton` *(Botão)* - O utilizador clica neste botão (ou prime Enter) para traduzir o texto.
* `TranslatedTextLabel` *(Etiqueta)* - A tradução do texto do utilizador é apresentada aqui.

Se estiver a fazer a sua própria versão deste formulário, não é preciso torná-la *exatamente* como a que foi aqui utilizada. Mas certifique-se de que as listas pendentes dos idiomas são grandes o suficiente para evitar cortar um nome de idioma.

## <a name="the-mainwindow-class"></a>A classe MainWindow

O ficheiro code-behind `MainWindow.xaml.cs` é para onde o código vai que faz o programa fazer o que ele faz. O trabalho ocorrerá em dois momentos:

* Quando o programa é iniciado e `MainWindow` é instanciado, obtém a lista de idiomas que utilizam o Translator e APIs e preenche os menus pendentes com eles. Esta tarefa é concluída uma vez, no início de cada sessão.

* Quando o utilizador clica no botão **Traduzir**, as seleções de idioma do utilizador são obtidas e o texto que introduziram e, em seguida, a API `Translate` é chamada para fazer a tradução. Também podem ser chamadas outras funções para determinar o idioma do texto e corrigir a ortografia antes da tradução.

Dê uma vista de olhos no início da classe:

```csharp
public partial class MainWindow : Window
{
    // Translator text subscription key from Microsoft Azure dashboard
    const string TEXT_TRANSLATION_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string TEXT_ANALYTICS_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";
    const string BING_SPELL_CHECK_API_SUBSCRIPTION_KEY = "ENTER KEY HERE";

    public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
    const string TEXT_ANALYTICS_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/";
    const string BING_SPELL_CHECK_API_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/";

    private string[] languageCodes;     // array of language codes

    // Dictionary to map language code from friendly name (sorted case-insensitively on language name)
    private SortedDictionary<string, string> languageCodesAndTitles =
        new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

    public MainWindow()
    {
        // at least show an error dialog if there's an unexpected error
        AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

        if (TEXT_TRANSLATION_API_SUBSCRIPTION_KEY.Length != 32
            || TEXT_ANALYTICS_API_SUBSCRIPTION_KEY.Length != 32
            || BING_SPELL_CHECK_API_SUBSCRIPTION_KEY.Length != 32)
        {
            MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
            System.Windows.Application.Current.Shutdown();
        }
        else
        {
            InitializeComponent();          // start the GUI
            GetLanguagesForTranslate();     // get codes and friendly names of languages that can be translated
            PopulateLanguageMenus();        // fill the drop-down language lists
        }
    }
// more to come
}
```

Duas variáveis de membro aqui declaradas contêm informações sobre os nossos idiomas:

|||
|-|-|
|`languageCodes`<br>matriz da cadeia|Coloca em cache os códigos de idioma. O serviço Translator utiliza códigos curtos, como `en` para inglês, para identificar idiomas.|
|`languageCodesAndTitles`<br>SortedDictionary|Mapeia os nomes "amigáveis" na interface do utilizador de volta para os códigos curtos utilizados na API. São mantidos ordenados por ordem alfabética, sem ter em conta as maiúsculas/minúsculas.|

O primeiro código executado pela aplicação é o construtor `MainWindow`. Primeiro, configure o método `HandleExceptions` como o processador de erros global. Desta forma, há, pelo menos, um alerta de erro se uma exceção não for processada.

Em seguida, verifique se que as chaves de subscrição da API têm todas exatamente 32 carateres. Se não tiverem, a razão mais provável é que *alguém* ainda não colou as suas chaves de API. Neste caso, apresente uma mensagem de erro e saia. (Passar neste teste não significa que as chaves são válidas.)

Se existirem chaves com, pelo menos o comprimento certo, a chamada `InitializeComponent()` obtém a interface do utilizador funcionar ao localizar, carregar e instanciar a descrição de XAML da janela principal da aplicação.

Por fim, configure os menus pendentes de idiomas. Esta tarefa exige três chamadas de método separado, que são abordadas em detalhe nas secções seguintes.

## <a name="get-supported-languages"></a>Obter idiomas suportados

O serviço Microsoft Translator suporta um total de 61 idiomas até ao momento e, de vez em quando, podem ser adicionados mais. Por isso, é melhor não codificar os idiomas suportados no seu programa. Em vez disso, pergunte ao serviço do Translator que idiomas suporta. Qualquer idioma suportado pode ser traduzido para qualquer outro idioma suportado.

Chame a API `Languages` para obter a lista de idiomas suportados.

A API `Languages` assume um parâmetro de consulta GET opcional, *âmbito*. O *âmbito* pode ter um de três valores: `translation`, `transliteration` e `dictionary`. Este código utiliza o valor `translation`.

A API `Languages` também utiliza um cabeçalho HTTP opcional, `Accept-Language`. O valor deste cabeçalho determina o idioma em que os nomes dos idiomas suportados são devolvidos. O valor deve ser uma etiqueta de idioma BCP 47 bem formada. Este código utiliza o valor `en` para obter os nomes de idiomas em inglês.

A API `Languages` devolve uma resposta JSON que é semelhante à seguinte.

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    },
...
}
```

Para que os códigos de idioma (por exemplo, `af`) e os nomes de idioma (por exemplo, `Afrikaans`) possam ser extraídos, este código utiliza o método NewtonSoft.Json [JsonConvert.DeserializeObject](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm).

Com este conhecimento em segundo plano, crie o método seguinte para recuperar os códigos de idioma e os respetivos nomes.

```csharp
private void GetLanguagesForTranslate()
{
    // send request to get supported language codes
    string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
    WebRequest WebRequest = WebRequest.Create(uri);
    WebRequest.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
    WebRequest.Headers.Add("Accept-Language", "en");
    WebResponse response = null;
    // read and parse the JSON response
    response = WebRequest.GetResponse();
    using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
    {
        var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
        var languages = result["translation"];

        languageCodes = languages.Keys.ToArray();
        foreach (var kv in languages)
        {
            languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
        }
    }
}
```

`GetLanguagesForTranslate()` cria primeiro o pedido HTTP. O parâmetro de cadeia de consulta `scope=translation` pede apenas esses idiomas suportados para a tradução de texto. A chave de subscrição da API de Tradução de Texto é adicionada aos cabeçalhos de pedido. O cabeçalho `Accept-Language` com o valor `en` é adicionado para que os idiomas suportados sejam devolvidos em inglês.

Depois de concluir o pedido, a resposta JSON é analisada e convertida num Dicionário e, em seguida, os códigos de idioma são adicionados à variável de membro `languageCodes`. Os pares de chave/valor que contêm os códigos de idioma e os nomes amigáveis de idiomas são colocados num ciclo e adicionados à variável de membro `languageCodesAndTitles`. (Os menus pendentes no formulário apresentam os nomes amigáveis, mas os códigos são precisos para pedir a tradução.)

## <a name="populate-the-language-menus"></a>Preencher os menus de idioma

A maioria da interface do utilizador é definida no XAML, pelo que é muito simples configurá-lo além da chamada `InitializeComponent()`. A única coisa que precisa de fazer é adicionar os nomes amigáveis de idiomas aos menus pendentes **Traduzir de** e **Traduzir para**, que é feito no `PopulateLanguageMenus()`.

```csharp
private void PopulateLanguageMenus()
{
    // Add option to automatically detect the source language
    FromLanguageComboBox.Items.Add("Detect");

    int count = languageCodesAndTitles.Count;
    foreach (string menuItem in languageCodesAndTitles.Keys)
    {
        FromLanguageComboBox.Items.Add(menuItem);
        ToLanguageComboBox.Items.Add(menuItem);
    }

    // set default languages
    FromLanguageComboBox.SelectedItem = "Detect";
    ToLanguageComboBox.SelectedItem = "English";
}
```

Preencher os menus é simplesmente iterar sobre o dicionário `languageCodesAndTitles` e adicionar cada chave, que é o nome "amigável", a ambos os menus. Depois de preencher os menus, os idiomas "para" e "de" predefinidos são definidos como **Detetar** (para detetar automaticamente o idioma) e **Inglês**.

> [!TIP]
> Sem uma seleção predefinida para os menus, o utilizador pode clicar em **Traduzir**, sem primeiro escolher um idioma "para" ou "de". As predefinições eliminam a necessidade de lidar com este problema.

Agora que `MainWindow` foi inicializado e a interface do utilizador criada, o código irá aguardar até que o utilizador clique no botão **Traduzir**.

## <a name="perform-translation"></a>Fazer a tradução

Quando o utilizador clica em **Traduzir**, o WPF invoca o processador de eventos `TranslateButton_Click()` mostrado aqui.

```csharp
private async void TranslateButton_Click(object sender, EventArgs e)
{
    string textToTranslate = TextToTranslate.Text.Trim();

    string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
    string fromLanguageCode;

    // auto-detect source language if requested
    if (fromLanguage == "Detect")
    {
        fromLanguageCode = DetectLanguage(textToTranslate);
        if (!languageCodes.Contains(fromLanguageCode))
        {
            MessageBox.Show("The source language could not be detected automatically " +
                "or is not supported for translation.", "Language detection failed",
                MessageBoxButton.OK, MessageBoxImage.Error);
            return;
        }
    }
    else
        fromLanguageCode = languageCodesAndTitles[fromLanguage];

    string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

    // spell-check the source text if the source language is English
    if (fromLanguageCode == "en")
    {
        if (textToTranslate.StartsWith("-"))    // don't spell check in this case
            textToTranslate = textToTranslate.Substring(1);
        else
        {
            textToTranslate = CorrectSpelling(textToTranslate);
            TextToTranslate.Text = textToTranslate;     // put corrected text into input field
        }
    }

    // handle null operations: no text or same source/target languages
    if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
    {
        TranslatedTextLabel.Content = textToTranslate;
        return;
    }

    // send HTTP request to perform the translation
    string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
    string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

    System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
    var requestBody = JsonConvert.SerializeObject(body);

    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = HttpMethod.Post;
        request.RequestUri = new Uri(uri);
        request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
        request.Headers.Add("Ocp-Apim-Subscription-Key", TEXT_TRANSLATION_API_SUBSCRIPTION_KEY);
        request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();

        var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
        var translations = result[0]["translations"];
        var translation = translations[0]["text"];

        // Update the translation field
        TranslatedTextLabel.Content = translation;
    }
}
```

A primeira etapa é obter os idiomas "para" e "de", juntamente com o texto que o utilizador introduziu do formulário.

Se o idioma de origem estiver definido como **Detetar**, faça uma chamada para `DetectLanguage()`, para determinar o idioma do texto. O texto poderá estar num idioma que as APIs do Translator não suportam (podem ser detetados mais que podem ser traduzidos) ou a API de Análise de Texto poderá não conseguir detetá-lo. Nesse caso, é apresentada uma mensagem a informar o utilizador e voltar sem tradução.

Se o idioma de origem é inglês (seja especificado ou detetado), verifique a ortografia de texto com `CorrectSpelling()` e aplique as correções precisas. O texto corrigido é inserido novamente no campo de texto, para que o utilizador saiba que a correção foi feita. (O utilizador pode preceder o texto a ser traduzido com um hífen para suprimir a correção ortográfica.)

Se o utilizador não tiver inserido qualquer texto, ou se os idiomas "para" e "de" são os mesmos, não é precisa uma conversão e o pedido pode ser evitado.

O código para fazer o pedido de tradução deve parecer familiar: compile o URI, crie um pedido, envie-o e analise a resposta. Para apresentar o texto, envie-o para o controlo `TranslatedTextLabel`.

Em seguida, passe o texto para a API `Translate` numa matriz JSON serializada no corpo de um pedido POST. A matriz JSON pode conter várias partes de texto a traduzir, mas aqui é precisa apenas uma.

O cabeçalho de HTTP chamado `X-ClientTraceId` é opcional. O valor deve ser um GUID. O ID de rastreio fornecido pelo cliente é útil para pedidos de rastreio quando as coisas não funcionam conforme esperado. No entanto, para ser útil, o valor de X-ClientTraceID tem de ser registado pelo cliente. Um ID de rastreio de cliente e a data dos pedidos podem ajudar a Microsoft a diagnosticar problemas que possam ocorrer.

> [!NOTE]
> Este tutorial concentra-se no serviço Microsoft Translator, então, os métodos `DetectLanguage()` e `CorrectSpelling()` não são descritos detalhadamente.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Referência da API de Texto do Microsoft Translator](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
