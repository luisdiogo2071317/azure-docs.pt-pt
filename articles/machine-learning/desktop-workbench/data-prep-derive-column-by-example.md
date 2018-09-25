---
title: Derivar coluna por transformação de exemplo com o Azure Machine Learning Workbench
description: O documento de referência para a transformação de "Derivar coluna por exemplo"
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
ms.openlocfilehash: 311fd39792274ef01b1b03fdf8252eb7ac93c922
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978867"
---
# <a name="derive-column-by-example-transformation"></a>Derivar coluna por transformação de exemplo

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



O **derivar coluna por exemplo** transformação permite aos utilizadores criarem um derivativo de um ou mais colunas existentes, com exemplos do fornecido pelo utilizador do resultado derivado. O derivativo pode ser qualquer combinação de cadeia de caracteres suportada, de data e transformações de número. 

Após a cadeia de caracteres, datas e número de transformações são suportadas:

**Transformações de cadeia de caracteres:** 

Incluindo extração inteligente de número e datas, valores de constantes de mapeamento de concatenação, manipulação de caso de subcadeia.

**Transformações de data:** 

Formato de data alterar, extrair partes de data, hora de mapeamento de mensagens em fila de caixas de tempo.

As transformações de data são bastante genéricas com algumas limitações importantes:
* Timezones não são suportadas.
* Alguns formatos comuns que não são suportados:
    * ISO 8601 semana do formato de ano (por exemplo "2009-W53-7") 
    * UNIX hora "Epoch".
* Todos os formatos diferenciam maiúsculas de minúsculas (as notavelmente "4 am" não é reconhecido como uma hora apesar de ser "4 AM").

**Transformações de número:** 

Arredondar, andar, invisível, Discretização, de preenchimento com zeros ou espaço, divisão ou multiplicação por uma potência de 1000.

**Transformações de composto:** 

Qualquer combinação de cadeia de caracteres, número ou transformações de data.

## <a name="how-to-use-this-transformation"></a>Como usar essa transformação

Para executar essa transformação, siga estes passos:
1. Selecione uma ou mais colunas que pretende derivar o valor de. 
2. Selecione **derivar coluna por exemplo** partir do **transforma** menu. Em alternativa, clique com o botão direito do rato no cabeçalho de qualquer uma das colunas selecionadas e selecione **derivar coluna por exemplo** no menu de contexto. O Editor de transformar é aberto e uma nova coluna é adicionada ao lado da coluna da direita mais selecionada. Colunas selecionadas podem ser identificadas pelas caixas de verificação nos cabeçalhos de coluna. Adição e remoção de colunas da seleção podem ser feitos usando as caixas de verificação nos cabeçalhos de coluna.
3. Escreva um exemplo do *saída* em relação a uma linha e prima introduza. Neste momento, o Workbench analisa a coluna de entrada, bem como a saída fornecida para sintetizar um programa que pode transformar as entradas de determinado na saída. O programa sintetizado é executado em relação a todas as linhas na grade de dados. Para casos ambíguos e complicados, talvez seja necessário vários exemplos. Dependendo se estiver no modo básico ou avançado, vários exemplos podem ser fornecidos de diferentes maneiras.
4. Reveja o resultado e clique em **OK** para aceitar a transformação.

### <a name="transform-editor-basic-mode"></a>Transformar editor: modo básico

Modo básico fornece uma linha interna experiência na grade de dados de edição. Pode fornecer exemplos de saída ao navegar para a célula de interesse e escrever o valor. 

O workbench analisa os dados e tenta identificar os casos extremos, que devem ser revistos pelo utilizador. Enquanto está a ser analisados os dados, **analisar dados** é mostrado no cabeçalho do Editor transformar. Um a análise estiver concluído, seja **sem sugestões** ou, **rever próxima linha sugerida** é apresentado no cabeçalho. Pode navegar pelos casos extremos, clicando em **rever próxima linha sugerida**. Caso o valor incorreto para uma linha, deve chave no valor correto como exemplo adicional. 

### <a name="transform-editor-advanced-mode"></a>Transformar editor: modo avançado

Modo avançado fornece uma experiência mais rica para efetuar a derivação de colunas com um exemplo. Todos os exemplos são apresentados num único local. Também pode rever todos os casos de borda num único lugar ao clicar em **Mostrar sugestões de exemplos**. 

