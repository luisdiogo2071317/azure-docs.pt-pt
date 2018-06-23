---
title: Como escrever uma aplicação do Microsoft tradutor WPF com c#, serviços cognitivos do Azure | Microsoft Docs
description: Saiba como utilizar o serviço de texto de conversor para traduzir texto, obter uma lista localizada de idiomas suportados e muito mais.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-text
ms.devlang: csharp
ms.topic: article
ms.date: 10/25/2017
ms.author: v-jansko
ms.openlocfilehash: fb58fd087de09561a0ea930748562e595d3dde1c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352603"
---
# <a name="how-to-write-a-microsoft-translator-wpf-application-in-c"></a>Como escrever uma aplicação do Microsoft tradutor WPF em c#

Neste tutorial, irá criar uma ferramenta de conversão de texto interativo com a Microsoft tradutor texto API (V3), uma parte dos serviços cognitivos da Microsoft no Azure. Irá aprender como:

> [!div class="checklist"]
> * Pediu uma lista dos códigos de curtos para os idiomas suportados pelo serviço
> * Obter uma lista de nomes de idioma localizado correspondente a estes códigos de idioma
> * Obter a versão traduzida do texto de utilizador introduzido a partir de um idioma para outro

Esta aplicação também funcionalidades de integração com duas outras cognitivos serviços Microsoft.

