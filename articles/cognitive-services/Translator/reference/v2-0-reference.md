---
title: Referência de v 2.0 do Microsoft tradutor texto API | Microsoft Docs
titleSuffix: Cognitive Services
description: Documentação de referência para a API de texto do conversor de Microsoft v 2.0.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 05/15/2018
ms.author: v-jansko
ms.openlocfilehash: e32e28608d2fecf27b61acff74af7eb6849f0ba1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355148"
---
# <a name="translator-text-api-v20"></a>V 2.0 de texto API tradutor

> [!IMPORTANT]
> Esta versão da API de texto tradutor foi preterido. [Ver a documentação para v3 da API de texto tradutor](v3-0-reference.md).

Microsoft tradutor V2 texto API pode ser integrado totalmente integrada para as suas aplicações, Web sites, ferramentas ou outras soluções para fornecer experiências de utilizador multilingues. Tirar partido das normas da indústria, pode ser utilizado em qualquer plataforma de hardware e com qualquer sistema operativo para efetuar a conversão de linguagem e outras operações relacionadas com o idioma, tais como a deteção de idiomas de texto ou de reconhecimento de voz do texto para. Clique aqui para obter mais informações sobre a API do conversor de Microsoft.

## <a name="getting-started"></a>Introdução
Para aceder à API de texto do Microsoft tradutor terá [inscrever-se no Microsoft Azure](../translator-text-how-to-signup.md).

## <a name="authorization"></a>Autorização
Todas as chamadas à API do Microsoft tradutor texto requerem uma chave de subscrição para se autenticar. A API suporta dois modos de autenticação:

* Utilizando um token de acesso. Utilize a chave de subscrição referenciada no **passo** 9 para gerar um token de acesso ao efetuar um pedido POST para o serviço de autorização. Consulte a documentação do serviço de tokens para obter mais detalhes. Transmita o token de acesso para o serviço de tradutor com o cabeçalho de autorização ou o parâmetro de consulta access_token. O token de acesso é válido durante 10 minutos. Obter um novo token de acesso a cada 10 minutos e continuar a utilizar o acesso ao mesmo token para pedidos repetidos estes 10 minutos.

* Utilizar uma chave de subscrição diretamente. A chave de subscrição de passar como um valor no `Ocp-Apim-Subscription-Key` cabeçalho incluído com o seu pedido para a API de conversor. Neste modo, não é necessário chamar o serviço de token de autenticação para gerar um token de acesso.

Considere a chave de subscrição e o token de acesso como segredos devem ser ocultados da vista.

## <a name="profanity-handling"></a>Processamento de Profanity
Normalmente, o serviço de tradutor irão manter profanity que está presente na origem na tradução. O grau de profanity e o contexto que torna palavras profanas diferem entre as culturas e como resultado o grau de profanity na linguagem de destino pode ser amplified ou reduzido.

Se quiser evitar obter profanity na tradução, independentemente da presença de profanity no texto de origem, pode utilizar o profanity filtragem opção para os métodos de que o suportam. A opção permite-lhe escolher se pretende ver profanity eliminado ou marcado com etiquetas adequadas ou nenhuma ação executada. Os valores aceites de `ProfanityAction` são `NoAction` (predefinição), assinalado e `Deleted`.


|ProfanityAction    |Ação |Origem de exemplo (japonês)  |Tradução de exemplo (inglês)  |
|:--|:--|:--|:--|
|NoAction   |Predefinição. Mesmo que não definir a opção. Profanity passarão a partir da origem de destino.        |彼はジャッカスです。     |Ele é um jackass.   |
|Marcada     |Palavras profanas irão ser rodeadas por etiquetas XML <profanity> e </profanity>.     |彼はジャッカスです。 |Ele é um <profanity>jackass</profanity>.    |
|Eliminada    |Palavras profanas serão removidas da saída sem substituição.     |彼はジャッカスです。 |Ele é um.   |

    
## <a name="excluding-content-from-translation"></a>Excluir o conteúdo da tradução
Ao traduzir o conteúdo com etiquetas como HTML (`contentType=text/html`), por vezes, é útil excluir o conteúdo específico da tradução. Pode utilizar o atributo `class=notranslate` para especificar o conteúdo que deve permanecer no respetivo idioma original. No exemplo seguinte, o conteúdo dentro da primeira `div` não será possível converter elemento enquanto o conteúdo no segundo `div` será possível converter o elemento.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>OBTER / traduzir

### <a name="implementation-notes"></a>Notas de implementação
Converte uma cadeia de texto a partir de um idioma para outro.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Devolve o valor:** uma cadeia que representa o texto traduzido.

Se tiver utilizado anteriormente `AddTranslation` ou `AddTranslationArray` para introduzir uma tradução com uma classificação de 5 ou superior para a mesma frase de origem, `Translate` devolve apenas a opção superior que está disponível para o sistema. O "frase origem mesmo" significa a exato mesmo (correspondência de 100%), exceto maiúsculas/minúsculas, espaço em branco, valores de etiqueta e pontuação no final de uma frase. Se nenhuma classificação é armazenada com uma classificação de 5 ou posterior, o resultado devolvido será a tradução automática pelo Microsoft Translator.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)

