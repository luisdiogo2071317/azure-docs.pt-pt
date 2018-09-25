---
title: Dividir coluna por transformação de exemplo com o Azure Machine Learning Workbench
description: O documento de referência para a transformação de "Dividir coluna por exemplo"
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc, reference
ms.topic: article
ms.date: 09/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 3edf49484e5bc05a297b8d8969632fb902aa1714
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46953740"
---
# <a name="split-column-by-example-transformation"></a>Dividir coluna por transformação de exemplo

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Essa transformação predictively divide o conteúdo de uma coluna nos limites significativos sem exigir a intervenção do utilizador. O algoritmo de divisão seleciona os limites depois de analisar o conteúdo da coluna. Estes limites poderiam ser definidos por
* Um delimitador fixo,
* Vários delimitadores arbitrários que aparecem em particular contextos, ou,
* Padrões de dados ou determinados tipos de entidade

Os utilizadores também podem controlar o comportamento de divisão no modo avançado, onde pode especificar os delimitadores ou ao fornecer exemplos de divisão pretendido.

Em teoria, também podem ser executadas operações de divisão a Bancada de trabalho usando uma série de *derivar coluna por exemplo* transforma. No entanto, se existirem várias colunas, efetuar a derivação de cada um deles individualmente, mesmo usando a abordagem de por exemplo pode ser muito demorada. Divisão preditiva permite que a divisão de fácil sem que o usuário que precisam fornecer exemplos para cada uma das colunas. 

## <a name="how-to-perform-this-transformation"></a>Como realizar esta transformação

1. Selecione a coluna que pretende dividir. 
2. Selecione **dividir coluna por exemplo** partir do **transforma** menu. Ou, no cabeçalho da coluna selecionada com o botão direito e selecione **dividir coluna por exemplo** no menu de contexto. O Editor de transformar é aberto e novas colunas são adicionadas ao lado da coluna selecionada. Neste momento, o Workbench analisa a coluna de entrada, determina os limites de divisão e sintetiza um programa para dividir a coluna, tal como apresentado na grade. O programa sintetizado é executado em relação a todas as linhas na coluna. Delimitadores, se houver, não inclui o resultado final.
3. Pode clicar no **modo avançado** para obter mais controle sobre a transformação de divisão. 
4. Reveja o resultado e clique em **OK** para aceitar a transformação. 

A transformação tem como objetivo para produzir o mesmo número de colunas resultantes para todas as linhas. Se qualquer linha não pode ser dividida nos limites de determinado, ele produz *nulo* para todas as colunas por predefinição. Esse comportamento pode ser alterado no **modo avançado**.

### <a name="transform-editor-advanced-mode"></a>Transformar editor: modo avançado
**Modo avançado,** fornece uma experiência mais rica para dividir colunas. 

Selecionando **manter o delimitador de colunas** inclui os delimitadores no resultado final. Delimitadores são excluídos por padrão.

Especificar **delimitadores** substitui a lógica de seleção automática delimitador. Vários delimitadores, um em cada linha, podem ser especificados como **delimitadores**. Todos esses caracteres são usados como delimitadores para dividir a coluna.

Às vezes, a divisão de um valor em limites de determinado produz um número diferente de colunas do que a maioria dos outros. Nesses casos, **preencher direção** é utilizado para decidir a ordem na qual as colunas devem ser preenchidas.

Clicar em **Mostrar sugestões de exemplos** apresenta as linhas representativas para os quais o usuário devem fornecer um exemplo de divisão. Usuário pode clicar no **cópia** seta à direita da linha sugerida para promover a linha como exemplo. 

O utilizador pode **eliminar coluna** ou **inserir novas colunas** clicando no cabeçalho dos **tabela de exemplos**.

Utilizador pode copiar e colar valores de uma célula para outro a fim de fornecer um exemplo de divisão.

Usuário pode alternar entre o **modo básico** e o **modo avançado** clicando nas hiperligações no Editor de transformação.

### <a name="transform-editor-send-feedback"></a>Transformar editor: enviar comentários

