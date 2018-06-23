---
title: Utilização de pesquisa de resposta do projeto e apresentar requisitos - serviços cognitivos Microsoft | Microsoft Docs
description: Utilize e apresentar os requisitos para o ponto final de pesquisa de resposta de projeto.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 6e8eaaaa2c83a1420f2de86b23e15f4f19f7a565
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354134"
---
# <a name="project-answer-search-use-and-display-requirements"></a>Pesquisa de resposta de projeto utilizam e apresentar requisitos

Requisitos de utilização e apresentar aplicam-se para qualquer implementação do conteúdo e as informações associadas, por exemplo, relações, metadados e outros sinais, disponíveis através de chamadas para a pesquisa de conhecimento do Bing, pesquisa do Bing personalizada, pesquisa de entidade, pesquisa de imagem, Notícias de última hora pesquisa, procura vídeo, pesquisa Visual Web pesquisa, ortográfica verificação e APIs de sugestão automática. Podem ser encontrados detalhes de implementação relacionados com estes requisitos na documentação para funcionalidades específicas e os resultados.

## <a name="1-bing-spell-check-and-bing-autosuggest-api"></a>1. Verificação de ortográfica do Bing e Bing de sugestão automática do API.

Não:

- copiar, armazenar ou colocar em cache todos os dados que receber a verificação de ortográfica do Bing ou APIs de sugestão automática do Bing
- dados de utilização receber a verificação de ortográfica do Bing ou APIs de sugestão automática do Bing como parte de qualquer aprendizagem ou atividade usam semelhante para preparar, avaliar ou melhorar os serviços de novos ou existentes que terceiros ou pode oferecer.

## <a name="2-definitions"></a>2. Definições

- "resposta" refere-se a uma categoria de resultados devolvido numa resposta. Por exemplo, uma resposta a partir da API de pesquisa do Bing Web pode incluir respostas nas categorias de resultados de página Web, imagem, vídeo e notícias;
- "resposta" significa todas e respostas e os dados recebidos em resposta a uma única chamada à API de pesquisa; associados
- "resultado" refere-se a um item de informações numa resposta. Por exemplo, o conjunto de dados ligados a um artigo de notícias único é um resultado de uma resposta de notícias de última hora.
- "APIs de pesquisa" significa, coletivamente, a pesquisa do Bing personalizada, pesquisa de entidade, pesquisa de imagem, notícias de última hora de pesquisa, pesquisa de vídeo, pesquisa Visual e APIs de pesquisa de Web. 


## <a name="3-search-apis"></a>3. Pesquisar APIs

Os requisitos em 3 nesta secção aplicam-se para as APIs de pesquisa.

**UM. Experiência de pesquisa de Internet.** Todos os dados devolvidos nas respostas só podem ser utilizados nas experiências de pesquisa de internet. Uma experiência de pesquisa de internet significa que o conteúdo apresentado, conforme aplicável: 
- é relevante e responder a consulta direta do utilizador final ou outra indicação de interesse de pesquisa do utilizador e a intenção (por exemplo, consulta de pesquisa indicado por utilizador); 
- ajuda os utilizadores a localizar e navegue para as origens de dados (por exemplo, os URLs fornecidos são implementados como hiperligações para o conteúdo ou a atribuição é numa ligação clicável conspicuously apresentada com os dados); ou, se da API de pesquisa do Bing entidade, visibly ligação para o URL de bing.com fornecido na resposta que permite ao utilizador navegar para os resultados da pesquisa para a consulta relevante no bing.com;
- inclui vários resultados para o utilizador final selecionar (por exemplo, são apresentados várias resultados da resposta de notícias, ou são devolvidos todos os resultados se menos de várias); 
- está limitado a uma quantidade apropriada servir o objetivo de procura (por exemplo, imagem miniaturas são miniatura tamanho in proportion to apresentar do utilizador); 
- inclui uma indicação visível ao utilizador final que o conteúdo é resultados de pesquisa de Internet (por exemplo, uma instrução que o conteúdo é "de web"); e
- inclui quaisquer outra combinação das medidas adequadas garantir a sua utilização dos dados recebidos a partir de APIs de pesquisa não violar as leis aplicáveis ou direitos de terceiros (por exemplo, se depender de uma licença de Commons criativos complying com a licença aplicável termos de licenciamento). Consulte a sua consultores legais para determinar o que poderão ser medidas adequadas.
O requisito de experiência de pesquisa de internet, a única exceção é para a deteção de URL, conforme descrito no seguinte do secção 3E (discovery URL de apresentação não). 

