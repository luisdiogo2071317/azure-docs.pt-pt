---
title: Media Services do Azure - uniforme transmissão em fluxo de protocolo (MS-SSTR) Amendment para HEVC | Microsoft Docs
description: Esta especificação descreve o protocolo e o formato para fragmentados com base em MP4 em direto de transmissão em fluxo com HEVC nos Media Services do Azure. Este é um amendment (MS-SSTR) para incluir suporte para a ingestão de HEVC documentaiton transmissão em fluxo uniforme de protocolo e a transmissão em fluxo. Apenas as alterações necessárias para fornecer HEVC são especificadas neste artigo, exceto foram "(não alterar)" indica o texto é copiado para esclarecimentos apenas.
services: media-services
documentationcenter: ''
author: cenkdin
manager: cfowler
editor: ''
ms.assetid: f27d85de-2cb8-4269-8eed-2efb566ca2c6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: johndeu;
ms.openlocfilehash: 7b4e1d933040225445e83d6e507cdee29a8f2363
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="smooth-streaming-protocol-ms-sstr-amendment-for-hevc"></a>Amendment de protocolo (MS-SSTR) transmissão em fluxo uniforme para HEVC

## <a name="1-introduction"></a>1 Introdução 

Este artigo fornece amendments detalhadas a aplicar a especificação de protocolo de transmissão em fluxo uniforme [MS-SSTR] para ativar as vídeo suave de transmissão em fluxo de HEVC codificado. Nesta especificação, iremos descrevem apenas as alterações necessárias para fornecer o codec de vídeo HEVC. O artigo acompanha o mesmo esquema numeração como a especificação [MS-SSTR]. Os títulos vazios apresentados em todo o artigo são fornecidos para incluída o leitor a respetiva posição na especificação [MS-SSTR].  "(Não alterar)" indica o texto é copiado esclarecimentos apenas para efeitos.

O artigo fornece os requisitos de implementação técnica para o sinalização codec de vídeo HEVC no manifesto de transmissão em fluxo uniforme e referências infrações à sejam atualizadas para referenciar as atuais normas MPEG incluem HEVC, encriptação comum de HEVC e caixa os nomes de formato de ficheiro de suporte de dados de Base de ISO foram atualizados para estar consistente com as especificações mais recentes. 