Clicar no **enviar comentários** abre-se de vincular a **comentários** forneceu a caixa de diálogo com a caixa de comentários pré-preenchidos com as seleções de parâmetro e o utilizador de exemplos. Utilizador deve rever o conteúdo da caixa de comentários e fornecer mais detalhes para ajudar-na compreender o problema. Se o utilizador não quer partilhar dados com a Microsoft, o utilizador deve eliminar os dados de exemplo pré-preenchida antes de clicar o **enviar Feedback** botão. 


### <a name="editing-an-existing-transformation"></a>Editar uma transformação existente

Um utilizador pode editar uma existente **dividir coluna por exemplo** transformar selecionando **editar** opção do passo de transformação. Clicar em **edite** é aberto o Editor de transformação de mensagens em fila no **modo avançado**, e são apresentados todos os exemplos que foram fornecidos durante a criação da transformação.

## <a name="examples-of-splitting-on-a-fixed-single-character-delimiter"></a>Exemplos de divisão de um delimitador fixo, caráter

É comum para campos de dados ser separado por um único delimitador fixo como uma vírgula num formato CSV. A transformação de divisão tenta inferir essas delimitadores automaticamente. Por exemplo, no cenário seguinte, irá inferir automaticamente o "." como um delimitador.

### <a name="splitting-ip-addresses"></a>Endereços IP de divisão

Os valores na primeira coluna predictively são divididos em quatro colunas.

|IP|IP_1|IP_2|IP_3|IP_4|
|:-----|:-----|:-----|:-----|:-----|
|192.168.0.102|192|168|0|102|
|192.138.0.101|192|138|0|101|
|192.168.0.102|192|168|0|102|
|192.158.1.202|192|158|1|202|
|192.168.0.102|192|168|0|102|
|192.169.1.102|192|169|1|102|

## <a name="examples-of-splitting-on-multiple-delimiters-within-particular-contexts"></a>Exemplos de divisão em vários delimitadores dentro de determinado contextos

Os dados do utilizador podem incluir muitos delimitadores diferentes separar campos diferentes. Além disso, apenas alguns ocorrências de uma cadeia de caracteres de delimitação podem ser um delimitador, mas não todas. Por exemplo, no caso seguinte o conjunto de delimitadores necessária é "-",","e":" para produzir a saída desejada. No entanto, não cada ocorrência da ":" deve ser um ponto de divisão, uma vez que não queremos dividir o tempo, mas mantenha-a numa única coluna. A transformação de divisão infere delimitadores dentro os contextos em que os dados de entrada em vez de qualquer possível ocorrência do delimitador que ocorrem. A transformação também está ciente dos tipos de dados comuns, como datas e horas.   

### <a name="splitting-store-opening-timings"></a>A divisão de tempos de abertura de arquivo

Os valores a seguir *temporizações* coluna predictively obter dividida em nove colunas mostradas na tabela abaixo do mesmo.

|Temporizações|
|:-----|
|Segunda - sexta-feira: 7 da manhã - das 18:00, Sábado: 9:00 - as 17:00, Domingo: fechados|
|Segunda - sexta-feira: 9:00 - das 18:00, Sábado: 4:00 - as 16h00, Domingo: fechados|
|Segunda - sexta-feira: 8 às 9:30h-7 das 12:00:00, Sábado:: 3:00 - 2:30 pm, Domingo: fechados|
|Segunda - sexta-feira: 8:00 - das 18:00, Sábado: 2:00 - às 15:00, Domingo: fechados|
|Segunda - sexta-feira: 4:00 - 7 das 12:00:00, Sábado: 9:00 - as 16h00, Domingo: fechados|
|Segunda - sexta-feira: 8 às 9:30h-4:30 pm, Sábado: 9:00 - as 17:00, Domingo: fechados|
|Segunda - sexta-feira: 5 às 9:30h -: 30 18:00, de Sábado: 5:00 - as 16h00, Domingo: fechados|
|Segunda - sexta-feira: 8 às 9:30h-8 17:30, Sábado: 6:00 - as 17:00, Domingo: fechados|
|Segunda - sexta-feira: 8:00 - 21 horas, Sábado: 9:00 - as 20:00, Domingo: fechados|
|Segunda - sexta-feira: 10:00 - 9 17:30, Sábado: 9h30 - às 15:00, Domingo: fechados|