cadeia

Tipo de conteúdo de resposta: application/xml 

### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição    |Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|AppID  |(vazio)    |Necessário. Se for utilizado o cabeçalho de autorização ou Ocp-Apim-Subscription-Key, deixe o campo de appid vazio ou incluir uma cadeia contendo "Portador" + "" + "access_token".|consulta|cadeia|
|texto|(vazio)   |Necessário. Uma cadeia que representa o texto a converter. O tamanho do texto não pode exceder 10000 carateres.|consulta|cadeia|
|de|(vazio)   |Opcional. Uma cadeia representando o código de idioma do texto tradução. Por exemplo, pt para inglês.|consulta|cadeia|
|para|(vazio) |Necessário. Uma cadeia representando o código de idioma para traduzir texto para.|consulta|cadeia|
|contentType|(vazio)    |Opcional. O formato de texto a ser traduzido. Os formatos suportados são texto/plain (predefinição) e o texto/html. Qualquer HTML tem de ser um elemento bem formado e completo.|consulta|cadeia|
|categoria|(vazio)   |Opcional. Uma cadeia que contém a categoria (domínio) da tradução. Por predefinição "geral".|consulta|cadeia|
|Autorização|(vazio)  |Necessário se o campo de appid ou o cabeçalho de Ocp-Apim-Subscription-Key não for especificado. O token de autorização: "Portador" + "" + "access_token".|cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)  |Necessário se o campo de ID da aplicação ou um cabeçalho de autorização não está especificado.|cabeçalho|cadeia|


### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Indique-no data aproximada do tempo do pedido e com o ID do pedido incluída no cabeçalho de resposta X-MS-Trans-Info.|
|503    |Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="post-translatearray"></a>PUBLIQUE /TranslateArray

### <a name="implementation-notes"></a>Notas de implementação
Utilize o `TranslateArray` método obter traduções para vários textos de origem.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

O formato de corpo do pedido deve ser o seguinte:

```
<TranslateArrayRequest>
  <AppId />
  <From>language-code</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" />
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</TranslateArrayRequest>
```

Elementos dentro de `TranslateArrayRequest` são:


* `appid`: Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.
* `from`: Opcional. Uma cadeia representando o código de idioma para traduzir texto do. Se deixado em branco a resposta irá incluir o resultado da deteção automática de idioma.
* `options`: Opcional. Um `Options` objeto que contém os valores apresentados abaixo. Estes são todos os opcionais e predefinido para as definições mais comuns. Elementos especificados tem de estar listados por ordem alfabética.
    - `Category`: Uma cadeia que contém a categoria (domínio) da tradução. Por predefinição `general`.
    - `ContentType`: O formato de texto a ser traduzido. Os formatos suportados são `text/plain` (predefinição), `text/xml` e `text/html`. Qualquer HTML tem de ser um elemento bem formado e completo.
    - `ProfanityAction`: Especifica a forma como profanities são processadas como explicado anteriormente. Aceite os valores de `ProfanityAction` são `NoAction` (predefinição), `Marked` e `Deleted`.
    - `State`: Estado do utilizador para ajudar a correlacionar pedido e resposta. O conteúdo mesmo será devolvido na resposta.
    - `Uri`: Filtre os resultados por este URI. Predefinição: `all`.
    - `User`: Filtre os resultados por este utilizador. Predefinição: `all`.
* `texts`: Necessário. Uma matriz que contém os textos para conversão. Todas as cadeias tem de ser do mesmo idioma. Total de todos os textos para ser convertido não pode exceder 10000 carateres. O número máximo de elementos de matriz é 2000.
* `to`: Necessário. Uma cadeia representando o código de idioma para traduzir texto para.

Podem ser omitidos elementos opcionais. Os elementos que são elementos subordinados diretos de TranslateArrayRequest tem de estar listados por ordem alfabética.

Método de TranslateArray aceita `application/xml` ou `text/xml` para `Content-Type`.

**Devolve o valor:** A `TranslateArrayResponse` matriz. Cada `TranslateArrayResponse` tem os seguintes elementos:

* `Error`: Indica se um Ocorreu um erro. Caso contrário, definido como nulo.
* `OriginalSentenceLengths`: Uma matriz de números inteiros que indica que o comprimento de cada frases no texto de origem original. O comprimento da matriz indica o número de frases.
* `TranslatedText`: O texto traduzido.
* `TranslatedSentenceLengths`: Uma matriz de números inteiros que indica que o comprimento de cada frases no texto traduzido. O comprimento da matriz indica o número de frases.
* `State`: Estado do utilizador para ajudar a correlacionar pedido e resposta. Devolve os mesmos conteúdos, como o pedido.

O formato do corpo da resposta é a seguinte.

```
<ArrayOfTranslateArrayResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <TranslateArrayResponse>
    <From>language-code</From>
    <OriginalTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </OriginalTextSentenceLengths>
    <State/>
    <TranslatedText>string-value</TranslatedText>
    <TranslatedTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </TranslatedTextSentenceLengths>
  </TranslateArrayResponse>
</ArrayOfTranslateArrayResponse>
```

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)
Uma resposta com êxito inclui uma matriz de `TranslateArrayResponse` no formato descrito acima.

