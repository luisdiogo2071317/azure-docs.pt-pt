---
title: "Corresponder as condições para o motor de regras do Azure CDN | Microsoft Docs"
description: "Condições de correspondência do motor de regras de documentação de referência para a rede de entrega de conteúdos do Azure."
services: cdn
documentationcenter: 
author: Lichard
manager: akucer
editor: 
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2017
ms.author: rli
ms.openlocfilehash: 08845355be0bfb7e7dde52d19949fee4a68ed54b
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="match-conditions-for-the-azure-cdn-rules-engine"></a>Condições de correspondência para o motor de regras da CDN do Azure
Este artigo apresenta uma lista de descrições detalhadas das condições disponíveis de correspondência de rede do Azure da entrega de conteúdos (CDN) [motor de regras](cdn-rules-engine.md).

A segunda parte de uma regra é a condição de correspondência. Uma condição de correspondência identifica tipos específicos de pedidos para o qual será efetuado um conjunto de funcionalidades.

Por exemplo, pode utilizar uma condição de correspondência para:
- Filtrar pedidos para o conteúdo numa localização específica.
- Filtrar pedidos gerados a partir de um determinado endereço IP ou o país.
- Filtrar pedidos por informações de cabeçalho.

## <a name="always-match-condition"></a>Sempre correspondem à condição

A condição de correspondência de sempre aplica-se um conjunto predefinido de funcionalidades para todos os pedidos.