|Timings_1|Timings_2|Timings_3|Timings_4|Timings_5|Timings_6|Timings_7|Timings_8|Timings_9|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Segunda-feira|Sexta-feira|7 da manhã|das 18:00|Sábado|09:00|as 17:00|Domingo|Fechado|
|Segunda-feira|Sexta-feira|09:00|das 18:00|Sábado|4:00|que|Domingo|Fechado|
|Segunda-feira|Sexta-feira|8 às 9:30h|7 21 horas|Sábado|: 3:00|2 17:30|Domingo|Fechado|
|Segunda-feira|Sexta-feira|feira|das 18:00|Sábado|2:00|às 15:00|Domingo|Fechado|
|Segunda-feira|Sexta-feira|4:00|7 21 horas|Sábado|09:00|que|Domingo|Fechado|
|Segunda-feira|Sexta-feira|8 às 9:30h|4:30 pm|Sábado|09:00|as 17:00|Domingo|Fechado|
|Segunda-feira|Sexta-feira|5:30 am|6 17:30|Sábado|5:00|que|Domingo|Fechado|
|Segunda-feira|Sexta-feira|8 às 9:30h|8 17:30|Sábado|6:00|as 17:00|Domingo|Fechado|
|Segunda-feira|Sexta-feira|feira|21 horas|Sábado|09:00|as 20:00|Domingo|Fechado|
|Segunda-feira|Sexta-feira|10:00|9 17:30|Sábado|9h30|às 15:00|Domingo|Fechado|

### <a name="splitting-iis-log"></a>Divisor log do IIS

Aqui está outro exemplo de vários delimitadores arbitrários. Este exemplo também inclui um delimitador contextual "/", que não têm de ser dividido dentro os URLs ou caminhos de ficheiros. É maçante para realizar essa divisão utilizando muitas *derivar coluna por exemplo* transformações e dar exemplos para cada campo. Usando a transformação de divisão podem realizar a divisão preditiva sem dar qualquer exemplos.