cadeia

Tipo de conteúdo de resposta: application/xml

### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|Autorização|(vazia)) |Necessário se o campo de appid ou o cabeçalho de Ocp-Apim-Subscription-Key não for especificado. O token de autorização: "Portador" + "" + "access_token".|cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)|Necessário se o campo de ID da aplicação ou um cabeçalho de autorização não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP   |Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas. Erros comuns incluem: <ul><li>Elemento de matriz não pode estar vazio</li><li>Categoria inválido</li><li>De idioma é inválido</li><li>Para o idioma é inválido</li><li>O pedido contém demasiados elementos</li><li>O idioma de From não é suportado</li><li>O idioma para não é suportado</li><li>Traduzir pedido tem excesso de dados</li><li>HTML não está num formato correto</li><li>Foram transmitidas demasiados cadeias no traduzir pedido</li></ul>|
|401    |Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Indique-no data aproximada do tempo do pedido e com o ID do pedido incluída no cabeçalho de resposta X-MS-Trans-Info.|
|503    |Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Notas de implementação
Obtém nomes amigáveis para os idiomas transmitido como o parâmetro `languageCodes`e localizado utilizando a linguagem de região transmitida.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

O corpo do pedido inclui uma matriz de cadeia que representa os códigos de idioma 639-1 ISO para obter os nomes amigáveis para. Por exemplo:

```
<ArrayOfstring xmlns:i="http://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Devolve o valor:** uma matriz de cadeia que contém nomes de idiomas suportados pelo serviço tradutor, localizado para o idioma pedido.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)
Uma matriz de cadeia que contém nomes de idiomas suportados pelo serviço tradutor, localizado para o idioma pedido.

cadeia

Tipo de conteúdo de resposta: application/xml
 
### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|AppID|(vazio)|Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.|consulta|cadeia|
|Região|(vazio) |Necessário. Uma cadeia que representa uma combinação de um código de idioma em minúsculas de duas letras ISO 639 associados a um idioma e um código de subculture maiúsculas de duas letras ISO 3166 para localizar os nomes de idioma ou um código de idioma minúsculas ISO 639 por si só.|consulta|cadeia|
|Autorização|(vazio)  |Necessário se o campo de appid ou `Ocp-Apim-Subscription-Key` cabeçalho não está especificado. O token de autorização: `"Bearer" + " " + "access_token"`.|cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)  |Necessário se o campo de appid ou `Authorization` cabeçalho não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Indique-no data aproximada do tempo do pedido e com o ID do pedido incluída no cabeçalho de resposta X-MS-Trans-Info.|
|503    |Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="get-getlanguagesfortranslate"></a>OBTER /GetLanguagesForTranslate

### <a name="implementation-notes"></a>Notas de implementação
Obter uma lista dos códigos de idioma que representa os idiomas suportados pelo serviço de tradução.  `Translate` e `TranslateArray` pode traduzir entre duas destas linguagens.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Devolve o valor:** uma matriz de cadeia que contém os códigos de idioma suportados pelos serviços de conversor.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)
Uma matriz de cadeia que contém os códigos de idioma suportados pelos serviços de conversor.

cadeia

Tipo de conteúdo de resposta: application/xml
 
### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|AppID|(vazio)|Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.|consulta|cadeia|
|Autorização|(vazio)  |Necessário se o `appid` campo ou `Ocp-Apim-Subscription-Key` cabeçalho não está especificado. O token de autorização: `"Bearer" + " " + "access_token"`.|cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)|Necessário se o `appid` campo ou `Authorization` cabeçalho não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Indique-no data aproximada do tempo do pedido e com o ID do pedido incluída no cabeçalho de resposta X-MS-Trans-Info.|
|503|Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="get-getlanguagesforspeak"></a>OBTER /GetLanguagesForSpeak

### <a name="implementation-notes"></a>Notas de implementação
Obtém os idiomas disponíveis para síntese de voz.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Devolve o valor:** uma matriz de cadeia que contém os códigos de idioma suportados para synthesis de reconhecimento de voz pelo serviço tradutor.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)
Uma matriz de cadeia que contém os códigos de idioma suportados para synthesis de reconhecimento de voz pelo serviço tradutor.

cadeia

Tipo de conteúdo de resposta: application/xml

### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|AppID|(vazio)|Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.|consulta|cadeia|
|Autorização|(vazio)|Necessário se o `appid` campo ou `Ocp-Apim-Subscription-Key` cabeçalho não está especificado. O token de autorização: `"Bearer" + " " + "access_token"`.|cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)|Necessário se o `appid` campo ou `Authorization` cabeçalho não está especificado.|cabeçalho|cadeia|
 
### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400|Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401|Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Forneça-nos com a aproximado data e hora do pedido e com o ID do pedido incluída no cabeçalho de resposta `X-MS-Trans-Info`.|
|503    |Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="get-speak"></a>OBTER / enunciar

### <a name="implementation-notes"></a>Notas de implementação
Devolve uma sequência wave ou mp3 do transmitido no texto a ser ditas o idioma pretendido.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Devolve o valor:** uma transmissão em fluxo wave ou mp3 do transmitido no texto a ser ditas o idioma pretendido.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)

Binário

Tipo de conteúdo de resposta: application/xml 

### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|AppID|(vazio)|Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.|consulta|cadeia|
|texto|(vazio)   |Necessário. Uma cadeia que contém uma ou frases frases do idioma especificado a ser ditas para o fluxo de wave. O tamanho do texto fale não pode exceder os 2000 caracteres.|consulta|cadeia|
|Idioma|(vazio)   |Necessário. Uma cadeia representando o código de idioma suportado fale o texto. O código tem de estar presente na lista de códigos devolvida do método `GetLanguagesForSpeak`.|consulta|cadeia|
|Formato|(vazio)|Opcional. Uma cadeia que especifica o ID do tipo de conteúdo. Atualmente, `audio/wav` e `audio/mp3` estão disponíveis. O valor predefinido é `audio/wav`.|consulta|cadeia|
|opções|(vazio)    |<ul><li>Opcional. Uma cadeia que especifica as propriedades de reconhecimento de voz sintetizado:<li>`MaxQuality` e `MinSize` estão disponíveis para especificar a qualidade dos sinais de áudio. Com `MaxQuality`, pode obter voices com a mais alta qualidade e com `MinSize`, pode obter voices com o tamanho mais pequeno. Predefinição é `MinSize`.</li><li>`female` e `male` estão disponíveis para especificar o sexo pretendido da voz. A predefinição é `female`. Utilizar a barra vertical '|` to include multiple options. For example  `MaxQuality|Male'.</li></li></ul> |consulta|cadeia|
|Autorização|(vazio)|Necessário se o `appid` campo ou `Ocp-Apim-Subscription-Key` cabeçalho não está especificado. O token de autorização: `"Bearer" + " " + "access_token"`.|cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)  |Necessário se o `appid` campo ou `Authorization` cabeçalho não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Forneça-nos com a aproximado data e hora do pedido e com o ID do pedido incluída no cabeçalho de resposta `X-MS-Trans-Info`.|
|503    |Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="get-detect"></a>OBTER / detetar

### <a name="implementation-notes"></a>Notas de implementação
Utilize o `Detect` método para identificar o idioma de um fragmento de texto selecionado.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Devolve o valor:** uma cadeia contendo um código de idioma de dois carateres para o texto especificado. .

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)

cadeia

Tipo de conteúdo de resposta: application/xml

### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|AppID|(vazio)  |Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.|consulta|cadeia|
|texto|(vazio)|Necessário. Uma cadeia que contém algum texto cujo idioma está a ser identificados. O tamanho do texto não pode exceder 10000 carateres.|consulta| cadeia|
|Autorização|(vazio)|Necessário se o `appid` campo ou `Ocp-Apim-Subscription-Key` cabeçalho não está especificado. O token de autorização: `"Bearer" + " " + "access_token"`.|cabeçalho|cadeia|
|OCP Apim-subscrição-chave  |(vazio)    |Necessário se o `appid` campo ou `Authorization` cabeçalho não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400|Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Forneça-nos com a aproximado data e hora do pedido e com o ID do pedido incluída no cabeçalho de resposta `X-MS-Trans-Info`.|
|503    |Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|


## <a name="post-detectarray"></a>PUBLIQUE /DetectArray

### <a name="implementation-notes"></a>Notas de implementação
Utilize o `DetectArray` método para identificar o idioma de uma matriz de cadeia em simultâneo. Efetua a deteção independente de cada elemento de matriz individuais e devolve um resultado para cada linha da matriz.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

O formato de corpo do pedido deve ser o seguinte.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

O tamanho do texto não pode exceder 10000 carateres.

**Devolve o valor:** uma matriz de cadeia contendo um idioma de caráter de dois códigos para cada linha da matriz de entrada.

O formato do corpo da resposta é a seguinte.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)
DetectArray foi concluída com êxito. Devolve uma matriz de cadeia contendo um códigos de idioma de dois carateres para cada linha da matriz de entrada.

cadeia

Tipo de conteúdo de resposta: application/xml
 
### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|AppID|(vazio)|Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.|consulta|cadeia|
|Autorização|(vazio)|Necessário se o `appid` campo ou `Ocp-Apim-Subscription-Key` cabeçalho não está especificado. O token de autorização: `"Bearer" + " " + "access_token"`.|cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)|Necessário se o `appid` campo ou o cabeçalho de autorização não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Indique-no data aproximada do tempo do pedido e com o ID do pedido incluída no cabeçalho de resposta X-MS-Trans-Info.|
|503    |Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="get-addtranslation"></a>OBTER /AddTranslation

### <a name="implementation-notes"></a>Notas de implementação

> [!IMPORTANT]
> **Nota de PRETERIÇÃO:** após 31 de Janeiro de 2018, este método não aceitará novo submissões de frases e receberá uma mensagem de erro. Consulte este anúncio sobre as alterações para as funções de tradução de colaboração.

Adiciona uma tradução para a memória da tradução.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)

cadeia

Tipo de conteúdo de resposta: aplicação: xml
 
### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados   |
|:--|:--|:--|:--|:--|
|AppID|(vazio)|Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.|consulta|cadeia|
|OriginalText|(vazio)|Necessário. Uma cadeia que contém o texto a converter a partir de. A cadeia tem um comprimento máximo de 1000 carateres.|consulta|cadeia|
|TranslatedText|(vazio) |Necessário. Uma cadeia que contém traduzido texto na linguagem de destino. A cadeia tem um comprimento máximo de 2000 caracteres.|consulta|cadeia|
|de|(vazio)   |Necessário. Uma cadeia representando o código de idioma do texto tradução. EN = inglês, Alemanha = Alemão etc...|consulta|cadeia|
|para|(vazio)|Necessário. Uma cadeia representando o código de idioma para traduzir texto para.|consulta|cadeia|
|classificação|(vazio) |Opcional. Um número inteiro que representa a classificação de qualidade para esta cadeia. O valor entre -10 e 10. Por predefinição, 1.|consulta|inteiro|
|contentType|(vazio)    |Opcional. O formato de texto a ser traduzido. Os formatos suportados são "text/plain" e "text/html". Qualquer HTML tem de ser um elemento bem formado e completo.   |consulta|cadeia|
|categoria|(vazio)|Opcional. Uma cadeia que contém a categoria (domínio) da tradução. Por predefinição "geral".|consulta|cadeia|
|Utilizador|(vazio)|Necessário. Uma cadeia utilizada para controlar o originador da submissão.|consulta|cadeia|
|uri|(vazio)|Opcional. Uma cadeia que contém a localização de conteúdos desta conversão.|consulta|cadeia|
|Autorização|(vazio)|Necessário se o campo de appid ou `Ocp-Apim-Subscription-Key` cabeçalho não está especificado. O token de autorização: `"Bearer" + " " + "access_token"`.    |cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)|Necessário se o `appid` campo ou `Authorization` cabeçalho não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|410|AddTranslation já não é suportada.|
|500    |Erro de servidor. Se o erro persistir, indique. Indique-no data aproximada do tempo do pedido e com o ID do pedido incluída no cabeçalho de resposta X-MS-Trans-Info.|
|503    |Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="post-addtranslationarray"></a>PUBLIQUE /AddTranslationArray

### <a name="implementation-notes"></a>Notas de implementação

> [!IMPORTANT]
> **Nota de PRETERIÇÃO:** após 31 de Janeiro de 2018, este método não aceitará novo submissões de frases e receberá uma mensagem de erro. Consulte este anúncio sobre as alterações para as funções de tradução de colaboração.

Adiciona uma matriz de traduções adicionar memória de conversão. Esta é uma versão de matriz de `AddTranslation`.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

Segue-se o formato de corpo do pedido.

```
<AddtranslationsRequest>
  <AppId></AppId>
  <From>A string containing the language code of the source language</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
  </Options>
  <To>A string containing the language code of the target language</To>
  <Translations>
    <Translation xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
      <OriginalText>string-value</OriginalText>
      <Rating>int-value</Rating>
      <Sequence>int-value</Sequence>
      <TranslatedText>string-value</TranslatedText>
    </Translation>
  </Translations>
