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
ms.openlocfilehash: 9986e654b076df099e3912f9da628728723b5c3d
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/11/2018
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
Diretório de caminho de URL | Identifica pedidos pelo respetivo caminho relativo.
Extensão de caminho de URL | Identifica pedidos pela respetiva extensão de nome de ficheiro.
Nome de ficheiro de caminho de URL | Identifica pedidos pelo respetivo nome de ficheiro.
Literal de caminho de URL | Compara o caminho relativo de um pedido para o valor especificado.
Regex de caminho de URL | Compara o caminho relativo de um pedido para a expressão regular especificada.
Caminho de URL com carateres universais | Compara o caminho relativo de um pedido para o padrão especificado.
Literal de consulta de URL | Compara a cadeia de consulta de um pedido para o valor especificado.
Parâmetro de consulta de URL | Identifica os pedidos que contêm o parâmetro de cadeia de consulta especificada definido como um valor que corresponde ao padrão especificado.
Regex de consulta de URL | Identifica os pedidos que contêm o parâmetro de cadeia de consulta especificada definido como um valor que corresponde a uma expressão regular especificada.
Consulta de URL com carateres universais | Compara o valor especificado em relação a cadeia de consulta do pedido.


## <a name="reference-for-rules-engine-match-conditions"></a>Referência para condições de correspondência do motor de regras

---
### <a name="always"></a>Sempre

A condição de correspondência de sempre aplica-se um conjunto predefinido de funcionalidades para todos os pedidos.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="as-number"></a>COMO número 
A rede de número AS é definida pelo respetivo número de sistema autónomo (ASN). É fornecida uma opção para indicar se esta condição irá ser cumprida quando um cliente de rede "Corresponde a" ou "Does não corresponde a" o ASN especificado.

Informações da chave:
- Especifique vários ASNs por delimiting cada um com um único espaço. Por exemplo, 64514 64515 corresponde pedidos que chegam 64514 ou 64515.
- Determinados pedidos podem não devolver um ASN válido. Um ponto de interrogação (?) corresponderá pedidos para o qual não foi possível determinar um ASN válido.
- Tem de especificar o ASN completo para a rede pretendido. Não serão possível corresponder valores parciais.
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
- Conteúdo do armazenamento de rede de entrega de conteúdos foi pedido.
- O pedido de URI utiliza o ponto de acesso ao conteúdo (por exemplo, /000001) que está definido nesta condição de correspondência.
  - URL de rede de entrega de conteúdo: O URI do pedido tem de conter o ponto de acesso ao conteúdo selecionado.
  - URL de CNAME Edge: A configuração de CNAME correspondente do contorno tem de apontar para o ponto de acesso ao conteúdo selecionado.
  
Informações da chave:
 - O ponto de acesso ao conteúdo identifica o serviço que deverá servir os conteúdos solicitados.
 - Não utilize uma instrução se e combinar determinadas condições de correspondência. Por exemplo, combinar uma condição de correspondência de origem de CDN com uma condição de correspondência de origem do cliente criaria um padrão de correspondência de que nunca foi possível corresponder. Por este motivo, as duas condições de correspondência de origem da CDN não podem ser combinadas através de uma instrução e se.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="client-ip-address"></a>Endereço IP do cliente
É fornecida uma opção para indicar se a condição de endereço IP do cliente irá ser cumprida quando um cliente endereço IP "Correspondências" ou "Does não corresponde a" endereços IP especificado.

Informações da chave:
- Certifique-se de que utilizam a notação CIDR.
- Especifique vários endereços IP e/ou blocos de endereços IP por delimiting cada um com um único espaço.
  - **Exemplo de IPv4**: 1.2.3.4 10.20.30.40 corresponde a quaisquer pedidos que chegam 1.2.3.4 ou 10.20.30.40.
  - **Exemplo de IPv6**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 corresponde a quaisquer pedidos que chegam 1:2:3:4:5:6:7:8 ou 10:20:30:40:50:60:70:80.
- A sintaxe para um bloco de endereços IP é o endereço IP base seguido por uma barra e o tamanho de prefixo.
  - **Exemplo de IPv4**: 5.5.5.64/26 corresponde a quaisquer pedidos que chegam de 5.5.5.64 através de 5.5.5.127.
  - **Exemplo de IPv6**: 1:2:3: / 48 corresponde a quaisquer pedidos que chegam de 1:2:3:0:0:0:0:0 através de 1:2:3:ffff:ffff:ffff:ffff:ffff.
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
O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o parâmetro de Cookie correspondem à condição irão ser satisfeitas.
- **Corresponde a**: necessita de um pedido para conter o cookie especificado com um valor que corresponde a, pelo menos, um dos valores definidos nesta condição de correspondência.
- **Não coincide com**: requer que o pedido de satisfazer um dos seguintes critérios:
  - Não contém o cookie especificado.
  - Contém o cookie especificado, mas o respetivo valor não corresponde a nenhum dos valores definidos nesta condição de correspondência.
  
