---
title: As API de conhecimento académico para académicas Microsoft Graph | Microsoft Docs
description: Utilize a API de conhecimento académico para interpretar consultas de utilizador e obter informações avançadas do gráfico académico nos serviços cognitivos da Microsoft.
services: cognitive-services
author: mvorvoreanu
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: mivorvor
ms.openlocfilehash: e241f9a87cd58b62eafd754bd3cb4283aa0a1e92
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352345"
---
# <a name="academic-knowledge-api"></a>API de Conhecimento Académico

Bem-vindo à API de conhecimento académico. Com este serviço, poderá interpretar as consultas do utilizador para fins académicos e obter informações importantes do Microsoft Academic Graph (MAG). A base de dados de conhecimento MAG é um gráfico de entidade heterogéneos web escala composto entidades scholarly atividades do modelo: campo prático, autor, instituição, documento, venue e eventos. 

Os dados MAG é mined o índice de web Bing, bem como uma base de dados de conhecimento interna do Bing. Como resultado em curso Bing indexação, esta API irá conter informações novas da Web segue a deteção e indexação ao Bing. Com base neste conjunto de dados, as APIs de dados de conhecimento académico permite que a caixa de diálogo condicionada por dados de conhecimento, interativa que combina perfeitamente pesquisa reativa com sugestão proativa experiências, resultados de pesquisa de gráfico de documento de pesquisa avançada e distribuições de histograma do valores de atributo para um conjunto de papers e entidades relacionadas.

Para obter mais informações sobre o Microsoft Graph académico, consulte [ http://aka.ms/academicgraph ](http://aka.ms/academicgraph).

A API de conhecimento académico foi movido de pré-visualização de serviços cognitivos a cognitivos serviços laboratórios. Home page novo para o projeto é: [ https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge ](https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge). A chave de API existente irá continuar a trabalhar até 24th de Maio de 2018. Após esta data, volte a gerar uma nova chave de API. Tenha em atenção que pré-visualização paga deixará de estar disponível assim que a sua chave existente expira. Entre em contacto com a nossa equipa se o escalão gratuito da API não é suficiente para os fins pretendidos. 

## <a name="features"></a>Funcionalidades
A API de conhecimento académico consiste em quatro pontos finais de REST relacionados:  
  1. **interpretar** – interpreta uma cadeia de consulta do utilizador de linguagem natural. Devolve interpretações anotadas para permitir experiências de conclusão automática de caixa de pesquisa que antecipam o que o utilizador está a escrever.  
  2. **avaliar** – avalia uma expressão de consulta e devolve dados de conhecimento académico entidade resultados.  
  3. **calchistogram** – calcula um histograma da distribuição de valores de atributo para as entidades académicas devolvidos por uma expressão de consulta, tais como a distribuição citações anualmente para um determinado autor.  
  4. **pesquisa de gráfico** – procura um gráfico fornecido padrão e devolve os resultados de entidade correspondente.

Utilizado em conjunto, estes métodos API permitem criar uma experiência de pesquisa avançada de semântica. Fornecida uma cadeia de consulta do utilizador, o **interpretar** método disponibiliza uma versão de consulta e uma expressão de consulta estruturada, ao concluir a consulta do utilizador com base na semântica da académicas subjacente opcionalmente anotada dados. Por exemplo, se a cadeia de tipos de um utilizador *s latentes*, a **interpretar** método pode fornecer um conjunto de interpretações classificados, sugerindo que o utilizador poderá procurar o campo de prático  *análise de semântica latentes*, o documento *analysis estrutura latentes*, ou outras expressões de entidade a partir *s latentes*. Estas informações podem ser utilizadas para rapidamente Guia do utilizador para os resultados da pesquisa pretendido.

O **avaliar** método pode ser utilizado para obter um conjunto de entidades de documento de base de dados de conhecimento académico e o **calchistogram** método pode ser utilizado para calcular a distribuição de valores de atributo para um conjunto de entidades de documento que pode ser utilizado para continuar a filtrar os resultados da pesquisa.        

O **pesquisa gráfico** método tem dois modos: *json* e *lambda*. O *json* modo pode efetuar a correspondência de acordo com os padrões de gráfico especificados por um objeto JSON de padrão de gráfico. O *lambda* modo pode efetuar cálculos do lado do servidor durante traversals de gráfico, de acordo com as expressões lambda de utilizador especificado.

## <a name="getting-started"></a>Introdução 
Consulte os subtópicos à esquerda para documentação detalhada.  Tenha em atenção que, para melhorar a legibilidade dos exemplos, as chamadas de REST API contém carateres (tais como espaços) que não foram codificados de URL.  O código irá precisar de aplicar as codificações de URL adequadas.
