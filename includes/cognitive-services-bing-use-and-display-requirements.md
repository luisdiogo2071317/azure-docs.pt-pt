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
ms.openlocfilehash: 174af83686eba665a729246be7a477b9a5054f30
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356079"
---
# <a name="bing-search-api-use-and-display-requirements"></a>API de pesquisa do Bing utilizar e apresentar requisitos

Requisitos de utilização e apresentar aplicam-se para qualquer implementação do conteúdo e as informações associadas. Por exemplo, os requisitos aplicam-se para outros sinais, metadados e relações. Estes podem estar disponíveis através de chamadas para as APIs seguintes:

- Pesquisa Personalizada do Bing
- Pesquisa de Entidades do Bing
- Pesquisa de Imagens do Bing
- Pesquisa de Notícias do Bing
- Pesquisa de Vídeos do Bing
- Pesquisa Visual do Bing
- Pesquisa Web do Bing
- Verificação Ortográfica do Bing
- Sugestão Automática do Bing

Pode encontrar os detalhes de implementação relacionados com estes requisitos da documentação para funcionalidades específicas e os resultados.     

## <a name="bing-spell-check-and-bing-autosuggest-apis"></a>Verificação de ortográfica do Bing e Bing APIs de sugestão automática

Não:

- Copie, armazenar ou colocar em cache todos os dados recebidos da verificação de ortográfica do Bing ou APIs de sugestão automática do Bing.
- Dados de utilização recebidos da verificação de ortográfica do Bing ou APIs de sugestão automática do Bing como parte de qualquer aprendizagem ou atividade usam semelhante. Utilize estes dados para formação, avaliar ou melhorar os serviços de novos ou existentes que terceiros ou pode oferecer.

## <a name="definitions"></a>Definições

- *Resposta* refere-se a uma categoria de resultados devolvido numa resposta. Por exemplo, uma resposta a partir da API de pesquisa do Bing Web pode incluir respostas nas categorias de resultados de página Web, imagem, vídeo, visual e notícias de última hora.   
- *Resposta* significa todas e respostas e recebidos em resposta a uma única chamada à API pesquisa de dados associados.
- *Resultado* refere-se a um item de informações numa resposta. Por exemplo, o conjunto de dados ligados a um artigo de notícias único é um resultado de uma resposta de notícias de última hora.
- *APIs de pesquisa* significa coletivamente, a pesquisa do Bing personalizada, pesquisa de entidade, pesquisa de imagem, notícias de última hora de pesquisa, pesquisa de vídeo, pesquisa Visual e APIs de pesquisa de Web. 


## <a name="search-apis"></a>Pesquisar APIs

Os requisitos nesta secção aplicam-se para as APIs de pesquisa. As APIs de pesquisa não incluem a verificação de ortográfica do Bing ou sugestão automática do Bing. Os requisitos para essas dois APIs descritos na secção anterior.

### <a name="internet-search-experience"></a>Experiência de pesquisa de Internet

Todos os dados devolvidos nas respostas só podem ser utilizados nas experiências de pesquisa de internet. Uma experiência de pesquisa de internet significa que o conteúdo apresentado, conforme aplicável: 
- É relevante e responder a consulta direta do utilizador final ou outra indicação de interesse de pesquisa do utilizador e a intenção (por exemplo, uma consulta de pesquisa indicado por utilizador). 
- Ajuda os utilizadores a localizar e navegue para as origens de dados (por exemplo, os URLs fornecidos são implementados como hiperligações, pelo que o conteúdo ou a atribuição é numa ligação clicável conspicuously apresentada com os dados). Em alternativa, se utilizar a API de pesquisa do Bing entidade, visibly ligação para o URL de bing.com fornecido na resposta que permite ao utilizador navegar para os resultados da pesquisa para a consulta no bing.com relevante.
- Inclui vários resultados para o utilizador selecionar entre (por exemplo, vários resultados da resposta de notícias são apresentados ou são devolvidos todos os resultados se menos de várias). 
- Está limitado a uma quantidade apropriada servir o objetivo de procura (por exemplo, imagem miniaturas são miniatura tamanho in proportion to apresentar do utilizador). 
- Inclui uma indicação visível ao utilizador que o conteúdo é resultados de pesquisa de internet (por exemplo, uma instrução que o conteúdo é "de web").
- Inclui qualquer outra combinação das medidas adequadas garantir a sua utilização dos dados recebidos a partir de APIs de pesquisa não violar as leis aplicáveis ou direitos de terceiros. Por exemplo, se são depender uma licença Commons criativos, poderá estar em conformidade com os termos de licenciamento aplicáveis. Consulte a sua consultores legais para determinar o que poderão ser medidas adequadas.
O requisito de experiência de pesquisa de internet, a única exceção é para a deteção de URL, conforme descrito neste artigo. 