Informações da chave:
- Nome do cookie: 
  - Os carateres especiais, incluindo um asterisco, não são suportados quando estiver a especificar um nome de cookie. Isto significa que apenas correspondências de nome de cookie exato são elegíveis para comparação.
  - Apenas um nome de cookie única pode ser especificado por instância desta condição de correspondência.
  - Comparações de nome de cookie são sensível.
- Valor do cookie: 
  - Especificar vários valores de cookie, delimiting cada um com um único espaço.
  - Um valor de cookie pode tirar partido de carateres especiais. 
  - Se não tiver sido especificado um caráter universal, apenas uma correspondência exata irá satisfazer esta condição de correspondência. Por exemplo, a especificação de "Valor" corresponderá a "Valor", mas não "Value1" ou "Value2."
  - O **ignorar caso** opção determina se será efetuada uma comparação de maiúsculas e minúsculas com um valor de cookie do pedido.
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
A condição de correspondência de Regex do parâmetro de Cookie define um valor e o nome do cookie. Pode utilizar expressões regulares para definir o valor do cookie pretendido. 

O **correspondências**/**não corresponde ao** opção determina as condições em que esta correspondem à condição irá ser satisfeito.
- **Corresponde a**: necessita de um pedido para conter o cookie especificado com um valor que corresponde à expressão regular especificada.
- **Não coincide com**: requer que o pedido de satisfazer um dos seguintes critérios:
  - Não contém o cookie especificado.
  - Contém o cookie especificado, mas o respetivo valor não corresponde a expressão regular especificada.
  
Informações da chave:
- Nome do cookie: 
  - As expressões regulares e carateres especiais, incluindo um asterisco, não são suportados quando estiver a especificar um nome de cookie. Isto significa que apenas correspondências de nome de cookie exato são elegíveis para comparação.
  - Apenas um nome de cookie única pode ser especificado por instância desta condição de correspondência.
  - Comparações de nome de cookie são sensível.
- Valor do cookie: 
  - Um valor de cookie pode tirar partido de expressões regulares.
  - O **ignorar caso** opção determina se será efetuada uma comparação de maiúsculas e minúsculas com um valor de cookie do pedido.
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
Pode especificar um país através do respetivo código de país. É fornecida uma opção para indicar se esta condição será preenchido quando o país/região do que um pedido tem origem "Correspondências" ou "Does não corresponde a" valores especificados.

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

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="customer-origin"></a>Origem de cliente

Informações da chave: 
- A condição de correspondência de origem de cliente vai ser satisfeita independentemente se solicitados através de um URL de rede de entrega de conteúdo ou um limite de URL de CNAME que aponta para a origem de cliente selecionado.
- Não é possível eliminar uma configuração de origem do cliente que é referenciada por uma regra a partir da página de origem do cliente. Antes de tentar eliminar uma configuração de origem do cliente, certifique-se de que as seguintes configurações não referencie-o:
  - Uma condição de correspondência de origem do cliente
  - Uma configuração de CNAME edge
- Não utilize uma instrução se e combinar determinadas condições de correspondência. Por exemplo, combinar uma condição de correspondência de origem do cliente com uma condição de correspondência de origem da CDN criaria um padrão de correspondência de que nunca foi possível corresponder. Por este motivo, as duas condições de correspondência de origem do cliente não podem ser combinadas através de uma instrução e se.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

---
### <a name="device"></a>Dispositivo

A condição de correspondência de dispositivo identifica os pedidos efetuados a partir de um dispositivo móvel com base nas respetivas propriedades. Deteção de dispositivos móveis é conseguida através de [WURFL](http://wurfl.sourceforge.net/). A tabela seguinte lista as capacidades de WURFL e as respetivas variáveis para o motor de regras de rede de entrega de conteúdos.
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
É Tablet | % {wurfl_cap_is_tablet} | Um valor boleano que indica se o dispositivo é um tablet. Esta é uma descrição de SO independentes. | true
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
- A lista de limite disponível CNAMEs está limitada às que foram configuradas na página de CNAMEs de limite que corresponde à plataforma em que o motor de regras de HTTP está a ser configurado.
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
O nome de anfitrião associado referência através do qual o conteúdo foi pedido determina se a condição de domínio que faça referência for satisfeita. É fornecida uma opção para indicar se esta condição irá ser cumprida quando o referência nome do anfitrião "Corresponde a" ou "Does não corresponde a" valores especificados.

Informações da chave:
- Especificar vários nomes de anfitrião, delimiting cada um com um único espaço.
- Esta condição de correspondência suporta carateres especiais.
- Se o valor especificado não contém um asterisco, tem de ser uma correspondência exata para nome de anfitrião de referência. Por exemplo, a especificação de "mydomain.com" não corresponde a "www.mydomain.com."
- O **ignorar caso** opção determina se será efetuada uma comparação de maiúsculas e minúsculas.
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
O **correspondências**/**não corresponde ao** opção determina as condições em que esta correspondem à condição irá ser satisfeito.
- **Correspondências**: precisa do pedido para conter o cabeçalho especificado. O valor tem de corresponder ao que está definido nesta condição de correspondência.
- **Não coincide com**: requer que o pedido de satisfazer um dos seguintes critérios:
  - Não contém o cabeçalho especificado.
  - Contém o cabeçalho especificado, mas o respetivo valor não corresponde ao que está definido nesta condição de correspondência.
  