No modo avançado, pode adicionar qualquer linha como uma linha de exemplo ao fazer duplo clique numa linha na grelha. Uma linha é copiada como uma linha de exemplo, também pode editar os dados nas colunas de origem para tornar um exemplo sintético. Ao fazer isso, pode adicionar casos que não estão atualmente presentes nos dados de exemplo.

Usuário pode alternar entre o **modo básico** e o **modo avançado** clicando nas hiperligações no Editor de transformação.

### <a name="transform-editor-send-feedback"></a>Transformar editor: enviar comentários

Clicar no **enviar comentários** abre-se de vincular a **comentários** forneceu a caixa de diálogo com a caixa de comentários pré-preenchido com o utilizador de exemplos. Utilizador deve rever o conteúdo da caixa de comentários e fornecer mais detalhes para ajudar-na compreender o problema. Se o utilizador não quer partilhar dados com a Microsoft, o utilizador deve eliminar os dados de exemplo pré-preenchida antes de clicar o **enviar Feedback** botão. 

### <a name="editing-existing-transformation"></a>Editar transformação existente

Um utilizador pode editar uma existente **derivar coluna por exemplo** transformar selecionando **editar** opção do passo de transformação. Clicar em **edite** é aberto o Editor de transformação de mensagens em fila no **modo avançado**, e são apresentados todos os exemplos que foram fornecidos durante a criação da transformação.

## <a name="examples-of-string-transformations-by-example"></a>Exemplos de transformações de cadeia de caracteres por meio de exemplos


>[!NOTE] 
>Os valores na **negrito** representam os exemplos fornecidos para concluir a transformação no conjunto de dados mostrado.


### <a name="s1-extracting-file-names-from-file-paths"></a>S1. Extrair nomes de ficheiros de caminhos de ficheiros

Número de exemplos que eram necessários para este caso: 2

|Input|Saída|
|:-----|:-----|
|C:\Python35\Tools\pynche\TypeinViewer.py|**TypeinViewer.py**|
|C:\Python35\Tools\pynche\webcolors.txt|webcolors.txt|
|C:\Python35\Tools\pynche\websafe.txt|websafe.txt|
|C:\Python35\Tools\pynche\X\rgb.txt|RGB.txt|
|C:\Python35\Tools\pynche\X\xlicense.txt|xlicense.txt|
|C:\Python35\Tools\Scripts\2to3.py|2to3.PY|
|C:\Python35\Tools\Scripts\analyze_dxp.py|**analyze_dxp.py**|
|C:\Python35\Tools\Scripts\byext.PY|byext.PY|
|C:\Python35\Tools\Scripts\byteyears.PY|byteyears.PY|
|C:\Python35\Tools\Scripts\checkappend.py|checkappend.PY|

### <a name="s2-case-manipulation-during-string-extraction"></a>S2. Manipulação de maiúsculas durante a extração de cadeia de caracteres

Número de exemplos que eram necessários para este caso: 3

|Input|Saída|
|:-----|:-----|
|REINDEER CT & final de mensagens não;  NOVO HANOVER; Estação 332; 2015-12-10 \@ 17:10:52;|**Novo Hanover**|
|CAMINHO de BRIAR & WHITEMARSH LN;  HATFIELD TOWNSHIP; Estação 345; 2015-12-10 \@ 17:29:21;|Hatfield Township|
|HAWS AVE; NORRISTOWN; 2015-12-10 \@ 14:39:21-estação: STA27;|**Norristown**|
|ST NADA & SWEDE ST;  NORRISTOWN; Estação 308A; 2015-12-10 \@ 16:47:36;|**Norristown**|
|CHERRYWOOD CT & final de mensagens não;  INFERIOR POTTSGROVE; Estação 329; 2015-12-10 \@ 16:56:52;|Pottsgrove inferior|
|CANNON AVE & W 9TH ST;  LANSDALE; Estação 345; 2015-12-10 \@ 15:39:04;|Lansdale|
|LAUREL AVE & ARDAR de OAKDALE;  HORSHAM; Estação 352; 2015-12-10 \@ 16:46:48;|Horsham|
|Área de trabalho remota COLLEGEVILLE & LYWISKI RD;  SKIPPACK; Estação 336; 2015-12-10 \@ 16:17:05;|Skippack|
|MAIN ST & antigo SUMNEYTOWN PIKE;  INFERIOR SALFORD; Estação 344; 2015-12-10 \@ 16:51:42;)|Salford inferior|
|BLUEROUTE &AMP; RAMP I476 NB PARA RD QUÍMICAS; PLYMOUTH; 2015-12-10 \@ 17:35:41;|Plymouth|
|RT202 PKWY &AMP; KNAPP RD; MONTGOMERY; 2015-12-10 \@ 17:33:50;|Montgomery|
|ÁREA DE TRABALHO REMOTA BROOK &AMP; COLWELL LN; PLYMOUTH; 2015-12-10 \@ 16:32:10;|Plymouth|

