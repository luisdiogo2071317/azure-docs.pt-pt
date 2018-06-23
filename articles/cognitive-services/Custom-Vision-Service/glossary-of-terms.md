---
title: Glossário de termos para personalizada de visão de serviço - serviços cognitivos do Azure | Microsoft Docs
description: Glossário de termos de serviço de visão personalizada.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/08/2017
ms.author: anroth
ms.openlocfilehash: 871617ce3c1c5a84df746c0c7d87c113b3a6f354
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352886"
---
# <a name="glossary-of-terms-for-custom-vision-service"></a>Glossário de termos de serviço de visão personalizada

Seguem-se alguns termos utilizados no serviço de visão personalizada e os respetivos significado.

## <a name="classifier"></a>Classificador

Um classificador é um modelo que cria utilizando o serviço de visão personalizada, utilizando algumas imagens de formação. Assim que tiver concluído a um novo classificador de preparação, receberá um avaliação ponto final (HTTPS) que pode adicionar à sua aplicação. Cada classificador que cria no seu próprio projeto, e pode ver todos os projetos assim que tiver iniciado a sessão.

## <a name="domain"></a>Domínio

Quando cria um projeto, selecione "domínio" para esse projeto. O domínio otimiza um classificador de um tipo específico do objeto na suas imagens. Por exemplo, se o seu cenário classificar entre as imagens do setor apple versus imagens de carrot cake, em seguida, selecione o domínio de "Prato". Se não souber de qual para escolher o domínio, em seguida, selecione o domínio "Genérico".

- **O domínio prato.** Otimizado para dishes que vir no menu restaurante. -Não foi otimizado para reconhecer acessíveis individuais ou vegetables. Se pretende classificar fotografias de fruta individuais ou vegetables, utiliza o domínio genérico para essa finalidade.
- **O domínio Landmark.** Otimizado para reconhecível landmarks, naturais e artificial. Este domínio funciona melhor quando o landmark é claramente visível no fotografia, mesmo que o landmark é ligeiramente obstructed por um grupo de pessoas com quem está a representar existentes à frente dele.
- **O domínio de revenda.** Otimizado para classificar imagens num catálogo compras ou no site de compras. Se pretender precisão elevada quando classificar dresses, pants, shirts, etc., em seguida, utilizar o domínio de revenda.
- **O domínio para adultos.** Otimizado para definir melhor entre conteúdo para adultos e conteúdo não adulto. Por exemplo, se pretender bloquear as imagens de pessoas bathing de encriptação, este domínio permite-lhe criar um classificador personalizado para o fazer.
- **O domínio geral.** Também é adequada para uma grande variedade de tarefas de classificação de imagem.

Os modelos gerados pelo **compactar domínios** podem ser exportados com a funcionalidade de exportação de iteração. São otimizados para as restrições de classificação em tempo real em dispositivos móveis. Classificadores criadas com um domínio de compactação podem ser ligeiramente inferior um domínio padrão com a mesma quantidade de dados de preparação. O compromisso é o que são suficientemente pequenas para ser executada localmente em quase em tempo real. 

## <a name="training-image"></a>Imagem de formação

Para criar um classificador de precisão elevada, serviço de visão personalizada tem várias imagens de formação. Uma imagem de formação é uma fotografia da imagem que pretende que o serviço de visão personalizada para classificar. Por exemplo, para classificar laranjas, terá de carregar várias imagens de laranjas para o serviço de visão personalizada para permitir que o serviço criar um classificador que pode reconhecer laranjas. Recomendamos que, pelo menos, 30 imagens por etiqueta.

## <a name="iteration"></a>Iteração

Cada vez que a formação ou voltar a preparar o classificador, criar uma nova iteração do modelo. Vamos manter iterações passadas várias para permitem-lhe comparar o seu progresso ao longo do tempo. Pode eliminar qualquer iteração que já não é útil. Lembre-se de que a eliminação uma iteração é permanente e também eliminar quaisquer imagens ou as alterações que foram exclusivas para essa iteração. 

## <a name="workspace"></a>Área de trabalho

A área de trabalho contém todas as imagens de formação e este reflete todas as alterações do seu iteração mais recente como removidas ou adicionadas imagens. Quando preparar o classificador, crie uma nova iteração do seu classificador, utilizando as imagens presentes na sua área de trabalho.

## <a name="tags"></a>Etiquetas

Utilize etiquetas para etiquetar objetos nas suas imagens de formação. Se estiver a criar um classificador para identificar dogs e ponies, seria colocar uma etiqueta de "preguiçoso" em imagens que contêm dogs, uma etiqueta de "pony" nas imagens que contêm ponies e ambos "preguiçoso" e uma etiqueta de "pony" nas imagens que contêm um preguiçoso e um pony.

## <a name="evaluation"></a>Avaliação

Depois de ter preparado o classificador, pode submeter qualquer imagem para avaliação utilizando o ponto final de https gerada automaticamente. O classificador devolve um conjunto de etiquetas previstos, por ordem de confiança.

## <a name="predictions"></a>Previsões

Como o classificador aceita novas imagens de classificar, armazena as imagens para si. Pode utilizar estas imagens para melhorar a precisão do seu classificador por marcação corretamente as imagens incorrectamente previstas. Em seguida, pode utilizar estas novas imagens para voltar a preparar o classificador.

## <a name="precision"></a>Precisão

Quando classifica uma imagem, como provável é o classificador corretamente classificar a imagem? Fora de todas as imagens utilizadas para preparar o classificador (dogs e ponies), que percentagem modelo obtiveram correto? 99 etiquetas corretas fora 100 imagens proporciona uma precisão de 99%.

## <a name="recall"></a>Recuperar

Fora de todas as imagens que devem ter sido corretamente classificadas, quantos o classificador identificar corretamente? Tal significa devolução de chamada de 100%, se existirem 38 imagens de preguiçoso nas imagens do utilizados para preparar o classificador, foram encontrados 38 dogs pelo classificador.

## <a name="settings"></a>Definições

Existem dois tipos de definições, as definições de nível de projeto e as definições de nível de utilizador.

- Definições de nível de projeto: 
  
  Ao nível do projecto definições aplicam-se a um projeto ou classificador. Estas definições incluem:

   - Domínio do projeto
   - Nome do Projeto
   - Descrição do Projeto
   - Utilização:
      - Número de imagens de formação
      - Número de sinalizadores criado
      - Número de iterações criado

- Definições de nível de utilizador: 
   - Chaves de subscrição: uma para formação, um para avaliação/predição.
   - Utilização:
      - Número de projetos criado
      - Número de chamadas à API de avaliação/predição.
