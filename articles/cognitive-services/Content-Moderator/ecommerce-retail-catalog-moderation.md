---
title: 'Tutorial: moderação do catálogo de comércio eletrónico - Content Moderator'
titlesuffix: Azure Cognitive Services
description: Moderar automaticamente catálogos de comércio eletrónico com aprendizagem automática e IA.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: sajagtap
ms.openlocfilehash: 0bd61c3f1a4f660076be4e87bb5443302e5dc013
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49363999"
---
# <a name="tutorial-ecommerce-catalog-moderation-with-machine-learning"></a>Tutorial: moderação do catálogo de comércio eletrónico com aprendizagem automática

Neste tutorial, iremos aprender a implementar moderação do catálogo de comércio eletrónico inteligente ao combinar tecnologias de IA assistida por máquina com moderação humana, para oferecer um sistema de catálogo inteligente.

![Imagens dos produtos classificados](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="business-scenario"></a>Cenário de negócio

Utilize tecnologias assistidas por máquina para classificar e moderar as imagens de produtos nestas categorias:

1. Adulto (Nudez)
2. Picante (Sugestivo)
3. Celebridades
4. Bandeiras dos E.U.A.
5. Brinquedos
6. Canetas

## <a name="tutorial-steps"></a>Passos do tutorial

Este tutorial explica como seguir estes passos:

1. Inscreva-se e crie uma equipa do Content Moderator.
2. Configure sinalizadores de moderação (etiquetas) para potenciais conteúdos de celebridade e sinalizador.
3. Utilize a API de imagem do Content Moderator para analisar potenciais conteúdos para adultos e picantes.
4. Utilize a API de Imagem Digitalizada para analisar a existência de potenciais celebridades.
5. Utilize o serviço de Visão Personalizada para analisar a possível presença de sinalizadores.
6. Apresente os resultados da análise com nuances de revisão humana e a tomada de decisão final.

## <a name="create-a-team"></a>Criar uma equipa

Veja a página [Início Rápido](quick-start.md) para inscrever-se no Content Moderator e criar uma equipa. Observe o **ID da Equipa** na página **Credenciais**.


## <a name="define-custom-tags"></a>Definir etiquetas personalizadas

Leia o artigo [Etiquetas](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) para adicionar etiquetas personalizadas. Além das etiquetas **adulto** e **picante**, as novas marcas permitem que a ferramenta de revisão apresente os nomes descritivos para as etiquetas.

No nosso caso, definimos estas etiquetas personalizadas (**celebridade**, **sinalizador**, **eua**, **brinquedo**, **caneta**):

![Configurar etiquetas personalizadas](images/tutorial-ecommerce-tags2.PNG)

## <a name="list-your-api-keys-and-endpoints"></a>Listar as chaves de API e os pontos finais

1. O tutorial utiliza três APIs e as chaves correspondentes e os pontos finais da API.
2. Os pontos finais da API são diferentes com base nas suas regiões de subscrição e o ID da Equipa de Revisão do Content Moderator.

> [!NOTE]
> O tutorial foi criado para utilizar chaves de subscrição nas regiões visíveis nos seguintes pontos finais. Certifique-se de que corresponde as chaves da API com as Uris da região, caso contrário, as suas chaves poderão não funcionar com os seguintes pontos finais:

         // Your API keys
        public const string ContentModeratorKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string ComputerVisionKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string CustomVisionKey = "XXXXXXXXXXXXXXXXXXXX";

        // Your end points URLs will look different based on your region and Content Moderator Team ID.
        public const string ImageUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessImage/Evaluate";
        public const string ReviewUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0/teams/YOURTEAMID/reviews";
        public const string ComputerVisionUri = "https://westcentralus.api.cognitive.microsoft.com/vision/v1.0";
        public const string CustomVisionUri = "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/Prediction/XXXXXXXXXXXXXXXXXXXX/url";

## <a name="scan-for-adult-and-racy-content"></a>Analisar conteúdos para adultos e picantes

1. A função aceita um URL de imagem e uma matriz de pares chave-valor, como parâmetros.
2. Ele chama a API de Imagem do Content Moderator para obter as classificações de Adulto e Picante.
3. Se a classificação for superior a 0,4 (o intervalo é de 0 a 1), define o valor na matriz **ReviewTags** para **Verdadeiro**.
4. A matriz **ReviewTags** é utilizada para destacar a etiqueta correspondente na ferramenta de revisão.

        public static bool EvaluateAdultRacy(string ImageUrl, ref KeyValuePair[] ReviewTags)
        {
            float AdultScore = 0;
            float RacyScore = 0;

            var File = ImageUrl;
            string Body = $"{{\"DataRepresentation\":\"URL\",\"Value\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(ImageUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // {“answers”:[{“answer”:“Hello”,“questions”:[“Hi”],“score”:100.0}]}
                // Parse the response body. Blocking!
                GetAdultRacyScores(response.Content.ReadAsStringAsync().Result, out AdultScore, out RacyScore);
            }

            ReviewTags[0] = new KeyValuePair();
            ReviewTags[0].Key = "a";
            ReviewTags[0].Value = "false";
            if (AdultScore > 0.4)
            {
                ReviewTags[0].Value = "true";
            }

            ReviewTags[1] = new KeyValuePair();
            ReviewTags[1].Key = "r";
            ReviewTags[1].Value = "false";
            if (RacyScore > 0.3)
            {
                ReviewTags[1].Value = "true";
            }
            return response.IsSuccessStatusCode;
        }