### <a name="s3-date-format-manipulation-during-string-extraction"></a>S3. Manipulação de formato de data durante a extração de cadeia de caracteres

Número de exemplos que eram necessários para este caso: 1

|Input|Saída|
|:-----|:-----|
|MONTGOMERY AVE & WOODSIDE RD;  INFERIOR MERION; Estação 313; 2015-12-11 \@ 04:11:35;|**12 de Novembro de 2015 4 AM**|
|DREYCOTT LN & W LANCASTER AVE;  INFERIOR MERION; Estação 313; 2015-12-11 \@ 01:29:52;|12 de Novembro de 2015 1 AM|
|ÁREA DE TRABALHO REMOTA E LEVERING MILL &AMP; CONSHOHOCKEN ESTADO RD; INFERIOR MERION; 2015-12-11 \@ 07:29:58;|12 de Novembro de 2015 7 AM|
|Área de trabalho remota VALLEY PENN & MANOR RD;  INFERIOR MERION; Estação 313; 2015-12-10 \@ 20:53:30;|12 de Outubro de 2015 8 PM|
|BELMONT AVE &AMP; OVERHILL RD; INFERIOR MERION; 2015-12-10 \@ 23:02:27;|12 de Outubro de 2015 23 horas|
|W MONTGOMERY AVE &AMP; PENNSWOOD RD; INFERIOR MERION; 2015-12-10 \@ 19:25:22;|12 de Outubro de 2015 7 PM|
|ROSEMONT AVE & final de mensagens não;  INFERIOR MERION; Estação 313; 2015-12-10 \@ 43: 18:07;|12 de Outubro de 2015 6 PM|
|AVIGNON DR & final de mensagens não; INFERIOR MERION; 2015-12-10 \@ 20:01:29-estação: STA24;|12 de Outubro de 2015 8 PM|

### <a name="s4-concatenating-strings"></a>S4. Concatenação de cadeias de caracteres

Número de exemplos que eram necessários para este caso: 1

>[!NOTE] 
>Neste exemplo, · caráter especial representa espaços na coluna de saída.

|Nome Próprio|Média inicial|Apelido|Saída|
|:-----|:-----|:-----|:-----|
|Laquanda||Lohmann|Laquanda··Lohmann|
|Claudio|A|Mastigar|**Claudio· A· Mastigar**|
|Sarah Joana|S|Smith|Sarah Jane· S· Smith|
|Brandi||Blumenthal|Brandi·· Blumenthal|
|Jesusita|R|Jornada|Jesusita· R· Jornada|
|Hermina||Hults|Hermina·· Hults|
|Anne Marie|W|Jones|Anne Marie· W· Jones|
|Rico||Ropp|Rico··Ropp|
|Lauren-Maio||Fullmer|Lauren May·· Fullmer|
|Marc|T|Maine|Marc· T· Maine|
|Angie||Adelman|Angie·· Adelman|
|John-Paul||Smith|John-Paul··Smith|
|Música|W|Staller|Song· W· Staller|
|Jill||Jefferies|Jill·· Jefferies|
|Tolerância de Ruby|M|Simmons|Ruby Grace· M· Simmons|

### <a name="s5-generating-initials"></a>S5. Gerar iniciais

Número de exemplos que eram necessários para este caso: 2