Nome | Objetivo
-----|--------
[Sempre](#always) | Aplica-se de um conjunto predefinido de funcionalidades para todos os pedidos.

## <a name="device-match-condition"></a>Condição de correspondência de dispositivo

A condição de correspondência de dispositivo identifica os pedidos efetuados a partir de um dispositivo móvel com base nas respetivas propriedades.  

Nome | Objetivo
-----|--------
[Dispositivo](#device) | Identifica os pedidos efetuados a partir de um dispositivo móvel com base nas respetivas propriedades.

## <a name="location-match-conditions"></a>Condições de correspondência de localização

As condições de correspondência de localização identificam pedidos com base na localização do autor do pedido.

Nome | Objetivo
-----|--------
[COMO número](#as-number) | Identifica pedidos provenientes de uma rede específico.
[País](#country) | Identifica pedidos provenientes de países/regiões especificados.

## <a name="origin-match-conditions"></a>Condições de correspondência de origem

As condições de correspondência de origem identificam pedidos que apontam para o armazenamento de rede de entrega de conteúdos ou um servidor de origem do cliente.

Nome | Objetivo
-----|--------
[Origem da CDN](#cdn-origin) | Identifica os pedidos de conteúdo armazenado no armazenamento de rede de entrega de conteúdo.
[Origem de cliente](#customer-origin) | Identifica os pedidos de conteúdo armazenado num servidor de origem do cliente específico.

## <a name="request-match-conditions"></a>Condições de correspondência de pedido

As condições de correspondência de pedido identificam pedidos com base nas respetivas propriedades.

Nome | Objetivo
-----|--------
[Endereço IP do cliente](#client-ip-address) | Identifica pedidos provenientes de um endereço IP específico.
[Parâmetro de cookie](#cookie-parameter) | Verifica os cookies associados a cada pedido para o valor especificado.
[Cookie parâmetro Regex](#cookie-parameter-regex) | Verifica os cookies associados a cada pedido para a expressão regular especificada.
[Limite Cname](#edge-cname) | Identifica os pedidos que apontam para um limite específico CNAME.
[Referência de domínio](#referring-domain) | Identifica os pedidos que foram consultados os nomes de anfitrião especificado.
[Literal de cabeçalho de pedido](#request-header-literal) | Identifica os pedidos que contêm o cabeçalho especificado definido como um valor especificado.
[Regex de cabeçalho de pedido](#request-header-regex) | Identifica os pedidos que contêm o cabeçalho especificado definido como um valor que corresponde à expressão regular especificada.
[Caráter universal de cabeçalho de pedido](#request-header-wildcard) | Identifica os pedidos que contêm o cabeçalho especificado definido como um valor que corresponde ao padrão especificado.
[Método de pedido](#request-method) | Identifica pedidos pelo respetivo método HTTP.
[Esquema de pedido](#request-scheme) | Identifica pedidos pelo respetivo protocolo HTTP.

## <a name="url-match-conditions"></a>Condições de correspondência de URL

As condições de correspondência de URL identificam pedidos com base no respetivos URLs.

Nome | Objetivo
-----|--------
[Diretório de caminho de URL](#url-path-directory) | Identifica pedidos pelo respetivo caminho relativo.
[Extensão de caminho de URL](#url-path-extension) | Identifica pedidos pela respetiva extensão de nome de ficheiro.
[Nome de ficheiro de caminho de URL](#url-path-filename) | Identifica pedidos pelo respetivo nome de ficheiro.
[Literal de caminho de URL](#url-path-literal) | Compara o caminho relativo de um pedido para o valor especificado.
[Regex de caminho de URL](#url-path-regex) | Compara o caminho relativo de um pedido para a expressão regular especificada.
[Caminho de URL com carateres universais](#url-path-wildcard) | Compara o caminho relativo de um pedido para o padrão especificado.
[Literal de consulta de URL](#url-query-literal) | Compara a cadeia de consulta de um pedido para o valor especificado.
[Parâmetro de consulta de URL](#url-query-parameter) | Identifica os pedidos que contêm o parâmetro de cadeia de consulta especificada definido como um valor que corresponde ao padrão especificado.
[Regex de consulta de URL](#url-query-regex) | Identifica os pedidos que contêm o parâmetro de cadeia de consulta especificada definido como um valor que corresponde a uma expressão regular especificada.
[Consulta de URL com carateres universais](#url-query-wildcard) | Compara o valor especificado em relação a cadeia de consulta do pedido.


## <a name="reference-for-rules-engine-match-conditions"></a>Referência para condições de correspondência do motor de regras

---
### <a name="always"></a>Sempre

A condição de correspondência de sempre aplica-se um conjunto predefinido de funcionalidades para todos os pedidos.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="as-number"></a>COMO número 
A rede de número AS é definida pelo respetivo número de sistema autónomo (ASN). 

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o número AS correspondem à condição é cumprida:
- **Corresponde a**: requer que o ASN da rede de cliente corresponde a um dos ASNs especificados. 
- **Não corresponde ao**: requer que o ASN da rede de cliente não corresponde a qualquer um dos ASNs especificados.

Informações da chave:
- Especifique vários ASNs por delimiting cada um com um único espaço. Por exemplo, 64514 64515 corresponde pedidos que chegam 64514 ou 64515.
- Determinados pedidos podem não devolver um ASN válido. Um ponto de interrogação (?) corresponderá pedidos para o qual não foi possível determinar um ASN válido.
- Especificar o ASN completo para a rede pretendido. Não serão possível corresponder valores parciais.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cdn-origin"></a>Origem da CDN
A condição de correspondência de origem da CDN é preenchida quando ambas as condições seguintes forem cumpridas:
- Foi pedido ao conteúdo do armazenamento da CDN.
- O URI do pedido utiliza o tipo de conteúdo de ponto de acesso (por exemplo, /000001) que está definido nesta condição de correspondência:
  - URL de CDN: O URI do pedido tem de conter o ponto de acesso ao conteúdo selecionado.
  - URL de CNAME Edge: A configuração de CNAME correspondente do contorno tem de apontar para o ponto de acesso ao conteúdo selecionado.
  
Informações da chave:
 - O ponto de acesso ao conteúdo identifica o serviço que deverá servir os conteúdos solicitados.
 - Não utilize uma instrução se e combinar determinadas condições de correspondência. Por exemplo, combinar uma condição de correspondência de origem de CDN com uma condição de correspondência de origem do cliente criaria um padrão de correspondência de que nunca foi possível corresponder. Por este motivo, as duas condições de correspondência de origem da CDN não podem ser combinadas através de uma instrução e se.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="client-ip-address"></a>Endereço IP do cliente
O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o endereço de IP do cliente correspondem à condição é cumprida:
- **Corresponde a**: requer que endereço IP do cliente corresponde a um dos endereços IP especificados. 
- **Não corresponde ao**: requer que endereço IP do cliente não corresponde a nenhum dos endereços IP especificados. 

Informações da chave:
- Utilize a notação CIDR.
- Especifique vários endereços IP e/ou blocos de endereços IP por delimiting cada um com um único espaço. Por exemplo:
  - **Exemplo de IPv4**: 1.2.3.4 10.20.30.40 corresponde a quaisquer pedidos que chegam do endereço 1.2.3.4 ou 10.20.30.40.
  - **Exemplo de IPv6**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 corresponde a quaisquer pedidos que chegam a partir do endereço 1:2:3:4:5:6:7:8 ou 10:20:30:40:50:60:70:80.
- A sintaxe para um bloco de endereços IP é o endereço IP base seguido por uma barra e o tamanho de prefixo. Por exemplo:
  - **Exemplo de IPv4**: 5.5.5.64/26 corresponde a quaisquer pedidos que chegam a partir de endereços 5.5.5.64 através de 5.5.5.127.
  - **Exemplo de IPv6**: 1:2:3: / 48 corresponde a quaisquer pedidos que chegam a partir de endereços 1:2:3:0:0:0:0:0 através de 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter"></a>Parâmetro de cookie
O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o parâmetro de Cookie correspondem à condição for satisfeita.
- **Corresponde a**: necessita de um pedido para conter o cookie especificado com um valor que corresponde a, pelo menos, um dos valores definidos nesta condição de correspondência.
- **Não coincide com**: requer que o pedido cumpre qualquer um dos seguintes critérios:
  - Não contém o cookie especificado.
  - Contém o cookie especificado, mas o respetivo valor não corresponde a nenhum dos valores definidos nesta condição de correspondência.
  
Informações da chave:
- Nome do cookie: 
  - Porque os valores de caráter universal, incluindo a série de asteriscos (*), não são suportados quando a especificar um nome de cookie, apenas correspondências de nome de cookie exato são elegíveis para comparação.
  - Apenas um nome de cookie única pode ser especificado por instância desta condição de correspondência.
  - Comparações de nome de cookie são sensível.
- Valor do cookie: 
  - Especificar vários valores de cookie, delimiting cada um com um único espaço.
  - Um valor de cookie pode tirar partido da [valores de caráter universal](cdn-rules-engine-reference.md#wildcard-values). 
  - Se não tiver sido especificado um valor de caráter universal, apenas uma correspondência exata satisfazer esta condição de correspondência. Por exemplo, a especificação de "Valor" corresponderá a "Valor", mas não "Value1" ou "Value2."
  - Utilize o **ignorar caso** opção para controlar se uma comparação de maiúsculas e minúsculas é feita com um valor de cookie do pedido.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="cookie-parameter-regex"></a>Cookie parâmetro Regex
A condição de correspondência de Regex do parâmetro de Cookie define um valor e o nome do cookie. Pode utilizar [expressões regulares](cdn-rules-engine-reference.md#regular-expressions) para definir o valor do cookie pretendido. 

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o Regex de parâmetro de Cookie correspondem à condição for satisfeita.
- **Corresponde a**: necessita de um pedido para conter o cookie especificado com um valor que corresponde à expressão regular especificada.
- **Não coincide com**: requer que o pedido cumpre qualquer um dos seguintes critérios:
  - Não contém o cookie especificado.
  - Contém o cookie especificado, mas o respetivo valor não corresponde a expressão regular especificada.
  
Informações da chave:
- Nome do cookie: 
  - Porque as expressões regulares e valores de caráter universal, incluindo a série de asteriscos (*), não são suportados quando estiver a especificar um nome de cookie, apenas correspondências de nome de cookie exato são elegíveis para comparação.
  - Apenas um nome de cookie única pode ser especificado por instância desta condição de correspondência.
  - Comparações de nome de cookie são sensível.
- Valor do cookie: 
  - Um valor de cookie pode tirar partido de expressões regulares.
  - Utilize o **ignorar caso** opção para controlar se uma comparação de maiúsculas e minúsculas é feita com um valor de cookie do pedido.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

--- 
### <a name="country"></a>País
Pode especificar um país através do respetivo código de país. 

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o país/região correspondem à condição é cumprida:
- **Correspondências**: precisa do pedido para conter os valores de código de país especificado. 
- **Não coincide com**: requer que o pedido não contém os valores de código de país especificado.

Informações da chave:
- Especificar vários indicativos de país, delimiting cada um com um único espaço.
- Carateres universais não são suportados quando estiver a especificar um código de país.
- Os indicativos de país "EU" e "Da Ásia-Pacífico" não abranger todos os endereços IP nessas regiões.
- Determinados pedidos podem não devolver um código de país válido. Um ponto de interrogação (?) corresponderá pedidos para o qual não foi possível determinar um código de país válido.
- Indicativos de país diferenciam maiúsculas de minúsculas.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

#### <a name="implementing-country-filtering-by-using-the-rules-engine"></a>Implementar a filtragem de país, utilizando o motor de regras
Esta condição de correspondência permite-lhe efetuar em diversos personalizações com base na localização do que um pedido teve origem. Por exemplo, o comportamento da funcionalidade filtragem de país pode ser replicado através da configuração seguinte:

- Correspondência de carateres universais de caminho de URL: definir o [caminho de URL com carateres universais correspondem à condição](#url-path-wildcard) para o diretório que esteja protegido. 
    Acrescente um asterisco ao fim do caminho relativo para se certificar de que o acesso a todos os respectivos valores secundários irá ser restringido por esta regra.

- Correspondência de país: definir a condição de correspondência de país ao conjunto pretendido de países.
   - Permitir: Definir o país/região correspondem à condição para **não corresponde ao** para permitir o acesso de países especificado para o conteúdo armazenado na localização definida a condição de correspondência de caminho de URL com carateres universais.
   - Bloco: Definir o país/região correspondem à condição para **correspondências** para bloquear os países de aceder ao conteúdo armazenado na localização definida a condição de correspondência de carateres universais de caminho do URL especificados.

- Funcionalidade de negação de acesso (403): Ativar o [funcionalidade negar acesso (403)](cdn-rules-engine-reference-features.md#deny-access-403) para replicar a parte ou de bloqueios da funcionalidade de filtragem de país.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="customer-origin"></a>Origem de cliente

Informações da chave: 
- A condição de correspondência de origem do cliente é cumprida, independentemente se solicitados através de um URL de CDN ou um limite de URL de CNAME que aponta para a origem de cliente selecionado.
- Não é possível eliminar uma configuração de origem do cliente que é referenciada por uma regra a partir da página de origem do cliente. Antes de tentar eliminar uma configuração de origem do cliente, certifique-se de que as seguintes configurações não referencie-o:
  - Uma condição de correspondência de origem do cliente
  - Uma configuração de CNAME edge
- Não utilize uma instrução se e combinar determinadas condições de correspondência. Por exemplo, combinar uma condição de correspondência de origem do cliente com uma condição de correspondência de origem da CDN criaria um padrão de correspondência de que nunca foi possível corresponder. Por este motivo, as duas condições de correspondência de origem do cliente não podem ser combinadas através de uma instrução e se.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="device"></a>Dispositivo

A condição de correspondência de dispositivo identifica os pedidos efetuados a partir de um dispositivo móvel com base nas respetivas propriedades. Deteção de dispositivos móveis é conseguida através de [WURFL](http://wurfl.sourceforge.net/). 

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o dispositivo correspondem à condição é cumprida:
- **Correspondências**: necessita de dispositivo do autor do pedido corresponde ao valor especificado. 
- **Não corresponde ao**: requer que o dispositivo de autor do pedido não corresponde ao valor especificado.

Informações da chave:

- Utilize o **ignorar caso** opção para especificar se o valor especificado é maiúsculas e minúsculas.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

#### <a name="string-type"></a>Tipo de cadeia
Uma capacidade WURFL normalmente aceita qualquer combinação de números, letras e símbolos. Devido à natureza flexível desta capacidade, tem de escolher a forma como o valor associado esta condição de correspondência é interpretado. A tabela seguinte descreve o conjunto de opções disponível:

Tipo     | Descrição
---------|------------
Literal  | Selecione esta opção para impedir que os carateres mais colocar no significado especial, utilizando as respetivas [valor literal](cdn-rules-engine-reference.md#literal-values).
Caráter universal | Selecione esta opção para tirar partido de todas as [carateres universais] ([valores de caráter universal](cdn-rules-engine-reference.md#wildcard-values).
RegEx    | Selecione esta opção para utilizar [expressões regulares](cdn-rules-engine-reference.md#regular-expressions). As expressões regulares são úteis para definir um padrão de carateres.

#### <a name="wurfl-capabilities"></a>Capacidades WURFL
Uma capacidade WURFL refere-se a uma categoria que descreve os dispositivos móveis. A capacidade de selecionado determina o tipo de descrição de dispositivo móvel que é utilizado para identificar os pedidos.

A tabela seguinte lista as capacidades de WURFL e as respetivas variáveis para o motor de regras.
<br>
> [!NOTE] 
> São suportadas as seguintes variáveis de **modificar cabeçalho de pedido do cliente** e **modificar cabeçalho de resposta de cliente** funcionalidades.

Capacidade | Variável | Descrição | Valores de exemplo
-----------|----------|-------------|----------------
Nome de marca | % {wurfl_cap_brand_name} | Uma cadeia que indica o nome de marca do dispositivo. | Samsung
SO do dispositivo | % {wurfl_cap_device_os} | Uma cadeia que indica o sistema operativo instalado no dispositivo. | IOS
Versão do SO do Dispositivo | % {wurfl_cap_device_os_version} | Uma cadeia que indica o número de versão do sistema operativo instalado no dispositivo. | 1.0.1
Orientação dupla | % {wurfl_cap_dual_orientation} | Um valor boleano que indica se o dispositivo suporta orientação dupla. | true
HTML preferencial DTD | % {wurfl_cap_html_preferred_dtd} | Uma cadeia que indica documento preferencial definição o dispositivo móvel (DTD) para o conteúdo HTML. | nenhum<br/>xhtml_basic<br/>HTML5
Imagem Inlining | % {wurfl_cap_image_inlining} | Um valor boleano que indica se o dispositivo suporta Base64 codificado imagens. | falso
É Android | % {wurfl_vcap_is_android} | Um valor boleano que indica se o dispositivo utiliza o SO Android. | true
É IOS | % {wurfl_vcap_is_ios} | Um valor boleano que indica se o dispositivo utiliza iOS. | falso
É TV inteligente | % {wurfl_cap_is_smarttv} | Um valor boleano que indica se o dispositivo é um Televisor inteligente. | falso
É Smartphone | % {wurfl_vcap_is_smartphone} | Um valor boleano que indica se o dispositivo é um smartphone. | true
É Tablet | % {wurfl_cap_is_tablet} | Um valor boleano que indica se o dispositivo é um tablet. Esta descrição é independente de SO. | true
É dispositivo sem fios | % {wurfl_cap_is_wireless_device} | Um valor boleano que indica se o dispositivo é considerado um dispositivo sem fios. | true
Nome de marketing | % {wurfl_cap_marketing_name} | Uma cadeia que indica o nome do dispositivo de marketing. | Pearl blackBerry 8100
Browser móvel | % {wurfl_cap_mobile_browser} | Uma cadeia que indica o browser que é utilizado para pedir conteúdo a partir do dispositivo. | Chrome
Versão do Browser móveis | % {wurfl_cap_mobile_browser_version} | Uma cadeia que indica a versão do browser que é utilizado para pedir conteúdo a partir do dispositivo. | 31
Nome do modelo | % {wurfl_cap_model_name} | Uma cadeia que indica o nome do modelo do dispositivo. | S3
Transferência progressiva | % {wurfl_cap_progressive_download} | Um valor boleano que indica se o dispositivo suporta a reprodução de áudio e vídeo enquanto ainda está a ser transferido. | true
Data da versão | % {wurfl_cap_release_date} | Uma cadeia que indica o ano e mês em que o dispositivo foi adicionado à base de dados WURFL.<br/><br/>Formato:`yyyy_mm` | 2013_december
Altura de resolução | % {wurfl_cap_resolution_height} | Um número inteiro que indica a altura do dispositivo em pixels. | 768
Largura de resolução | % {wurfl_cap_resolution_width} | Um número inteiro que indica a largura do dispositivo em pixéis. | 1024

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="edge-cname"></a>Limite Cname
Informações da chave: 
- A lista de limite disponível CNAMEs está limitada a esses CNAMEs de limite que tenham sido configurados na página de CNAMEs limite para a plataforma em que está a ser configurado o motor de regras.
- Antes de tentar eliminar uma configuração de CNAME edge, certifique-se de que uma condição de correspondência de Edge Cname não referencie-o. As configurações de CNAME de limite que foram definidas numa regra não não possível eliminar a partir da página de Edge CNAMEs. 
- Não utilize uma instrução se e combinar determinadas condições de correspondência. Por exemplo, combinar uma condição de correspondência de limite Cname com uma condição de correspondência de origem do cliente criaria um padrão de correspondência de que nunca foi possível corresponder. Por este motivo, as duas condições de correspondência de limite Cname não podem ser combinadas através de uma instrução e se.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="referring-domain"></a>Referência de domínio
O nome de anfitrião associado referência através do qual o conteúdo foi pedido determina se a condição de domínio que faça referência for satisfeita. 

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o domínio que faça referência correspondem à condição é cumprida:
- **Correspondências**: requer que o nome de anfitrião referente a correspondem aos valores especificados. 
- **Não corresponde ao**: requer que o nome de anfitrião referente não corresponde ao valor especificado.

Informações da chave:
- Especificar vários nomes de anfitrião, delimiting cada um com um único espaço.
- Esta condição de correspondência suporta [valores de caráter universal](cdn-rules-engine-reference.md#wildcard-values).
- Se o valor especificado não contém um asterisco, tem de ser uma correspondência exata para nome de anfitrião de referência. Por exemplo, a especificação de "mydomain.com" não corresponde a "www.mydomain.com."
- Utilize o **ignorar caso** opção para controlar se é efetuada uma comparação de maiúsculas e minúsculas.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-literal"></a>Literal de cabeçalho de pedido
O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o pedido de cabeçalho Literal correspondem à condição for satisfeita.
- **Correspondências**: precisa do pedido para conter o cabeçalho especificado. O valor tem de corresponder ao que está definido nesta condição de correspondência.
- **Não coincide com**: requer que o pedido cumpre qualquer um dos seguintes critérios:
  - Não contém o cabeçalho especificado.
  - Contém o cabeçalho especificado, mas o respetivo valor não corresponde ao que está definido nesta condição de correspondência.
  
Informações da chave:
- Comparações de nome de cabeçalho são sempre sensível. Utilize o **ignorar caso** opção para controlar a sensibilidade de comparações de valor de cabeçalho.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---  
### <a name="request-header-regex"></a>Regex de cabeçalho de pedido
O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o Regex de cabeçalho de pedido correspondem à condição for satisfeita.
- **Correspondências**: precisa do pedido para conter o cabeçalho especificado. O valor tem de corresponder ao padrão que está definido no especificado [expressão regular](cdn-rules-engine-reference.md#regular-expressions).
- **Não coincide com**: requer que o pedido cumpre qualquer um dos seguintes critérios:
  - Não contém o cabeçalho especificado.
  - Contém o cabeçalho especificado, mas o respetivo valor não corresponde a expressão regular especificada.

Informações da chave:
- Nome do cabeçalho: 
  - Comparações de nome de cabeçalho são sensível.
  - Substitua espaços no nome de cabeçalho "% 20". 
- Valor de cabeçalho: 
  - Um valor de cabeçalho pode tirar partido de expressões regulares.
  - Utilize o **ignorar caso** opção para controlar a sensibilidade de comparações de valor de cabeçalho.
  - A condição de correspondência é cumprida apenas quando um valor de cabeçalho corresponde exatamente a, pelo menos, um de padrões de especificado.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx. 

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-header-wildcard"></a>Caráter universal de cabeçalho de pedido
O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o caráter universal cabeçalho de pedido correspondem à condição for satisfeita.
- **Correspondências**: precisa do pedido para conter o cabeçalho especificado. O valor tem de corresponder ao, pelo menos, um dos valores definidos nesta condição de correspondência.
- **Não coincide com**: requer que o pedido cumpre qualquer um dos seguintes critérios:
  - Não contém o cabeçalho especificado.
  - Contém o cabeçalho especificado, mas o respetivo valor não corresponde a nenhum dos valores especificados.
  
Informações da chave:
- Nome do cabeçalho: 
  - Comparações de nome de cabeçalho são sensível.
  - Os espaços no nome do cabeçalho devem ser substituídos com "% 20". Também pode utilizar este valor para especificar um valor de cabeçalho espaços.
- Valor de cabeçalho: 
  - Um valor de cabeçalho pode tirar partido da [valores de caráter universal](cdn-rules-engine-reference.md#wildcard-values).
  - Utilize o **ignorar caso** opção para controlar a sensibilidade de comparações de valor de cabeçalho.
  - Esta condição de correspondência é preenchida quando um valor de cabeçalho corresponde exatamente a, pelo menos, um dos padrões de especificado.
  - Especificar vários valores, delimiting cada um com um único espaço.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-method"></a>Método de pedido
A condição de correspondência do método de pedido é cumprida apenas quando são solicitados recursos através do método de pedido seleccionado. Os métodos de pedido disponíveis são:
- GET
- CABEÇALHO 
- POST 
- OPÇÕES 
- PUT 
- DELETE 
- RASTREIO 
- LIGAR 

Informações da chave:
- Por predefinição, apenas o método de pedido GET pode gerar o conteúdo em cache na rede. Todos os outros métodos de pedido são efetuados através da rede.
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="request-scheme"></a>Esquema de pedido
A condição de correspondência do esquema de pedido é cumprida apenas quando são solicitados recursos através do protocolo selecionado. Os protocolos disponíveis são: 
- HTTP
- HTTPS

Informações da chave:
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-directory"></a>Diretório de caminho de URL
Identifica um pedido pelo caminho dela relativo, que exclui o nome de ficheiro do elemento pedido.

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o diretório de caminho de URL correspondem à condição for satisfeita.
- **Corresponde a**: precisa do pedido para conter um caminho relativo de URL, excluindo o nome de ficheiro, que corresponde ao padrão do URL especificado.
- **Não corresponde ao**: precisa do pedido para conter um caminho relativo de URL, excluindo o nome de ficheiro, que não corresponde ao padrão do URL especificado.

Informações da chave:
- Utilize o **relativo ao** opção para especificar se a comparação de URL é iniciada antes ou depois do ponto de acesso ao conteúdo. O ponto de acesso ao conteúdo é a parte do caminho que aparece entre o nome do anfitrião da Verizon CDN e o caminho relativo para o elemento de pedido (por exemplo, /800001/CustomerOrigin). Identifica uma localização por tipo de servidor (por exemplo, origem CDN ou cliente) e o número de conta de cliente.

   Os valores seguintes estão disponíveis para o **relativo ao** opção:
   - **Raiz**: indica que o ponto de comparação de URL começa imediatamente após o nome de anfitrião do CDN. 

     Por exemplo: http:\//wpc.0001.&lt; domínio&gt;/**myorigin/800001/myfolder**/index.htm

   - **Origem**: indica que o ponto de comparação de URL começa depois do ponto de acesso ao conteúdo (por exemplo, myorigin /000001 ou/800001 /). Porque o \*. azureedge.net CNAME é criada relativos ao diretório de origem no nome do anfitrião da Verizon CDN por predefinição, os utilizadores da CDN do Azure devem utilizar o **origem** valor. 

     Por exemplo: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

     Este URL aponta para o nome do anfitrião da Verizon CDN seguinte: http:\//wpc.0001.&lt; Domínio&gt;/800001/myorigin/**myfolder**/index.htm

- Uma extremidade CNAME URL é rescrita para um URL de CDN antes da comparação de URL.

    Por exemplo, ambos os seguintes URLs de apontam para o mesmo elemento, pelo que tem o mesmo caminho de URL.
    - URL de CDN: http:\//wpc.0001.&lt; Domínio&gt;/800001/CustomerOrigin/path/asset.htm
    
    - URL de CNAME Edge: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Informações adicionais:
    - Domínio personalizado: https:\//my.domain.com/path/asset.htm
    
    - Caminho de URL (relativo à raiz): 800001/CustomerOrigin/caminho /
    
    - Caminho de URL (relativo à origem): /path/

- A parte do URL que é utilizado para as extremidades de comparação de URL imediatamente antes do nome de ficheiro do elemento pedido. Uma barra à direita é o último caráter neste tipo de caminho.
    
- Substituir quaisquer espaços no padrão de caminho de URL "% 20".
    
- Cada padrão do caminho de URL pode conter um ou mais série de asteriscos (*), onde cada asterisco corresponde a uma sequência de um ou mais carateres.
    
- Especificar vários caminhos de URL no padrão de por delimiting cada um com um único espaço.

    Por exemplo: * /sales/ * /marketing/

- A especificação de um caminho de URL pode tirar partido da [valores de caráter universal](cdn-rules-engine-reference.md#wildcard-values).

- Utilize o **ignorar caso** opção para controlar se é efetuada uma comparação de maiúsculas e minúsculas.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-extension"></a>Extensão de caminho de URL
Identifica pedidos pela extensão de ficheiro do elemento pedido.

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais a extensão de caminho de URL correspondem à condição for satisfeita.
- **Corresponde a**: requer o URL do pedido para conter uma extensão de ficheiro que corresponde exatamente ao padrão especificado.

   Por exemplo, se especificar "htm", "htm" ativos são feita a correspondência, mas não os recursos de "html".  

- **Não corresponde ao**: precisa do pedido de URL que contém uma extensão de ficheiro não corresponde ao padrão especificado.

Informações da chave:
- Especificar as extensões de ficheiro a corresponder no **valor** caixa. Não incluir um ponto à esquerda; Por exemplo, utilize htm em vez. htm.

- Utilize o **ignorar caso** opção para controlar se é efetuada uma comparação de maiúsculas e minúsculas.

- Especificar várias extensões de ficheiro, delimiting cada extensão com um único espaço. 

    Por exemplo: htm html

- Por exemplo, especificar "htm" corresponde a "htm" ativos, mas não os recursos de "html".


#### <a name="sample-scenario"></a>Cenário de exemplo

A seguinte configuração de exemplo parte do princípio de que esta condição de correspondência é preenchida quando um pedido corresponde das extensões especificadas.

Especificação de valor: asp aspx php html

Esta condição de correspondência é preenchida quando localizar URLs que terminem com as seguintes extensões:
- . asp
- . aspx
- SH
- *.HTML

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-filename"></a>Nome de ficheiro de caminho de URL
Identifica os pedidos com o nome de ficheiro do elemento pedido. Para efeitos desta condição de correspondência, um nome de ficheiro é composta pelo nome do recurso de pedido, um período e a extensão de ficheiro (por exemplo, index.html).

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o nome de ficheiro do URL do caminho correspondem à condição for satisfeita.
- **Corresponde a**: precisa do pedido para conter um nome de ficheiro no respetivo caminho de URL que corresponde ao padrão especificado.
- **Não corresponde ao**: precisa do pedido para conter um nome de ficheiro no respetivo caminho de URL não corresponde ao padrão especificado.

Informações da chave:
- Utilize o **ignorar caso** opção para controlar se é efetuada uma comparação de maiúsculas e minúsculas.

- Para especificar várias extensões de ficheiro, separe cada extensão com um único espaço.

    Por exemplo: index.htm index.html

- Substitua os espaços de um valor de nome de ficheiro "% 20".
    
- Um valor de nome de ficheiro pode tirar partido de [valores de caráter universal](cdn-rules-engine-reference.md#wildcard-values). Por exemplo, cada padrão de nome de ficheiro pode incluir um ou mais série de asteriscos (*), onde cada asterisco corresponde a uma sequência de um ou mais carateres.
    
- Se não forem especificados carateres universais, apenas uma correspondência exata satisfazer esta condição de correspondência.

    Por exemplo, a especificação de "presentation.ppt" corresponde a um recurso com o nome "presentation.ppt", mas não uma denominada "presentation.pptx."

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-literal"></a>Literal de caminho de URL
Compara o caminho do URL de um pedido, incluindo o nome do ficheiro, o valor especificado.

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o Literal de caminho do URL correspondem à condição for satisfeita.
- **Corresponde a**: precisa do pedido para conter um caminho de URL que corresponde ao padrão especificado.
- **Não corresponde ao**: precisa do pedido para conter um caminho de URL não corresponde ao padrão especificado.

Informações da chave:
- Utilize o **relativo ao** opção para especificar se o ponto de comparação de URL começa antes ou depois do ponto de acesso ao conteúdo. 

    Os valores seguintes estão disponíveis para o **relativo ao** opção:
     - **Raiz**: indica que o ponto de comparação de URL começa imediatamente após o nome de anfitrião do CDN.

       Por exemplo: http:\//wpc.0001.&lt; Domínio&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origem**: indica que o ponto de comparação de URL começa depois do ponto de acesso ao conteúdo (por exemplo, myorigin /000001 ou/800001 /). Porque o \*. azureedge.net CNAME é criada relativos ao diretório de origem no nome do anfitrião da Verizon CDN por predefinição, os utilizadores da CDN do Azure devem utilizar o **origem** valor. 

       Por exemplo: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     Este URL aponta para o nome do anfitrião da Verizon CDN seguinte: http:\//wpc.0001.&lt; Domínio&gt;/800001/myorigin/**myfolder/index.htm**

- Uma extremidade CNAME URL é rescrita para um URL de CDN antes de uma comparação de URL.

   Por exemplo, ambos os seguintes URLs de apontam para o mesmo elemento, pelo que tem o mesmo caminho de URL:
    - URL de CDN: http:\//wpc.0001.&lt; Domínio&gt;/800001/CustomerOrigin/path/asset.htm
    - URL de CNAME Edge: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

   Informações adicionais:
    
    - Caminho de URL (relativo à raiz): /800001/CustomerOrigin/path/asset.htm
   
    - Caminho de URL (relativo à origem): /path/asset.htm

- Cadeias de consulta no URL são ignoradas.
- Utilize o **ignorar caso** opção para controlar se é efetuada uma comparação de maiúsculas e minúsculas.
- O valor especificado para esta condição de correspondência irá ser comparado com o caminho relativo do pedido exato efetuado pelo cliente.

- Para fazer corresponder a todos os pedidos efetuados para um diretório específico, utilize o [diretório de caminho de URL](#url-path-directory) ou [caminho de URL com carateres universais](#url-path-wildcard) correspondem à condição.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-regex"></a>Regex de caminho de URL
Compara o caminho de URL de um pedido para especificado [expressão regular](cdn-rules-engine-reference.md#regular-expressions).

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o Regex de caminho de URL correspondem à condição for satisfeita.
- **Corresponde a**: precisa do pedido para conter um caminho de URL que corresponda à expressão regular especificada.
- **Não corresponde ao**: precisa do pedido para conter um caminho de URL não corresponde a expressão regular especificada.

Informações da chave:
- Uma extremidade CNAME URL é rescrita para um URL de CDN antes de comparação de URL. 
 
   Por exemplo, ambos os URLs de apontar para o mesmo elemento, pelo que tem o mesmo caminho de URL.

     - URL de CDN: http:\//wpc.0001.&lt; Domínio&gt;/800001/CustomerOrigin/path/asset.htm

     - URL de CNAME Edge: http:\//my.domain.com/path/asset.htm

   Informações adicionais:
    
     - Caminho de URL: /800001/CustomerOrigin/path/asset.htm

- Cadeias de consulta no URL são ignoradas.
    
- Utilize o **ignorar caso** opção para controlar se é efetuada uma comparação de maiúsculas e minúsculas.
    
- Os espaços no caminho de URL devem ser substituídos com "% 20".

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-path-wildcard"></a>Caminho de URL com carateres universais
Compara caminho relativo de URL um pedido para o padrão de caráter universal especificado.

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o caminho de URL de caráter universal correspondem à condição for satisfeita.
- **Corresponde a**: precisa do pedido para conter um caminho de URL que corresponda ao padrão especificado com carateres universais.
- **Não corresponde ao**: precisa do pedido para conter um caminho de URL não corresponde ao padrão especificado com carateres universais.

Informações da chave:
- **Relativamente à** opção: esta opção determina se o ponto de comparação de URL começa antes ou depois do ponto de acesso ao conteúdo.

   Esta opção pode ter os seguintes valores:
     - **Raiz**: indica que o ponto de comparação de URL começa imediatamente após o nome de anfitrião do CDN.

       Por exemplo: http:\//wpc.0001.&lt; Domínio&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origem**: indica que o ponto de comparação de URL começa depois do ponto de acesso ao conteúdo (por exemplo, myorigin /000001 ou/800001 /). Porque o \*. azureedge.net CNAME é criada relativos ao diretório de origem no nome do anfitrião da Verizon CDN por predefinição, os utilizadores da CDN do Azure devem utilizar o **origem** valor. 

       Por exemplo: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     Este URL aponta para o nome do anfitrião da Verizon CDN seguinte: http:\//wpc.0001.&lt; Domínio&gt;/800001/myorigin/**myfolder/index.htm**

- Uma extremidade CNAME URL é rescrita para um URL de CDN antes de comparação de URL.

   Por exemplo, ambos os seguintes URLs de apontam para o mesmo elemento, pelo que tem o mesmo caminho de URL:
     - URL de CDN: http://wpc.0001. &lt;Domínio&gt;/800001/CustomerOrigin/path/asset.htm
     - URL de CNAME Edge: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

   Informações adicionais:
    
     - Caminho de URL (relativo à raiz): /800001/CustomerOrigin/path/asset.htm
    
     - Caminho de URL (relativo à origem): /path/asset.htm
    
- Especificar vários caminhos de URL, delimiting cada um com um único espaço.

   Por exemplo: /marketing/asset.* /sales/*.htm

- Cadeias de consulta no URL são ignoradas.
    
- Utilize o **ignorar caso** opção para controlar se é efetuada uma comparação de maiúsculas e minúsculas.
    
- Substitua espaços no caminho de URL "% 20".
    
- O valor especificado para um caminho de URL pode tirar partido da [valores de caráter universal](cdn-rules-engine-reference.md#wildcard-values). Cada padrão do caminho de URL pode conter um ou mais série de asteriscos (*), onde cada asterisco pode corresponder a uma sequência de um ou mais carateres.

#### <a name="sample-scenarios"></a>Cenários de exemplo

As configurações de exemplo na seguinte tabela partem do princípio de que esta condição de correspondência é preenchida quando um pedido corresponde ao padrão especificado do URL:

Valor                   | Caminho relativo    | Resultado 
------------------------|----------------|-------
*/Test.HTML */test.php  | Raiz ou a origem | Este padrão é correspondido com pedidos para recursos com o nome "test.html" ou "test.php" em qualquer pasta.
/ 80ABCD origem/texto / *   | Raiz           | Este padrão é correspondido quando o recurso solicitado cumpre os seguintes critérios: <br />--Tem de residir numa origem de cliente denominada "origem". <br />-O caminho relativo tem de começar com uma pasta denominada "text". Ou seja, o recurso pedido ou pode residir na pasta "text" ou uma das respetivas subpastas recursiva.
*/CSS/*   */js /*          | Raiz ou a origem | Este padrão é correspondido por todos os CDN ou edge URLs de CNAME que contêm uma pasta css ou js.
*.jpg *.gif *.png       | Raiz ou a origem | Este padrão é correspondido por todos os URLs CNAME CDN ou o Microsoft edge terminando. jpg, GIF ou PNG. Uma maneira de especificar este padrão é com o [extensão de caminho de URL correspondem à condição](#url-path-extension).
imagens / * / suporte de dados / *      | Origem         | Este padrão é correspondido pela CDN ou o Microsoft edge URLs de CNAME cujo caminho relativo começa com uma pasta "imagens" ou "suporte de dados". <br />-URL de CDN: http:\//wpc.0001.&lt; Domínio&gt;/800001/myorigin/images/sales/event1.png<br />-Limite CNAME URL de exemplo: http:\//cdn.mydomain.com/images/sales/event1.png

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-literal"></a>Literal de consulta de URL
Compara a cadeia de consulta de um pedido para o valor especificado.

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o Literal de consulta de URL correspondem à condição for satisfeita.
- **Corresponde a**: precisa do pedido para conter uma cadeia de consulta de URL que corresponde à cadeia de consulta especificada.
- **Não corresponde ao**: precisa do pedido para conter uma cadeia de consulta de URL não coincide com a cadeia de consulta especificada.

Informações da chave:

- Apenas corresponde da cadeia de consulta exato satisfazer esta condição de correspondência.
    
- Utilize o **ignorar caso** opção para controlar a sensibilidade de comparações de cadeias de consulta.
    
- Não inclua um leading ponto de interrogação (?) no texto de valor de cadeia de consulta.
    
- Determinados caracteres requerem codificação do URL. Utilize o símbolo de percentagem para URL codifique os carateres seguintes:

   Caráter | Codificação do URL
   ----------|---------
   Espaço     | % 20
   &         | % 25

- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
   - Concluir o preenchimento da Cache
   - Idade de máx. de interno predefinida
   - Forçar interna de atribuição de idade máxima
   - Ignorar a Cache não de origem
   - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-parameter"></a>Parâmetro de consulta de URL
Identifica os pedidos que contêm o parâmetro de cadeia de consulta especificada. Este parâmetro estiver definido para um valor que corresponde ao padrão especificado. Parâmetros de cadeia de consulta (por exemplo, o parâmetro = valor) no pedido de URL, determine se esta condição for satisfeita. Esta condição de correspondência identifica um parâmetro de cadeia de consulta pelo respetivo nome e aceita um ou mais valores para o valor do parâmetro. 

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o parâmetro de consulta de URL correspondem à condição for satisfeita.
- **Corresponde a**: necessita de um pedido para conter o parâmetro especificado com um valor que corresponde a, pelo menos, um dos valores definidos nesta condição de correspondência.
- **Não coincide com**: requer que o pedido cumpre qualquer um dos seguintes critérios:
  - Não contém o parâmetro especificado.
  - Contém o parâmetro especificado, mas o respetivo valor não corresponde a nenhum dos valores definidos nesta condição de correspondência.

Esta condição de correspondência fornece uma forma fácil para especificar combinações de nome/valor do parâmetro. Para uma maior flexibilidade se correspondente a um parâmetro de cadeia de consulta, considere a utilização de [consulta de URL com carateres universais](#url-query-wildcard) correspondem à condição.

Informações da chave:
- Apenas um único URL consulta nome de parâmetro pode ser especificado por instância desta condição de correspondência.
    
- Porque os valores de caráter universal não são suportados quando é especificado um nome de parâmetro, apenas correspondências de nome de parâmetro exato são elegíveis para comparação.
- Podem incluir valores de parâmetro [valores de caráter universal](cdn-rules-engine-reference.md#wildcard-values).
   - Cada padrão de valor de parâmetro pode ser composto por um ou mais série de asteriscos (*), onde cada asterisco pode corresponder a uma sequência de um ou mais carateres.
   - Determinados caracteres requerem codificação do URL. Utilize o símbolo de percentagem para URL codifique os carateres seguintes:

       Caráter | Codificação do URL
       ----------|---------
       Espaço     | % 20
       &         | % 25

- Especificar vários valores de parâmetro de cadeia de consulta, delimiting cada um com um único espaço. Esta condição de correspondência é preenchida quando um pedido contém um das combinações de nome/valor especificado.

   - Exemplo 1:

     - Configuração:

       ValueA ValueB

     - Os seguintes parâmetros de cadeia de consulta corresponde a esta configuração:

       Parameter1 = ValueA
    
       Parameter1 = ValueB

   - Exemplo 2:

     - Configuração: 

        O valor % 20A valor % 20B

     - Os seguintes parâmetros de cadeia de consulta corresponde a esta configuração:

       Parameter1 = valor % 20A

       Parameter1 = valor % 20B

- Esta condição de correspondência é cumprida apenas quando existe uma correspondência exata com, pelo menos, um das combinações de nome/valor de cadeia de consulta especificada.

   Por exemplo, se utilizar a configuração no exemplo anterior, o parâmetro de nome/valor combinação "Parameter1 = ValueAdd" que não possam ser considerados uma correspondência. No entanto, se especificar um dos seguintes valores, corresponderá essa combinação nome/valor:

   - ValueA ValueB ValueAdd
   - ValueA * ValueB

- Utilize o **ignorar caso** opção para controlar a sensibilidade de comparações de cadeias de consulta.
    
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
   - Concluir o preenchimento da Cache
   - Idade de máx. de interno predefinida
   - Forçar interna de atribuição de idade máxima
   - Ignorar a Cache não de origem
   - Interna obsoleta máx.

#### <a name="sample-scenarios"></a>Cenários de exemplo
O exemplo seguinte demonstra como esta opção funciona em situações específicas:

Nome      | Valor |  Resultado
----------|-------|--------
Utilizador      | Joe   | Este padrão é correspondido quando a cadeia de consulta para um URL pedido é "? utilizador = joe."
Utilizador      | *     | Este padrão é correspondido quando a cadeia de consulta para um URL de pedido contém um parâmetro de utilizador.
E-mail Joe | *     | Este padrão é correspondido quando a cadeia de consulta para um URL de pedido contém um parâmetro de E-Mail que começa com "João".

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-regex"></a>Regex de consulta de URL
Identifica os pedidos que contêm o parâmetro de cadeia de consulta especificada. Este parâmetro estiver definido como um valor que corresponde a uma determinada [expressão regular](cdn-rules-engine-reference.md#regular-expressions).

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o Regex de consulta de URL correspondem à condição for satisfeita.
- **Corresponde a**: precisa do pedido para conter uma cadeia de consulta de URL que corresponda à expressão regular especificada.
- **Não corresponde ao**: precisa do pedido para conter uma cadeia de consulta de URL não corresponde a expressão regular especificada.

Informações da chave:
- Apenas exato corresponde a expressão regular especificada satisfazer esta condição de correspondência.
    
- Utilize o **ignorar caso** opção para controlar a sensibilidade de comparações de cadeias de consulta.
    
- Para efeitos desta opção, uma cadeia de consulta for iniciado com o primeiro caráter após o delimitador de ponto de interrogação (?) para a cadeia de consulta.
    
- Determinados caracteres requerem codificação do URL. Utilize o símbolo de percentagem para URL codifique os carateres seguintes:

   Caráter | Codificação do URL | Valor
   ----------|--------------|------
   Espaço     | % 20          | \%20
   &         | % 25          | \%25

   Tenha em atenção que tem de ser escape símbolos de percentagem.

- Os carateres de escape duplo especial, expressão regular (por exemplo, \^$. +) para incluir uma barra invertida na expressão regular.

   Por exemplo:

   Valor | Interpretado como 
   ------|---------------
   \\+    | +
   \\\+   | \\+

- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
   - Concluir o preenchimento da Cache
   - Idade de máx. de interno predefinida
   - Forçar interna de atribuição de idade máxima
   - Ignorar a Cache não de origem
   - Interna obsoleta máx.


[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="url-query-wildcard"></a>Consulta de URL com carateres universais
Compara o valor (es) especificado na cadeia de consulta do pedido.

O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o caráter universal de consulta de URL correspondem à condição for satisfeita.
- **Corresponde a**: precisa do pedido para conter uma cadeia de consulta de URL que corresponde ao valor de caráter universal especificado.
- **Não corresponde ao**: precisa do pedido para conter uma cadeia de consulta de URL não corresponde ao valor de caráter universal especificado.

Informações da chave:
- Para efeitos desta opção, uma cadeia de consulta for iniciado com o primeiro caráter após o delimitador de ponto de interrogação (?) para a cadeia de consulta.
- Podem incluir valores de parâmetro [valores de caráter universal](cdn-rules-engine-reference.md#wildcard-values):
   - Cada padrão de valor de parâmetro pode ser composto por um ou mais série de asteriscos (*), onde cada asterisco pode corresponder a uma sequência de um ou mais carateres.
   - Determinados caracteres requerem codificação do URL. Utilize o símbolo de percentagem para URL codifique os carateres seguintes:

     Caráter | Codificação do URL
     ----------|---------
     Espaço     | % 20
     &         | % 25

- Especificar vários valores, delimiting cada um com um único espaço.

   Por exemplo: *Parameter1 = ValueA* *ValueB* *Parameter1 = ValueC & Parameter2 = ValueD*

- Apenas exato corresponde a pelo menos uma os padrões de cadeia de consulta especificada satisfazer esta condição de correspondência.
    
- Utilize o **ignorar caso** opção para controlar a sensibilidade de comparações de cadeias de consulta.
    
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
   - Concluir o preenchimento da Cache
   - Idade de máx. de interno predefinida
   - Forçar interna de atribuição de idade máxima
   - Ignorar a Cache não de origem
   - Interna obsoleta máx.

#### <a name="sample-scenarios"></a>Cenários de exemplo
O exemplo seguinte demonstra como esta opção funciona em situações específicas:

 Nome                 | Descrição
 ---------------------|------------
utilizador = joe              | Este padrão é correspondido quando a cadeia de consulta para um URL pedido é "? utilizador = joe."
\*utilizador =\* \*optout =\* | Este padrão é correspondido quando a consulta de URL de CDN contém o utilizador ou o parâmetro optout.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

## <a name="next-steps"></a>Passos Seguintes
* [Descrição geral de rede de entrega de conteúdos do Azure](cdn-overview.md)
* [Referência do motor de regras](cdn-rules-engine-reference.md)
* [Motor de regras de expressões condicionais](cdn-rules-engine-reference-conditional-expressions.md)
* [Funcionalidades do motor de regras](cdn-rules-engine-reference-features.md)
* [Substituir o comportamento HTTP predefinido utilizando o motor de regras](cdn-rules-engine.md)