A especificação de protocolo de transmissão em fluxo uniforme referenciada [MS-SSTR] descreve o formato wire utilizado para entregar em direto e a pedido digital suportes de dados, como de áudio e vídeo, no manners seguintes: de um codificador para um servidor web, de um servidor para outro servidor e de um servidor a um cliente HTTP.
A utilização de um MPEG-4 ([[MPEG4 RA])](http://go.microsoft.com/fwlink/?LinkId=327787)-permite a entrega de estrutura com base de dados através de HTTP totalmente integrada de mudança em tempo real entre níveis diferentes de qualidade de conteúdo de multimédia comprimido. O resultado é uma experiência de reprodução constante para o utilizador final do cliente HTTP, mesmo se a rede e condições de composição vídeo alterar para o computador cliente ou dispositivo.

## <a name="11-glossary"></a>1.1 glossário 

Os termos seguintes são definidos na *[MS-GLOS]*:

>   **Identificador exclusivo universalmente de identificador exclusivo global (GUID) (UUID)**

Os termos seguintes são específicos deste documento:

>  **tempo de composição:** o tempo de uma amostra é apresentada no cliente, tal como definido no [[ISO/IEC-14496-12].](http://go.microsoft.com/fwlink/?LinkId=183695)

>   **CENC**: encriptação comum, tal como definido na edição segundo [norma ISO/IEC 23001 7].

>   **tempo de descodificar:** uma amostra é necessária a hora para possível descodificar no cliente, tal como definido no [[ISO/IEChttp://go.microsoft.com/fwlink/?LinkId=18369514496-12].](http://go.microsoft.com/fwlink/?LinkId=183695)

**fragmento:** uma unidade de forma independente transferível **multimédia** que é composto por um ou mais **amostras**.

>   **HEVC:** elevada eficiência vídeo codificação, como definido em [norma ISO/IEC 23008-2]

>   **manifesto:** metadados sobre o **apresentação** que permite que um cliente fazer pedidos **multimédia**. **suporte de dados:** comprimidos dados de áudio, vídeo e texto utilizados pelo cliente de reproduzir uma **apresentação**. **formato de suporte de dados:** um formato definido especificamente para representar as vídeo ou áudio como um comprimido **exemplo**.

>   **apresentação:** o conjunto de todos os **fluxos** e metadados relacionados necessários para reproduzir um filme único. **pedido:** mensagem de um HTTP enviada do cliente para o servidor, tal como definido no [[RFC2616].](http://go.microsoft.com/fwlink/?LinkId=90372) **resposta:** mensagem de um HTTP enviada do servidor para o cliente, tal como definido no [[RFC2616].](http://go.microsoft.com/fwlink/?LinkId=90372)

>   **exemplo:** a unidade fundamental menor (por exemplo, um intervalo) na qual **multimédia** são armazenados e processados.

>   **Pode, SHOULD, tem, deve não, não pode:** estes termos (em todos os caps) são utilizados, conforme descrito em [[RFC2119].](http://go.microsoft.com/fwlink/?LinkId=90317) Todas as instruções de utilização de comportamento opcional: pode, SHOULD ou não deve.

## <a name="12-references"></a>1.2 referências 
-----------

>   As referências a documentação de especificações aberto da Microsoft não incluem um ano publicação porque as ligações para a versão mais recente dos documentos, que são frequentemente atualizadas. As referências a outros documentos incluem um ano de publicação quando estiver disponível.

 ### <a name="121-normative-references"></a>1.2.1 referências infrações à 

>  [MS-SSTR] Protocolo de transmissão em fluxo uniforme *v20140502* [ http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/. pdf do [MS-SSTR]](http://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-SSTR%5d.pdf)

>   [NORMA ISO/IEC 14496-12] International Organization para uniformização, "tecnologia de informação – codificação dos objetos audio-visual – parte 12: formato de ficheiro de suporte de dados Base ISO", ISO/IEC 14496-12:2014 de 4 de edição, mais Corrigendum 1, Amendments 1 & 2.
>   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c061988_ISO_IEC_14496-12_2012.zip>

>   [NORMA ISO/IEC 14496-15] International Organization para uniformização, "tecnologia de informação – codificação dos objetos audio-visual – parte 15: avanço de vídeo de unidade estruturado NAL o formato de ficheiro de suporte de dados Base do ISO", ISO 14496-15:2015, 3 de edição.
>   <http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=65216>

>   [NORMA ISO/IEC 23008-2] Tecnologia de informação – eficiência elevada codificação e o suporte de dados de entrega em ambientes heterogéneos – parte 2: codificação de vídeo de alta eficiência: edição mais recente ou 2013   <http://standards.iso.org/ittf/PubliclyAvailableStandards/c035424_ISO_IEC_23008-2_2013.zip>

>   [NORMA ISO/IEC 23001 7] Tecnologia de informações — tecnologias de sistemas MPEG-parte 7: encriptação comum no ISO base ficheiros de formato de ficheiro do suporte de dados, 2:2015 CENC edição <http://www.iso.org/iso/catalogue_detail.htm?csnumber=65271>

>   [RFC 6381] IETF RFC-6381, "a 'Codecs' e 'Perfis' parâmetros para"registo"tipos de suportes de dados" <http://tools.ietf.org/html/rfc6381>

>   [MPEG4 RA] A autoridade de registo MP4, "MP4REG" [http://www.mp4ra.org   ](http://go.microsoft.com/fwlink/?LinkId=327787)

>   [RFC2119] Bradner, s.., "chave palavras para utilização num RFCs para níveis de requisito indique" BCP 14, RFC 2119, Março de 1997,   [http://www.rfc-editor.org/rfc/rfc2119.txt   ](http://go.microsoft.com/fwlink/?LinkId=90317)

### <a name="122-informative-references"></a>1.2.2 referências informativos 

>   [MS-GLOS] Microsoft Corporation "*Windows protocolos mestre glossário*."

>   [RFC3548] Josefsson, s.., Ed., "O Base16, Base32 e codificações de dados de Base64", RFC 3548, de 2003 Julho, [http://www.ietf.org/rfc/rfc3548.txt   ](http://go.microsoft.com/fwlink/?LinkId=90432)

>   [RFC5234] Crocker, d, Ed. e Overell, P., "aumentado BNF para especificações de sintaxe: ABNF", STD 68 RFC 5234, de 2008 Janeiro,   [http://www.rfc-editor.org/rfc/rfc5234.txt   ](http://go.microsoft.com/fwlink/?LinkId=123096)


## <a name="13-overview"></a>1.3 Descrição geral de 
---------

>   Apenas as alterações a especificação de transmissão em fluxo uniforme necessário para a entrega de HEVC são especificadas abaixo. Cabeçalhos de secção inalterados são listados para manter a localização na referenciado transmissão em fluxo uniforme especificação [MS-SSTR].

## <a name="14-relationship-to-other-protocols"></a>1.4 relação a outros protocolos 
--------------------------------

## <a name="15-prerequisitespreconditions"></a>1.5 pré-requisitos/pré-condições 
----------------------------

## <a name="16-applicability-statement"></a>1.6 declaração de aplicabilidade 
------------------------

## <a name="17-versioning-and-capability-negotiation"></a>1.7 controlo de versões e negociação de capacidade 
--------------------------------------

## <a name="18-vendor-extensible-fields"></a>1.8 campos fornecedor extensível 
-------------------------

>   DEVERÁ ser utilizado o método seguinte identificar fluxos utilizando o formato de vídeo HEVC:

>   * **Códigos de descritivo personalizado para formatos de suporte de dados:** esta capacidade é fornecida pelo **FourCC** campo, conforme especificado na secção *2.2.2.5*.
>   Os implementadores podem Certifique-se de que as extensões entram em conflito registando códigos de extensão com o RA MPEG4, conforme especificado no [[ISO/IEC-14496-12] ](http://go.microsoft.com/fwlink/?LinkId=183695)

## <a name="19-standards-assignments"></a>1.9 normas atribuições 
----------------------

# <a name="2-messages"></a>2 mensagens 

## <a name="21-transport"></a>2.1 transporte de 
----------

## <a name="22-message-syntax"></a>2.2 sintaxe de mensagem 
---------------

### <a name="221-manifest-request"></a>2.2.1 pedido manifesto 

### <a name="222-manifest-response"></a>2.2.2 resposta manifesto 

#### <a name="2221-smoothstreamingmedia"></a>2.2.2.1 SmoothStreamingMedia 

>   **MinorVersion (variável):** a versão secundária da mensagem de resposta de manifesto. TEM de ser definida para 2. (Não alterar)

>   **Escala temporal (variável):** a escala de tempo do atributo duração, especificado como o número de incrementos num segundo. O valor predefinido é
>   10000000. (Não alterar)

>   O valor recomendado é 90000 para que representa a duração exata dos frames vídeos e fragmentos contendo as vídeo framerate fracional (por exemplo, 30/1.001 Hz).

#### <a name="2222-protectionelement"></a>2.2.2.2 ProtectionElement 

O ProtectionElement deverá existir quando encriptação comum (CENC) foi aplicada ao fluxos de vídeos ou áudio. Fluxos HEVC encriptado em conformidade com a encriptação comum edição 2nd [norma ISO/IEC 23001 7]. Apenas os dados de setor na VCL NAL unidades deverá ser encriptados.

#### <a name="2223-streamelement"></a>2.2.2.3 StreamElement 

>   **StreamTimeScale (variável):** o dimensionamento de tempo para a duração e o tempo de valores neste fluxo especificado como o número de incrementos num segundo. Para fluxos HEVC, recomenda-se um valor de 90000. Para fluxos de áudio, recomenda-se um valor correspondente a frequência de exemplo waveform (por exemplo, 48000 ou 44100).

##### <a name="22231-streamprotectionelement"></a>2.2.2.3.1 StreamProtectionElement

#### <a name="2224-urlpattern"></a>2.2.2.4 UrlPattern 

#### <a name="225-trackelement"></a>2.2.5 TrackElement 

>   **FourCC (variável):** um código de quatro carateres que identifica a qual o formato de suporte de dados é utilizado para cada amostra. O intervalo de valores seguinte está reservado com os significados semânticos seguintes:

>  * "hev1": amostras de vídeo para este controlar utilizam vídeo HEVC, utilizando o formato de descrição de exemplo 'hev1' especificado no [ISO/IEC-14496-15].

>   **CodecPrivateData (variável):** dados que especifica os parâmetros específicos para o formato de suporte de dados e comuns a todas as amostras em controlar, representada como uma cadeia de codificado em hex bytes. O formato e a semântico significado da sequência de bytes varia de acordo com o valor da **FourCC** campo da seguinte forma:

>   * Quando um TrackElement descreve vídeo HEVC, o **FourCC** campo deverá ser igual **"hev1"** e;

>   O **CodecPrivateData** campo deverá conter uma representação de cadeia hexadecimal-coded da seguinte sequência de bytes, especificada no ABNF [[RFC5234]:](http://go.microsoft.com/fwlink/?LinkId=123096) (nenhuma alteração MS SSTR)

>   * %x 00 %x 00 %x 00 %x 01 SPSField %x 00 %x 00 %x 00 %x 01 PPSField

>   * SPSField contém a sequência de parâmetro definido (SP).

>   * PPSField contém o setor parâmetro definido (PPS).

>   Nota: As vídeo parâmetro definido (VPS) não está contido no CodecPrivateData, mas deve estar incluído no cabeçalho do ficheiro de ficheiros armazenados na caixa 'hvcC'. Os sistemas através do protocolo de transmissão em fluxo uniforme tem assinalar descodificar os parâmetros adicionais (por exemplo, a camada de HEVC) utilizando o atributo personalizado "codecs."

##### <a name="22251-customattributeselement"></a>2.2.2.5.1 CustomAttributesElement 

#### <a name="226-streamfragmentelement"></a>2.2.6 StreamFragmentElement 

>   O **MajorVersion do SmoothStreamingMedia** campo tem de ser definido como 2, e **MinorVersion** campo tem de ser definido como 2. (Não alterar)

##### <a name="22261-trackfragmentelement"></a>2.2.2.6.1 TrackFragmentElement 

### <a name="223-fragment-request"></a>2.2.3 pedido de fragmento 

>   **Tenha em atenção**: O formato de suporte de dados predefinido pedida para **MinorVersion** 2 e 'hev1' for 'iso8' marca ISO Base de dados de formato de ficheiro especificada [norma ISO/IEC 14496-12] ISO Base suporte de dados de ficheiro de formato quarta edição e [norma ISO/IEC 23001 7] Edição de segundo encriptação comum.

### <a name="224-fragment-response"></a>2.2.4 resposta de fragmento 

#### <a name="2241-moofbox"></a>2.2.4.1 MoofBox 

#### <a name="2242-mfhdbox"></a>2.2.4.2 MfhdBox 

#### <a name="2243-trafbox"></a>2.2.4.3 TrafBox 

#### <a name="2244-tfxdbox"></a>2.2.4.4 TfxdBox 

>   O **TfxdBox** foi preterido e a respetiva função substituído por controlar fragmento descodificar tempo caixa ('tfdt') especificada na secção [norma ISO/IEC 14496-12] 8.8.12.

>   **Tenha em atenção**: um cliente pode calcular a duração de um fragmento por summing as durações de exemplo apresentadas na caixa de execução de controlar ('trun') ou multiplicando o número de amostras vezes a duração de exemplo predefinida. BaseMediaDecodeTime 'tfdt' plus fragmento duração é igual do parâmetro da hora de URL para o fragmento seguinte.

>   Uma caixa de tempo de referência produtor ('prft') devem ser inserida antes de uma caixa de fragmento de filmes ('moof'), conforme necessário indicar a hora UTC correspondente para o tempo de descodificar de fragmento para controlar o primeiro exemplo referenciado pela caixa de fragmento de filmes, como especificado no [ISO/IEC 14496 secção de-12] 8.16.5.

#### <a name="2245-tfrfbox"></a>2.2.4.5 TfrfBox 

>   O **TfrfBox** foi preterido e a respetiva função substituído por controlar fragmento descodificar tempo caixa ('tfdt') especificada na secção [norma ISO/IEC 14496-12] 8.8.12.

>   **Tenha em atenção**: um cliente pode calcular a duração de um fragmento por summing as durações de exemplo apresentadas na caixa de execução de controlar ('trun') ou multiplicando o número de amostras vezes a duração de exemplo predefinida. BaseMediaDecodeTime 'tfdt' plus fragmento duração é igual do parâmetro da hora de URL para o fragmento seguinte. Procure avançar endereços são preteridos porque estes atrasar a transmissão em fluxo em direto.

#### <a name="2246-tfhdbox"></a>2.2.4.6 TfhdBox 

>   O **TfhdBox** e os campos relacionados encapsulam as predefinições para, por exemplo metadados o fragmento. A sintaxe do **TfhdBox** campo é um subconjunto restrito da sintaxe da caixa de cabeçalho de fragmento de controlar definido no [[ISO/IEC-14496-12]](http://go.microsoft.com/fwlink/?LinkId=183695) secção 8.8.7.

>   **BaseDataOffset (8 bytes):** o deslocamento, em bytes, do início do **MdatBox** campo para o campo de exemplo no **MdatBox** campo. Sinalizar esta restrição, tem de definir o sinalizador de predefinição-base-for-moof (0x020000).

#### <a name="2247-trunbox"></a>2.2.4.7 TrunBox 

>   O **TrunBox** e os campos relacionados encapsulam por metadados de exemplo para o fragmento de pedido. A sintaxe de **TrunBox** é um subconjunto restrito versão 1 controlar fragmento executar da caixa de definido em [[ISO/IEC-14496-](http://go.microsoft.com/fwlink/?LinkId=183695)*12]* secção 8.8.8.

>   **SampleCompositionTimeOffset (4 bytes):** desvio da hora de composição de exemplo a cada exemplo ajustadas para que o tempo de apresentação do primeiro exemplo apresentado o fragmento é igual à hora decode do primeiro exemplo descodificado. Exemplo de vídeo negativo composição desvios deverá ser utilizados,

>   tal como definido no [[ISO/IEC-14496-12].](http://go.microsoft.com/fwlink/?LinkId=183695)

>   Nota: Isto evita a um erro de sincronização de vídeo causado por vídeo lagging áudio igual a atraso de remoção de memória intermédia de imagem descodificados maior e mantém a temporização de apresentação entre fragmentos alternativos que possam ter atrasos de remoção diferentes.

>   A sintaxe dos campos definidos nesta secção, especificado no ABNF [[RFC5234]](http://go.microsoft.com/fwlink/?LinkId=123096) permanece o mesmo, exceto da seguinte forma:

>   SampleCompositionTimeOffset = SIGNED_INT32

#### <a name="2248-mdatbox"></a>2.2.4.8 MdatBox 

#### <a name="2249-fragment-response-common-fields"></a>2.2.4.9 campos comuns de resposta de fragmento 

### <a name="225-sparse-stream-pointer"></a>2.2.5 fluxo disperso ponteiro 

### <a name="226-fragment-not-yet-available"></a>2.2.6 de fragmento ainda não disponível 

### <a name="227-live-ingest"></a>2.2.7 ingestão em direto 

#### <a name="2271-filetype"></a>2.2.7.1 FileType 

>   **FileType (variável):** Especifica a utilização de subtipo e previsto de MPEG-4 ([[MPEG4 RA])](http://go.microsoft.com/fwlink/?LinkId=327787) ficheiro e atributos de alto nível.

>   **MajorBrand (variável):** a marca do ficheiro de suporte de dados principal. TEM de ser definido como "isml."

>   **MinorVersion (variável):** a versão secundária do ficheiro de suporte de dados. TEM de ser definido para 1.

>   **CompatibleBrands (variável):** Especifica as marcas de MPEG-4 suportadas.
>   TEM de incluir "ccff" e "iso8."

>   A sintaxe dos campos definidos nesta secção, especificado no ABNF [[RFC5234]](http://go.microsoft.com/fwlink/?LinkId=123096) é o seguinte:

    FileType = MajorBrand MinorVersion CompatibleBrands
    MajorBrand = STRING_UINT32
    MinorVersion = STRING_UINT32
    CompatibleBrands = "ccff" "iso8" 0\*(STRING_UINT32)

**Tenha em atenção**: as marcas de compatibilidade 'ccff' e 'iso8' indicam que fragmentos está em conformidade com "Formato de ficheiro de contentor comum" e encriptação comum [norma ISO/IEC 23001 7] e ficheiros de suporte de dados de Base de ISO formato 4 edição [norma ISO/IEC 14496-12].

#### <a name="2272-streammanifestbox"></a>2.2.7.2 StreamManifestBox 

##### <a name="22721-streamsmil"></a>2.2.7.2.1 StreamSMIL 

#### <a name="2273-liveservermanifestbox"></a>2.2.7.3 LiveServerManifestBox 

##### <a name="22731-livesmil"></a>2.2.7.3.1 LiveSMIL 

#### <a name="2274-moovbox"></a>2.2.7.4 MoovBox 

#### <a name="2275-fragment"></a>2.2.7.5 o fragmento de 

##### <a name="22751-track-fragment-extended-header"></a>2.2.7.5.1 o fragmento de controlar expandido cabeçalho 

### <a name="228-server-to-server-ingest"></a>2.2.8 ingestão servidor para servidor 

# <a name="3-protocol-details"></a>3 detalhes de protocolo 


## <a name="31-client-details"></a>3.1 detalhes do cliente 

### <a name="311-abstract-data-model"></a>3.1.1 modelo de dados abstrato 

#### <a name="3111-presentation-description"></a>3.1.1.1 Descrição de apresentação 

>   O elemento de dados de descrição de apresentação contém todos os metadados para a apresentação.

>   Metadados de apresentação: Um conjunto de metadados que são comuns a todos os fluxos na apresentação. Metadados de apresentação inclui os seguintes campos especificados na secção *2.2.2.1*:

>   * **MajorVersion**
>   * **MinorVersion**
>   * **TimeScale**
>   * **Duração**
>   * **IsLive**
>   * **LookaheadCount**
>   * **DVRWindowLength**

>   Apresentações que contém HEVC fluxos deverá definir:

    MajorVersion = 2
    MinorVersion = 2

>   LookaheadCount = 0 (Nota: caixas preteridas)

>   Apresentações também devem definir:

    TimeScale = 90000

>   Coleção de fluxo: Uma coleção de elementos de dados de descrição de fluxo, como especificada na secção *3.1.1.1.2*.

>   Descrição de proteção: Uma coleção de elementos de dados de descrição de metadados do sistema de proteção, como especificada na secção *3.1.1.1.1*.

##### <a name="31111-protection-system-metadata-description"></a>3.1.1.1.1 Descrição de metadados do sistema de proteção 

>   O elemento de dados de descrição de metadados do sistema de proteção encapsula metadados específicos para um único sistema de proteção de conteúdo. (Não alterar)

>   Descrição de cabeçalho de proteção: Metadados de proteção de conteúdos pertencente a um único sistema de proteção de conteúdo. Descrição de cabeçalho de proteção inclui os seguintes campos especificados na secção *2.2.2.2*:

>   * **SystemID**
>   * **ProtectionHeaderContent**

##### <a name="31112-stream-description"></a>3.1.1.1.2 descrição do fluxo de 

###### <a name="311121-track-description"></a>3.1.1.1.2.1 Descrição de controlar 

###### <a name="3111211-custom-attribute-description"></a>3.1.1.1.2.1.1 Descrição do atributo personalizado 

##### <a name="3113-fragment-reference-description"></a>3.1.1.3 descrição de referência de fragmento 

###### <a name="31131-track-specific-fragment-reference-description"></a>3.1.1.3.1 Descrição de referência de fragmento controlar específico 

#### <a name="3112-fragment-description"></a>3.1.1.2 descrição de fragmento 

##### <a name="31121-sample-description"></a>3.1.1.2.1 Descrição de exemplo 

### <a name="312-timers"></a>3.1.2 os temporizadores de 

### <a name="313-initialization"></a>3.1.3 inicialização de 

### <a name="314-higher-layer-triggered-events"></a>3.1.4 camada superior de Acionada eventos 

#### <a name="3141-open-presentation"></a>3.1.4.1 apresentação aberta 

#### <a name="3142-get-fragment"></a>3.1.4.2 obter fragmento 

#### <a name="3143-close-presentation"></a>3.1.4.3 fechar apresentação 

### <a name="315-processing-events-and-sequencing-rules"></a>3.1.5 a processar eventos e as regras de sequenciação 

#### <a name="3151-manifest-request-and-manifest-response"></a>3.1.5.1 manifesto do pedido e resposta de manifesto 

#### <a name="3152-fragment-request-and-fragment-response"></a>3.1.5.2 pedido de fragmento e resposta de fragmento

## <a name="32-server-details"></a>3.2 detalhes do servidor de

## <a name="33-live-encoder-details"></a>3.3 detalhes do codificador em direto 

# <a name="4-protocol-examples"></a>4 exemplos de protocolo 

# <a name="5-security"></a>Segurança 5 

## <a name="51-security-considerations-for-implementers"></a>5.1 considerações de segurança de para os implementadores 
-----------------------------------------

>   Se o conteúdo transportado utilizar este protocolo tem valor comercial, um sistema de proteção de conteúdos deve ser utilizado para impedir a utilização não autorizada do conteúdo. O **ProtectionElement** pode ser utilizado para transportar os metadados relacionados com a utilização de um sistema de proteção de conteúdo. Protegido de áudio e vídeo conteúdos encriptados conforme especificado pela MPEG comuns encriptação segunda edição: 2015 [norma ISO/IEC 23001 7].

>   **Tenha em atenção**: para vídeo HEVC, apenas os dados de setor na VCL NALs são encriptados. Cabeçalhos do setor e outros NALs estão acessíveis para aplicações de apresentação antes de desencriptação. um caminho de vídeo seguro, informações encriptadas não estão disponíveis para aplicações de apresentação.

# <a name="52-index-of-security-parameters"></a>5.2 índice de parâmetros de segurança 
-----------------------------


| **Parâmetro de segurança**  | **Secção**         |
|-------------------------|---------------------|
| ProtectionElement       | *2.2.2.2*           |
| Caixas de encriptação comum | *[ISO/IEC 23001-7]* |

# <a name="53-common-encryption-boxes"></a>5.3 caixas de encriptação comuns
-----------------------

As seguintes caixas poderão estar presentes no respostas de fragmento quando encriptação comum é aplicada e estão especificados na [norma ISO/IEC 23001 7] ou [norma ISO/IEC 14496-12]:

1.  Proteção sistema específico cabeçalho caixa ('pssh')

2.  Exemplo de caixa de encriptação ('senc')

3.  Exemplo auxiliar informações de deslocamento caixa ('saio')

4.  Caixa de tamanho de informações de auxiliar do exemplo ('saiz')

5.  Caixa de descrição de grupo de exemplo ('sgpd')

6.  Exemplo para a caixa de grupo ('sbgp')

-----------------------

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png