## <a name="scan-for-celebrities"></a>Analisar celebridades

1. Inscreva-se numa [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) da [API de Imagem Digitalizada](https://azure.microsoft.com/services/cognitive-services/computer-vision/).
2. Clique no botão **Obter Chave da API**.
3. Aceite os termos.
4. Para iniciar sessão, escolha a partir da lista de contas da Internet disponíveis.
5. Observe as chaves de API apresentadas na sua página de serviço.
    
   ![Chaves de API de Imagem Digitalizada](images/tutorial-computer-vision-keys.PNG)
    
6. Veja o código de origem do projeto para a função que analisa a imagem com a API de Imagem Digitalizada.

         public static bool EvaluateComputerVisionTags(string ImageUrl, string ComputerVisionUri, string ComputerVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage Response = CallAPI(ComputerVisionUri, ComputerVisionKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (Response.IsSuccessStatusCode)
            {
                ReviewTags[2] = new KeyValuePair();
                ReviewTags[2].Key = "cb";
                ReviewTags[2].Value = "false";

                ComputerVisionPrediction CVObject = JsonConvert.DeserializeObject<ComputerVisionPrediction>(Response.Content.ReadAsStringAsync().Result);

                if ((CVObject.categories[0].detail != null) && (CVObject.categories[0].detail.celebrities.Count() > 0))
                {                 
                    ReviewTags[2].Value = "true";
                }
            }

            return Response.IsSuccessStatusCode;
        }

## <a name="classify-into-flags-toys-and-pens"></a>Classifique em sinalizadores, brinquedos e canetas

1. [Inicie sessão](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/) na [pré-visualização da API de Visão Personalizada](https://www.customvision.ai/).
2. Utilize o [Início Rápido](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) para criar o seu classificador personalizado para detetar a presença potencial de sinalizadores, brinquedos e canetas.
   ![Imagens de Formação de Visão Personalizada](images/tutorial-ecommerce-custom-vision.PNG)
3. [Obter o URL de ponto final de predição](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api) para o seu classificador personalizado.
4. Veja o código de origem do projeto para ver a função que chama o seu ponto de final de predição de classificador personalizado para analisar a sua imagem.

        public static bool EvaluateCustomVisionTags(string ImageUrl, string CustomVisionUri, string CustomVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(CustomVisionUri, CustomVisionKey, CallType.POST,
                                                   "Prediction-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // Parse the response body. Blocking!
                SaveCustomVisionTags(response.Content.ReadAsStringAsync().Result, ref ReviewTags);
            }
            return response.IsSuccessStatusCode;
        }       
 
## <a name="reviews-for-human-in-the-loop"></a>Revisões de humanos em ciclo

1. Nas secções anteriores, analisou as imagens de entrada para adultos e picantes (Content Moderator), celebridades (Imagem Digitalizada) e de Sinalizadores (Visão Personalizada).
2. Com base nos nossos limiares de correspondência para cada análise, disponibilize os casos com nuances para revisão humana na ferramenta de revisão.
        public static bool CreateReview(string ImageUrl, KeyValuePair[] Metadata) {

            ReviewCreationRequest Review = new ReviewCreationRequest();
            Review.Item[0] = new ReviewItem();
            Review.Item[0].Content = ImageUrl;
            Review.Item[0].Metadata = new KeyValuePair[MAXTAGSCOUNT];
            Metadata.CopyTo(Review.Item[0].Metadata, 0);

            //SortReviewItems(ref Review);

            string Body = JsonConvert.SerializeObject(Review.Item);

            HttpResponseMessage response = CallAPI(ReviewUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            return response.IsSuccessStatusCode;
        }

## <a name="submit-batch-of-images"></a>Submeter lote de imagens

1. Este tutorial pressupõe um diretório "C:Test" com um ficheiro de texto que tem uma lista de Urls de imagem.
2. O código a seguir verifica a existência do ficheiro e lê todos os Urls na memória.
            // Verifique um diretório de teste num ficheiro de texto com a lista de URLs de Imagem para analisar var topdir = @"C:\test\"; var Urlsfile = topdir + "Urls.txt";

            if (!Directory.Exists(topdir))
                return;

            if (!File.Exists(Urlsfile))
            {
                return;
            }

            // Read all image URLs in the file
            var Urls = File.ReadLines(Urlsfile);

## <a name="initiate-all-scans"></a>Iniciar todas as análises

1. Esta função de nível superior circula todos os URLs de imagens no ficheiro de texto que mencionámos anteriormente.
2. Ele analisa cada API e se a classificação de confiança de correspondência estiver dentro dos nossos critérios, cria uma revisão para moderadores humanos.
             // para cada URL da imagem no ficheiro... foreach (var Url in Urls) { // Inicialize uma nova matriz de marcas de revisão ReviewTags = new KeyValuePair[MAXTAGSCOUNT];

                // Evaluate for potential adult and racy content with Content Moderator API
                EvaluateAdultRacy(Url, ref ReviewTags);

                // Evaluate for potential presence of celebrity (ies) in images with Computer Vision API
                EvaluateComputerVisionTags(Url, ComputerVisionUri, ComputerVisionKey, ref ReviewTags);

                // Evaluate for potential presence of custom categories other than Marijuana
                EvaluateCustomVisionTags(Url, CustomVisionUri, CustomVisionKey, ref ReviewTags);

                // Create review in the Content Moderator review tool
                CreateReview(Url, ReviewTags);
            }

## <a name="license"></a>Licença

Todos os exemplos e SDKs dos Serviços Cognitivos da Microsoft estão licenciados com a Licença do MIT. Para obter mais detalhes, veja [LICENSE](https://microsoft.mit-license.org/) (LICENÇA).

## <a name="developer-code-of-conduct"></a>Código de Conduta do Programador

Espera-se que os programadores que utilizam os Serviços Cognitivos, incluindo esta biblioteca de cliente e exemplo, sigam o "Código de Conduta do Programador dos Serviços Cognitivos da Microsoft", disponível em http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Passos seguintes

Crie e expanda o tutorial com os [ficheiros de origem do projeto](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) no Github.