</AddtranslationsRequest>
```

Elementos no elemento AddtranslationsRequest são:

* `AppId`: Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.
* `From`: Necessário. Uma cadeia que contém o código de idioma do idioma de origem. Tem de ser um dos idiomas devolvidos pelo `GetLanguagesForTranslate` método.
* `To`: Necessário. Uma cadeia que contém o código de idioma do idioma de destino. Tem de ser um dos idiomas devolvidos pelo `GetLanguagesForTranslate` método.
* `Translations`: Necessário. Uma matriz de traduções para adicionar a memória da tradução. Cada tradução tem de conter: originalText, translatedText e classificação. O tamanho de cada originalText e translatedText está limitado a 1000 carateres. O total de todos os originalText(s) e translatedText(s) não pode exceder 10000 carateres. O número máximo de elementos de matriz é 100.
* `Options`: Necessário. Um conjunto de opções, incluindo a categoria, o ContentType, o Uri e o utilizador. O utilizador é necessário. Categoria, ContentType e Uri são opcionais. Elementos especificados tem de estar listados por ordem alfabética.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)
Método de AddTranslationArray foi concluída com êxito. Após a 31 de Janeiro de 2018, não serão aceites submissões de frases. O serviço irá responder com o código de erro 410.

cadeia

Tipo de conteúdo de resposta: application/xml
 
### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|Autorização|(vazio)|Necessário se o campo de appid ou o cabeçalho de Ocp-Apim-Subscription-Key não for especificado. O token de autorização: "Portador" + "" + "access_token".|cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)|Necessário se o campo de ID da aplicação ou um cabeçalho de autorização não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|410    |AddTranslation já não é suportada.|
|500    |Erro de servidor. Se o erro persistir, indique. Forneça-nos com a aproximado data e hora do pedido e com o ID do pedido incluída no cabeçalho de resposta `X-MS-Trans-Info`.|
|503|Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="get-breaksentences"></a>OBTER /BreakSentences

### <a name="implementation-notes"></a>Notas de implementação
Interrompe a um fragmento de texto em frases e devolve uma matriz que contém os comprimentos em cada frases.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Devolve o valor:** uma matriz de números inteiros que representa os comprimentos das frases. O comprimento da matriz é o número de frases e os valores são o comprimento de cada frases.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)
Uma matriz de números inteiros que representa os comprimentos das frases. O comprimento da matriz é o número de frases e os valores são o comprimento de cada frases.

inteiro

Tipo de conteúdo de resposta: application/xml 

### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|AppID|(vazio)  |Necessário. Se for utilizado o cabeçalho de autorização ou Ocp-Apim-Subscription-Key, deixe o campo de appid vazio ou incluir uma cadeia contendo "Portador" + "" + "access_token".|consulta| cadeia|
|texto|(vazio)   |Necessário. Uma cadeia que representa o texto para dividir em frases. O tamanho do texto não pode exceder 10000 carateres.|consulta|cadeia|
|Idioma   |(vazio)    |Necessário. Uma cadeia representando o código de idioma de texto de entrada.|consulta|cadeia|
|Autorização|(vazio)|Necessário se o campo de appid ou o cabeçalho de Ocp-Apim-Subscription-Key não for especificado. O token de autorização: "Portador" + "" + "access_token".    |cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)|Necessário se o campo de ID da aplicação ou um cabeçalho de autorização não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400|Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401|Credenciais inválidas|
|500|Erro de servidor. Se o erro persistir, indique. Indique-no data aproximada do tempo do pedido e com o ID do pedido incluída no cabeçalho de resposta X-MS-Trans-Info.|
|503|Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Notas de implementação
Obtém uma matriz de traduções para um par de determinado idioma da loja e o motor de MT. GetTranslations difere Translate devolve todas as traduções disponíveis.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

O corpo do pedido inclui o objeto TranslationOptions opcional que tem o seguinte formato.

```
<TranslateOptions xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
  <Category>string-value</Category>
  <ContentType>text/plain</ContentType>
  <ReservedFlags></ReservedFlags>
  <State>int-value</State>
  <Uri>string-value</Uri>
  <User>string-value</User>
