---
title: Tutorial para aprender a adicionar expressões a uma aplicação LUIS com C# | Microsoft Docs
description: Neste tutorial, vai aprender a chamar uma aplicação LUIS com C#.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: v-geberr
ms.openlocfilehash: d9b3ca46cc635d961edadf3e3555f9656b6b5a1d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264248"
---
# <a name="tutorial-add-utterances-to-a-luis-app-using-c"></a>Tutorial: Adicionar expressões a uma aplicação LUIS com C# 
Neste tutorial, vai escrever um programa para adicionar uma expressão a uma intenção com as APIs de Criação em C#.

<!-- green checkmark -->
> [!div class="checklist"]
> * Criar o projeto de consola do Visual Studio 
> * Adicionar o método para chamar a API LUIS para adicionar a expressão e preparar a aplicação
> * Adicionar o ficheiro JSON com expressões de exemplo para a intenção BookFlight
> * Executar a consola e ver o estado de preparação para as expressões

Para obter mais informações, consulte a documentação técnica para as APIs [adicionar expressão de exemplo à intenção](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [preparar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) e [estado de preparação](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="prerequisites"></a>Pré-requisitos

* A [**edição da Comunidade do Visual Studio**](https://www.visualstudio.com/downloads/) mais recente. 
* A **[chave de criação](luis-concept-keys.md#authoring-key)** do LUIS. Pode encontrar esta chave em Definições da Conta no site do [LUIS](luis-reference-regions.md).
* O [**ID da aplicação**](./luis-get-started-create-app.md) LUIS existente. O ID da aplicação é apresentado no dashboard de aplicações. A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar este código. O código neste artigo não cria as intenções e as entidades. Adiciona as expressões para intenções e entidades existentes. 
* O **ID da versão** na aplicação que recebe as expressões. O ID predefinido é "0.1"
* Crie um novo ficheiro denominado `add-utterances.cs` no VSCode.

> [!NOTE] 
> A solução C# completa, incluindo um ficheiro de exemplo `utterances.json`, está disponível no repositório do Github [**LUIS-Samples**](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/csharp/).

## <a name="create-the-project-in-visual-studio"></a>Criar o projeto no Visual Studio

No Visual Studio, crie uma nova aplicação **Consola de Ambiente de Trabalho Clássico do Windows** com o .Net Framework. 

![Tipo de projeto do Visual Studio](./media/luis-quickstart-cs-add-utterance/vs-project-type.png)

## <a name="add-the-systemweb-dependency"></a>Adicionar a dependência System.Web

No Visual Studio, o projeto precisa de **System.Web**. No Explorador de Soluções, clique com o botão direito do rato em **Referências** e selecione **Adicionar Referência**.

![Adicionar referência System.web](./media/luis-quickstart-cs-add-utterance/system.web.png)

## <a name="write-the-c-code"></a>Escrever o código C#
O ficheiro **Program.cs** deve ser:

```CSharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp3
{
    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}

```

Adicione as dependências System.IO e System.Net.Http.

   [!code-csharp[Add the dependencies](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=1-6 "Add the dependencies")]


Adicione os IDs e as cadeias do LUIS à classe **Programa**.

   [!code-csharp[Add the LUIS IDs and strings](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=12-30&dedent=8 "Add the LUIS IDs and strings")]

Adicione o método JsonPrettyPrint.

   [!code-csharp[Add the JsonPrettyPrint method](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=32-92 "Add the JsonPrettyPrint method")]

Adicione o pedido GET utilizado para criar expressões ou iniciar a preparação. 

   [!code-csharp[SendGet](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=93-103 "SendGet")]


Adicione o pedido POST utilizado para criar expressões ou iniciar a preparação. 

   [!code-csharp[SendPost](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=104-115 "SendPost")]

Adicione a função `AddUtterances`.

   [!code-csharp[AddUtterances method](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=116-125 "AddUtterances method")]


Adicione a função `Train`. 

   [!code-csharp[Train](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=126-136 "Train")]

Adicione a função `Status`.

   [!code-csharp[Status](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=137-143 "Status")]

Para gerir argumentos, adicione o código principal.

   [!code-csharp[Main code](~/samples-luis/documentation-samples/authoring-api-samples/csharp/ConsoleApp1/Program.cs?range=144-172 "Main code")]

## <a name="specify-utterances-to-add"></a>Especificar as expressões a adicionar
Crie e edite o ficheiro `utterances.json` para especificar a **matriz de expressões** que quer adicionar à aplicação LUIS. A intenção e as entidades **têm** de já estar na aplicação LUIS.

> [!NOTE]
> A aplicação LUIS com as intenções e entidades utilizadas no ficheiro `utterances.json` tem de existir antes de executar o código em `add-utterances.cs`. O código neste artigo não cria as intenções e as entidades. Adiciona apenas as expressões para intenções e entidades existentes.

O campo `text` contém o texto da expressão. O campo `intentName` tem de corresponder ao nome de uma intenção na aplicação LUIS. O campo `entityLabels` é obrigatório. Se não quiser etiquetar nenhuma entidade, forneça uma lista vazia, conforme mostrado no exemplo seguinte:

Se a lista entityLabels não estiver vazia, `startCharIndex` e `endCharIndex` têm de marcar a entidade referida no campo `entityName`. Ambos os índices são contagens baseadas em zero, o que significa que o 6 no exemplo anterior se refere ao "S" de Seattle e não ao espaço antes do S maiúsculo.

```json
[
    {
        "text": "go to Seattle",
        "intentName": "BookFlight",
        "entityLabels": [
            {
                "entityName": "Location::LocationTo",
                "startCharIndex": 6,
                "endCharIndex": 12
            }
        ]
    },
    {
        "text": "book a flight",
        "intentName": "BookFlight",
        "entityLabels": []
    }
]
```

## <a name="mark-the-json-file-as-content"></a>Marcar o ficheiro JSON como conteúdo
No Explorador de Soluções, clique com o botão direito do rato em `utterances.json` e selecione **Propriedades**. Na janela de propriedades, marque a **Ação de Compilação** de `Content` e **Copiar para Diretório de Saída** de `Copy Always`.  

![Marcar o ficheiro JSON como conteúdo](./media/luis-quickstart-cs-add-utterance/content-properties.png)

## <a name="add-an-utterance-from-the-command-line"></a>Adicionar uma expressão a partir da linha de comandos

Crie e execute a aplicação a partir de uma linha de comandos com C# no diretório /bin/Debug. Certifique-se de que o ficheiro utterances.json também está neste diretório.

Chamar add-utterances.cs com apenas utterance.json como argumento adiciona mas não prepara o LUIS para as novas expressões.
````
ConsoleApp\bin\Debug> ConsoleApp1.exe utterances.json
````

Esta linha de comandos apresenta os resultados da chamada à API de expressões a adicionar. O campo `response` está neste formato para as expressões adicionadas. `hasError` é falso, o que indica que a expressão foi adicionada.  

```json
    "response": [
        {
            "value": {
                "UtteranceText": "go to seattle",
                "ExampleId": -5123383
            },
            "hasError": false
        },
        {
            "value": {
                "UtteranceText": "book a flight",
                "ExampleId": -169157
            },
            "hasError": false
        }
    ]
```

## <a name="add-an-utterance-and-train-from-the-command-line"></a>Adicionar uma expressão e preparar a partir da linha de comandos
Chame add-utterance com o argumento `-train` para enviar um pedido para preparar. 

````
ConsoleApp\bin\Debug> ConsoleApp1.exe -train utterances.json
````

> [!NOTE]
> As expressões duplicadas não são adicionadas novamente, mas não originam um erro. `response` contém o ID da expressão original.

O JSON seguinte mostra o resultado de um pedido para preparar com êxito:

```json
{
    "request": null,
    "response": {
        "statusId": 9,
        "status": "Queued"
    }
}
```

Depois de o pedido para preparar ser colocado em fila de espera, pode demorar alguns minutos a concluir a preparação.

## <a name="get-training-status-from-the-command-line"></a>Obter o estado de preparação a partir da linha de comandos
Chame a aplicação com o argumento `-status` para verificar o estado de preparação e apresentar detalhes do estado.

````
ConsoleApp\bin\Debug> ConsoleApp1.exe -status
````

```
Requested training status.
[
   {
      "modelId": "eb2f117c-e10a-463e-90ea-1a0176660acc",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c1bdfbfc-e110-402e-b0cc-2af4112289fb",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "863023ec-2c96-4d68-9c44-34c1cbde8bc9",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "82702162-73ba-4ae9-a6f6-517b5244c555",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "37121f4c-4853-467f-a9f3-6dfc8cad2763",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "de421482-753e-42f5-a765-ad0a60f50d69",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "80f58a45-86f2-4e18-be3d-b60a2c88312e",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c9eb9772-3b18-4d5f-a1e6-e0c31f91b390",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "2afec2ff-7c01-4423-bb0e-e5f6935afae8",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "95a81c87-0d7b-4251-8e07-f28d180886a1",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   }
]
```

## <a name="clean-up-resources"></a>Limpar recursos
Quando tiver terminado o tutorial, remova o Visual Studio e a aplicação de consola, se já não forem necessários. 

## <a name="next-steps"></a>Passos seguintes
> [!div class="nextstepaction"] 
> [Criar uma aplicação LUIS programaticamente](luis-tutorial-node-import-utterances-csv.md) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