|Nome Completo|Saída|
|:-----|:-----|
|Laquanda Lohmann|**L.L.**|
|Claudio mastigar|C.C.|
|Sarah Joana Silva|S.S.|
|Brandi Blumenthal|B.B.|
|Percurso de Jesusita|J.J.|
|Hermina Hults|H.H.|
|Anne Marie Jones|A.J.|
|Rico Ropp|R.R.|
|Fullmer Lauren-Maio|L.F.|
|Marc Maine|M.M.|
|Angie Adelman|A.A.|
|John Paul Smith|**J.S.**|
|Música Staller|S.S.|
|Jill Jefferies|J.J.|
|Simmons de tolerância de Ruby|R.S.|


### <a name="s6-mapping-constant-values"></a>S6. Valores de constantes de mapeamento

Número de exemplos que eram necessários para este caso: 3

|Sexo administrativo|Saída|
|:-----|:-----:|
|Masculino|**0**|
|Feminino|**1**|
|Desconhecidos|**2**|
|Feminino|1|
|Feminino|1|
|Masculino|0|
|Desconhecidos|2|
|Masculino|0|
|Feminino|1|

## <a name="examples-of-number-transformations-by-example"></a>Exemplos de transformações de número, por exemplo

>[!NOTE] 
>Os valores na **negrito** representam os exemplos fornecidos para concluir a transformação no conjunto de dados mostrado.


### <a name="n1-rounding-to-nearest-10"></a>N1. Arredondamento para o 10 mais próximo

Número de exemplos que eram necessários para este caso: 1

|Input|Saída|
|-----:|-----:|
|112|**110**|
|117|120|
|11112|11110|
|11119|11120|
|548|550|

### <a name="n2-rounding-down-to-nearest-10"></a>N2. Arredondamento para mais próximo de 10

Número de exemplos que eram necessários para este caso: 2

|Input|Saída|
|-----:|-----:|
|112|**110**|
|117|**110**|
|11112|11110|
|11119|11110|
|548|540|

### <a name="n3-rounding-to-nearest-005"></a>N3. Arredondamento para 0,05 mais próximo

Número de exemplos que eram necessários para este caso: 2

|Input|Saída|
|-----:|-----:|
|-75.5812935|**-75.60**|
|-75.2646799|-75.25|
|-75.3519752|-75.35|
|-75.343513|**-75.35**|
|-75.6033497|-75.60|
|-75.283245|-75.30|

### <a name="n4-binning"></a>N4. Discretização

Número de exemplos que eram necessários para este caso: 1

|Input|Saída|
|-----:|:-----:|
|20.16|**20-25**|
|14.32|10-15|
|5.44|5-10|
|3.84|0-5|
|3.73|0-5|
|7.36|5-10|

### <a name="n5-scaling-by-1000"></a>N5. Dimensionamento de 1000

Número de exemplos que eram necessários para este caso: 1

|Input|Saída|
|-----:|-----:|
|-243|**-243000**|
|-12.5|-12500|
|-2345.23292|-2345232.92|
|-1202.3433|-1202343.3|
|1202.3433|1202343.3|

### <a name="n6-padding"></a>N6. Preenchimento

Número de exemplos que eram necessários para este caso: 1

|Código|Saída|
|-----:|-----:|
|5828|**05828**|
|44130|44130|
|49007|49007|
|29682|29682|
|4759|04759|
|10029|10029|
|7204|07204|

## <a name="examples-of-date-transformations-by-example"></a>Exemplos de transformações de data por meio de exemplos

>[!NOTE] 
>Os valores na **negrito** representam os exemplos fornecidos para concluir a transformação no conjunto de dados mostrado.


### <a name="d1-extracting-date-parts"></a>D1. Extrair partes de data

Essas partes de data foram extraídos usando transformações por exemplo diferente no mesmo conjunto de dados. Cadeias de caracteres em negrito representam os exemplos que foram fornecidos na respetiva transformação respectiva.