</TranslateOptions>
```

O `TranslateOptions` objeto contém os valores apresentados abaixo. Estes são todos os opcionais e predefinido para as definições mais comuns. Elementos especificados tem de estar listados por ordem alfabética.

* `Category`: Uma cadeia que contém a categoria (domínio) da tradução. Por predefinição "geral".
* `ContentType`: Suportada o único, não sendo o predefinido, opção "text/plain".
* `IncludeMultipleMTAlternatives`: sinalizador booleano para determinar se alternativas mais do que um devem ser devolvido do motor MT. Os valores válidos são VERDADEIRO ou FALSO (maiúsculas e minúsculas). Predefinição é falsa e inclui apenas 1 alternativo. Permite que a definição do sinalizador como true para gerar alternativas artificial num tradução, totalmente integrada com o framework de colaboração traduções (CTF). A funcionalidade permite para devolver alternativas para frases que não tenham nenhuma alternativas na CTF, adicionando alternativas artificial da lista n melhor do descodificador.
    - As classificações as classificações são aplicadas da seguinte forma: 1) a tradução automática melhor tem uma classificação de 5. 2) as alternativas de CTF refletem a autoridade do revisor, de -10 para + 10. 3) as alternativas gerado automaticamente (melhor-n) tradução têm uma classificação de 0 e têm um grau de correspondência de 100.
    - Número de alternativas, o número de alternativas devolvidos até maxTranslations, mas pode ser menor.
    - Pares de idioma esta funcionalidade não está disponível para traduções entre simplificado e chinês tradicional, ambas as direções. Está disponível para todos os outros pares de idioma Translator Microsoft suportadas.
* `State`: Estado do utilizador para ajudar a correlacionar pedido e resposta. O conteúdo mesmo será devolvido na resposta.
* `Uri`: Filtre os resultados por este URI. Não se for definido nenhum valor, a predefinição é tudo.
* `User`: Filtre os resultados por este utilizador. Não se for definido nenhum valor, a predefinição é tudo.

Pedir `Content-Type` deve ser `text/xml`.

**Devolve o valor:** o formato da resposta é o seguinte.

```
<GetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <From>Two character language code</From>
  <State/>
  <Translations>
    <TranslationMatch>
      <Count>int-value</Count>
      <MatchDegree>int-value</MatchDegree>
      <MatchedOriginalText/>
      <Rating>int value</Rating>
      <TranslatedText>string-value</TranslatedText>
    </TranslationMatch>
  </Translations>
