---
title: Análise de vídeo em tempo real com a API de visão do computador | Microsoft Docs
description: Saiba como efetuar uma análise de quase em tempo real sobre Jumbo obtidos a partir de um fluxo de vídeo em direto utilizando a API de visão de computador nos serviços cognitivos.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 01/20/2017
ms.author: kefre
ms.openlocfilehash: d75b1a887e5e4557d5464d8062e1bde628e7adab
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352454"
---
# <a name="how-to-analyze-videos-in-real-time"></a>Como analisar vídeos em tempo real
Este guia demonstrar como efetuar uma análise de quase em tempo real sobre Jumbo obtidos a partir de um fluxo de vídeo em direto. Os componentes básicos num sistema são:
- Adquirir frames de uma origem de vídeo
- Selecione as fotogramas para analisar
- Submeter estes frames para a API
- Consumir cada resultado da análise que é devolvido da chamada de API

Estes exemplos são escritos em c# e o código pode ser encontrado no GitHub aqui: [ https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis ](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/).

## <a name="the-approach"></a>A abordagem
Existem várias formas de resolver o problema de análise de quase em tempo real em execução no vídeos fluxos. Iremos começará por definido que estipule as três abordagens aumentar os níveis de sofisticação.

### <a name="a-simple-approach"></a>Uma abordagem Simple
A estrutura mais simples para um sistema de análise de quase em tempo real encontra um ciclo infinito, onde em cada iteração que captar uma moldura, analisá-lo e, em seguida, consumir o resultado:
```CSharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        AnalysisResult r = await Analyze(f);
        ConsumeResult(r);
    }
}
```
Se o nosso analysis consistiu num algoritmo simples do lado do cliente, esta abordagem seria adequada. No entanto, quando o nosso Analysis Services está a acontecer na nuvem, a latência envolvidos significa que uma chamada à API poderá demorar vários segundos, durante o período de tempo não está a capturar imagens e nosso thread essencialmente está a fazer nada. A nossa velocidade máxima de moldura é limitada pela latência das chamadas de API.

### <a name="parallelizing-api-calls"></a>Parallelizing chamadas de API
Enquanto um ciclo único thread simple faz sentido para um algoritmo simples do lado do cliente, não se ajustam corretamente com a latência envolvida em chamadas de API de nuvem. A solução para este problema é permitir que as chamadas de API de execução longa ser executado em paralelo com a obtenção de moldura. Em c#, iremos foi alcançar isto utilizando o paralelismo baseado em tarefas, por exemplo:
```CSharp
while (true)
{
    Frame f = GrabFrame();
    if (ShouldAnalyze(f))
    {
        var t = Task.Run(async () => 
        {
            AnalysisResult r = await Analyze(f);
            ConsumeResult(r);
        }
    }
}
```
Isto inicia cada análise uma tarefa separada, que pode ser executado em segundo plano enquanto continuar obtenção frames novo. Isto evita a bloquear o thread principal enquanto a aguardar uma chamada à API devolver, no entanto, ter perdido algumas as garantias de que a versão simple fornecida – várias chamadas de API poderão ocorrer em paralelo e os resultados poderão obter devolvidos pela ordem incorreta. Também pode causar vários threads introduzir a função de ConsumeResult() em simultâneo, que pode ser perigosos, se a função não é seguro para thread. Por fim, este código simple não manter controlar das tarefas que obterem criadas, pelo que as exceções silenciosamente desaparecerá. Assim, o ingredient final para a adicionar é um thread de "consumidor" que será controlar as tarefas de análise, aumentar as exceções, cancelar tarefas demoradas e certifique-se de que os resultados obterem consumidos na ordem correta, um de cada vez.