**B. Restrições.** Não:

- copiar, armazenar ou colocar em cache todos os dados de respostas (exceto a retenção até ao limite permitido pela seguinte de secção "Continuidade do serviço"); 
- Utilize dados recebidos as APIs de pesquisa como parte de qualquer aprendizagem ou atividade usam semelhante para preparar, avaliar ou melhorar os serviços de novos ou existentes que terceiros ou pode oferecer.
- modificar o conteúdo de resultados (outros que ao reformatá-los de forma a que não violam quaisquer outros requisitos), a menos que exigido por lei ou acordado pela Microsoft; 
- omita a atribuição e URLs associados com conteúdo do resultado;
- reordenação, incluindo por omissão, os resultados apresentados numa resposta quando uma ordem ou a classificação é fornecida (para a API de pesquisa do Bing personalizada, esta regra não é aplicável a reordenação implementado através do portal de customsearch.ai), exceto se exigido por lei ou acordado pela Microsoft ;
- apresentar outros conteúdos de qualquer parte de uma resposta de uma forma que faria levar um utilizador final para considerar que o conteúdo de outro faz parte da resposta; 
- apresentar publicidade que não é fornecida pela Microsoft em qualquer página que apresenta qualquer parte de uma resposta; -apresentar publicidade com respostas (i) do Bing imagens, notícias ou APIs de pesquisa de vídeo; ou (ii) que são filtrados ou limitadas principalmente (ou apenas) para os resultados da imagem, notícias e/ou o vídeo.

**C. Avisos e de imagem corporativa.** 

- Forma destacada incluem uma hiperligação para a declaração de privacidade do Microsoft, disponível em funcional https://go.microsoft.com/fwlink/?LinkId=521839, perto de cada ponto de experiência de utilizador (UX) que oferece a capacidade de uma consulta de pesquisa de entrada de um utilizador. Etiqueta hiperligação "Declaração de privacidade da Microsoft".
- Apresentar forma destacada Bing imagem corporativa, consistente com as diretrizes disponíveis em https://go.microsoft.com/fwlink/?linkid=833278, perto de cada ponto de UX que oferece a capacidade de uma consulta de pesquisa de entrada de um utilizador.  Este tipo de imagem corporativa tem claramente denota ao utilizador que Microsoft está a ligar a experiência de pesquisa de internet.
- Pode atributo cada resposta (ou parte de uma resposta) apresentados a partir do Bing Web, imagem, notícias e APIs de vídeo para a Microsoft, tal como descrito no https://go.microsoft.com/fwlink/?linkid=833278, a menos que a Microsoft especifique o contrário escrever para utilização. 
- Não o atributo respostas (ou partes de respostas) apresentados a partir da API de pesquisa do Bing personalizada para a Microsoft, a menos que a Microsoft especifique o contrário escrever para utilização específica.


**D. Transferência de respostas.** Se ativar um utilizador transferir uma resposta de uma API de pesquisa para outro utilizador, tal como através de uma aplicação de serviço de mensagens ou publicação de redes sociais, situações seguintes: 
- Tem de respostas transferidas:
  - Consistem de conteúdo que é inalterado do conteúdo de respostas apresentada ao utilizador transferir (são permitidas alterações de formatação);
  - Não incluir quaisquer dados no formato de metadados;
  - Para as respostas da Bing Web, imagem, notícias e APIs de vídeo, o idioma de apresentação que indica a resposta foi obtido através de uma experiência de pesquisa de internet com tecnologia do Bing (por exemplo, "Ligada ao Bing," "mais informações sobre esta imagem no Bing", ou utilizando o logótipo do Bing);
  - Para as respostas a partir da API de pesquisa de personalizado Bing, o idioma de apresentação que indica a resposta foi obtido através de uma experiência de pesquisa de internet (por exemplo, "Saiba mais sobre este resultado de pesquisa");
  - Apresentar forma destacada a consulta completa utilizada para gerar a resposta; e
  - Inclua uma ligação prominent ou atribuição semelhante à origem de subjacente da resposta, diretamente ou através do motor de busca (bing.com, m.bing.com ou o serviço de pesquisa personalizada, conforme aplicável).