|DateTime|Dia da semana|Date|Mês|Ano|Hora|Minuto|Segundo|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|05:54:18 de Janeiro – 31-2031|**Sexta-feira**|**31**|**Jan**|**2031**|**5**|**54**|**18**|
|17-Janeiro de 1990 13:32:01|Qua.|17|Jan|1990|13|32|01|
|14-Fev-2034 05:36:07|Ter.|14|Fev|2034|5|36|07|
|14-Mar-2002 13:16:16|Qui.|14|Mar.|2002|13|16|16|
|21-Janeiro de 1985 05:44:43|Seg.|21|Jan|1985|5|44|**43**|
|16-Agosto de 1985 01: 11:56|Sex.|16|Ago|1985|1|11|56|
|20-Dezembro-2033 18:36:29|Ter.|20|Dez|2033|18|36|29|
|16-Jul-1984 10:21:59|Seg.|16|Jul.|1984|10|21|59|
|13-Jan-2038 10:59:36|Qua.|13|Jan|2038|10|59|36|
|14-Agosto-1982 15:13:54|Sáb.|14|Ago|1982|15|13|54|
|22-Novembro-2030 08:18:08|Sex.|22|Nov|2030|8|18|08|
|21-Outubro de 1997 08:42:58|Ter.|21|Out|1997|8|42|58|
|28-Novembro de 2006 14:19:15|Ter.|28|Nov|2006|14|19|15|
|29-Abril-2031 04:59:45|Ter.|29|Abr.|2031|4|59|45|
|29-Jan-2032 02:38:36|Qui.|29|Jan|2032|2|38|36|
|11-Maio-2028 15:31:52|Qui.|11|Maio|2028|15|31|52|
|15-Jul-1977 12:45:39|Sex.|15|Jul.|1977|12|45|39|
|27-Jan-2029 05:55:41|Sáb.|27|Jan|2029|5|55|41|
|03-Mar-2024 10:17:49|Dom.|3|Mar.|2024|10|17|49|
|14-Abril de 2010 00:23:13|Qua.|14|Abr.|2010|0|23|13|

### <a name="d2-formatting-dates"></a>D2. Formatação de datas

Estes formattings data realizadas usando transformações por exemplo diferente no mesmo conjunto de dados. Cadeias de caracteres em negrito representam os exemplos que foram fornecidos na respetiva transformação respectiva.

|DateTime|Format1|Format2|Format3|Format4|Format5|
|-----:|-----:|-----:|-----:|-----:|-----:|
|05:54:18 de Janeiro – 31-2031|**1/31/2031**|**Sexta-feira, 31 de Janeiro de 2031**|**01312031 5:54**|**31/1/2031 5 54 AM**|**2031 P1**|
|17-Janeiro de 1990 13:32:01|1/17/1990|Quarta-feira, 17 de Janeiro de 1990|01171990 13:32|17/1/1990 1:32 PM|P1 1990|
|14-Fev-2034 05:36:07|2/14/2034|Terça-feira, 14 de Fevereiro de 2034|02142034 5:36|14/2/2034 5 36 AM|2034 P1
|14-Mar-2002 13:16:16|3/14/2002|Quinta-feira, 14 de Março de 2002|03142002 13:16|14/3/2002 1 16 PM|P1 2002
|21-Janeiro de 1985 05:44:43|1/21/1985|Segunda-feira, 21 de Janeiro de 1985|01211985 5:44|21/1/1985 5 44 AM|P1 1985
|16-Agosto de 1985 01: 11:56|8/16/1985|Sexta-feira, 16 de Agosto de 1985|08161985 1:11|8/16/1985 ÀS 11H 1:|P3 1985
|20-Dezembro-2033 18:36:29|12/20/2033|Terça-feira, 20 de Dezembro de 2033|12202033 18:36|20/12/2033 6 36 PM|2033 P4
|16-Jul-1984 10:21:59|7/16/1984|Segunda-feira, 16 de Julho de 1984|07161984 10:21|7/16/1984 10:21 AM|P3 1984
|13-Jan-2038 10:59:36|1/13/2038|Quarta-feira, 13 de Janeiro de 2038|01132038 10:59|1/13/2038 10:59 AM|P1 2038
|14-Agosto-1982 15:13:54|8/14/1982|Sábado, 14 de Agosto de 1982|08141982 15:13|8/14/1982 3 13 PM|P3 1982
|22-Novembro-2030 08:18:08|11/22/2030|Sexta-feira, 22 de Novembro de 2030|11222030 8:18|11/22/2030 8 18 AM|2030 P4
|21-Outubro de 1997 08:42:58|10/21/1997|Terça-feira, 21 de Outubro de 1997|10211997 8:42|21/10/1997 8 42 AM|P4 1997
|28-Novembro de 2006 14:19:15|11/28/2006|Terça-feira, 28 de Novembro de 2006|11282006 14:19|11/28/2006 19 14:00,|2006 P4
|29-Abril-2031 04:59:45|4/29/2031|Terça-feira, 29 de Abril de 2031|04292031 4:59|4/29/2031 4:59 AM|Q2 2031
|29-Jan-2032 02:38:36|1/29/2032|Quinta-feira, 29 de Janeiro de 2032|01292032 2:38|29/1/2032 2AM: 38|2032 P1
|11-Maio-2028 15:31:52|5/11/2028|Quinta-feira, 11 de Maio de 2028|05112028 15:31|11/5/2028 3 31 PM|2028 P2
|15-Jul-1977 12:45:39|7/15/1977|Sexta-feira, 15 de Julho de 1977|07151977 12:45|15/7/1977 ÀS 17:45 12:|P3 1977
|27-Jan-2029 05:55:41|1/27/2029|Sábado, 27 de Janeiro de 2029|01272029 5:55|27/1/2029 5 55 AM|2029 P1
|03-Mar-2024 10:17:49|3/3/2024|Domingo, 3 de Março de 2024|03032024 10:17|3/3/2024 10 17 AM|2024 P1
|14-Abril de 2010 00:23:13|4/14/2010|Quarta-feira, 14 de Abril de 2010|04142010 0:23|14/4/2010 12AM: 23|Q2 2010