### <a name="restrictions"></a>Restrições

Não:

- Copie, armazenar ou colocar em cache todos os dados de respostas (exceto a retenção até ao limite permitido pela continuidade da secção "Serviço neste artigo"). 
- Utilize dados recebidos as APIs de pesquisa como parte de qualquer aprendizagem ou atividade usam semelhante. Utilize estes dados para formação, avaliar ou melhorar os serviços de novos ou existentes que terceiros ou pode oferecer.
- Modificar o conteúdo de resultados (exceto ao reformatá-los de forma a que não violam quaisquer outros requisitos), exceto se exigido por lei ou Aceito pela Microsoft. 
- Omita URLs associados com conteúdo de resultado e de atribuição.
- Reordene, incluindo por omissão, os resultados apresentados numa resposta, quando uma ordem ou a classificação é fornecida, se exigido por lei ou Aceito pela Microsoft. (Para a API de pesquisa do Bing personalizada, esta regra não é aplicável a reordenação implementado através do portal de customsearch.ai.)
- Apresenta outros conteúdos de qualquer parte de uma resposta de uma forma que faria levar um utilizador se for da opinião que os outros conteúdos faz parte da resposta. 
- Apresentar publicidade que não é fornecida pela Microsoft em qualquer página que apresenta qualquer parte de uma resposta. 
- Apresentar publicidade com respostas (i) do Bing imagens, notícias de última hora de pesquisa, procura de vídeo ou APIs de pesquisa do Visual; ou (ii) que são filtrados ou limitadas principalmente (ou apenas) para a imagem, de notícias e/ou de vídeo ou visual resultados.

### <a name="notices-and-branding"></a>Avisos e de imagem corporativa 

