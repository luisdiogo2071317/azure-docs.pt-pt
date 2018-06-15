---
title: As variáveis de HTTP para o motor de regras do Azure CDN | Microsoft Docs
description: Variáveis HTTP permitem-lhe obter metadados de pedido e resposta HTTP.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: v-deasim
ms.openlocfilehash: 36c1b20219fabd1b7c02247d9a93bb7b7cfc898d
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "34011463"
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>Variáveis de HTTP para o motor de regras da CDN do Azure
Variáveis HTTP fornecem os meios através do qual pode obter metadados de pedido e resposta HTTP. Estes metadados podem ser utilizados para alterar dinamicamente um pedido ou uma resposta. A utilização de variáveis HTTP é restringida às seguintes funcionalidades do motor de regras:

- [Chave da cache reescrever](cdn-rules-engine-reference-features.md#cache-key-rewrite)
- [Modificar o cabeçalho de pedido de cliente](cdn-rules-engine-reference-features.md#modify-client-request-header)
- [Modificar o cabeçalho de resposta do cliente](cdn-rules-engine-reference-features.md#modify-client-response-header)
- [URL de redirecionamento](cdn-rules-engine-reference-features.md#url-redirect)
- [URL reescrever](cdn-rules-engine-reference-features.md#url-rewrite)

## <a name="definitions"></a>Definições
A tabela seguinte descreve as variáveis HTTP suportadas. Um valor em branco é devolvido quando a Georreplicação metadados (por exemplo, código postal) não estão disponível para um pedido específico.


| Nome | Variável | Descrição | Valor da amostra |
| ---- | -------- | ----------- | ------------ |
| ASN (autor de pedido) | % {geo_asnum} | Indica o autor do pedido como número. <br /><br />**Preteridos:** % {virt_dst_asnum}. <br />Esta variável foi preterida favor % {geo_asnum}. Embora uma regra que utiliza esta variável preterido continuarão a funcionar, deve ser atualizado para utilizar a variável nova. | AS15133 |
| Cidade (autor de pedido) | % {geo_city} | Indica a cidade do autor do pedido. | Los Angeles |
| Continente (autor de pedido) | % {geo_continent} | Indica o continente do autor do pedido através do respetiva abreviatura. <br />Os valores válidos são: <br />AF: África<br />COMO: Ásia<br />EU: Europa<br />NO: América do Norte<br />OC: Oceania<br />SA: América do Sul<br /><br />**Preteridos:** % {virt_dst_continent}. <ber />Esta variável foi preterida favor % {geo_continent}. <br />Embora uma regra que utiliza esta variável preterido continuarão a funcionar, deve ser atualizado para utilizar a variável nova.| N/A |
| Valor do cookie | % {cookie_Cookie} | Devolve o valor correspondente para a chave de cookie identificada pelo termo de Cookie. | Utilização de exemplo: <br />% {cookie__utma}<br /><br />Valor de exemplo:<br />111662281.2.10.1222100123 |
| País (autor de pedido) | % {geo_country} | Indica o país do autor do pedido de origem através do respetivo código de país. <br />**Preteridos:** % {virt_dst_country}. <br /><br />Esta variável foi preterida favor % {geo_country}. Embora uma regra que utiliza esta variável preterido continuarão a funcionar, deve ser atualizado para utilizar a variável nova. | EUA |
| Área de mercado designado (autor de pedido) | % {geo_dma_code} |Indica o mercado de suporte de dados do autor do pedido pelo respetivo código de região. <br /><br />Este campo só é aplicável a pedidos provenientes dos Estados Unidos.| 745 |
| Método de pedido de HTTP | % {request_method} | Indica o método de pedido HTTP. | GET |
| Código de estado de HTTP | % {status} | Indica o código de estado HTTP para a resposta. | 200 |
| Endereço IP (autor de pedido) | % {virt_dst_addr} | Indica o endereço IP do autor do pedido. | 192.168.1.1 |
| Latitude (autor de pedido) | % {geo_latitude} | Indica a latitude do autor do pedido. | 34.0995 |
| Longitude (autor de pedido) | % {geo_longitude} | Indica a longitude do autor do pedido. | -118.4143 |
| Análises da área metropolitana (autor de pedido) | % {geo_metro_code} | Indica a área metropolitana do autor do pedido. <br /><br />Este campo só é aplicável a pedidos provenientes dos Estados Unidos.<br />| 745 |
| Porta (autor de pedido) | % {virt_dst_port} | Indica as portas efémeras do autor do pedido. | 55885 |
| Código postal (autor de pedido) | % {geo_postal_code} | Indica o código postal do autor do pedido. | 90210 |
| Cadeia de consulta encontrada | % {is_args} | O valor desta variável varia de acordo com se o pedido contém uma cadeia de consulta.<br /><br />-Encontrada de cadeia de consulta:?<br />-Nenhuma cadeia de consulta: nulo | ? |
| Foi encontrado um parâmetro de cadeia de consulta | % {is_amp} | O valor desta variável varia de acordo com se o pedido contém pelo menos um parâmetro de cadeia de consulta.<br /><br />-Parâmetro encontrado: &<br />-Não existem parâmetros: nulo | & |
| Valor de parâmetro de cadeia de consulta | % {arg_&lt;parâmetro&gt;} | Devolve o valor correspondente para o parâmetro de cadeia de consulta identificado pelo &lt;parâmetro&gt; termo. | Utilização de exemplo: <br />% {arg_language}<br /><br />Parâmetro de cadeia de consulta de exemplo: <br />? idioma = pt<br /><br />Valor de exemplo: en |
| Valor de cadeia de consulta | % {query_string} | Indica o valor de cadeia de consulta todo definido no URL do pedido. |key1 = val1 & key2 = val2 & key3 = val3 |
| Domínio de referência | % {referring_domain} | Indica o domínio definido no cabeçalho do pedido de referenciador. | www.google.com |
| Região (autor de pedido) | % {geo_region} | Indica a região do autor do pedido (por exemplo, distrito) através do respetiva abreviatura alfanumérica. | AC |
| Valor de cabeçalho de pedido | % {http_RequestHeader} | Devolve o valor correspondente para o cabeçalho do pedido identificado pelo termo RequestHeader. <br /><br />Se o nome do cabeçalho do pedido contém um traço (por exemplo,-agente de utilizador), substitua-o por um caráter de sublinhado (por exemplo, User_Agent).| Utilização de exemplo: % {http_Connection}<br /><br />Valor de exemplo: Ligação Keep-Alive | 
| Anfitrião de pedido | % {anfitrião} | Indica o anfitrião definido no URL do pedido. | www.mydomain.com |
| Protocolo de pedido | % {request_protocol} | Indica o protocolo de pedido. | HTTP/1.1 |
| Esquema de pedido | % {esquema} | Indica o esquema de pedido. |http |
| Pedido de URI (caminho relativo) | % {request_uri} | Indica o caminho relativo, incluindo a cadeia de consulta, definida no URI do pedido. | /marketing/foo.js?loggedin=TRUE |
| Pedido de URI (relativo sem cadeia de consulta) | % {uri} | Indica o caminho relativo para o conteúdo pedido. <br /><br/>Informações da chave:<br />-Este caminho relativo exclui a cadeia de consulta.<br />-Este caminho relativo reflete reescritas de URL. Um URL irá ser foi reescrito sob as seguintes condições:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-Funcionalidade de reescrever URL: Esta funcionalidade reescreve o caminho relativo definido no URI do pedido.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;-URL de CNAME limite: Este tipo de pedido é rescrito para o URL de CDN correspondente. |/800001/corigin/rewrittendir/foo.js |
| URI de pedido | % {pedido} | Descreve o pedido. <br />Sintaxe: &lt;método HTTP&gt; &lt;caminho relativo&gt; &lt;protocolo HTTP&gt; | OBTER /marketing/foo.js?loggedin=true HTTP/1.1 |
| Valor de cabeçalho de resposta | % {resp_&lt;ResponseHeader&gt;} | Devolve o valor correspondente para o cabeçalho de resposta identificado pelo &lt;ResponseHeader&gt; termo. <br /><br />Se o nome do cabeçalho de resposta contiver um traço (por exemplo,-agente de utilizador), substitua-o por um caráter de sublinhado (por exemplo, User_Agent). | Utilização de exemplo: % {resp_Content_Length}<br /><br />Valor de exemplo: 100 |

## <a name="usage"></a>Utilização
A tabela seguinte descreve a sintaxe adequada para especificar uma variável HTTP.


| Sintaxe | Exemplo | Descrição |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | % {anfitrião} | Utilize esta sintaxe para obter o valor inteiro correspondente ao especificado &lt;HTTPVariable&gt;. |
| %{&lt;HTTPVariableDelimiter&gt;} | % {anfitrião} | Utilize esta sintaxe para definir as maiúsculas e minúsculas para o valor inteiro correspondente ao especificado &lt;HTTPVariableDelimiter&gt;. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{Host/=^www\.([^\.] +)\.([^\.:] +) /cdn.$2.$3:80} | Utilize uma expressão regular para &lt;HTTPVariableDelimiterExpression&gt; para substituir, eliminar ou manipular valor de uma variável HTTP. |

Os nomes de variáveis de HTTP só suportam carateres alfabéticos e carateres de sublinhado. Converta carateres não suportados carateres de sublinhado.

## <a name="delimiter-reference"></a>Referência do delimitador
Pode ser especificado um delimitador depois de uma variável HTTP para executar um dos seguintes efeitos:

- O valor associado a variável de transformação.

     Exemplo: Converter o valor inteiro em minúsculas.

- Elimine o valor associado a variável.

- Manipular o valor associado a variável.

     Exemplo: Utilizar expressões regulares para alterar o valor associado a variável HTTP.

Os delimitadores são descritos na seguinte tabela.

| Delimitador | Descrição |
| --------- | ----------- |
| := | Indica que um valor predefinido será atribuído à variável, quando está: <br />-Em falta <br />-Definido como nulo. |
| :+ | Indica que um valor predefinido será atribuído à variável, quando um valor foi atribuído ao mesmo. |
| : | Indica que uma subcadeia do valor atribuído à variável será expandida. |
| # | Indica se o padrão especificado após este delimitador deve ser eliminado quando estiver no início do valor associado a variável. |
| % | Indica se o padrão especificado após este delimitador deve ser eliminado quando encontra-se no final do valor associado a variável. <br />Esta definição só é aplicável quando o símbolo de % é utilizado como um delimitador. |
| / | Delimits uma variável HTTP ou um padrão. |
| // | Localize e substitua todas as instâncias de padrão especificado. |
| /= | Localizar, copiar e reescrever todas as ocorrências do padrão especificado. |
| , | Converta o valor associado a variável HTTP para minúsculas. |
| ^ | Converta o valor associado a variável HTTP em maiúsculas. |
| ,, | Converta todas as instâncias do caráter especificado no valor associado a variável HTTP para minúsculas. |
| ^^ | Converta todas as instâncias do caráter especificado no valor associado a variável HTTP em maiúsculas. |

## <a name="exceptions"></a>Exceções
A tabela seguinte descreve as circunstâncias em que o texto especificado não é tratado como uma variável HTTP.

| Condição | Descrição | Exemplo |
| --------- | ----------- | --------|
| Símbolo de % de escape | O símbolo de percentagem pode escape através da utilização de uma barra invertida. <br />O valor de exemplo para a direita será tratado como um valor literal e não como uma variável HTTP.| \%{anfitrião} |
| Variáveis desconhecidas | Uma cadeia vazia sempre é devolvida para variáveis desconhecidas. | % {unknownvariable} |
| Sintaxe ou carateres inválidos | As variáveis que contêm carateres inválidos ou uma sintaxe são tratadas como valores de literais. <br /><br />Exemplo #1: O valor especificado contém um caráter inválido (por exemplo,-). <br /><br />Exemplo #2: O valor especificado contém um conjunto duplo de chavetas. <br /><br />Exemplo #3: O valor especificado está em falta uma chaveta de fecho.<br /> | Exemplo #1: % {resp_user-agente} <br /><br />Exemplo #2: % {{anfitrião}} <br /><br />Exemplo #3: % {anfitrião |
| Falta o nome da variável | Um valor nulo sempre é devolvido se não for especificada uma variável. | %{} |
| Carateres à direita | Carateres que o registo de uma variável são tratadas como valores de literais. <br />O valor de exemplo para a direita contém uma chaveta direita será tratada como um valor literal. | % {anfitrião}} |

## <a name="setting-default-header-values"></a>Definir valores de cabeçalho predefinido
Um valor predefinido pode ser atribuído a um cabeçalho quando cumpre qualquer uma das seguintes condições:
- Em falta /
- Definido como nulo.

A tabela seguinte descreve como definir um valor predefinido.

| Condição | Sintaxe | Exemplo | Descrição |
| --------- | ------ | --------| ----------- |
| Defina um cabeçalho para um valor predefinido quando cumpre qualquer uma das seguintes condições: <br /><br />-Cabeçalho em falta <br /><br />-O valor de cabeçalho está definido como nulo.| % {Variável: = Value} | % {http_referer: = não especificado} | O cabeçalho de Referenciador só será definido para *não especificado* quando está em falta ou foi definido como nulo. Nenhuma ação ocorrerá se esta tiver sido definida. |
| Defina um cabeçalho para um valor predefinido quando está em falta. | % {Variável = Value} | % {http_referer = não especificado} | O cabeçalho de Referenciador só será definido para *não especificado* quando este está em falta. Nenhuma ação ocorrerá se esta tiver sido definida. |
| Defina o cabeçalho para um valor predefinido quando não cumpre nenhum dos seguintes condições: <br /><br />-Em falta<br /><br /> -Definido como nulo. | % {Variável: + valor} | % {http_referer: + não especificado} | O cabeçalho de Referenciador só será definido para *não especificado* quando foi atribuído um valor para o mesmo. Nenhuma ação ocorrerá se este está em falta ou foi definido como nulo. |

## <a name="manipulating-variables"></a>Manipulação de variáveis
Variáveis podem ser manipuladas das seguintes formas:

- A expansão de subcadeias
- Remover padrões

### <a name="substring-expansion"></a>Expansão da subcadeia
Por predefinição, uma variável será expandido para o valor inteiro. Utilize a seguinte sintaxe para expandir apenas uma subcadeia de valor da variável.

`%<Variable>:<Offset>:<Length>}`

Informações da chave:

- O valor atribuído ao termo de deslocamento determina o caráter da subcadeia inicial:

     - Positivo: O caráter da subcadeia inicial é calculado a partir do primeiro caráter em cadeia.
     - Zero: O caráter da subcadeia inicial é o primeiro carácter na cadeia.
     - Negativo: O caráter da subcadeia inicial é calculado a partir do último caráter na cadeia.

- O comprimento da subcadeia é determinado pelo *comprimento* termo:

     - Omitido: Omitindo o termo de comprimento permite a subcadeia incluir todos os carateres entre o caráter inicial e o fim da cadeia.
     - Positivo: Determina o comprimento da subcadeia do caráter de partida para a direita.
     - Negativo: Determina o comprimento da subcadeia do caráter de partida para a esquerda.

#### <a name="example"></a>Exemplo:

O exemplo seguinte baseia-se no URL do pedido de exemplo seguinte:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

A cadeia seguinte demonstra vários métodos para manipular variáveis:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Com base no URL do pedido de exemplo, a manipulação de variável acima irá produzir o seguinte valor:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### <a name="pattern-removal"></a>Remoção de padrão
Texto que corresponda a um padrão específico pode ser removido do início ou fim do valor da variável.

| Sintaxe | Ação |
| ------ | ------ |
| % {Variável #Pattern} | Remova o texto quando o padrão especificado encontra-se no início do valor da variável. |
| % {Variável % padrão} | Remova o texto quando o padrão especificado encontra-se no final do valor da variável. |

#### <a name="example"></a>Exemplo:

Neste cenário de exemplo, o *request_uri* variável é definida como:

`/800001/myorigin/marketing/product.html?language=en-US`

A tabela seguinte demonstra como funciona esta sintaxe.

| Sintaxe de exemplo | Resultados |
| ------------- | ------- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/Product.HTML?Language=en-us | Porque a variável começa com o padrão, foi substituído. |
| % {request_uri % html} htm | /800001/myorigin/marketing/Product.HTML?Language=en-us | Porque a variável não termina com o padrão, não ocorreu nenhuma alteração.|

### <a name="find-and-replace"></a>Localizar e substituir
A sintaxe de localizar e substituir é descrita na seguinte tabela.

| Sintaxe | Ação |
| ------ | ------ |
| %{Variable/Find/replace} | Localizar e substituir a primeira ocorrência do padrão especificado. |
| %{Variable//Find/replace} | Localize e substitua todas as ocorrências do padrão especificado. |
| % {Variável ^} |Converter o valor inteiro em maiúsculas. |
| % {Variável ^ localizar} | Converta a primeira ocorrência do padrão especificado em maiúsculas. |
| % {Variável} | Converter o valor inteiro em minúsculas. |
| % {Variável, localizar} | Converta a primeira ocorrência do padrão especificado em minúsculas. |

### <a name="find-and-rewrite"></a>Localizar e rescreva
Para uma variação localizar e substituir, utilize o texto que corresponde ao padrão especificado quando esta conversão. A sintaxe de localizar e rescreva é descrita na seguinte tabela.

| Sintaxe | Ação |
| ------ | ------ |
| %{Variable/=Find/Rewrite} | Localizar, copiar e reescrever todas as ocorrências do padrão especificado. |
| %{Variable/^Find/Rewrite} | Localizar, copiar e reescrever o padrão especificado quando ocorrer no início da variável. |
| %{Variable/$Find/Rewrite} | Localizar, copiar e reescrever o padrão especificado quando ocorrer no final da variável. |
| %{Variable/Find} | Localizar e elimine todas as ocorrências do padrão especificado. |

Informações da chave:

- Expanda o texto que corresponde ao padrão especificado, especificando um cifrão seguido de um número inteiro todo (por exemplo, $1).

- É possível especificar vários padrões. A ordem em que é especificado o padrão determina o número inteiro que será atribuído ao mesmo. No exemplo seguinte, primeiro correspondências padrão "www"., o segundo padrão corresponder o domínio de segundo nível e o terceiro padrão corresponde ao domínio de nível superior:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- O valor de conversão pode consistir em qualquer combinação de texto e estes marcadores de posição.

    No exemplo anterior, o nome do anfitrião é rescrita para `cdn.$2.$3:80` (por exemplo, cdn.mydomain.com:80).

- O caso de um marcador de posição do padrão (por exemplo, $1) pode ser modificado através de sinalizadores seguintes:
     - U: maiúsculas o valor expandido.

         Sintaxe de exemplo:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: minúsculas o valor expandido.

         Sintaxe de exemplo:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- Tem de ser especificado um operador antes do padrão. O operador especificado determina o comportamento padrão a capturar:

     - `=`: Indica que todas as ocorrências do padrão especificado tem de ser capturadas e foi reescritas.
     - `^`: Indica que irá ser capturado apenas texto que começa com o padrão especificado.
     - `$`: Indica que apenas o texto que termina com o padrão especificado será captura.
 
- Se omitir o */reescrever* valor, o texto que corresponda ao padrão é eliminado.