- Não pode automatizar a transferência de respostas. Uma transferência tem de ser iniciada por uma ação do utilizador evidencing claramente o objetivo para transferir uma resposta.
- Só pode ativar um utilizador transferir as respostas que foram apresentadas em resposta a consulta do utilizador transferir.

**I. Continuidade do serviço.** Não copie, armazenar ou colocar em cache todos os dados de respostas de API de pesquisa. No entanto, para ativar a continuidade de acesso de serviço e de composição de dados, pode manter os resultados apenas mediante as seguintes condições:

**Dispositivo.** Pode ativar um utilizador final manter os resultados num dispositivo para o menor (i) 24 horas desde o momento da consulta ou (ii) até que um utilizador final submete outra consulta para obter resultados atualizados, desde que os resultados retidos só podem ser utilizados:

- para permitir que o utilizador final aceder aos resultados devolvidos anteriormente para esse utilizador final nesse dispositivo (por exemplo, em caso de interrupção do serviço); ou
- para armazenar os resultados devolvidos na sua consulta proativa personalizadas no anticipation necessidades do utilizador final com base no sinais desse utilizador final (por exemplo, em caso de interrupção do serviço previsto).

**Servidor.** Pode manter os resultados específicos para um único utilizador final em segurança num servidor de controlar e apresentar apenas os resultados retidos:

- para permitir que o utilizador final aceder a um relatório de histórico de resultados devolvido para esse utilizador na sua solução, desde que os resultados podem não ser (i) retidos por mais de 21 dias da hora de consulta inicial do utilizador final e (ii) apresentado em resposta a uma extremidade u anteriormente consulta do ser novo ou repetido; ou
- para armazenar os resultados devolvidos na sua consulta proativa personalizadas no anticipation necessidades de um utilizador final com base no sinais desse utilizador final para o menor (i) 24 horas desde o momento da consulta ou (ii) até que um utilizador final submete outra consulta para obter resultados atualizados.

Sempre que é mantido, os resultados para um utilizador específico não podem ser commingled com resultados para outro utilizador, ou seja, os resultados de cada utilizador tem de ser mantidos e entregue separadamente.

**Geral.** Para todos os apresentação de resultados retidos:

- incluir um aviso claras e visível da hora de que consulta foi enviada,
- apresentar ao utilizador um botão ou semelhantes significa que efetuar uma nova consulta e obter resultados atualizados, 
- manter o Bing imagem corporativa na apresentação de resultados, e
- Eliminar (e atualizar com uma nova consulta, se necessário) os resultados armazenados dentro de intervalos de tempo especificados.

**F. Deteção de URL de apresentação não.** Pode utilizar apenas respostas de pesquisa numa experiência de pesquisa na internet não unicamente para efeitos de deteção de URLs de fontes de informação a responder a uma consulta do seu utilizador ou o cliente. Pode copiar esses URLs num relatório ou resposta semelhante que fornecer (i) apenas para esse utilizador ou o cliente, em resposta a essa consulta e (ii) que inclui significativo importante conteúdo adicional relevante para a consulta. Os requisitos de nas secções 3A através de 3E destes requisitos de utilização e apresentar não se aplicam a esta utilização não apresentar, exceto: 

- Não deverá colocar em cache, copiar ou armazenar quaisquer dados ou conteúdo de ou derivada da, a resposta de pesquisa, que não seja a cópia de URL limitado descrito anteriormente;
- Certifique-se de que a utilização de dados (incluindo os URLs) recebida as APIs de pesquisa não violar as leis aplicáveis ou direitos de terceiros; e
- Não deverá utilizar os dados (incluindo os URLs) recebidos as APIs de pesquisa como parte de nenhum índice de pesquisa ou aprendizagem ou atividade usam semelhante para criar a formação, avaliar ou melhorar os serviços de terceiros ou pode oferecer.

## <a name="next-steps"></a>Passos Seguintes
[Descrição geral de pesquisa de resposta](overview.md)