### <a name="a-producer-consumer-design"></a>Um Design de consumidor produtor
No nosso sistema final "produtor consumidor", temos de um thread produtor muito semelhante a nossa anterior ciclo infinito. No entanto, em vez de consumir resultados de análise, assim que estiverem disponíveis, o produtor simplesmente coloca as tarefas para uma fila para controlá-los.
```CSharp
// Queue that will contain the API call tasks. 
var taskQueue = new BlockingCollection<Task<ResultWrapper>>();
     
// Producer thread. 
while (true)
{
    // Grab a frame. 
    Frame f = GrabFrame();
 
    // Decide whether to analyze the frame. 
    if (ShouldAnalyze(f))
    {
        // Start a task that will run in parallel with this thread. 
        var analysisTask = Task.Run(async () => 
        {
            // Put the frame, and the result/exception into a wrapper object.
            var output = new ResultWrapper(f);
            try
            {
                output.Analysis = await Analyze(f);
            }
            catch (Exception e)
            {
                output.Exception = e;
            }
            return output;
        }
        
        // Push the task onto the queue. 
        taskQueue.Add(analysisTask);
    }
}
```
Além disso, temos um thread de consumidor, que está a demorar tarefas fora da fila, a aguardar que sejam concluídas, e apresentar o resultado ou gerar uma exceção que foi emitida. Ao utilizar a fila, podemos pode garantir que os resultados obter foram consumido um de cada vez, na ordem correta, sem limitação de velocidade de fotogramas máxima do sistema.
```CSharp
// Consumer thread. 
while (true)
{
    // Get the oldest task. 
    Task<ResultWrapper> analysisTask = taskQueue.Take();
 
    // Await until the task is completed. 
    var output = await analysisTask;
     
    // Consume the exception or result. 
    if (output.Exception != null)
    {
        throw output.Exception;
    }
    else
    {
        ConsumeResult(output.Analysis);
    }
}
```

## <a name="implementing-the-solution"></a>Implementar a solução
### <a name="getting-started"></a>Introdução
Para obter a aplicação de cópia de segurança e em execução mais rapidamente possível, podemos ter implementado o sistema descrito acima, destiná-lo para ser flexível o suficiente para implementar vários cenários, enquanto a ser fáceis de utilizar. Para o código de acesso, aceda a [ https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis ](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis).

A biblioteca contém a classe FrameGrabber, que implementa o sistema de consumidor produtor analisado acima para processar as vídeos frames a partir de uma câmara Web. O utilizador pode especificar o formato exato a chamada de API e a classe utiliza eventos para permitir que o código de chamada saber quando um novo intervalo é adquirido ou um resultado da análise nova está disponível.

Para ilustrar alguns das possibilidades, existem duas aplicações de exemplo que utiliza a biblioteca. O primeiro é uma aplicação de consola simples e uma versão simplificada disto é reproduzida abaixo. Grabs fotogramas da câmara Web predefinido e envia-os para a API de letra para a deteção de rostos em.
```CSharp
using System;
using VideoFrameAnalyzer;
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;
     
namespace VideoFrameConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            // Create grabber, with analysis type Face[]. 
            FrameGrabber<Face[]> grabber = new FrameGrabber<Face[]>();
            
            // Create Face API Client. Insert your Face API key here.
            FaceServiceClient faceClient = new FaceServiceClient("<subscription key>");

            // Set up our Face API call.
            grabber.AnalysisFunction = async frame => return await faceClient.DetectAsync(frame.Image.ToMemoryStream(".jpg"));

            // Set up a listener for when we receive a new result from an API call. 
            grabber.NewResultAvailable += (s, e) =>
            {
                if (e.Analysis != null)
                    Console.WriteLine("New result received for frame acquired at {0}. {1} faces detected", e.Frame.Metadata.Timestamp, e.Analysis.Length);
            };
            
            // Tell grabber to call the Face API every 3 seconds.
            grabber.TriggerAnalysisOnInterval(TimeSpan.FromMilliseconds(3000));

            // Start running.
            grabber.StartProcessingCameraAsync().Wait();

            // Wait for keypress to stop
            Console.WriteLine("Press any key to stop...");
            Console.ReadKey();
            
            // Stop, blocking until done.
            grabber.StopProcessingAsync().Wait();
        }
    }
}
```
A segunda aplicação de exemplo é um pouco mais interessante e permite-lhe escolher qual API para chamar as vídeos fotogramas. No lado esquerdo, a aplicação mostra uma versão de pré-visualização do vídeo em direto, no lado direito mostra o resultado de API mais recente overlaid na moldura correspondente.