</GetTranslationsResponse>
```

Isto inclui um `GetTranslationsResponse` elemento que contém os seguintes valores:

* `Translations`: Uma matriz de corresponde à encontrado, armazenados em objetos de TranslationMatch (ver abaixo). As traduções podem incluir ligeiras variantes do texto original (correspondência por semelhantes). As traduções irão ser ordenadas: 100% em primeiro lugar, corresponde correspondências difusa abaixo.
* `From`: Se o método não especificou um idioma de From, este será o resultado da deteção de idioma automática. Caso contrário, será o fornecido do idioma.
* `State`: Estado do utilizador para ajudar a correlacionar pedido e resposta. Contém o mesmo valor que é especificado no parâmetro TranslateOptions.

Objeto de TranslationMatch consiste no seguinte:

* `Error`: Se tiver ocorrido um erro para uma cadeia de entrada específica, o código de erro é armazenado. Caso contrário, o campo está vazio.
* `MatchDegree`: O sistema corresponde à entrada frases contra loja, incluindo inexatas.  MatchDegree indica como o texto de entrada se aproxima do texto original encontrado no arquivo. O valor devolvido intervalos de 0 a 100, onde é 0 não semelhança e 100 é uma correspondência exata sensível às maiúsculas e minúsculas.
MatchedOriginalText: Texto Original foi correspondentes para este resultado. Apenas devolvido se o texto original correspondente foi diferente do texto de entrada. Utilizado para devolver o texto de origem de uma correspondência por semelhantes. Não é devolvido para Microsoft Translator resultados.
* `Rating`: Indica a autoridade da pessoa que efetuar a decisão de qualidade. Resultados de tradução automática terá uma classificação de 5. Traduções anonimamente fornecidas geralmente terá uma classificação diferente de 1 a 4, enfrenta traduções autoritativamente fornecidas geralmente terá uma classificação de 6 a 10.
* `Count`: O número de vezes que este tradução com esta classificação foi seleccionada. O valor será 0 para a resposta traduzida automaticamente.
* `TranslatedText`: O texto traduzido.

### <a name="response-class-status-200"></a>Classe de resposta (estado 200)
A `GetTranslationsResponse` objeto no formato descrito acima.

cadeia

Tipo de conteúdo de resposta: application/xml
 
### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|AppID|(vazio)|Necessário. Se o `Authorization` ou `Ocp-Apim-Subscription-Key` cabeçalho é utilizado, deixe o campo de appid vazio ou incluir uma cadeia que contém `"Bearer" + " " + "access_token"`.|consulta|cadeia|
|texto|(vazio)|Necessário. Uma cadeia que representa o texto a converter. O tamanho do texto não pode exceder 10000 carateres.|consulta|cadeia|
|de|(vazio)|Necessário. Uma cadeia representando o código de idioma do texto tradução.|consulta|cadeia|
|para |(vazio)    |Necessário. Uma cadeia representando o código de idioma para traduzir texto para.|consulta|cadeia|
|maxTranslations|(vazio)|Necessário. Um número inteiro que representa o número máximo de traduções para devolver.|consulta|inteiro|
|Autorização| (vazio)|Necessário se o `appid` campo ou `Ocp-Apim-Subscription-Key` cabeçalho não está especificado. O token de autorização: `"Bearer" + " " + "access_token"`.|cadeia| cabeçalho|
|OCP Apim-subscrição-chave|(vazio)  |Necessário se o `appid` campo ou `Authorization` cabeçalho não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Forneça-nos com a aproximado data e hora do pedido e com o ID do pedido incluída no cabeçalho de resposta `X-MS-Trans-Info`.|
|503|Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="post-gettranslationsarray"></a>PUBLIQUE /GetTranslationsArray

### <a name="implementation-notes"></a>Notas de implementação
Utilize o `GetTranslationsArray` método obter vários tradução candidatos para vários textos de origem.

O pedido de URI é `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

Segue-se o formato de corpo do pedido.

```
<GetTranslationsArrayRequest>
  <AppId></AppId>
  <From>language-code</From>
  <MaxTranslations>int-value</MaxTranslations>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"/>
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</GetTranslationsArrayRequest>
```

`GetTranslationsArrayRequest` inclui os seguintes elementos:

* `AppId`: Necessário. Se for utilizado o cabeçalho de autorização, deixe o campo de appid vazio incluem ou uma cadeia que contém `"Bearer" + " " + "access_token"`.
* `From`: Necessário. Uma cadeia representando o código de idioma do texto tradução.
* `MaxTranslations`: Necessário. Um número inteiro que representa o número máximo de traduções para devolver.
* `Options`: Opcional. Um objeto de opções que contém os valores apresentados abaixo. Estes são todos os opcionais e predefinido para as definições mais comuns. Elementos especificados tem de estar listados por ordem alfabética.
    - Categoria ': uma cadeia contendo a categoria (domínio) da tradução. Por predefinição, geral.
    - `ContentType`: Suportada o único, não sendo o predefinido, opção texto/simples.
    - `IncludeMultipleMTAlternatives`: sinalizador booleano para determinar se alternativas mais do que um devem ser devolvido do motor MT. Os valores válidos são VERDADEIRO ou FALSO (maiúsculas e minúsculas). Predefinição é falsa e inclui apenas 1 alternativo. Permite que a definição do sinalizador como true para gerar alternativas artificial num tradução, totalmente integrada com o framework de colaboração traduções (CTF). A funcionalidade permite para devolver alternativas para frases que não tenham nenhuma alternativas na CTF, adicionando alternativas artificial da lista n melhor do descodificador.
        - As classificações as classificações são aplicadas da seguinte forma: 1) a tradução automática melhor tem uma classificação de 5. 2) as alternativas de CTF refletem a autoridade do revisor, de -10 para + 10. 3) as alternativas gerado automaticamente (melhor-n) tradução têm uma classificação de 0 e têm um grau de correspondência de 100.
        - Número de alternativas, o número de alternativas devolvidos até maxTranslations, mas pode ser menor.
        - Pares de idioma esta funcionalidade não está disponível para traduções entre simplificado e chinês tradicional, ambas as direções. Está disponível para todos os outros pares de idioma Translator Microsoft suportadas.