Informações da chave:
- Comparações de nome de cabeçalho são sempre sensível. O **ignorar caso** opção determina a sensibilidade de comparações de valor de cabeçalho.
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
O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o Regex de cabeçalho de pedido correspondem à condição irão ser satisfeitas.
- **Correspondências**: precisa do pedido para conter o cabeçalho especificado. O valor tem de corresponder ao padrão que está definido a expressão regular especificada.
- **Não coincide com**: requer que o pedido de satisfazer um dos seguintes critérios:
  - Não contém o cabeçalho especificado.
  - Contém o cabeçalho especificado, mas o respetivo valor não corresponde a expressão regular especificada.

Informações da chave:
- Nome do cabeçalho: 
  - Comparações de nome de cabeçalho são sensível.
  - Os espaços no nome do cabeçalho devem ser substituídos com "% 20". 
- Valor de cabeçalho: 
  - Um valor de cabeçalho pode tirar partido de expressões regulares.
  - O **ignorar caso** opção determina a sensibilidade de comparações de valor de cabeçalho.
  - Apenas cabeçalho exato valor corresponde a pelo menos uma os padrões especificados irá satisfazer esta condição.
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
O **correspondências**/**não corresponde ao** opção determina as condições sob as quais o caráter universal cabeçalho de pedido correspondem à condição irão ser satisfeitas.
- **Correspondências**: precisa do pedido para conter o cabeçalho especificado. O valor tem de corresponder ao, pelo menos, um dos valores definidos nesta condição de correspondência.
- **Não coincide com**: requer que o pedido de satisfazer um dos seguintes critérios:
  - Não contém o cabeçalho especificado.
  - Contém o cabeçalho especificado, mas o respetivo valor não corresponde a nenhum dos valores especificados.
  
Informações da chave:
- Nome do cabeçalho: 
  - Comparações de nome de cabeçalho são sensível.
  - Os espaços no nome do cabeçalho devem ser substituídos com "% 20". Também pode utilizar este valor para especificar um valor de cabeçalho espaços.
- Valor de cabeçalho: 
  - Um valor de cabeçalho pode tirar partido de carateres especiais.
  - O **ignorar caso** opção determina a sensibilidade de comparações de valor de cabeçalho.
  - Apenas cabeçalho exato valor corresponde a pelo menos uma os padrões especificados irá satisfazer esta condição.
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
Apenas os recursos que são pedidos através do método de pedido seleccionado satisfaçam a condição de método de pedido. Os métodos de pedido disponíveis são:
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
Apenas os recursos que são pedidos através do protocolo selecionado satisfaçam a condição de esquema do pedido. Os protocolos disponíveis são HTTP e HTTPS.

Informações da chave:
- Devido da forma na cache de que as definições são registadas, esta condição de correspondência é incompatível com as seguintes funcionalidades:
  - Concluir o preenchimento da Cache
  - Idade de máx. de interno predefinida
  - Forçar interna de atribuição de idade máxima
  - Ignorar a Cache não de origem
  - Interna obsoleta máx.

[Voltar ao início](#match-conditions-for-the-azure-cdn-rules-engine)

</br>

## <a name="next-steps"></a>Passos Seguintes
* [Descrição geral de rede de entrega de conteúdos do Azure](cdn-overview.md)
* [Referência do motor de regras](cdn-rules-engine-reference.md)
* [Motor de regras de expressões condicionais](cdn-rules-engine-reference-conditional-expressions.md)
* [Funcionalidades do motor de regras](cdn-rules-engine-reference-features.md)
* [Substituir o comportamento HTTP predefinido utilizando o motor de regras](cdn-rules-engine.md)

