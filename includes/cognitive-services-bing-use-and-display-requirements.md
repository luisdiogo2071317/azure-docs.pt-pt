---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: MikeDodaro
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 04/19/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: f166ceac1ae848565f861a94781ce0500c24747e
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51289160"
---
Requisitos de utilização e a exibição se aplicam a qualquer implementação do conteúdo e informações associadas. Por exemplo, os requisitos se aplicam a outros sinais, metadados e relações. Estes podem ser disponíveis por meio de chamadas para as seguintes APIs:

- Pesquisa Personalizada do Bing
- Pesquisa de Entidades do Bing
- Pesquisa de Imagens do Bing
- Pesquisa de Notícias do Bing
- Pesquisa de Vídeos do Bing
- Pesquisa Visual do Bing
- Pesquisa Web do Bing
- Verificação Ortográfica do Bing
- Sugestão Automática do Bing

Pode encontrar detalhes de implementação relacionados a esses requisitos na documentação de funcionalidades específicas e os resultados.     

## <a name="bing-spell-check-and-bing-autosuggest-apis"></a>Verificação ortográfica do Bing e o Bing APIs de sugestão automática

Não:

- Copiar, armazenar ou colocar em cache todos os dados recebidos da verificação de ortografia do Bing ou APIs de sugestão automática do Bing.
- Utilize dados recebidos de verificação ortográfica do Bing ou APIs de sugestão automática do Bing como parte de qualquer machine learning ou de uma atividade de algoritmo semelhante. Não utilize estes dados para treinar, avaliar ou melhorar os serviços de novos ou existentes que pode oferecer a ou de terceiros.

## <a name="definitions"></a>Definições

- *Resposta* refere-se a uma categoria de resultados retornados numa resposta. Por exemplo, uma resposta da API de pesquisa Web do Bing pode incluir respostas nas categorias de resultados de página Web, imagem, vídeo, visual e notícias.   
- *Resposta* significa que quaisquer e todas as respostas e associados a dados recebidos em resposta a uma única chamada para uma API de pesquisa.
- *Resultado* refere-se a um item de informações numa resposta. Por exemplo, o conjunto de dados ligados com um artigo de notícias único é um resultado numa resposta de notícias.
- *APIs de pesquisa* significa, coletivamente, a pesquisa personalizada do Bing, pesquisa de entidades, pesquisa de imagens, pesquisa de notícias, pesquisa de vídeos, pesquisa Visual e APIs de pesquisa da Web. 


## <a name="search-apis"></a>Pesquisar APIs

Os requisitos nesta secção aplicam-se para as APIs de pesquisa. As APIs de pesquisa não incluem a verificação de ortografia do Bing ou sugestão automática do Bing. Os requisitos para essas duas APIs são abordados na secção anterior.

### <a name="internet-search-experience"></a>Experiência de pesquisa da Internet

Todos os dados retornados em respostas só podem ser utilizados em experiências de pesquisa da internet. Uma experiência de pesquisa da internet significa que o conteúdo exibido, conforme aplicável: 
- É relevante e responsivo a consulta direta do utilizador final ou outra indicação do interesse de pesquisa do usuário e a intenção (por exemplo, uma consulta de pesquisa indicado por utilizador). 
- Ajuda os usuários a localizar e navegue para as origens de dados (por exemplo, os URLs fornecidos são implementados como hiperlinks, para que o conteúdo ou a atribuição é um link clicável visível apresentado com os dados). Em alternativa, se utilizar a API de pesquisa de entidades do Bing, visivelmente ligar para o URL de bing.com fornecido na resposta que permite que o usuário navegue para os resultados da pesquisa para a consulta relevante no bing.com.
- Inclui vários resultados para o utilizador selecionar a partir de (por exemplo, vários resultados da resposta de notícias são apresentados ou são devolvidos todos os resultados se menos de vários). 
- Está limitado a uma quantidade adequada atender a finalidade de pesquisa (por exemplo, imagem miniaturas são miniatura em tamanho normal em proporção de exibição ao usuário). 
- Inclui uma indicação visível ao utilizador que o conteúdo está resultados de pesquisa da internet (por exemplo, uma instrução que o conteúdo é "a partir da web").
- Inclui qualquer outra combinação de medidas apropriadas para garantir a sua utilização dos dados recebidos a partir as APIs de pesquisa não viole qualquer leis aplicáveis ou direitos de terceiros. Por exemplo, se está contando com uma licença Creative Commons, está em conformidade com os termos de licenciamento aplicáveis. Consulte os consultores legais para determinar o que podem ser medidas adequadas.
A única exceção para o requisito de experiência de pesquisa de internet é para a deteção de URL, conforme descrito neste artigo. 