Na maioria dos modos, existirá um atraso visível entre o vídeo em direto no lado esquerdo e a análise visualizada no lado direito. Este atraso é o tempo necessário para efetuar a chamada de API. A exceção a esta está no modo de "EmotionsWithClientFaceDetect", que executa a deteção de rostos em localmente no computador cliente utilizando OpenCV, antes de submeter quaisquer imagens para serviços cognitivos. Ao fazê-lo, iremos pode visualizar o enfrentam detetado imediatamente e, em seguida, atualizar os emotions mais tarde após a chamada de API. Isto demonstra a possibilidade de uma abordagem de "híbrido", em que alguns processamento simple pode ser efetuado no cliente e, em seguida, cognitivos APIs de serviços podem ser utilizadas para aumentar este com a análise mais avançado, quando necessário.

![HowToAnalyzeVideo](../../Video/Images/FramebyFrame.jpg)

### <a name="integrating-into-your-codebase"></a>Integrar o seu código base
Para começar a utilizar com este exemplo, siga estes passos:

1. Obter chaves de API para as APIs de imagem digitalizada do [subscrições](https://azure.microsoft.com/try/cognitive-services/). Análise de moldura de vídeo, as APIs aplicáveis são:
    - [API de Imagem Digitalizada](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)
    - [Emoções API](https://docs.microsoft.com/azure/cognitive-services/emotion/home)
    - [API Face](https://docs.microsoft.com/azure/cognitive-services/face/overview)
2. Clone o [Cognitive-amostras-VideoFrameAnalysis](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/) repositório do GitHub

3. Abra o exemplo no Visual Studio 2015, criar e executar as aplicações de exemplo:
    - Para BasicConsoleSample, a chave de API de rostos em é "hard-coded" diretamente no [BasicConsoleSample/Program.cs](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/blob/master/Windows/BasicConsoleSample/Program.cs).
    - Para LiveCameraSample, as chaves devem ser introduzidas no painel de definições da aplicação. Estes irão ser persistente entre sessões como dados de utilizador.
        

Quando estiver pronto para integrar, **simplesmente referenciar a biblioteca de VideoFrameAnalyzer dos seus projetos.** 



## <a name="developer-code-of-conduct"></a>Código de Conduta do Programador
Como com todos os serviços cognitivos, os programadores programar com o nosso APIs e os exemplos são necessários para seguir o "[para programadores código de conduta para serviços do Microsoft Cognitive](https://azure.microsoft.com/support/legal/developer-code-of-conduct/)." 


A imagem, voz, vídeo ou texto compreender as capacidades de VideoFrameAnalyzer utiliza serviços cognitivos da Microsoft. Microsoft irá receber as imagens, áudio, vídeo e outros dados que carregar (através desta aplicação) e pode utilizá-las para efeitos de melhoramento do serviço. Vamos pedir para o ajudar a proteger as pessoas cujos dados a aplicação envia aos serviços cognitivos da Microsoft. 


## <a name="summary"></a>Resumo
Este guia, aprendeu como executar a análise de quase em tempo real em fluxos de vídeos em direto utilizando a letra, visão do computador e APIs de emoções e como pode utilizar o nosso código de exemplo para começar a utilizar. Pode começar a utilizar criar uma aplicação com chaves de API livres, o [página de inscrição de serviços cognitivos Microsoft](https://azure.microsoft.com/try/cognitive-services/). 

Hesite fornecer comentários e sugestões no [repositório do GitHub](https://github.com/Microsoft/Cognitive-Samples-VideoFrameAnalysis/), ou para obter mais amplo API comentários sobre, no nosso [UserVoice site](https://cognitive.uservoice.com/).