|||
|-|-|
|[Análise de texto](https://azure.microsoft.com/services/cognitive-services/text-analytics/)|Utilizado para detetar opcionalmente automaticamente o idioma da origem do texto para ser convertido|
|[Verificação de ortográfica do Bing](https://azure.microsoft.com/services/cognitive-services/spell-check/)|Para texto de origem em inglês, utilizado para corrigir misspellings pelo tradução é mais exata

![[O tutorial programa em execução]](media/translator-text-csharp-session.png)

## <a name="prerequisites"></a>Pré-requisitos

Para este tutorial, terá de qualquer edição do Visual Studio 2017, incluindo a edição de Comunidade.

Também precisa de chaves de subscrição para os três serviços do Azure utilizados no programa. Pode obter uma chave para o serviço de texto de conversor a partir do dashboard do Azure. Um escalão de preço gratuito está disponível que lhe permite converter carateres de dois milhões por mês, sem encargos.

A análise de texto e verificação de ortográfica do Bing serviços oferta gratuitas, que pode inscrever-se no [tente serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/). Também pode criar uma subscrição para o serviço através do dashboard do Azure. Análise de texto tem um escalão gratuito.

Código de origem para este tutorial está disponível abaixo. As chaves de subscrição devem ser copiadas para o código de origem, como as variáveis `TEXT_TRANSLATION_API_SUBSCRIPTION_KEY`, e assim sucessivamente, no `MainWindow.xaml.cs`.

> [!IMPORTANT]
> O serviço de análise de texto está disponível em várias regiões. O URI no nosso código do tutorial de origem está a ser o `westus` região, o que é a região utilizados gratuitamente avaliações. Se tiver uma subscrição noutra região, atualize este URI em conformidade.

## <a name="source-code"></a>Código de origem

Este é o código de origem para o texto do Microsoft Translator API. Para executar a aplicação, copie o código de origem para o ficheiro adequado num novo projeto do WPF no Visual Studio.

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Este é o ficheiro code-behind que fornece uma funcionalidade da aplicação.

```cs
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
    /// This WPF application demonstrates the use of the Microsoft Translator Text API to translate a brief text string
    /// one language to another. The langauges are selected from a drop-down menu. The text of the translation is displayed.
    /// The source language may optionally be automatically detected.  English text is spell-checked.
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
            // at least show an error dialog when we get an unexpected error
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

            // read and and parse JSON response
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

Este ficheiro define a interface de utilizador para a aplicação, um formulário da WPF. Se pretender conceber a sua própria versão do formulário, não terá este XAML.

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

O serviço Microsoft Translator tem vários pontos finais que fornecem várias partes da funcionalidade da tradução. São aqueles que iremos utilizar neste tutorial:

|||
|-|-|
|`Languages`|Devolve o conjunto de idiomas atualmente suportados por outras operações da API de texto tradutor.|
|`Translate`|Tendo em conta o texto de origem, um código de idioma da origem e um código de idioma de destino, devolve uma conversão de texto de origem para o idioma de destino.|

## <a name="the-translation-app"></a>A aplicação de conversão

Interface de utilizador da nossa aplicação tradutor é criada com o Windows Presentation Foundation (WPF). Crie um novo projeto do WPF no Visual Studio, seguindo estes passos.

* Do **ficheiro** menu, escolha **novo > projeto**.
* Na janela novo projeto, abra **instalada > modelos > Visual c#**. É apresentada uma lista dos modelos de projeto disponível no centro da caixa de diálogo.
* Certifique-se **.NET Framework 4.5.2** é escolhido no menu pendente acima a lista de modelo de projeto.
* Clique em **aplicação WPF (.NET Framework)** na lista de modelo de projeto.
* Utilizar os campos na parte inferior da caixa de diálogo, dê um nome novo projeto e a solução que contém.
* Clique em **OK** para criar o novo projeto e a solução.

![[Criar uma nova aplicação WPF no Visual Studio]](media/translator-text-csharp-new-project.png)

Adicione referências para as assemblagens seguintes do .NET framework ao seu projeto.

* System.Runtime.Serialization
* System. Web
* System.Web.Extensions

Além disso, instale o pacote NuGet `Newtonsoft.Json` no seu projeto.

Localizar agora o `MainWindow.xaml` ficheiro no Explorador de soluções e abri-lo. Está em branco inicialmente. Eis o que a interface de utilizador foi terminada deve ter o seguinte aspeto, anotada com os nomes dos controlos azul. Utilize os mesmos nomes para os controlos na sua interface de utilizador, uma vez que também aparecem no código.

![[Vista anotada da janela principal no designer Visual Studio]](media/translator-text-csharp-xaml.png)

> [!NOTE]
> O código de origem para este tutorial inclui origem XAML deste formulário. Pode colá-la ao seu projeto em vez de criar o formulário no Visual Studio.

* `FromLanguageComboBox` *(ComboBox)*  -Mostra uma lista de idiomas suportados pelo Microsoft Translator para conversão de texto. O utilizador seleciona o idioma são traduzir a partir de.
* `ToLanguageComboBox` *(ComboBox)*  -Apresenta a mesma lista de idiomas como `FromComboBox`, mas é utilizado para selecionar o idioma do utilizador é traduzir para.
* `TextToTranslate` *(Caixa de texto)*  -O utilizador introduz o texto a ser traduzido aqui.
* `TranslateButton` *(Botão)*  -O utilizador clica neste botão (ou quando prime Enter) para converter o texto.
* `TranslatedTextLabel` *(Etiqueta)*  -A conversão de texto do utilizador são apresentados aqui.

Se estiver a efetuar a sua própria versão deste formulário, não é necessária para torná-lo *exatamente* como ours. Mas, certifique-se que o idioma listas pendentes, são suficientemente alargadas para evitar corte desativar um nome de idioma.

## <a name="the-mainwindow-class"></a>A classe de MainWindow

O ficheiro code-behind `MainWindow.xaml.cs` é onde o código passa que faz com que o programa efetue o que faz. O trabalho ocorre em duas vezes:

* Quando inicia o programa. Quando `MainWindow` é instanciado, estamos a obter a lista de idiomas através do conversor `GetLanguagesForTranslate` e `GetLanguageNames` APIs e preencher a nossa menus de lista pendente com os mesmos. Esta tarefa é efetuada uma vez, no início de sessão de cada.

* Quando o utilizador clica o **Translate** botão, obtemos seleção do idioma do utilizador e o texto que introduziu. Em seguida, podemos chamar a `Translate` API para efetuar a conversão. Pode também chamamos a outras funções para determinar o idioma do texto e para corrigir a respetiva ortografia antes da tradução.

Vamos ver como podemos começar a nossa classe:

```cs
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
        // at least show an error dialog when we get an unexpected error
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

Duas variáveis de membros declaradas aqui contém informações sobre os nossos idiomas:

|||
|-|-|
|`languageCodes`<br>matriz da cadeia|Coloca em cache os códigos de idioma. O serviço de tradutor utiliza códigos de curtos, tais como `en` para inglês, para identificar idiomas.|
|`languageCodesAndTitles`<br>SortedDictionary|Efetuar cópias os nomes "amigáveis" na interface de utilizador de mapas para os códigos de curtos utilizados na API. Mantidos ordenados por ordem alfabética sem regard para maiúsculas e minúsculas.|

O primeiro código executado pela nossa aplicação é o `MainWindow` construtor. Em primeiro lugar, iremos configurar o método `HandleExceptions` como o processador de erros global. Desta forma, pelo menos, obtemos um alerta de erro se qualquer excepção não processada.

Em seguida, iremos verificar para se certificar de que as chaves de subscrição de API são todos os exatamente 32 carateres. Se não estão, a razão mais provável é que *alguém* não colado nas respetivas chaves de API (tsk). Neste caso, iremos apresentar uma mensagem de erro e bail. (Passagem este teste não significam que as chaves são válidas, obviamente.)

Se tivermos chaves que têm, pelo menos, o comprimento da direita, a `InitializeComponent()` chamada obtém a interface de utilizador sucessiva ao localizar, carregar e instanciar a descrição de XAML da janela principal da aplicação.

Por último, iremos definir os menus de lista pendente de idioma. Esta tarefa requer três chamadas de método separado. Iremos abordar estes métodos em detalhe nas secções seguintes.

## <a name="get-supported-languages"></a>Obter os idiomas suportados

O serviço Microsoft Translator suporta um total de 61 idiomas desta redação e mais podem ser adicionados ocasionalmente. Por isso,-tem melhor não codificar e idiomas suportados no seu programa. Colocar em vez disso, o serviço de conversor que idiomas suporta. Qualquer idioma suportado pode ser convertido em qualquer outro idioma suportado.

Chamar o `Languages` API para obter a lista de idiomas suportados.

O `Languages` API assume um parâmetro opcional para a consulta GET, *âmbito*. *âmbito* pode ter um dos três valores: `translation`, `transliteration`, e `dictionary`. Iremos utilizar o valor `translation`.

O `Languages` API também demora um cabeçalho de HTTP opcional, `Accept-Language`. O valor do cabeçalho determina o idioma em que os nomes dos idiomas suportados são devolvidos. O valor deve ser uma tag de idioma do BCP 47 corretamente formada. Iremos utilizar o valor `en` para obter os nomes de idiomas em inglês.

O `Languages` API devolve uma resposta JSON que se assemelha ao seguinte.

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

Queremos extrair os códigos de idioma (por exemplo, `af`) e os nomes de idioma (por exemplo, `Afrikaans`). Utilizamos o método Newtonsoft [JsonConvert.DeserializeObject](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm) para efetuar este procedimento.

Com este conhecimento em segundo plano, criamos o método seguinte para obter os códigos de idioma e os respetivos nomes.

```cs
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

`GetLanguagesForTranslate()` cria pela primeira vez o pedido HTTP. O `scope=translation` especifica de parâmetro de cadeia que queremos os idiomas suportados para a conversão de texto de consulta. Iremos adicionar a chave de subscrição de API de conversão de texto para o nosso cabeçalhos do pedido. Iremos também adicionar o `Accept-Language` cabeçalho com o valor `en` para indicar que queremos os idiomas suportados devolvidos em inglês.

Depois de concluir o pedido, vamos analisar a resposta JSON e convertê-lo para um dicionário. Iremos adicionar os códigos de idioma para o `languageCodes` variável de membro. Vamos percorrer, em seguida, os pares chave/valor que contêm os códigos de idioma e os nomes de idioma amigável e adicione-os para o `languageCodesAndTitles` variável de membro. (Os menus de lista pendente no nosso formulário apresentam os nomes amigáveis, mas precisamos os códigos de pedidos de tradução).

## <a name="populate-the-language-menus"></a>Preencher os menus de idioma

A maioria da nossa interface de utilizador está definido em XAML, pelo que não temos de fazer muito configurá-la para além de chamada `InitializeComponent()`. A apenas outra coisa a fazer é adicionar os nomes amigáveis de idioma para e de listas pendentes, que é efetuada na `PopulateLanguageMenus()`.

```cs
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

Preencher os menus é uma simples independentemente de iterating através de `languageCodesAndTitles` dicionário e adicionar cada chave, o que é o nome "amigável", para ambos os menus. Depois de preencher os menus, iremos definir a predefinição e para os idiomas para detetar (para detetar automaticamente o idioma) e em inglês.

> [!TIP]
> Se não definimos uma seleção predefinida para o nosso menus, o utilizador pode clicar em **Translate** sem escolher um para ou a partir de idioma. As predefinições eliminar a necessidade de lidar com este problema.

Agora `MainWindow` foi inicializado, a interface de utilizador a criar. Não obtemos controlo novamente até a utilizador clica no **Translate** botão.

## <a name="perform-translation"></a>Efetuar a conversão

Quando o utilizador clica **Translate**, WPF invoca o `TranslateButton_Click()` processador de eventos apresentado aqui.

```cs
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

Aqui, vamos obter primeiro para e de idiomas, juntamente com o texto do utilizador tem introduzido, a partir do formulário.

Se o idioma de origem está definido como detetar, chamamos `DetectLanguage()` para determinar o idioma do texto. Poderá ser o texto num idioma que as APIs de conversor não suporte (muitos mais idiomas podem ser detetados que possa ser convertido) ou a API de análise de texto poderá não conseguir detetá-lo. Nesse caso, iremos apresentar uma mensagem a informar o utilizador e voltar atrás sem traduzir.

Se o idioma de origem é inglês (se especificado ou detetada), iremos-verificação ortográfica texto com `CorrectSpelling()` e aplicar correções. O texto corrigido é peluche novamente para o campo de entrada para que o utilizador conhece que as correcções foi efetuada. (O utilizador pode preceder o texto a ser traduzido com um hífen para suprimir a correção de ortografia).

Se o utilizador não tiver introduzido qualquer texto ou se para e de idiomas são os mesmos, nenhuma conversão é necessário. Neste caso, iremos evitar que efetua o pedido.

O código para efetuar o pedido de tradução deve ter um aspeto familiar. Vamos criar o URI, criar um pedido, enviá-lo e analisar a resposta. Para apresentar o texto, armazenamos-na `TranslatedTextLabel` controlo.

Podemos transmitir texto para o `Translate` API numa matriz JSON serializada no corpo de um pedido POST. A matriz JSON pode conter várias peças de texto traduzir, mas aqui apenas incluímos um.

O cabeçalho de HTTP chamado `X-ClientTraceId` é opcional. O valor deve ser um GUID. O ID de rastreio fornecido pelo cliente é útil para pedidos de rastreio quando coisas não funcionam conforme esperado. No entanto, para ser útil, o valor X ClientTraceID tem registado pelo cliente. Um ID de rastreio de cliente e a data de pedidos podem ajudar a Microsoft a diagnosticar problemas que possam ocorrer.

> [!NOTE]
> Este tutorial centra-se num serviço Microsoft Translator, pelo que iremos não abrange o `DetectLanguage()` e `CorrectSpelling()` métodos em detalhe. Os serviços de análise de texto e a verificação de ortográfica do Bing fornecem respostas no JSON em vez de XML e análise de texto requer que o pedido ser formatado, bem como JSON. Estas características de conta para a maioria das diferenças de código de métodos que viu já.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API do Microsoft tradutor texto](http://docs.microsofttranslator.com/text-translate.html)