### <a name="restrictions"></a>Restrições

Não:

- Copiar, armazenar ou colocar em cache todos os dados de respostas (exceto a retenção até ao limite permitido pela secção "Continuidade do serviço" neste artigo). 
- Utilize dados recebidos a partir as APIs de pesquisa como parte de qualquer machine learning ou de uma atividade de algoritmo semelhante. Não utilize estes dados para treinar, avaliar ou melhorar os serviços de novos ou existentes que pode oferecer a ou de terceiros.
- Modificar o conteúdo de resultados (exceto ao reformatá-los de forma que não viole a qualquer outro requisito), a menos que o exigido por lei ou acordado pela Microsoft. 
- Omita URLs associadas com conteúdo de resultado e de atribuição.
- Reordene, incluindo a omissão, resultados exibidos numa resposta, quando uma ordem ou a classificação é fornecida, se exigido por lei ou concordou em pela Microsoft. (Para a API de pesquisa personalizada do Bing, esta regra não é aplicável a reordenação implementado através do portal de customsearch.ai.)
- Exiba outros tipos de conteúdo dentro de qualquer parte de uma resposta de uma forma que poderia levar um utilizador para acreditar que o outro tipo de conteúdo é parte da resposta. 
- Apresentar anúncios que não é fornecido pela Microsoft em qualquer página que apresenta qualquer parte de uma resposta. 
- Apresentar publicidade com respostas (i) a partir de imagens do Bing, pesquisa de notícias, pesquisa de vídeos ou APIs de pesquisa Visual; ou (ii) que são filtrados ou limitada principalmente (ou unicamente) para imagens, notícias e/ou vídeo ou resultados visual.

### <a name="notices-and-branding"></a>Avisos e de imagem corporativa 