### <a name="d3-mapping-time-to-time-periods"></a>D3. Períodos de tempo para tempo de mapeamento

Estes Datetimes para mapeamentos de período realizadas usando transformações por exemplo diferente no mesmo conjunto de dados. Cadeias de caracteres em negrito representam os exemplos que foram fornecidos na respetiva transformação respectiva.

|DateTime|Period(seconds)|Period(minutes)|Período (duas horas)|Período (30 minutos)|
|-----:|-----:|-----:|-----:|-----:|
|05:54:18 de Janeiro – 31-2031|**0-20**|**45-60**|**5AM-7AM**|**5:30-6:00**|
|17-Janeiro de 1990 13:32:01|**0-20**|30-45|1 PM - 3 PM|13:30-14:00|
|14-Fev-2034 05:36:07|0-20|30-45|5 AM - 7 AM|5:30-6:00|
|14-Mar-2002 13:16:16|0-20|15-30|1 PM - 3 PM|13:00-13:30|
|21-Janeiro de 1985 05:44:43|40-60|30-45|5 AM - 7 AM|5:30-6:00|
|16-Agosto de 1985 01: 11:56|40-60|0-15|1AM - 3 AM|1:00-1:30|
|20-Dezembro-2033 18:36:29|20-40|30-45|5PM-7PM|18:30-19:00|
|16-Jul-1984 10:21:59|40-60|15-30|ÀS 9H - 11H|10:00-10:30|
|13-Jan-2038 10:59:36|20-40|45-60|ÀS 9H - 11H|10:30-11:00|
|14-Agosto-1982 15:13:54|40-60|0-15|3PM-5PM|15:00-15:30|
|22-Novembro-2030 08:18:08|0-20|15-30|7 AM -9H|8:00-8:30|
|21-Outubro de 1997 08:42:58|40-60|30-45|7 AM -9H|8:30-9:00|
|28-Novembro de 2006 14:19:15|0-20|15-30|1 PM - 3 PM|14:00-14:30|
|29-Abril-2031 04:59:45|40-60|45-60|AM DE 3-5 AM|4:30-5:00|
|29-Jan-2032 02:38:36|20-40|30-45|1AM - 3 AM|2:30-3:00|
|11-Maio-2028 15:31:52|40-60|30-45|3PM-5PM|15:30-16:00|
|15-Jul-1977 12:45:39|20-40|45-60|PM ÀS 11H-1|12:30-13:00|
|27-Jan-2029 05:55:41|40-60|45-60|5 AM - 7 AM|5:30-6:00|
|03-Mar-2024 10:17:49|40-60|15-30|ÀS 9H - 11H|10:00-10:30|
|14-Abril de 2010 00:23:13|0-20|15-30|23 HORAS - 1 AM|0:00-0:30|

