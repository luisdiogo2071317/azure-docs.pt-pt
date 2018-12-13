---
title: Consumo de serviço da web de gestão de modelos do Machine Learning do Azure | Documentos da Microsoft
description: Este documento descreve os passos e conceitos envolvidos no uso de serviços da web implementados com gestão de modelos do Azure Machine Learning.
services: machine-learning
author: aashishb
ms.author: aashishb
manager: hjerez
ms.reviewer: jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/06/2017
ROBOTS: NOINDEX
ms.openlocfilehash: bbd36e5389208b21704f1749beee016e30cc4ec5
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53253855"
---
# <a name="consuming-web-services"></a>Consumir serviços web

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Depois de implementar um modelo como um serviço da web em tempo real, pode enviar os dados e obter previsões de uma variedade de plataformas e aplicativos. O serviço da web em tempo real expõe uma API REST para obter previsões de indisponibilidade. Pode enviar dados para o serviço web no formato único ou de linha múltiplas para obter previsões de um ou mais a uma hora.

Com o [serviço Web do Azure Machine Learning](model-management-service-deploy.md), uma aplicação externa comunica forma síncrona com um modelo preditivo, fazendo a chamada de HTTP POST para o URL do serviço. Para fazer uma chamada do serviço web, a aplicação cliente tem de especificar a chave de API que foi criada quando implementar uma predição e colocar os dados de pedido no corpo do pedido POST.

> [!NOTE]
> Tenha em atenção que as chaves de API só estão disponíveis no modo de implementação de cluster. Serviços da local web não tem as chaves.

## <a name="service-deployment-options"></a>Opções de implementação de serviço
Serviços Web do Machine Learning do Azure podem ser implementados para os clusters baseados na nuvem para cenários de produção e teste e estações de trabalho locais com o motor do docker. A funcionalidade do modelo preditivo em ambos os casos permanece igual. Implantação com base em cluster fornece dimensionável e solução de alto desempenho com base nos serviços de contentor do Azure, enquanto a implementação local pode ser utilizada para depuração. 

A API e da CLI do Azure Machine Learning fornece comandos convenientes para criar e gerir ambientes para implementações de serviços através de computação a ```az ml env``` opção. 

## <a name="list-deployed-services-and-images"></a>Serviços de lista implementado e imagens
Pode listar os serviços implementados atualmente e a imagens do Docker com o comando da CLI ```az ml service list realtime -o table```. Tenha em atenção que este comando sempre funciona no contexto do atual ambiente de computação. Não apresentaria serviços que são implementados num ambiente que não está configurado para ser atual. Para definir o uso de ambiente ```az ml env set```. 

## <a name="get-service-information"></a>Obtenha informações de serviço
Depois de ter sido implementado com êxito o serviço web, utilize o seguinte comando para obter o URL do serviço e outros detalhes para chamar o ponto final de serviço. 

```
az ml service usage realtime -i <web service id>
```

Este comando imprime o URL do serviço, cabeçalhos de solicitação necessários, swagger URL e dados de exemplo para chamar o serviço se o esquema de API do serviço foi fornecido no momento da implementação.

Pode testar o serviço diretamente a partir da CLI sem compor uma solicitação HTTP, introduzindo o comando da CLI de exemplo com os dados de entrada:

```
az ml service run realtime -i <web service id> -d "Your input data"
```

## <a name="get-the-service-api-key"></a>Obter a chave de API de serviço
Para obter a chave de serviço da web, utilize o seguinte comando:

```
az ml service keys realtime -i <web service id>
```
Ao criar pedido HTTP, utilize a chave no cabeçalho de autorização: "Autorização": "Portador <key>"

## <a name="get-the-service-swagger-description"></a>Obter a descrição do Swagger de serviço
Se o esquema de API do serviço foi fornecido, o ponto final de serviço exporia um documento Swagger em ```http://<ip>/api/v1/service/<service name>/swagger.json```. O documento Swagger pode ser utilizado para gerar o cliente do serviço e explorar os dados de entrada esperados e outros detalhes sobre o serviço automaticamente.