- Forma destacada incluem uma hiperligação funcional para o [declaração de privacidade do Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839), perto de cada ponto de experiência de utilizador (UX) que oferece a capacidade de uma consulta de pesquisa de entrada de um utilizador. Etiqueta hiperligação **declaração de privacidade do Microsoft**.
- Apresentar forma destacada Bing imagem corporativa, consistente com o [diretrizes de utilização do Bing marca](https://go.microsoft.com/fwlink/?linkid=833278), perto de cada ponto de UX que oferece a capacidade de uma consulta de pesquisa de entrada de um utilizador. Este tipo de imagem corporativa tem claramente denota ao utilizador que Microsoft está a ligar a experiência de pesquisa de internet.
- Pode atributo cada resposta (ou parte de uma resposta) apresentados a partir de pesquisa na Web Bing, pesquisa de imagem, notícias de última hora de pesquisa, procura de vídeo e APIs de pesquisa do Visual para a Microsoft, a menos que a Microsoft especifique o contrário escrever para utilização. Isto é descrito no [diretrizes de utilização do Bing marca](https://go.microsoft.com/fwlink/?linkid=833278). 
- Não o atributo respostas (ou partes de respostas) apresentados a partir da API de pesquisa do Bing personalizada para a Microsoft, a menos que a Microsoft especifique o contrário escrever para utilização específica.

### <a name="transferring-responses"></a>Transferência de respostas

Se ativar um utilizador transferir uma resposta de uma API de pesquisa para outro utilizador, tal como através de uma aplicação de serviço de mensagens ou publicação de redes sociais, situações seguintes: 
- Tem de respostas transferidas:
  - Consistem de conteúdo que é inalterado do conteúdo de respostas apresentada ao utilizador transferir. São permitidas alterações de formatação.
  - Não inclua quaisquer dados no formato de metadados.
  - Para as respostas da Bing Web, imagem, notícias de última hora, vídeo e Visual APIs, idioma de apresentação que indica a resposta foi obtido através de uma experiência de pesquisa de internet com tecnologia do Bing. Por exemplo, pode apresentar os idiomas, tais como "Ligada ao Bing" ou "Saiba" mais informações sobre esta imagem no Bing ou pode utilizar o logótipo do Bing.
  - Para as respostas a partir da API de pesquisa de personalizado Bing, o idioma de apresentação que indica a resposta foi obtido através de uma experiência de pesquisa de internet. Por exemplo, pode apresentar os idiomas, tais como "Obtenha mais informações sobre este resultado da pesquisa."
  - Apresente forma destacada a consulta completa utilizada para gerar a resposta.
  - Inclua uma ligação prominent ou atribuição semelhante à origem de subjacente da resposta, diretamente ou através do motor de busca (bing.com, m.bing.com ou o serviço de pesquisa personalizada, conforme aplicável).
- Não pode automatizar a transferência de respostas. Uma transferência tem de ser iniciada por uma ação do utilizador evidencing claramente o objetivo para transferir uma resposta.
- Só pode ativar um utilizador transferir as respostas que foram apresentadas em resposta a consulta do utilizador transferir.

### <a name="continuity-of-service"></a>Continuidade do serviço 

Não copie, armazenar ou colocar em cache todos os dados de respostas de API de pesquisa. No entanto, para ativar a continuidade de acesso de serviço e de composição de dados, pode manter os resultados apenas mediante as seguintes condições:

**dispositivo.** Pode ativar um utilizador manter os resultados num dispositivo para o menor (i) 24 horas desde o momento da consulta ou (ii) até que um utilizador submete outra consulta para obter resultados atualizados, desde que os resultados retidos só podem ser utilizados:

- Para ativar o utilizador aceda aos resultados devolvidos anteriormente para esse utilizador nesse dispositivo (por exemplo, em caso de interrupção do serviço).
- Para armazenar os resultados devolvidos na sua consulta proativa personalizadas no anticipation necessidades do utilizador, com base no sinais desse utilizador (por exemplo, em caso de interrupção do serviço previsto).

**Servidor.** Pode manter os resultados específicos para um único utilizador em segurança num servidor de que controlar e apresentar apenas os resultados retidos:

- Para permitir que o utilizador aceder a um relatório de histórico de resultados devolvidos anteriormente para esse utilizador na sua solução. Os resultados podem não ser (i) retidos por mais de 21 dias da hora de consulta inicial do utilizador final e (ii) apresentados em resposta à consulta de um utilizador novo ou repetido.
- Para armazenar os resultados devolvidos na sua consulta proativa personalizadas no anticipation necessidades do utilizador, com base no sinais desse utilizador. Pode armazenar estes resultados para o menor (i) 24 horas desde o momento da consulta ou (ii) até que um utilizador submete outra consulta para obter resultados atualizados.

Sempre que retidos, não podem ser commingled resultados para um utilizador específico com resultados para outro utilizador. Ou seja, os resultados de cada utilizador tem de ser mantidos e entregue separadamente.

### <a name="general"></a>Geral 

Para todos os apresentação de resultados retidos:

- Inclua um aviso claras e visível da hora de que consulta foi enviada.
- Presente significa que o utilizador com um botão ou semelhante para efetuar uma nova consulta e obter a atualizar os resultados. 
- Manter o Bing imagem corporativa na apresentação dos resultados.
- Eliminar (e atualizar com uma nova consulta, se necessário) os resultados armazenados dentro de intervalos de tempo especificados.

### <a name="non-display-url-discovery"></a>Deteção de URL de apresentação não 

Pode utilizar apenas respostas de pesquisa numa experiência de pesquisa na internet não unicamente para efeitos de deteção de URLs de fontes de informação a responder a uma consulta do seu utilizador ou o cliente. Pode copiar esses URLs num relatório ou resposta semelhante, que fornece:

- Apenas para esse utilizador ou o cliente, em resposta a essa consulta.
- Apenas se inclui significativos importantes conteúdos adicionais, relevantes para a consulta.

Utilizam as secções anteriores das APIs de pesquisa e requisitos de apresentação não se aplicam a esta utilização não apresentar, exceto o seguinte: 

- Não colocar em cache, copiar ou armazenar quaisquer dados ou conteúdo, ou derivado da resposta de pesquisa, que não seja o URL limitado copiar descrito anteriormente.
- Certifique-se de que a utilização de dados (incluindo os URLs) recebida as APIs de pesquisa não violar as leis aplicáveis ou direitos de terceiros.
- Não utilize os dados (incluindo os URLs) recebidos as APIs de pesquisa como parte de qualquer aprendizagem máquina ou índice de pesquisa ou atividade usam semelhante. Utilize estes dados para criar a formação, avaliar ou melhorar os serviços de terceiros ou pode oferecer.

## <a name="gdpr-compliance"></a>Conformidade GDPR  

No que respeita à quaisquer dados pessoais sujeitos da União Europeia geral regulamento de proteção de dados (GDPR) e que é processada em chamadas para as APIs de pesquisa, a API de verificação ortográfica do Bing ou a API de sugestão automática do Bing, compreender que a Microsoft, sendo controladores de dados independentes sob o GDPR. Está independentemente responsável pelo cumprimento da sua com o GDPR.  