## <a name="examples-of-composite-transformations-by-example"></a>Exemplos de transformações compostos por meio de exemplos

|tripduration|StartTime|iniciar o id de estação|começar a latitude de estação|começar a longitude de estação|UserType|Coluna|
|-----:|-----:|-----:|-----:|-----:|-----:|-----:|
|61|2016-01-08 16:09:32|107|42.3625|-71.08822|Subscritor|**Um subscritor escolhidos uma bicicleta de estação 107, lat/longa (42.363,-71.088), 08 de Janeiro de 2016, às 16:00 cerca. Duração da viagem foi 61 minutos**|
|61|2016-01-17 09:28:10|74|42.373268|-71.118579|Cliente|Um cliente escolhido uma bicicleta de uma estação de 74, lat/longa (42.373,-71.119), em 17 de Janeiro de 2016, às 9h cerca. Duração da viagem foi 61 minutos|
|62|2016-01-25 08:10:26|176|42.386748020450561|-71.119018793106079|Subscritor|Um subscritor escolhidos uma bicicleta de estação 176, lat/longa (42.387,-71.119), 25 de Janeiro de 2016, às 8:00 cerca. Duração da viagem foi 62 minutos|
|63|2016-01-08 10:10:29|107|42.3625|-71.08822|Subscritor|Um subscritor escolhidos uma bicicleta de estação 107, lat/longa (42.363,-71.088), 08 de Janeiro de 2016, em cerca de 10 AM. Duração da viagem foi 63 minutos|
|64|2016-01-15 19:42:08|68|42.36507|-71.1031|Subscritor|Um subscritor escolhidos uma bicicleta de estação 68, lat/longa (42.365,-71.103), 15 de Janeiro de 2016, em cerca de 7 PM. Duração da viagem foi 64 minutos|
|64|2016-01-22 18:16:13|115|42.387995|-71.119084|Subscritor|Um subscritor escolhidos uma bicicleta de estação 115, Considerando lat/longa (42.388,-71.119), 22 de Janeiro de 2016 para as 18:00 cerca. Duração da viagem foi 64 minutos|
|68|2016-01-18 09:51:52|178|42.359573201090441|-71.101294755935669|Subscritor|Um subscritor escolhidos uma bicicleta de estação 178, lat/longa (42.360,-71.101), 18 de Janeiro de 2016 às 9h cerca. Duração da viagem foi 68 minutos|
|69|2016-01-14 08:57:55|176|42.386748020450561|-71.119018793106079|Subscritor|Um subscritor escolhidos uma bicicleta de estação 176, lat/longa (42.387,-71.119), 14 de Janeiro de 2016, às 8:00 cerca. Duração da viagem foi 69 minutos|
|69|2016-01-13 22:12:55|141|42.363560158429884|-71.08216792345047|Subscritor|Um subscritor escolhidos uma bicicleta de estação 141, lat/longa (42.364,-71.082), em 13 de Janeiro de 2016, em cerca de 10 PM. Duração da viagem foi 69 minutos|
|69|2016-01-15 08:13:09|176|42.386748020450561|-71.119018793106079|Subscritor|Um subscritor escolhidos uma bicicleta de estação 176, lat/longa (42.387,-71.119), 15 de Janeiro de 2016, às 8:00 cerca. Duração da viagem foi 69 minutos|


## <a name="technical-notes"></a>Notas técnicas

### <a name="conditional-transformations"></a>Transformações condicionais
Em alguns casos, não é possível localizar uma transformação única que coincida com os exemplos de determinado. Nesses casos, derivar coluna por exemplo transformar tenta agrupar as entradas com base em algum padrão e Aprenda a transformação separada para cada grupo. Chamamos a isto **transformação condicional**. **Transformação condicional** é tentada apenas para transformações com uma única coluna de entrada. 

### <a name="reference"></a>Referência
Podem encontrar mais informações sobre a transformação de cadeia de caracteres através de uma tecnologia de exemplo na [esta publicação](https://www.microsoft.com/research/publication/automating-string-processing-spreadsheets-using-input-output-examples/).