|logtext|
|:-----|
|192.128.138.20 – [Oct/16/2016 16:22:33-0200] "GET /images/picture.gif http/1.1" 234 343 www.yahoo.com "http://www.example.com/" "Mozilla/4.0 (compatível; MSIE 4)""-"|
|10.128.72.213 – [17/Oct/2016 12:43:12 +0300] "GET /news/stuff.html HTTP/1.1" 200 6233 www.aol.com "http://www.sample.com/" "(MSIE) Mozilla/5.0" "-"|
|192.165.71.165 – [12/Nov/2016 14:22:44-0500] "GET /sample.ico HTTP/1.1" 342 7342 www.facebook.com "-" "Mozilla/5.0 (Windows; U; Windows NT 5.1; Rv:1.7.3) ""-"|
|10.166.64.165 – [23/Nov/2016 01:52:45-0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "(Windows) Mozilla/5.0" "-"|
|192.167.1.193 – [Jan/16/2017 22:34:56 + 0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com "http://www.illustration.com/index.html" "(Windows) Mozilla/5.0" "-"|
|192.147.76.193 – [Jan/28/2017 26:36:16 +0800] "GET /search.php HTTP/1.1" 400 1777 www.bing.com "-" "Mozilla/4.0 (compatível; MSIE 6.0; Windows NT 5.1)""-"|
|192.166.64.165 – [23/Mar/2017 01:55:25-0800] "GET /style.css HTTP/1.1" 200 2552 www.google.com "http://www.test.com/index.html" "(Windows) Mozilla/5.0" "-"|
|11.167.1.193 – [16/Abril/2017 34: 11:36 + 0200] "GET /js/ads.js HTTP/1.1" 200 23462 www.microsoft.com "http://www.illustration.com/index.html" "(Windows) Mozilla/5.0" "-"|

Obtém divididas em:

|logtext_1|logtext_2|logtext_3|logtext_4|logtext_5|logtext_6|logtext_7|logtext_8|logtext_9|logtext_10|logtext_11|logtext_12|logtext_13|logtext_14|logtext_15|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|192.128.138.20|16/Oct/2016|16:22:33|-0200|GET|Images/Picture.gif|HTTP|1.1|234|343|www.yahoo.com|http://www.example.com/|Mozilla|4.0|compatível; MSIE 4|
|10.128.72.213|17/Oct/2016|12:43:12|+0300|GET|news/stuff.html|HTTP|1.1|200|6233|www.aol.com|http://www.sample.com/|Mozilla|5.0|MSIE|
|192.165.71.165|12/Nov/2016|14:22:44|-0500|GET|sample.ico|HTTP|1.1|342|7342|www.facebook.com|-|Mozilla|5.0|Windows; U; Windows NT 5.1; Rv:1.7.3|
|10.166.64.165|Novembro/23/2016|01:52:45|-0800|GET|style. css|HTTP|1.1|200|2552|www.google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|192.167.1.193|Jan/16/2017|22:34:56|+0200|GET|js/ADs.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|
|192.147.76.193|Jan/28/2017|26:36:16|+0800|GET|Search.php|HTTP|1.1|400|1777|www.bing.com|-|Mozilla|4.0|compatível; MSIE 6.0; Windows NT 5.1|
|192.166.64.165|Mar/23/2017|01:55:25|-0800|GET|style. css|HTTP|1.1|200|2552|www.google.com|http://www.test.com/index.html|Mozilla|5.0|Windows|
|11.167.1.193|Abril/16/2017|11:34:36|+0200|GET|js/ADs.js|HTTP|1.1|200|23462|www.microsoft.com|http://www.illustration.com/index.html|Mozilla|5.0|Windows|

## <a name="examples-of-splitting-without-delimiters"></a>Exemplos de divisão sem delimitadores
Em alguns casos, não há nenhum delimitadores reais e campos de dados podem ocorrer de forma contígua próximos uns dos outros. Neste caso, a transformação de divisão Deteta automaticamente os padrões nos dados provavelmente inferir pontos de divisão. Por exemplo, no cenário seguinte, queremos separar a quantidade do tipo de moeda e divisão automaticamente infere os limites entre os dados numéricos e não numéricos como o ponto de divisão.

### <a name="splitting-amount-with-currency-symbol"></a>A divisão de quantidade com o símbolo de moeda

|Montante|Amount_1|Amount_2|
|:-----|:-----|:-----|
|\$14|$|14|
|£9|£|9|
|\$34|$|34|
|€ 18|€ |18|
|\$42|$|42|
|\$7|$|7|
|£42|£|42|
|\$16|$|16|
|€ 16|€ |16|
|\$15|$|15|
|\$16|$|16|
|€ 64|€ |64|

No exemplo a seguir, gostamos de separar os valores de peso de unidades de medida. Mais uma vez a inferência de tipos de divisão Deteta automaticamente os limites do significativo e prefere-lo através de outros delimitadores possíveis, como o "." caráter. 

### <a name="splitting-weights-with-units"></a>A divisão de pesos com unidades

|Peso|Weight_1|Weight_2|
|:-----|:-----|:-----|
|2.27KG|2.27|KG|
|1L|1|L|
|2.5 KG|2.5|KG|
|2KG|2|KG|
|1.7KGA|1.7|KGA|
|3KG|3|KG|
|2KG|2|KG|
|125G|125|G|
|500G|500|G|
|1.5KGA|1.5|KGA|

## <a name="technical-notes"></a>Notas técnicas

A funcionalidade de transformação de divisão se baseia a **preditiva Program Synthesis** técnica. Nesta técnica, programas de transformação de dados adquiridos automaticamente com base nos dados de entrada. Os programas são sintetizados numa linguagem específica do domínio. O DSL baseia-se a delimitadores e os campos que ocorrem em particular contextos de expressão regular. Obter mais informações sobre esta tecnologia podem ser encontradas num [publicação recente sobre esse tópico](https://www.microsoft.com/research/publication/automated-data-extraction-using-predictive-program-synthesis/). 