## <a name="get-service-logs"></a>Obter registos de serviço
Para compreender o comportamento de serviço e diagnosticar problemas, existem várias formas de obter os registos do serviço:
- Comando da CLI ```az ml service logs realtime -i <service id>```. Este comando funciona em cluster e modos de locais.
- Se o registo do serviço foram ativado na implementação, os registos do serviço serão também enviados para AppInsight. O comando da CLI ```az ml service usage realtime -i <service id>``` mostra o URL do AppInsight. Tenha em atenção que os registos do AppInsight podem sofrer um atraso por 2 a 5 minutos.
- Registos do cluster podem ser visualizados através da consola do Kubernetes que está ligada ao definir o atual ambiente de cluster com ```az ml env set```
- Registos do local docker estão disponíveis através de registos do motor do docker, quando o serviço está em execução localmente.

## <a name="call-the-service-using-c"></a>Chamar o serviço com c#
Utilize o URL de serviço para enviar um pedido a partir de uma aplicação de consola c#. 

1. No Visual Studio, crie uma nova aplicação de consola: 
    * No menu de clique, arquivo -> novo -> projeto
    * No Visual Studio c#, clique em ambiente de trabalho do Windows classe, em seguida, selecione a aplicação de consola.
2. Introduza `MyFirstService` como o nome do projeto, em seguida, clique em OK.
3. Em referências de projeto, defina referências `System.Net`, e `System.Net.Http`.
4. Clique em Ferramentas -> Gestor de pacotes NuGet -> Package Manager Console, em seguida, instalar o **Microsoft.AspNet.WebApi.Client** pacote.
5. Open **Program.cs** de ficheiro e substitua o código com o código a seguir:
6. Atualização do `SERVICE_URL` e `API_KEY` parâmetros com as informações do seu serviço web.
7. Execute o projeto.

```csharp
using System;
using System.Collections.Generic;
using System.Net.Http;
using System.Net.Http.Headers;
using Newtonsoft.Json;

namespace MyFirstService
{
    class Program
    {
        // Web Service URL (update it with your service url)
        private const string SERVICE_URL = "http://<service ip address>:80/api/v1/service/<service name>/score";
        private const string API_KEY = "your service key";

        static void Main(string[] args)
        {
            Program.PostRequest();
            Console.ReadLine();
        }

        private static void PostRequest()
        {
            HttpClient client = new HttpClient();
            client.BaseAddress = new Uri(SERVICE_URL);
            //For local web service, comment out this line.
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", API_KEY);

            var inputJson = new List<RequestPayload>();
            RequestPayload payload = new RequestPayload();
            List<InputDf> inputDf = new List<InputDf>();
            inputDf.Add(new InputDf()
            {
                feature1 = value1,
                feature2 = value2,
            });
            payload.Input_df_list = inputDf;
            inputJson.Add(payload);

            try
            {
                var request = new HttpRequestMessage(HttpMethod.Post, string.Empty);
                request.Content = new StringContent(JsonConvert.SerializeObject(payload));
                request.Content.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                var response = client.SendAsync(request).Result;

                Console.Write(response.Content.ReadAsStringAsync().Result);
            }
            catch (Exception e)
            {
                Console.Out.WriteLine(e.Message);
            }
        }
        public class InputDf
        {
            public double feature1 { get; set; }
            public double feature2 { get; set; }
        }
        public class RequestPayload
        {
            [JsonProperty("input_df")]
            public List<InputDf> Input_df_list { get; set; }
        }
    }
}
```

## <a name="call-the-web-service-using-python"></a>Chamar o serviço web com Python
Utilize Python para enviar um pedido para seu serviço web em tempo real. 

1. Copie o seguinte exemplo de código para um novo ficheiro de Python.
2. Atualize os parâmetros de dados, url e api_key. Para os serviços da local web, remova o cabeçalho "Authorization".
3. Execute o código. 

```python
import requests
import json

data = "{\"input_df\": [{\"feature1\": value1, \"feature2\": value2}]}"
body = str.encode(json.dumps(data))

url = 'http://<service ip address>:80/api/v1/service/<service name>/score'
api_key = 'your service key' 
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

resp = requests.post(url, body, headers=headers)
resp.text
```
