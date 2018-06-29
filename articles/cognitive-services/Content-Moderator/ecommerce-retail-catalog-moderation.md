---
title: Moderação de interrupção do eCommerce catálogo com machine learning e AI com Moderator conteúdo do Azure | Microsoft Docs
description: ECommerce automaticamente moderada catálogos com machine learning e AI
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/25/2017
ms.author: sajagtap
ms.openlocfilehash: 6177758eaa3e611ad67da0778d889df48b052d90
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37095756"
---
# <a name="ecommerce-catalog-moderation-with-machine-learning"></a>Moderação de interrupção do eCommerce catálogo com machine learning

Neste tutorial, vamos aprender a implementar a moderação de interrupção do machine learning baseada em inteligente ecommerce catálogo através da combinação de tecnologias de AI auxiliada a uma máquina com moderação de interrupção humana para fornecer um sistema de catálogo inteligente.

![Imagens de produto classificada](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="business-scenario"></a>Cenário de negócio

Utilize tecnologias de assistida por máquina para classificar e moderada imagens de produto destas categorias:

1. Adulto (Nudity)
2. Racy (Suggestive)
3. Celebridades para
4. E.u. a sinalizadores
5. Toys
6. Pens

## <a name="tutorial-steps"></a>Passos do tutorial

O tutorial orienta-o estes passos:

1. Inscrever-se e criar um agrupamento de conteúdo Moderator.
2. Configure a moderação interrupção as etiquetas (etiquetas) para o conteúdo celebrity e sinalizador potencial.
3. Utilize a API de imagem do conteúdo Moderator para analisar para o conteúdo para adultos e racy potencial.
4. Utilize a API de visão do computador para procurar celebridades para potenciais.
5. Utilize o serviço de visão personalizada para procurar a presença possível de sinalizadores.
6. Apresente os resultados da análise nuanced para revisão humano e efetuar decisão final.

## <a name="create-a-team"></a>Criar um agrupamento

Consulte o [início rápido](quick-start.md) página inscreva Moderator conteúdo e criar um agrupamento. Tenha em atenção o **equipa ID** do **credenciais** página.


## <a name="define-custom-tags"></a>Definir etiquetas personalizadas

Consulte o [etiquetas](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) artigo para adicionar etiquetas personalizadas. Para além de incorporada **para adultos** e **racy** etiquetas, a ferramenta de revisão visualizar os nomes descritivos para as etiquetas de permitir que as etiquetas de novo.

No nosso caso, iremos definir estas etiquetas personalizadas (**celebrity**, **sinalizador**, **nos**, **toy**, **caneta**):

![Configurar etiquetas personalizadas](images/tutorial-ecommerce-tags2.PNG)

## <a name="list-your-api-keys-and-endpoints"></a>Listar as chaves de API e os pontos finais

1. O tutorial utiliza três APIs e as correspondentes chaves e pontos finais de API.
2. Os pontos finais de API são diferentes com base nas suas regiões de subscrição e o ID da equipa de revisão Moderator conteúdos.

> [!NOTE]
> O tutorial foi concebido para utilizar chaves de subscrição nas regiões visíveis nos seguintes pontos finais. Lembre-se de que as chaves de API com a região Uris; caso contrário, as chaves podem não funcionar com os seguintes pontos finais de corresponder ao:

         // Your API keys
        public const string ContentModeratorKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string ComputerVisionKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string CustomVisionKey = "XXXXXXXXXXXXXXXXXXXX";

        // Your end points URLs will look different based on your region and Content Moderator Team ID.
        public const string ImageUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessImage/Evaluate";
        public const string ReviewUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0/teams/YOURTEAMID/reviews";
        public const string ComputerVisionUri = "https://westcentralus.api.cognitive.microsoft.com/vision/v1.0";
        public const string CustomVisionUri = "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/Prediction/XXXXXXXXXXXXXXXXXXXX/url";

## <a name="scan-for-adult-and-racy-content"></a>Análise de conteúdo para adultos e racy

1. A função aceita um URL de imagem e uma matriz de pares chave-valor como parâmetros.
2. Chama o Moderator conteúdo imagem API para obter as pontuações adulto e Racy.
3. Se a classificação é superior ao 0.4 (intervalo é de 0 a 1), define o valor no **ReviewTags** de matriz para **verdadeiro**.
4. O **ReviewTags** matriz é utilizada para Realce a etiqueta correspondente na ferramenta de revisão.

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

## <a name="scan-for-celebrities"></a>Procurar celebridades para

1. Inscrever-se um [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) do [computador visão API](https://azure.microsoft.com/services/cognitive-services/computer-vision/).
2. Clique em de **obter chave de API** botão.
3. Aceite os termos.
4. Para iniciar sessão, escolha na lista de contas da Internet disponíveis.
5. Tenha em atenção as chaves de API apresentadas na página do seu serviço.
    
   ![Chaves de API de visão do computador](images/tutorial-computer-vision-keys.PNG)
    
6. Consulte o código de origem do projeto para a função que analisa a imagem com a API de visão do computador.

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

## <a name="classify-into-flags-toys-and-pens"></a>Classificar sinalizadores, toys e pens

1. [Inicie sessão no](https://azure.microsoft.com/en-us/services/cognitive-services/custom-vision-service/) para o [pré-visualização de API de visão personalizada](https://www.customvision.ai/).
2. Utilize o [início rápido](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) para criar o classificador personalizado para detetar a presença de sinalizadores, toys e pens potencial.
   ![Imagens de formação de visão personalizada](images/tutorial-ecommerce-custom-vision.PNG)
3. [Obter o URL de ponto final de predição](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api) para sua classificador personalizado.
4. Consulte o código de origem do projeto para ver a função que chama o ponto final da predição classificador personalizado para digitalizar a imagem.

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
 
## <a name="reviews-for-human-in-the-loop"></a>Revisões humanos no ciclo for

1. Nas secções anteriores, analisado as entrada imagens para adultos e racy (Moderator conteúdo), sinalizadores (visão personalizada) e celebridades para (visão de computador).
2. Com base na nossa limiares de correspondência de cada análise, disponibilizar os casos nuanced para revisão humana na ferramenta de revisão.
        bool estático público {CreateReview (cadeia ImageUrl, KeyValuePair [] metadados)

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

## <a name="submit-batch-of-images"></a>Submeter o lote de imagens

1. Este tutorial assume um diretório de "C:Test" com um ficheiro de texto que tem uma lista de Urls de imagem.
2. O seguinte código verifica a existência do ficheiro e lê todos os Urls na memória.
            Verifique a existência de um diretório de teste para um ficheiro de texto com a lista de URLs de imagem para analisar var topdir = @"C:\test\"; var Urlsfile = topdir +"Urls.txt";

            if (!Directory.Exists(topdir))
                return;

            if (!File.Exists(Urlsfile))
            {
                return;
            }

            // Read all image URLs in the file
            var Urls = File.ReadLines(Urlsfile);

## <a name="initiate-all-scans"></a>Iniciar todas as análises

1. Esta função de nível superior repetido ao longo de todos os URLs de imagem no ficheiro de texto que foi mencionados anteriormente.
2. Este analisa-os com cada API e se o modelo de pontuação de confiança correspondência está dentro do nosso critérios, cria uma revisão para moderators humanos.
             para cada imagem de URL no ficheiro … foreach (var Url em Urls) {/ / Initiatize uma matriz de etiquetas de rever novos ReviewTags = KeyValuePair novo [MAXTAGSCOUNT];

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

Todos os Microsoft cognitivos serviços SDKs e amostras são licenciadas com a licença MIT. Para obter mais detalhes, consulte [licença](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Código de Conduta do Programador

Os programadores utilizando serviços cognitivos, incluindo esta biblioteca de cliente & exemplo, são esperados para seguir o "Programador código de conduta para Microsoft serviços cognitivos", encontrado em http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Passos Seguintes

Criar e expandir o tutorial, utilizando o [ficheiros de origem do projeto](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) no Github.