- Destaque incluir um hiperlink funcional para o [declaração de privacidade do Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839), junto a cada ponto na experiência do usuário (UX) que oferece a capacidade de inserir uma consulta de pesquisa de um utilizador. Etiquetar o hiperlink **declaração de privacidade do Microsoft**.
- Destaque apresentar Bing identidade visual, consistente com o [diretrizes de utilização de marcas registadas do Bing](https://go.microsoft.com/fwlink/?linkid=833278), junto a cada ponto na UX que oferece a capacidade de inserir uma consulta de pesquisa de um utilizador. Essa imagem corporativa tem claramente indicar ao utilizador que a Microsoft está incluindo a experiência de pesquisa da internet.
- Pode atributo cada resposta (ou parte de uma resposta) apresentados a partir de pesquisa Web Bing, pesquisa de imagens, pesquisa de notícias, pesquisa de vídeos e APIs de pesquisa Visual à Microsoft, a menos que a Microsoft especifique o contrário escrever para a sua utilização. Isso é descrito em [diretrizes de utilização de marcas registadas do Bing](https://go.microsoft.com/fwlink/?linkid=833278). 
- Não atributo respostas (ou partes de respostas) apresentados a partir da API de pesquisa personalizada do Bing para a Microsoft, a menos que a Microsoft especifique o contrário escrever para a sua utilização particular.

### <a name="transferring-responses"></a>Transferência de respostas

Se ativar um utilizador para transferir uma resposta de uma API de pesquisa para outro utilizador, tal como através de uma aplicação de mensagens ou lançamento de mídia social, situações seguintes: 
- Tem de respostas transferidas:
  - Consistem em conteúdo que é sem modificações do conteúdo das respostas apresentada ao utilizador a transferir. Alterações de formatação são permitidas.
  - Não inclua quaisquer dados em formato de metadados.
  - Para as respostas da Web do Bing, imagem, notícias, vídeo e Visual APIs, o idioma de apresentação que indica que a resposta foi obtido através de uma experiência de pesquisa da internet com tecnologia Bing. Por exemplo, pode exibir o idioma, como "Ligado ao Bing" ou "Saiba mais" mais informações sobre esta imagem no Bing, ou pode utilizar o logótipo do Bing.
  - Para as respostas da API de pesquisa personalizada do Bing, o idioma de apresentação que indica que a resposta foi obtido através de uma experiência de pesquisa da internet. Por exemplo, pode exibir o idioma, como "Saiba mais sobre este resultado de pesquisa."
  - Destaque-apresente a consulta completa utilizada para gerar a resposta.
  - Inclua um link proeminente ou atribuição semelhante para a origem subjacente da resposta, diretamente ou por meio do mecanismo de pesquisa (bing.com, m.bing.com ou seu serviço de pesquisa personalizada, conforme aplicável).
- Não, pode automatizar a transferência de respostas. Uma transferência tem de ser iniciada por uma ação do utilizador evidencing claramente uma intenção de transferência de uma resposta.
- Apenas pode ativar um utilizador para transferir as respostas que foram apresentadas em resposta à consulta de transferência do utilizador.

### <a name="continuity-of-service"></a>Continuidade do serviço 

Não copiar, armazenar ou colocar em cache todos os dados de respostas de API de pesquisa. No entanto, para ativar a continuidade de acesso de serviço e composição de dados, pode manter os resultados apenas nas seguintes condições:

**Dispositivo.** Pode permitir que um utilizador manter os resultados num dispositivo para o menor de (i) 24 horas desde o momento da consulta, ou (ii) até que um usuário envia outra consulta para obter resultados atualizados, desde que manteve resultados podem ser utilizados apenas:

- Para permitir que o usuário acessar os resultados devolvidos anteriormente para esse utilizador nesse dispositivo (por exemplo, em caso de interrupção do serviço).
- Para armazenar os resultados devolvidos na sua consulta proativa personalizadas antes das necessidades do usuário, com base em sinais desse usuário (por exemplo, em caso de interrupção do serviço antecipado).

**Servidor.** Pode manter os resultados específicos para um único utilizador em segurança num servidor que controla e exibir os resultados retidos apenas:

- Para permitir que o utilizador aceder a um relatório de histórico de resultados devolvido anteriormente ao que o utilizador na sua solução. Os resultados podem não ser (i) mantidos durante mais de 21 dias a partir da hora de consulta de inicial do usuário final e (ii) exibidos em resposta a consulta de um utilizador novo ou repetidas.
- Para armazenar os resultados devolvidos na sua consulta proativa personalizadas antes das necessidades do usuário, com base em sinais desse utilizador. Pode armazenar estes resultados para o menor de (i) 24 horas desde o momento da consulta, ou (ii) até que um usuário envia outra consulta para obter resultados atualizados.

Sempre que estiverem, resultados de um utilizador específico não podem ser commingled com resultados para outro utilizador. Ou seja, os resultados de cada utilizador devem ser retidos e entregue em separado.

### <a name="general"></a>Geral 

Para todos os apresentação de resultados retidos:

- Inclua um aviso claro e visível do tempo que a consulta foi enviada.
- Atualizar o presente, significa que o utilizador com um botão ou semelhante para efetuar uma nova consulta e obter resultados. 
- Manter o Bing a imagem corporativa na apresentação dos resultados.
- Eliminar (e atualizar com uma nova consulta, se necessário) os resultados armazenados dentro os períodos de tempo especificados.

### <a name="non-display-url-discovery"></a>Deteção de URL de apresentação não 

Só poderá utilizar as respostas da pesquisa numa experiência de pesquisa não internet para o único propósito de URLs de fontes de informações de capacidade de resposta a uma consulta do cliente ou utilizador a detetar. Pode copiar esses URLs num relatório ou resposta semelhante que é fornecer:

- Apenas para esse utilizador ou o cliente, em resposta a essa consulta.
- Apenas se ele inclui significativo valiosos conteúdos adicionais, relevantes para a consulta.

Utilizam as secções anteriores das APIs de pesquisa e requisitos de apresentação não são aplicáveis a esta utilização não apresentar, exceto para o seguinte: 

- Não colocar em cache, copiar ou armazenar quaisquer dados ou conteúdo a partir de, ou derivada, a resposta da pesquisa, que não seja a URL limitado cópia descrito anteriormente.
- Certifique-se de que a utilização de dados (incluindo os URLs) recebida das APIs de pesquisa não viole qualquer leis aplicáveis ou direitos de terceiros.
- Não utilize os dados (incluindo os URLs) recebidos das APIs de pesquisa como parte de qualquer aprendizado de máquina ou índice de pesquisa ou de uma atividade de algoritmo semelhante. Não utilize estes dados para criar a treinar, avaliar ou melhorar os serviços que ou de terceiros pode oferecer.

## <a name="gdpr-compliance"></a>Conformidade do GDPR  

Em relação a quaisquer dados pessoais sujeitos a União Europeia geral regulamento de proteção de dados (GDPR) e que é processado juntamente com chamadas a APIs de pesquisa, a API de verificação ortográfica do Bing ou a API de sugestão automática do Bing, compreende que a Microsoft e são controladores de dados independente no GDPR. É responsável independentemente de sua conformidade com o GDPR.  