* `State`: Estado do utilizador para ajudar a correlacionar pedido e resposta. O conteúdo mesmo será devolvido na resposta.
* `Uri`: Filtre os resultados por este URI. Não se for definido nenhum valor, a predefinição é tudo.
* `User`: Filtre os resultados por este utilizador. Não se for definido nenhum valor, a predefinição é tudo.
* `Texts`: Necessário. Uma matriz que contém os textos para conversão. Todas as cadeias tem de ser do mesmo idioma. Total de todos os textos para ser convertido não pode exceder 10000 carateres. O número máximo de elementos de matriz é 10.
* `To`: Necessário. Uma cadeia representando o código de idioma para traduzir texto para.

Podem ser omitidos elementos opcionais. Direcionam os elementos que são subordinados `GetTranslationsArrayRequest` têm de estar listados por ordem alfabética.

Pedir `Content-Type` deve ser `text/xml`.

**Devolve o valor:** o formato da resposta é o seguinte.

```
<ArrayOfGetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
  <GetTranslationsResponse>
    <From>language-code</From>
    <State/>
    <Translations>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText>string-value</MatchedOriginalText>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText/>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
    </Translations>
  </GetTranslationsResponse>
</ArrayOfGetTranslationsResponse>
```

Cada `GetTranslationsResponse` elemento contém os seguintes valores:

* `Translations`: Uma matriz de correspondências encontrado, armazenado no `TranslationMatch` objetos (ver abaixo). As traduções podem incluir ligeiras variantes do texto original (correspondência por semelhantes). As traduções irão ser ordenadas: 100% em primeiro lugar, corresponde correspondências difusa abaixo.
* `From`: Se o método não especificou um `From` idioma, este será o resultado da deteção de idioma automática. Caso contrário, será o fornecido do idioma.
* `State`: Estado do utilizador para ajudar a correlacionar pedido e resposta. Contém o mesmo valor que o indicado no `TranslateOptions` parâmetro.

`TranslationMatch` objeto consiste no seguinte:
* `Error`: Se tiver ocorrido um erro para uma cadeia de entrada específica, o código de erro é armazenado. Caso contrário, o campo está vazio.
* `MatchDegree`: O sistema corresponde à entrada frases contra loja, incluindo inexatas.  `MatchDegree` indica como o texto de entrada se aproxima do texto original encontrado no arquivo. O valor devolvido intervalos de 0 a 100, onde é 0 não semelhança e 100 é uma correspondência exata sensível às maiúsculas e minúsculas.
* `MatchedOriginalText`: Texto original foi correspondentes para este resultado. Apenas devolvido se o texto original correspondente foi diferente do texto de entrada. Utilizado para devolver o texto de origem de uma correspondência por semelhantes. Não é devolvido para Microsoft Translator resultados.
* `Rating`: Indica a autoridade da pessoa que efetuar a decisão de qualidade. Resultados de tradução automática terá uma classificação de 5. Traduções anonimamente fornecidas geralmente terá uma classificação diferente de 1 a 4, enfrenta traduções autoritativamente fornecidas geralmente terá uma classificação de 6 a 10.
* `Count`: O número de vezes que este tradução com esta classificação foi seleccionada. O valor será 0 para a resposta traduzida automaticamente.
* `TranslatedText`: O texto traduzido.


### <a name="response-class-status-200"></a>Classe de resposta (estado 200)

cadeia

Tipo de conteúdo de resposta: application/xml
 
### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:--|:--|:--|:--|:--|
|Autorização  |(vazio)    |Necessário se o `appid` campo ou `Ocp-Apim-Subscription-Key` cabeçalho não está especificado. O token de autorização: `"Bearer" + " " + "access_token"`.|cabeçalho|cadeia|
|OCP Apim-subscrição-chave|(vazio)  |Necessário se o `appid` campo ou `Authorization` cabeçalho não está especificado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|
|:--|:--|
|400    |Pedido incorreto. Verifique os parâmetros de entrada e a resposta de erro detalhadas.|
|401    |Credenciais inválidas|
|500    |Erro de servidor. Se o erro persistir, indique. Forneça-nos com a aproximado data e hora do pedido e com o ID do pedido incluída no cabeçalho de resposta `X-MS-Trans-Info`.|
|503    |Serviço temporariamente indisponível. Tente novamente e informe-nos se o erro persistir.|

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Migrar para o texto de tradutor v3 API](../migrate-to-v3.md)










