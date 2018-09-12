---
title: A Tradução de Texto obtém o comprimento de frases com o Java | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Neste início rápido, pode encontrar o comprimento de frases no texto através da API de Texto do Microsoft Translator com o Java nos Serviços Cognitivos.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/21/2018
ms.author: nolachar
ms.openlocfilehash: 1e68f6b888aa670644ef1e05e5f4f3e26cf70b18
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "43770743"
---
# <a name="quickstart-get-sentence-lengths-with-java"></a>Início rápido: obter o comprimento de frases com o Java

Neste início rápido, pode encontrar o comprimento de frases no texto através da API de Texto do Microsoft Translator.

## <a name="prerequisites"></a>Pré-requisitos

Irá precisar do [JDK 7 ou 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) para compilar e executar este código. Pode utilizar um IDE Java se tiver um favorito, mas o editor de texto também funciona.

Para utilizar a API de Texto do Microsoft Translator, também precisa de uma chave de subscrição, veja [Como iniciar sessão para a API de Texto do Microsoft Translator](translator-text-how-to-signup.md).

## <a name="breaksentence-request"></a>Pedido de BreakSentence

O seguinte código separa o texto de origem em frases através do método [BreakSentence](./reference/v3-0-break-sentence.md).

1. Crie um novo projeto Java no seu editor de código favorito.
2. Adicione o código indicado abaixo.
3. Substitua o valor `subscriptionKey` por uma chave de acesso válida para a sua subscrição.
4. Execute o programa.

```java
import java.io.*;
import java.net.*;
import java.util.*;
import javax.net.ssl.HttpsURLConnection;

/*
 * Gson: https://github.com/google/gson
 * Maven info:
 *     groupId: com.google.code.gson
 *     artifactId: gson
 *     version: 2.8.1
 */
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

/* NOTE: To compile and run this code:
1. Save this file as BreakSentences.java.
2. Run:
    javac BreakSentences.java -cp .;gson-2.8.1.jar -encoding UTF-8
3. Run:
    java -cp .;gson-2.8.1.jar BreakSentences
*/

public class BreakSentences {

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
    static String subscriptionKey = "ENTER KEY HERE";

    static String host = "https://api.cognitive.microsofttranslator.com";
    static String path = "/breaksentence?api-version=3.0";

    static String text = "How are you? I am fine. What did you do today?";

    public static class RequestBody {
        String Text;

        public RequestBody(String text) {
            this.Text = text;
        }
    }

    public static String Post (URL url, String content) throws Exception {
        HttpsURLConnection connection = (HttpsURLConnection) url.openConnection();
        connection.setRequestMethod("POST");
        connection.setRequestProperty("Content-Type", "application/json");
        connection.setRequestProperty("Content-Length", content.length() + "");
        connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey);
        connection.setRequestProperty("X-ClientTraceId", java.util.UUID.randomUUID().toString());
        connection.setDoOutput(true);

        DataOutputStream wr = new DataOutputStream(connection.getOutputStream());
        byte[] encoded_content = content.getBytes("UTF-8");
        wr.write(encoded_content, 0, encoded_content.length);
        wr.flush();
        wr.close();

        StringBuilder response = new StringBuilder ();
        BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream(), "UTF-8"));
        String line;
        while ((line = in.readLine()) != null) {
            response.append(line);
        }
        in.close();

        return response.toString();
    }

    public static String BreakSentences () throws Exception {
        URL url = new URL (host + path);

        List<RequestBody> objList = new ArrayList<RequestBody>();
        objList.add(new RequestBody(text));
        String content = new Gson().toJson(objList);

        return Post(url, content);
    }

    public static String prettify(String json_text) {
        JsonParser parser = new JsonParser();
        JsonElement json = parser.parse(json_text);
        Gson gson = new GsonBuilder().setPrettyPrinting().create();
        return gson.toJson(json);
    }

    public static void main(String[] args) {
        try {
            String response = BreakSentences ();
            System.out.println (prettify (response));
        }
        catch (Exception e) {
            System.out.println (e);
        }
    }
}
```

## <a name="breaksentence-response"></a>Resposta do BreakSentence

Uma resposta com êxito é devolvida em JSON, tal como é apresentado no exemplo seguinte:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Passos seguintes

Explore o código de exemplo neste início rápido e noutros, incluindo a tradução e a transliteração, assim como outros exemplos de projetos de Tradução de Texto no GitHub.

> [!div class="nextstepaction"]
> [Explorar exemplos do Java no GitHub](https://aka.ms/TranslatorGitHub?type=&language=java)
