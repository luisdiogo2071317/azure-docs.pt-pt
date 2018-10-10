---
title: Migrar a partir da API de voz do Translator para o serviço de voz
titleSuffix: Azure Cognitive Services
description: Utilize este tópico para migrar as suas aplicações de API de voz do Translator para o serviço de voz.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: Speech
ms.topic: article
ms.date: 10/01/2018
ms.author: aahi
ms.openlocfilehash: b50cdc6978519c0ec9da447c324237c00577d9fd
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48885277"
---
# <a name="migrate-from-the-translator-speech-api-to-the-speech-service"></a>Migrar a partir da API de voz do Translator para o serviço de voz

Utilize este artigo para migrar as suas aplicações de API de voz do Microsoft Translator para o [serviço de voz](index.yml). Este guia descreve as diferenças entre a API de voz do Translator e o serviço de voz e sugere estratégias para migrar seus aplicativos.

> [!NOTE]
> A chave de subscrição de API de voz do Translator não aceite pelo serviço de voz. Terá de iniciar uma nova subscrição do serviço de voz.

## <a name="comparison-of-features"></a>Comparação de funcionalidades

| Funcionalidade                                           | API de Voz do Microsoft Translator                                  | Serviço de Voz | Detalhes                                                                                                                                                                                                                                                                            |
|---------------------------------------------------|-----------------------------------------------------------------|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Tradução de texto                               | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Tradução de voz                             | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Ponto final global                                   | : heavy_check_mark:                                              | : heavy_minus_sign:                 | O serviço de voz não oferece um ponto final global. Um ponto final global automaticamente pode direcionar o tráfego para o ponto de extremidade regional mais próximo, reduzindo a latência em seu aplicativo.                                                    |
| Pontos finais regionais                                | : heavy_minus_sign:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Limite de tempo de ligação                             | 90 minutos                                               | É ilimitado com o SDK. 10 minutos com uma conexão websocket                                                                                                                                                                                                                                                                                   |
| Chave de autenticação no cabeçalho                                | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Vários idiomas traduzido num único pedido | : heavy_minus_sign:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| SDKs disponíveis                                    | : heavy_minus_sign:                                              | : heavy_check_mark:                 | Consulte a [documentação de serviço de voz](index.yml) para SDKs disponíveis.                                                                                                                                                    |
| Ligações de Websocket                             | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| API de idiomas                                     | : heavy_check_mark:                                              | : heavy_minus_sign:                 | O serviço de voz suporta o mesmo intervalo de idiomas descritas a [referência de idiomas da API do Translator](../translator-speech/languages-reference.md) artigo. |
| Filtro de palavras ofensivas e marcador                       | : heavy_minus_sign:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| . WAV/PCM como entrada                                 | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Outros tipos de arquivo como entrada                         | : heavy_minus_sign:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Resultados parciais                                   | : heavy_check_mark:                                              | : heavy_check_mark:                 |                                                                                                                                                                                                                                                                                    |
| Informações de temporização                                       | : heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                 |
| ID de Correlação                                    | : heavy_check_mark:                                              | : heavy_minus_sign:                 |                                                                                                                                                                                                                                                                                    |
| Modelos de voz personalizada                              | : heavy_minus_sign:                                              | : heavy_check_mark:                 | O serviço de voz oferece modelos de voz personalizada que lhe permitem personalizar o reconhecimento de fala para vocabulário exclusivo da sua organização.                                                                                                                                           |
| Modelos de tradução personalizados                         | : heavy_minus_sign:                                              | : heavy_check_mark:                 | Subscrever a API de tradução de texto do Microsoft permite-lhe utilizar [Translator personalizado](https://www.microsoft.com/translator/business/customization/) (atualmente em pré-visualização) para utilizar os seus dados para traduções mais precisas.                                                 |

## <a name="migration-strategies"></a>Estratégias de migração

Se ou sua organização tiver aplicações em desenvolvimento ou de produção que utilizam a API de voz do Translator, atualize-os para utilizar o serviço de voz. Consulte a [serviço de voz](index.yml) documentação para disponíveis SDKs, exemplos de códigos e tutoriais. Seguem-se alguns aspetos a considerar durante a migração:

* O serviço de voz não oferece um ponto final global. Terá de determinar se o seu aplicativo funcionará com eficiência com um único ponto de final de regional para todo o tráfego é. Se não irá, terá de usar a localização geográfica para determinar o ponto de extremidade mais eficiente.

* Se a sua aplicação utiliza ligações longa duração e não é possível utilizar os SDK disponíveis, pode utilizar uma conexão websocket e gerir o limite de tempo limite de 10 minutos, a restabelecer ligação nos momentos apropriados.

* Se a sua aplicação utilizar a API de texto do Translator e a API de voz do Translator para ativar modelos de tradução personalizadas, será possível adicionar os IDs de 'Category' diretamente com o serviço de voz.

* O serviço de voz podem concluir as traduções em vários idiomas numa única solicitação, ao contrário da API de voz do Translator.

## <a name="next-steps"></a>Passos Seguintes

* [Experimente gratuitamente o serviço de voz](get-started.md)
* [Início rápido: Reconhecer voz numa aplicação UWP utilizando o SDK de voz](quickstart-csharp-uwp.md)

## <a name="see-also"></a>Consulte também

* [O que é o serviço de voz](overview.md)
* [Documentação do serviço de voz e o SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-qsg)
