---
title: "Notas de versão de Media Services | Microsoft Docs"
description: "Notas de versão de Media Services"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ca2d7af-1cf0-45fa-9585-3b73f3ee057d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 10/18/2017
ms.author: juliako
ms.openlocfilehash: 4775374b7e91930daa686e48e2869b4891615c4c
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="azure-media-services-release-notes"></a>Notas de versão de Media Services do Azure
Estas notas de versão para Media Services do Azure resumem as alterações de versões anteriores e problemas conhecidos.

> [!NOTE]
> Queremos ouvi dos nossos clientes, de modo a que iremos poder concentrar na corrigir problemas que afetam. Para comunicar um problema ou colocar questões, submeter um pedido post no [fórum MSDN do Azure Media Services].
> 
> 

## <a name="a-idissuescurrently-known-issues"></a><a id="issues"/>Problemas conhecidos atualmente
### <a name="a-idgeneralissuesmedia-services-general-issues"></a><a id="general_issues"/>Problemas gerais de Media Services

| Problema | Descrição |
| --- | --- |
| Vários cabeçalhos HTTP comuns não são fornecidos na REST API. |Se desenvolver aplicações de Media Services utilizando a API REST, encontrará que alguns campos de cabeçalho HTTP comuns (incluindo CLIENT-REQUEST-ID, o ID de pedido e retorno-CLIENT-REQUEST-ID) não são suportadas. Os cabeçalhos serão adicionados numa atualização futura. |
| Não é permitida a codificação de percentagem. |Os Media Services utiliza o valor da propriedade IAssetFile.Name ao criar os URLs para os conteúdos de transmissão em fluxo (por exemplo, http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters). Por este motivo, a percentagem de codificação não é permitida. O valor da propriedade de nome não pode ter qualquer um dos seguintes [por cento codificação-reservados carateres](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Além disso, podem existir apenas um "." para a extensão de nome de ficheiro. |
| O método de ListBlobs que faz parte da falha de 3 de versão do SDK de armazenamento do Azure. |Os Media Services gera os URLs de SAS com base no [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) versão. Se pretender utilizar o SDK de armazenamento para blobs de lista num contentor de BLOBs, utilize o [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) método que faz parte da versão do SDK de armazenamento de 2. x. |
| Os serviços de suporte de dados de limitação mecanismo restringe a utilização de recursos para aplicações que efetuam pedidos excessivos no serviço. O serviço poderá devolveu o código de estado HTTP "Serviço indisponível" 503. |Para obter mais informações, consulte a descrição do código de estado HTTP 503 no [códigos de erro de Media Services](media-services-encoding-error-codes.md). |
| Quando consultar entidades, um limite de 1000 entidades é devolvido em simultâneo porque o resto público versão 2 limita os resultados da consulta 1.000 resultados. |Utilize ignorar e tomar (.NET) / principais (REST), conforme descrito em [neste exemplo .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) e [neste exemplo de REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Alguns clientes podem ter através de um problema de repetições etiqueta no manifesto de transmissão em fluxo uniforme. |Para obter mais informações, consulte [nesta secção](media-services-deliver-content-overview.md#known-issues). |
| Objetos de SDK .NET dos Media Services não podem ser serializados e não funcionam com a Cache de Redis do Azure. |Se tentar serializar o objeto de SDK AssetCollection adicioná-lo para a Cache de Redis do Azure, é emitida uma exceção. |


## <a name="a-idrestversionhistoryrest-api-version-history"></a><a id="rest_version_history"/>Histórico da versão de REST API
Para obter informações sobre o histórico da versão de API de REST dos serviços de suporte de dados, consulte o [referência da API de REST de serviços de suporte de dados do Azure].

## <a name="october-2017-release"></a>Versão de Outubro de 2017
> [!IMPORTANT] 
> Serviços de suporte de dados está a descontinuar o suporte para chaves de autenticação do serviço de controlo de acesso do Azure. No dia 1 de Junho de 2018, já não pode autenticar com o serviços de suporte de dados de back-end através de código através da utilização de chaves do serviço de controlo de acesso. Tem de atualizar o código para utilizar o Azure Active Directory (Azure AD) por [authentication do Azure baseada no AD](media-services-use-aad-auth-to-access-ams-api.md). Procurar avisos sobre esta alteração no portal do Azure.

### <a name="updates-for-october-2017"></a>Atualizações de Outubro de 2017
#### <a name="sdks"></a>SDKs
* O SDK .NET foi atualizado para suportar a autenticação do Azure AD. Suporte para a autenticação do serviço de controlo de acesso foi removido do SDK mais recente do .NET no Nuget.org a encorajar migração mais rápida para o Azure AD. 
* O SDK de JAVA foi atualizado para suportar a autenticação do Azure AD. Foi adicionado suporte para a autenticação do Azure AD para o SDK de Java. Para obter informações sobre como utilizar o SDK de Java com os Media Services, consulte [começar com o cliente de Java SDK de Media Services do Azure](media-services-java-how-to-use.md)

#### <a name="file-based-encoding"></a>Codificação baseados em ficheiros
* Agora, pode utilizar o codificador Premium para codificar o conteúdo para o vídeo de alta-eficiência H.265 codificação codec de vídeo (HEVC). Não há nenhum impacto preço se escolher H.265 através de outros codecs, tais como 264. Para obter informações sobre licenças patente HEVC, consulte [termos de serviço Online](https://azure.microsoft.com/support/legal/).
* Para as vídeo de origem que está codificada com o codec de vídeo H.265 (HEVC), tais como as vídeo capturado utilizando iOS11 ou GoPro heroína 6, agora, pode utilizar o codificador Premium ou o codificador padrão para codificar os vídeos. Para obter informações sobre licenças patente, consulte [termos de serviço Online](https://azure.microsoft.com/support/legal/).
* Para conteúdo que contém vários controla de áudio de idioma, os valores de idioma tem de ser corretamente com a etiqueta, de acordo com a especificação de formato de ficheiro (por exemplo, MP4 ISO) correspondente. Em seguida, pode utilizar o codificador padrão para codificar o conteúdo de transmissão em fluxo. O localizador de transmissão em fluxo resultante lista os idiomas de áudio disponíveis.
* Codificador padrão suporta agora dois novos só de áudio predefinições do sistema, "AAC áudio" e "AAC boa qualidade áudio". Ambos produzem stereo avançadas áudio codificação saída (AAC), às taxas de bits de 128 Kbps e 192 Kbps, respetivamente.
* Codificador Premium suporta agora formatos de ficheiro QuickTime/MOV como entrada. O codec vídeo tem de ser o [Apple ProRes tipos apresentadas neste artigo do GitHub](https://docs.microsoft.com/azure/media-services/media-services-media-encoder-standard-formats). Áudio tem de ser ambos AAC ou impulsos modulação de código (PCM). Codificador Premium não suporta, por exemplo, as vídeo DVC/DVCPro encapsulada nos ficheiros de QuickTime/MOV como entrada. Codificador padrão suporta estas codecs vídeos.
* Foram efetuadas as seguintes correções de erros codificadores:

    * Agora pode submeter tarefas ao utilizar um recurso de entrada. Depois de concluir estas tarefas, pode modificar o elemento (por exemplo, adicionar, eliminar ou mudar o nome de ficheiros dentro do elemento) e submeter as tarefas adicionais.
    * A qualidade de miniaturas JPEG produzido pelo codificador padrão é melhorada.
    * Codificador padrão processa metadados de entrada e de geração em miniatura melhor no vídeos tão curta duração.
    * Melhoramentos para o descodificador 264 utilizado no codificador padrão eliminar determinados raros artefactos. 

#### <a name="media-analytics"></a>Análise de Multimédia
Disponibilidade geral do Redactor de suporte de dados do Azure: este processador de multimédia efetua anonymization por diária esbater pessoal faces de indivíduos selecionados e é ideal para utilização em situações de segurança e de suporte de dados de notícias públicas. 

Para uma descrição geral sobre esta nova processador, consulte [esta mensagem de blogue](https://azure.microsoft.com/blog/azure-media-redactor/). Para obter informações sobre a documentação e as definições, consulte [Redact faces análise de multimédia do Azure](media-services-face-redaction.md).



## <a name="june-2017-release"></a>Versão de Junho de 2017

Os Media Services agora suporta [authentication do Azure baseada no AD](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Atualmente, os Media Services suportam o modelo de autenticação do serviço de controlo de acesso. Autorização de serviço de controlo de acesso será preterida no dia 1 de Junho de 2018. Recomendamos que migre para o modelo de autenticação do Azure AD assim que for possível.

## <a name="march-2017-release"></a>Versão de Março de 2017

Agora, pode utilizar o codificador padrão para [gerar automaticamente um ladder de velocidade de transmissão](media-services-autogen-bitrate-ladder-with-mes.md) especificando o "transmissão em fluxo adaptável" cadeia de configuração predefinida quando cria uma tarefa de codificação. Codificar um vídeo para transmissão em fluxo com os Media Services, utilize a predefinição "Transmissão em fluxo adaptável". Para personalizar uma codificação da configuração predefinida para o seu cenário específico, pode começar com [estas predefinições](media-services-mes-presets-overview.md).

Agora pode utilizar o codificador de multimédia Standard ou o fluxo de trabalho do suporte de dados codificador Premium para [criar uma tarefa de codificação que gera fMP4 segmentos](media-services-generate-fmp4-chunks.md). 

## <a name="february-2017-release"></a>Versão de Fevereiro de 2017

A partir de 1 de Abril de 2017, qualquer registo de tarefas na sua conta mais antiga do que 90 dias é eliminado automaticamente, juntamente com os respetivos registos de tarefas associada. Eliminação ocorre mesmo que o número total de registos é inferior a quota máxima. Para arquivar as informações de tarefa de tarefas, pode utilizar o código descrito [gerir recursos e entidades relacionadas com o SDK de Media Services .NET](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>Versão de Janeiro de 2017

Nos Media Services, um ponto final de transmissão em fluxo representa um serviço de transmissão em fluxo que pode proporcionar o conteúdo diretamente a uma aplicação de leitor de cliente ou a uma rede de entrega de conteúdos (CDN) para uma maior distribuição. Os Media Services também fornecem uma integração perfeita rede de entrega de conteúdos do Azure. O fluxo de saída de um serviço de StreamingEndpoint pode ser uma transmissão em fluxo em direto, um vídeo a pedido ou uma transferência progressiva do seu elemento na sua conta de Media Services. Cada conta de Media Services inclui uma predefinição de ponto final de transmissão em fluxo. Os pontos finais de transmissão em fluxo adicionais podem ser criados com a conta. 

Existem duas versões de transmissão em fluxo pontos finais, 1.0 e 2.0. A partir de 10 de Janeiro de 2017, as contas de serviços de suporte de dados recentemente criadas incluem a predefinição de versão 2.0 ponto final de transmissão em fluxo. Os pontos finais transmissão em fluxo adicionais que adicionar a esta conta também são versão 2.0. Esta alteração não afeta as contas existentes. Os pontos finais de transmissão em fluxo existentes têm a versão 1.0 e podem ser atualizados para a versão 2.0. Existem comportamento, faturação e alterações de funcionalidade com esta alteração. Para obter mais informações, consulte [descrição geral de pontos finais de transmissão em fluxo](media-services-streaming-endpoints-overview.md).

Começando com a versão 2.15, os Media Services adicionadas as seguintes propriedades para a entidade de ponto final de transmissão em fluxo:

* CdnProvider 
* CdnProfile
* FreeTrialEndTime 
* StreamingEndpointVersion 

Para obter mais informações sobre estas propriedades, consulte [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>Versão de Dezembro de 2016

 Agora, pode utilizar os Media Services para aceder a dados de telemetria/métricas para os respetivos serviços. Pode utilizar a versão atual dos serviços de suporte de dados para recolher dados de telemetria para canais em direto, ponto final de transmissão em fluxo e arquivar entidades. Para obter mais informações, consulte [telemetria de Media Services](media-services-telemetry-overview.md).

## <a name="a-idjulychanges16july-2016-release"></a><a id="july_changes16"/>Versão de Julho de 2016
### <a name="updates-to-the-manifest-file-ism-generated-by-encoding-tasks"></a>Atualizações para o ficheiro de manifesto (*. ISM) gerado por tarefas de codificação
Quando uma tarefa de codificação foi submetida para o codificador de multimédia Standard ou Premium do codificador de suporte de dados, a tarefa de codificação gera um [ficheiro de manifesto de transmissão em fluxo](media-services-deliver-content-overview.md) (* ISM) no elemento de saída. Com a versão mais recente do serviço, a sintaxe deste ficheiro de manifesto de transmissão em fluxo foi atualizada.

> [!NOTE]
> A sintaxe do ficheiro de manifesto (ISM) transmissão em fluxo está reservada para utilização interna. Trata-se sujeita a alterações em futuros lançamentos. Não modifique ou manipular os conteúdos deste ficheiro.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Um manifesto de cliente novo (*. Ficheiro ISMC) é gerado no elemento de saída quando uma tarefa de codificação produz um ou mais ficheiros MP4
Começando com a versão mais recente do serviço, após a conclusão de uma tarefa de codificação que gera um ou mais ficheiros MP4, o elemento de saída contém também um ficheiro de manifesto (*.ismc) transmissão em fluxo do cliente. O ficheiro .ismc ajuda a melhorar o desempenho de transmissão em fluxo dinâmico. 

> [!NOTE]
> A sintaxe do ficheiro de manifesto (.ismc) do cliente está reservada para utilização interna. Trata-se sujeita a alterações em futuros lançamentos. Não modifique ou manipular os conteúdos deste ficheiro.
> 
> 

Para obter mais informações, consulte [este blogue](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/).

### <a name="known-issues"></a>Problemas conhecidos
Alguns clientes podem ter através de um problema de repetições etiqueta no manifesto de transmissão em fluxo uniforme. Para obter mais informações, consulte [nesta secção](media-services-deliver-content-overview.md#known-issues).

## <a id="apr_changes16"></a>Versão de Abril de 2016
### <a name="media-analytics"></a>Análise de Multimédia
 Os Media Services introduzida análise de multimédia para intelligence vídeo poderoso. Para obter mais informações, consulte [descrição geral da análise de serviços de suporte de dados](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (pré-visualização)
Agora, pode utilizar os Media Services encriptar de forma dinâmica a sua HTTP Live Streaming (HLS) conteúdo do FairPlay da Apple. Também pode utilizar o serviço de entrega de licença de Media Services para fornecer licenças do FairPlay aos clientes. Para obter mais informações, consulte "Utilizar serviços de multimédia do Azure para transmitir o seu conteúdo HLS protegido com do FairPlay da Apple."

## <a id="feb_changes16"></a>Versão de Fevereiro de 2016
A versão mais recente do que o SDK de Media Services para .NET (3.5.3) contém uma correção de erros relacionados com Widevine da Google. Foi impossível reutilizar AssetDeliveryPolicy para vários recursos encriptados com Widevine. Como parte desta correção de erro, a seguinte propriedade foi adicionada ao SDK: WidevineBaseLicenseAcquisitionUrl.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>Versão de Janeiro de 2016
Unidades reservadas de codificação foram mudadas para reduzir confusões com nomes de codificador.

As unidades de reservado codificação básicas, Standard e Premium foram mudadas para S1, S2, e unidades reservadas de S3, respetivamente. Os clientes que utilizam unidades reservadas de codificação básicas hoje veem S1 como a etiqueta no portal do Azure (e na factura). Os clientes que utilizam Standard e Premium ver as etiquetas S2 e S3, respetivamente. 

## <a id="dec_changes_15"></a>Versão de Dezembro de 2015

### <a name="media-encoder-deprecation-announcement"></a>Anúncio de descontinuação do codificador de multimédia

 Codificador de multimédia vão ser preterida a partir de aproximadamente 12 meses a partir da versão de codificador de multimédia Standard.

### <a name="azure-sdk-for-php"></a>SDK do Azure para PHP
A equipa do Azure SDK publicado uma nova versão do [Azure SDK para PHP](http://github.com/Azure/azure-sdk-for-php) pacote que contém as atualizações e novas funcionalidades para os Media Services. Em particular, o SDK de Media Services para PHP agora suporta a versão mais recente [conteúdo proteção](media-services-content-protection-overview.md) funcionalidades. Estas funcionalidades são encriptação dinâmica com AES e DRM (PlayReady e Widevine) com e sem restrições de token. Também suporta dimensionamento [unidades codificação](media-services-dotnet-encoding-units.md).

Para obter mais informações, consulte:

* O [SDK de Media Services para PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blogue.
* O seguinte [exemplos de código](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) ajudá-lo a começar rapidamente a:
  * **vodworkflow_aes.php**: ficheiro este PHP mostra como utilizar a encriptação dinâmica AES-128 e o serviço de entrega de chave. Se baseia no exemplo .NET explicado [encriptação dinâmica de utilizar AES-128 e o serviço de entrega de chave](media-services-protect-with-aes128.md).
  * **vodworkflow_aes.php**: ficheiro este PHP mostra como utilizar a encriptação dinâmica PlayReady e o serviço de entrega de licença. Se baseia no exemplo .NET explicado [utilize PlayReady e/ou Widevine a encriptação comum dinâmica](media-services-protect-with-playready-widevine.md).
  * **scale_encoding_units.php**: ficheiro este PHP mostra como escalar unidades reservadas de codificação.

## <a id="nov_changes_15"></a>Versão de Novembro de 2015
 Os Media Services oferece agora o serviço de entrega de licença Widevine na nuvem. Para obter mais informações, consulte [este blogue](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Além disso, consulte [neste tutorial](media-services-protect-with-playready-widevine.md) e [repositório do GitHub](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Serviços de entrega de licença Widevine fornecidos pelos serviços de suporte de dados estão em pré-visualização. Para obter mais informações, consulte [este blogue](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>Versão de Outubro de 2015
Serviços de suporte de dados está agora em direto nos seguintes centros de dados: sul do Brasil, Índia Ocidental, Sul da Índia e Índia Central. Agora, pode utilizar o portal do Azure para [criar contas de serviço de multimédia](media-services-portal-create-account.md) e realizar várias tarefas descritas no [página Web de documentação de Media Services](https://azure.microsoft.com/documentation/services/media-services/). Codificação em direto não está ativada nestes centros de dados. Além disso, nem todos os tipos de unidades codificação reservada estão disponíveis nestes centros de dados.

* Sul do Brasil: Só estão disponível, Standard e Basic unidades codificação reservada.
* Índia Ocidental, Sul da Índia e Índia Central: apenas básico unidades codificação reservada estão disponível.

## <a id="september_changes_15"></a>Versão de Setembro de 2015
Agora, os Media Services oferece a capacidade de proteger ambas as vídeo a pedido e fluxos em direto com a tecnologia de DRM modular Widevine. Pode utilizar os seguintes parceiros de serviços de entrega para o ajudar a fornecer licenças Widevine:
* [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) 
* [EZDRM](http://ezdrm.com/) 
* [castLabs](http://castlabs.com/company/partners/azure/) 

Para obter mais informações, consulte [este blogue](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
Pode utilizar o [SDK .NET dos Media Services](https://www.nuget.org/packages/windowsazure.mediaservices/) (começando com a versão 3.5.1) ou a API REST para configurar AssetDeliveryConfiguration para utilizar Widevine. 
* Serviços de suporte de dados foi adicionado suporte para Apple ProRes vídeos. Pode agora carregar os ficheiros de vídeos de origem QuickTime utilizam Apple ProRes ou outros codecs. Para obter mais informações, consulte [este blogue](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* Agora, pode utilizar codificador de multimédia Standard para extração de arquivo subclipping e em direto. Para obter mais informações, consulte [este blogue](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* Foram efetuadas as seguintes atualizações de filtragem: 
  
  * Agora pode utilizar o formato de Apple HLS com um filtro de só de áudio. Pode utilizar esta atualização para remover um controlar só de áudio, especificando (só de áudio = false) no URL.
  * Quando definir os filtros para os elementos, agora pode combinar vários filtros (até três) num URL único.
    
    Para obter mais informações, consulte [este blogue](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).
* Os Media Services suportam agora posso-frames Jumbo no HLS versão 4. Suporte de I-frame otimiza fast-forward e rewind operações. Por predefinição, todas as saídas da versão 4 de HLS incluem a lista de reprodução de I-frame (EXT-X-I-FRAME-STREAM-INF).
Para obter mais informações, consulte [este blogue](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).

## <a id="august_changes_15"></a>Versão de Agosto de 2015
* O SDK de Media Services para a versão do Java versão 0.8.0 e amostras novo estão agora disponíveis. Para obter mais informações, consulte:
  
  * [Esta mensagem de blogue](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
  * [O repositório de exemplos de Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
* O leitor de multimédia do Azure foi atualizado com suporte de sequência de áudio múltiplos. Para obter mais informações, consulte [esta mensagem de blogue](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/).

## <a id="july_changes_15"></a>Versão de Julho de 2015
* Foi anunciou a disponibilidade geral do codificador de multimédia Standard. Para obter mais informações, consulte [esta mensagem de blogue](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Codificador de multimédia Standard utiliza predefinições, conforme descrito em [nesta secção](http://go.microsoft.com/fwlink/?LinkId=618336). Quando utiliza uma predefinição de 4K codifica, obter o tipo de unidade Premium reservado. Para obter mais informações, consulte [escala codificação](media-services-scale-media-processing-overview.md).
* Foram utilizadas legendas em tempo real em direto com Media Services e o leitor de multimédia. Para obter mais informações, consulte [esta mensagem de blogue](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/).

### <a name="media-services-net-sdk-updates"></a>Atualizações do SDK .NET dos Media Services
O SDK do .NET dos serviços de suporte de dados está agora versão 3.4.0.0. Foram efetuadas as seguintes atualizações: 

* Suporte foi implementado para arquivo em direto. Não é possível transferir um elemento que contenha um arquivo em direto.
* Suporte foi implementado para filtros dinâmicos.
* Funcionalidade foi implementada para que os utilizadores podem manter um contentor de armazenamento, enquanto que eliminar um recurso.
* Correções de erros foram efetuadas relacionadas para repetir as políticas de canais.
* O fluxo de trabalho do suporte de dados codificador Premium foi ativado.

## <a id="june_changes_15"></a>Versão de Junho de 2015
### <a name="media-services-net-sdk-updates"></a>Atualizações do SDK .NET dos Media Services
O SDK do .NET dos serviços de suporte de dados está agora versão 3.3.0.0. Foram efetuadas as seguintes atualizações: 

* Foi adicionado suporte para a especificação de deteção OpenId Connect.
* Foi adicionado suporte para processamento de rollover de chaves do lado do fornecedor de identidade.

Se utilizar um fornecedor de identidade que expõe um documento de identificação OpenID Connect (como o Azure AD, Google e da Salesforce fazer), pode instruir os Media Services para obter as chaves de assinatura de validação de JSON Web Tokens (JWTs) da especificação de deteção OpenID Connect. 

Para obter mais informações, consulte [chaves de web JSON de utilização da especificação de deteção OpenID Connect para trabalhar com a autenticação nos serviços de suporte de dados JWT](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>Versão de Maio de 2015
As seguintes novas funcionalidades foram anunciadas:

* [Uma versão de pré-visualização de live encoding com Media Services](media-services-manage-live-encoder-enabled-channels.md)
* [Manifesto dinâmico](media-services-dynamic-manifest-overview.md)
* [Uma versão de pré-visualização do processador de multimédia Hyperlapse de multimédia do Azure](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>Versão de Abril de 2015
### <a name="general-media-services-updates"></a>Serviços de suporte de dados gerais de atualizações
* [Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/) foi comunicada.
* Canais que estão configurados para a ingestão de um protocolo de mensagens de em tempo real (RTMP) a partir de 2.10 de REST de serviços de suporte de dados, são criados com o principal e secundário os URLs de inserção. Para obter mais informações, consulte [configurações de inserção do canal](media-services-live-streaming-with-onprem-encoders.md#channel_input).
* Indexador de suporte de dados do Azure foi atualizado.
* Foi adicionado suporte para o idioma espanhol.
* Foi adicionada uma nova configuração para o formato XML.

Para obter mais informações, consulte [este blogue](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Atualizações do SDK .NET dos Media Services
O SDK do .NET dos serviços de suporte de dados está agora a versão 3.2.0.0. Foram efetuadas as seguintes atualizações:

* Alteração de última hora: TokenRestrictionTemplate.Issuer e TokenRestrictionTemplate.Audience foram alterados para ter um tipo de cadeia.
* Foram efectuadas relacionadas com a criação de políticas de repetição personalizado.
* Correções de erros foram efetuadas relacionadas para carregar e transferir ficheiros.
* A classe de MediaServicesCredentials agora aceita pontos finais de controlo de acesso primária e secundária para se autenticarem nas.

## <a id="march_changes_15"></a>Versão de Março de 2015
### <a name="general-media-services-updates"></a>Serviços de suporte de dados gerais de atualizações
* Os Media Services fornecem agora integração de rede de entrega de conteúdos. Para suportar a integração, a propriedade CdnEnabled foi adicionada ao StreamingEndpoint. CdnEnabled pode ser utilizado com as APIs REST a partir da versão 2.9. Para obter mais informações, consulte [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). CdnEnabled pode ser utilizado com o SDK .NET a partir da versão 3.1.0.2. Para obter mais informações, consulte [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx).
* O fluxo de trabalho do suporte de dados codificador Premium foi comunicado. Para obter mais informações, consulte [Introducing Premium codificação na Media Services do Azure](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>Versão de Fevereiro de 2015
### <a name="general-media-services-updates"></a>Serviços de suporte de dados gerais de atualizações
A API de REST de serviços de suporte de dados está agora versão 2.9. Começando com esta versão, pode ativar a integração de rede de entrega de conteúdos com pontos finais de transmissão em fluxo. Para obter mais informações, consulte [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>Versão de Janeiro de 2015
### <a name="general-media-services-updates"></a>Serviços de suporte de dados gerais de atualizações
Foi anunciou a disponibilidade geral da proteção de conteúdos com a encriptação dinâmica. Para obter mais informações, consulte [dos Media Services melhora a segurança de transmissão em fluxo com disponibilidade geral da tecnologia DRM](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Atualizações do SDK .NET dos Media Services
O SDK do .NET dos serviços de suporte de dados está agora versão 3.1.0.1.

Esta versão marcado o construtor de Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate predefinido como obsoleto. Este novo construtor aceita TokenType como um argumento.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>Versão de Dezembro de 2014
### <a name="general-media-services-updates"></a>Serviços de suporte de dados gerais de atualizações
* Algumas atualizações e novas funcionalidades adicionadas para o indexador de suporte de dados. Para obter mais informações, consulte [notas de versão do indexador de suporte de dados do Azure versão 1.1.6.7](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Foi adicionada uma nova API de REST a que pode utilizar ao atualizar a codificação de unidades reservadas. Para obter mais informações, consulte [EncodingReservedUnitType com REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* Foi adicionado suporte CORS para o serviço de entrega de chave.
* Melhorias de desempenho foram efetuadas para consultar as opções de política de autorização.
* No Centro de dados China, o [URL de entrega de chave](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) é agora por cliente (tal como outros centros de dados).
* Duração de destino HLS automática foi adicionada. Ao efetuar a transmissão em fluxo em direto, HLS é sempre empacotada dinamicamente. Por predefinição, os Media Services calcula automaticamente o HLS segmento empacotamento rácio (FragmentsPerSegment) com base num intervalo de keyframe (KeyFrameInterval). Este método é também referido como um grupo de imagens (GOP) que é recebido do codificador em direto. Para obter mais informações, consulte [transmissão em fluxo em direto de trabalho com os Media Services](http://msdn.microsoft.com/library/azure/dn783466.aspx).

### <a name="media-services-net-sdk-updates"></a>Atualizações do SDK .NET dos Media Services
O [SDK .NET dos Media Services](http://www.nuget.org/packages/windowsazure.mediaservices/) está agora versão 3.1.0.0. Foram efetuadas as seguintes atualizações:

* A dependência do SDK do .NET foi atualizada para o .NET 4.5 Framework.
* Foi adicionada uma nova API que pode utilizar ao atualizar a codificação de unidades reservadas. Para obter mais informações, consulte [atualização reservados de um tipo de unidade e o aumento unidades codificação reservada utilizando o .NET](media-services-dotnet-encoding-units.md).
* Foi adicionado o suporte para autenticação de token JWT. Para obter mais informações, consulte [autenticação de token JWT nos serviços de suporte de dados e a encriptação dinâmica](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* Foram adicionados desvios relativos para BeginDate e ExpirationDate no modelo de licença PlayReady.

## <a id="november_changes_14"></a>Versão de Novembro de 2014
* Agora, pode utilizar os Media Services para inserir conteúdos de transmissão em fluxo uniforme (fMP4) em direto através de uma ligação SSL. Para a ingestão através de SSL, certifique-se atualizar o URL de inserção para HTTPS. Atualmente, os Media Services não suporta SSL com domínios personalizados. Para obter mais informações sobre a transmissão em fluxo em direto, consulte [trabalhar com o Azure Media Services em direto de transmissão em fluxo](http://msdn.microsoft.com/library/azure/dn783466.aspx).
* Atualmente, não é possível inserir um fluxo em direto RTMP através de uma ligação SSL.
* Pode transmitir através de SSL apenas se o ponto de final transmissão em fluxo a partir da qual a fornecer o conteúdo foi criado após 10 de Setembro de 2014. Se os URLs de transmissão em fluxo baseiam-se nos pontos finais de transmissão em fluxo criados após 10 de Setembro de 2014, o URL contém "streaming.mediaservices.windows.net" (o novo formato). URL de transmissão em fluxo que contenham "origin.mediaservices.windows.net" (o formato antigo) não suporta SSL. Se o seu URL tem um formato antigo e pretende que seja transmitido em fluxo através de SSL, [criar um novo ponto de final de transmissão em fluxo](media-services-portal-manage-streaming-endpoints.md). Para transmitir o seu conteúdo através de SSL, utilize os URLs com base no novo ponto de final de transmissão em fluxo.

## <a id="october_changes_14"></a>Versão de Outubro de 2014
### <a id="new_encoder_release"></a>Versão de codificador de serviços de multimédia
 Foi anunciou a nova versão do codificador de multimédia do suporte de dados de serviços do Azure. Com o codificador de multimédia mais recente, a de lhe cobrados apenas GBs de saída. Caso contrário, o codificador nova é compatível com o codificador anterior de funcionalidade. Para obter mais informações, consulte [detalhes de preços dos Media Services].

### <a id="oct_sdk"></a>.NET SDK dos Media Services
O SDK de Media Services para extensões de .NET é agora versão 2.0.0.3.

O SDK de Media Services para .NET é agora versão 3.0.0.8. Foram efetuadas as seguintes atualizações:

* Refactoring foi implementado em classes de política de repetição.
* Uma cadeia de agente do utilizador foi adicionada a cabeçalhos de pedido HTTP.
* Foi adicionado um passo de compilação de restauro do NuGet.
* Testes de cenário foram corrigidos para utilizar x509 cert do repositório.
* Foram adicionadas definições de validação para quando atualizar o canal e o fim de transmissão em fluxo.

### <a name="new-github-repository-to-host-media-services-samples"></a>Novo repositório do GitHub para exemplos de Media Services do anfitrião
São exemplos no [repositório do GitHub de amostras de Media Services](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>Versão de Setembro de 2014
Os metadados de REST de serviços de suporte de dados estão agora versão 2.7. Para mais informações sobre as atualizações mais recentes do REST, consulte o [referência da API de REST de serviços de suporte de dados](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

O SDK de Media Services para .NET é agora versão 3.0.0.7

### <a id="sept_14_breaking_changes"></a>As alterações de última hora
* Origem foi mudada para [StreamingEndpoint].
* Foi efetuada uma alteração no comportamento predefinido ao utilizar o portal do Azure para codificar e, em seguida, publicar ficheiros MP4.

### <a id="sept_14_GA_changes"></a>Novos funcionalidades/cenários que fazem parte da versão de disponibilidade geral
* O processador de multimédia do indexador de suporte de dados foi introduzido. Para obter mais informações, consulte [ficheiros de suporte de dados com o indexador de suporte de dados do índice](http://msdn.microsoft.com/library/azure/dn783455.aspx).
* Pode utilizar o [StreamingEndpoint] entidade para adicionar nomes de domínio personalizado (anfitrião).
  
    Para utilizar um nome de domínio personalizado, como o nome de ponto final de transmissão em fluxo de Media Services, adicione nomes de anfitrião personalizado para o ponto final de transmissão em fluxo. Utilize as APIs de REST de serviços de suporte de dados ou o SDK .NET para adicionar nomes de anfitrião personalizado.
  
    Aplicam as seguintes considerações:
  
  * Tem de ter a propriedade de nome de domínio personalizado.
  * A propriedade do nome de domínio tem de ser validada pelos Media Services. Para validar o domínio, crie um CName que mapeia o domínio principal MediaServicesAccountId para verificar DNS mediaservices--zona dns.
  * Tem de criar outro CName que mapeia o nome de anfitrião personalizado (por exemplo, sports.contoso.com) para o nome de anfitrião StreamingEndpoint de serviços de suporte de dados (por exemplo, amstest.streaming.mediaservices.windows.net).

    Para obter mais informações, consulte a propriedade CustomHostNames no [StreamingEndpoint](http://msdn.microsoft.com/library/azure/dn783468.aspx) artigo.

### <a id="sept_14_preview_changes"></a>Novos funcionalidades/cenários que fazem parte da versão de pré-visualização pública
* Em direto pré-visualização de transmissão em fluxo. Para obter mais informações, consulte [transmissão em fluxo em direto de trabalho com os Media Services](http://msdn.microsoft.com/library/azure/dn783466.aspx).
* Serviço de entrega de chave. Para obter mais informações, consulte [encriptação dinâmica de utilizar AES-128 e o serviço de entrega de chave](http://msdn.microsoft.com/library/azure/dn783457.aspx).
* Encriptação dinâmica AES. Para obter mais informações, consulte [encriptação dinâmica de utilizar AES-128 e o serviço de entrega de chave](http://msdn.microsoft.com/library/azure/dn783457.aspx).
* Serviço de entrega de licença PlayReady. Para obter mais informações, consulte [encriptação dinâmica PlayReady de utilização e o serviço de entrega de licença](http://msdn.microsoft.com/library/azure/dn783467.aspx).
* Encriptação dinâmica PlayReady. Para obter mais informações, consulte [encriptação dinâmica PlayReady de utilização e o serviço de entrega de licença](http://msdn.microsoft.com/library/azure/dn783467.aspx).
* Modelo de licença PlayReady de serviços de suporte de dados. Para obter mais informações, consulte o [descrição geral do modelo de licença PlayReady de serviços de suporte de dados].
* Recursos de fluxo encriptada de armazenamento. Para obter mais informações, consulte [transmitir conteúdo encriptado armazenamento](http://msdn.microsoft.com/library/azure/dn783451.aspx).

## <a id="august_changes_14"></a>Versão de Agosto de 2014
Quando codificar um elemento, um elemento de saída é produzido quando a tarefa de codificação for concluída. Até que esta versão, o codificador de serviços de suporte de dados produzidos metadados sobre recursos de saída. A partir desta versão, o codificador produz também metadados sobre recursos de entrada. Para obter mais informações, consulte [metadados de entrada] e [metadados de saída].

## <a id="july_changes_14"></a>Versão de Julho de 2014
Foram efetuadas as seguintes correções de erros para a Packager de serviços de suporte de dados do Azure e o encriptador:

* Quando um recurso de arquivo em direto é transmitido para HLS, áudio apenas reproduz: este problema foi corrigido e, agora podem reproduzir áudio e vídeo.
* Quando um recurso é empacotado para encriptação de envelope HLS e AES de 128 bits, os fluxos de em pacote não reproduzir em dispositivos Android: foi corrigido este erro e a sequência em pacote reproduz em dispositivos Android que suportam HLS.

## <a id="may_changes_14"></a>Versão de Maio de 2014
### <a id="may_14_changes"></a>Serviços de suporte de dados gerais de atualizações
Agora, pode utilizar [empacotamento dinâmico] fluxo HLS versão 3. Para HLS versão de fluxo de 3, adicione o seguinte formato para o caminho de localização de origem: * .ism/manifest(format=m3u8-aapl-v3). Para obter mais informações, consulte [este blogue](http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/).

Empacotamento dinâmico agora também suporta HLS (versão 3 e versão 4) encriptado com PlayReady com base na transmissão em fluxo uniforme estaticamente encriptado com PlayReady de entrega. Para obter informações sobre como encriptar a transmissão em fluxo uniforme com PlayReady, consulte [proteger transmissão em fluxo uniforme com PlayReady](http://msdn.microsoft.com/library/azure/dn189154.aspx).

### <a name="may_14_donnet_changes"></a>Atualizações do SDK .NET dos Media Services
O SDK do .NET dos serviços de suporte de dados está agora versão 3.0.0.5. Foram efetuadas as seguintes atualizações:

* Velocidade e resiliência são melhor quando carregar e transferir os recursos de suporte de dados.
* Foram efetuados melhoramentos no processamento de exceções de lógica e os transitório de repetição: 
  
  * Lógica de deteção e volte a tentar erro transitório foram melhoradas para exceções são causadas quando consultar, guardar as alterações e carregar ou transferir os ficheiros. 
  * Quando obtiver exceções web (por exemplo, durante um pedido de token de serviço de controlo de acesso), erros fatais falharem mais rapidamente agora.

Para obter mais informações, consulte [repetir lógica em que o SDK de Media Services para .NET].

## <a id="april_changes_14"></a>Versão de codificador de Abril de 2014
### <a name="april_14_enocer_changes"></a>Atualizações de codificador de serviços de multimédia
* Foi adicionado suporte para a ingestão de ficheiros AVI que são criados utilizando o editor de nonlinear Grass Valley EDIUS. Este processo, o vídeo ligeiramente é comprimido utilizando o codec Grass Valley HQ/HQX. Para obter mais informações, consulte [Grass Valley announces 7 EDIUS de transmissão em fluxo através da nuvem].
*  Foi adicionado suporte para especificar a Convenção de nomenclatura para os ficheiros produzido pelo codificador de serviços de suporte de dados. Para obter mais informações, consulte [codificador de serviços de suporte de dados de controlo de nomes de ficheiro de saída](http://msdn.microsoft.com/library/azure/dn303341.aspx).
*  Foi adicionado suporte para a sobreposição de vídeo e/ou áudio. Para obter mais informações, consulte [criar as Sobreposições](http://msdn.microsoft.com/library/azure/dn640496.aspx).
*  Foi adicionado suporte para stitch em conjunto vários segmentos de vídeos. Para obter mais informações, consulte [Stitch as vídeos segmentos](http://msdn.microsoft.com/library/azure/dn640504.aspx).
* Foi corrigido um erro que foi relacionados com a transcodificação MP4s onde áudio codificado com MPEG-1 áudio camada 3 (também conhecido como MP3).

## <a id="jan_feb_changes_14"></a>Versões de Janeiro/Fevereiro de 2014
### <a name="jan_fab_14_donnet_changes"></a>SDK .NET 3.0.0.1, 3.0.0.2 e 3.0.0.3 dos Media Services
As alterações 3.0.0.1 e 3.0.0.2 incluem:

* Problemas relacionados com a utilização de consultas LINQ com instruções OrderBy foram corrigidos.
* Testar soluções no [GitHub] foram dividida em com base na unidade de testes e testes de com base no cenário.

Para obter mais informações sobre as alterações, consulte o [versões do SDK de Media Services .NET 3.0.0.1 e 3.0.0.2](http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/).

As seguintes alterações foram efetuadas na versão 3.0.0.3:

* Dependências de armazenamento do Azure foram atualizadas para utilizar a versão 3.0.3.0.
* Foi corrigido um problema de compatibilidade com versões anteriores para 3.0. *.* versões.

## <a id="december_changes_13"></a>Versão de Dezembro de 2013
### <a name="dec_13_donnet_changes"></a>.NET SDK 3.0.0.0 dos Media Services
> [!NOTE]
> As versões de 3.0.x.x não são compatíveis com versões anteriores com as versões de 2.4.x.x.
> 
> 

A versão mais recente do SDK de serviços de suporte de dados está agora 3.0.0.0. Pode transferir o pacote mais recente a partir do NuGet ou obter os bits de [GitHub].

Começando com o SDK de Media Services versão 3.0.0.0, pode reutilizar o [serviço de controlo de acesso do Azure AD](http://msdn.microsoft.com/library/hh147631.aspx) tokens. Para obter mais informações, consulte a secção "Tokens de serviço de controlo de acesso de reutilização" em [ligar aos Media Services com o SDK de Media Services para .NET](http://msdn.microsoft.com/library/azure/jj129571.aspx).

### <a name="dec_13_donnet_ext_changes"></a>Extensões do SDK .NET dos Media Services 2.0.0.0
 As extensões do SDK .NET dos Media Services são um conjunto de métodos de extensão e funções de programa auxiliar que simplificam o seu código e facilitam a desenvolver com os Media Services. Pode obter os bits mais recentes do [extensões do SDK .NET dos Media Services](https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev).

## <a id="november_changes_13"></a>Versão de Novembro de 2013
### <a name="nov_13_donnet_changes"></a>Alterações do SDK .NET dos Media Services
A partir desta versão, o SDK de Media Services para .NET identificadores erro transitório erros que podem ocorrer quando são efetuadas chamadas para a camada de API de REST dos serviços de suporte de dados.

## <a id="august_changes_13"></a>Versão de Agosto de 2013
### <a name="aug_13_powershell_changes"></a>Cmdlets do PowerShell de serviços de suporte de dados incluídos nas ferramentas do Azure SDK
Os seguintes cmdlets PowerShell de serviços de suporte de dados estão agora incluídos nas [ferramentas do Azure SDK](https://github.com/Azure/azure-sdk-tools):

* Get-AzureMediaServices 

    Por exemplo: `Get-AzureMediaServicesAccount`
* New-AzureMediaServicesAccount 
  
    Por exemplo: `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`
* New-AzureMediaServicesKey 
  
    Por exemplo: `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`
* Remove-AzureMediaServicesAccount 
  
    Por exemplo: `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`

## <a id="june_changes_13"></a>Versão de Junho de 2013
### <a name="june_13_general_changes"></a>Alterações dos serviços de suporte de dados
As seguintes alterações mencionadas nesta secção são atualizações incluídas em versões de Junho de 2013 Media Services:

* Capacidade para ligar a várias contas de armazenamento para uma conta de serviço de suporte de dados. 
    * StorageAccount
    * Asset.StorageAccountName e Asset.StorageAccount
* Capacidade para atualizar Job.Priority. 
* Entidades relacionadas com a notificação e propriedades: 
    * JobNotificationSubscription
    * NotificationEndPoint
    * Tarefa
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Alterações do SDK .NET dos Media Services
As seguintes alterações estão incluídas em Junho de 2013 liberta o SDK de Media Services. O SDK de Media Services mais recente está disponível no GitHub.

* A partir da versão 2.3.0.0, suporta o SDK de Media Services ligar o armazenamento várias contas para uma conta de Media Services. As APIs seguintes suportam esta funcionalidade:
  
    * Tipo de IStorageAccount
    * Propriedade de Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts
    * Propriedade de StorageAccount
    * Propriedade de StorageAccountName
  
    Para obter mais informações, consulte [ativos gerir serviços de suporte de dados em várias contas do storage](http://msdn.microsoft.com/library/azure/dn271889.aspx).
* APIs de relacionadas com a notificação. A partir da versão 2.2.0.0, pode escutar notificações de armazenamento de filas do Azure. Para obter mais informações, consulte [notificações da tarefa processar Media Services](http://msdn.microsoft.com/library/azure/dn261241.aspx).
  
    * Propriedade de Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions
    * Tipo de Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint
    * Tipo de Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription
    * Tipo de Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection
    * Tipo de Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType
* Dependência no cliente do armazenamento SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll)
* Dependência de OData 5.5 (Microsoft.Data.OData.dll)

## <a id="december_changes_12"></a>Versão de Dezembro de 2012
### <a name="dec_12_dotnet_changes"></a>Alterações do SDK .NET dos Media Services
* O IntelliSense: Falta o IntelliSense documentação foi adicionado para muitos tipos.
* Microsoft.Practices.TransientFaultHandling.Core: Um problema foi corrigido onde o SDK ainda tinha uma dependência de uma versão antiga desta assemblagem. O SDK agora referencia a versão 5.1.1209.1 desta assemblagem.

Correções para problemas encontrados na Novembro de 2012 SDK:

* IAsset.Locators.Count: Esta contagem é agora corretamente comunicada no novo IAsset interfaces depois de todos os localizadores são eliminados.
* IAssetFile.ContentFileSize: Este valor é agora corretamente definido após um carregamento por IAssetFile.Upload(filepath).
* IAssetFile.ContentFileSize: Esta propriedade agora pode ser definida quando cria um ficheiro de recurso. -Foi anteriormente só de leitura.
* IAssetFile.Upload(filepath): Um problema foi corrigido onde este método de carregamento síncronos foi argumentoutofrangeexception o seguinte erro quando vários ficheiros foram carregados para o elemento. O erro foi "Falha ao autenticar o pedido de servidor. Certifique-se de que o valor de cabeçalho de autorização está formado corretamente, incluindo a assinatura."
* IAssetFile.UploadAsync: Um problema foi corrigido que limitada o carregamento de ficheiros a cinco ficheiros em simultâneo.
* IAssetFile.UploadProgressChanged: Este evento é agora fornecido pelo SDK.
* IAssetFile.DownloadAsync (cadeia, BlobTransferClient, ILocator, CancellationToken): Esta sobrecarga de método é agora fornecida.
* IAssetFile.DownloadAsync: Um problema foi corrigido que limitada a transferência de ficheiros a cinco ficheiros em simultâneo.
* IAssetFile.Delete(): Um problema foi corrigido onde chamar delete poderão acionar uma excepção se nenhum ficheiro foi carregado para o IAssetFile.
* Tarefas: Um problema foi corrigido onde encadeamento um "MP4 a tarefa de fluxos uniforme" com um "PlayReady a tarefa de proteção" utilizando um modelo de tarefa não tiver criado quaisquer tarefas de todo.
* EncryptionUtils.GetCertificateFromStore(): Este método já não emite uma exceção de referência nula devido a uma falha no localizar o certificado com base em problemas de configuração do certificado.

## <a id="november_changes_12"></a>Versão de Novembro de 2012
As alterações mencionadas nesta secção foram atualizações incluídas em Novembro de 2012 (versão 2.0.0.0) SDK. Estas alterações podem requerer qualquer código escrito para a pré-visualização de Junho de 2012 versão do SDK a modificação ou foi reescrita.

* Elementos
  
    * IAsset.Create(assetName) é o *apenas* a função de criação do recurso. IAsset.Create carrega já não ficheiros como parte da chamada de método. Utilize IAssetFile para carregar.
    * O método de IAsset.Publish e o valor de enumeração AssetState.Publish foram removidas do SDK dos serviços. Qualquer código que se baseie neste valor tem de ser foi reescrito.
* FileInfo
  
    * Esta classe foi removida e substituída pelo IAssetFile.
  
* IAssetFiles
  
    * IAssetFile substitui FileInfo e tem um comportamento diferente. Utilizá-la, instanciar o objeto IAssetFiles, seguido de um carregamento de ficheiros através da utilização do SDK de Media Services ou o SDK de armazenamento. Podem ser utilizadas as sobrecargas de IAssetFile.Upload seguintes:
  
        * IAssetFile.Upload(filePath): Este método síncrono bloqueia o thread e recomendamos apenas quando carregar um único ficheiro.
        * IAssetFile.UploadAsync (filePath blobTransferClient, localizador, cancellationToken): Este método assíncrono é o mecanismo de carregamento preferencial. 
    
            Erro conhecido: Se utilizar o token de cancelamento, o carregamento foi cancelado. As tarefas podem ter vários Estados de cancelamento. Corretamente tem de detetar e processar as exceções.
* Localizadores
  
    * As versões de origem específicos foram removidas. O contexto de SAS específicos. Locators.CreateSasLocator (asset, accessPolicy) serão marcadas como preterida ou removida por disponibilidade geral. Consulte a secção "Localizadores" em "Nova funcionalidade de" para o comportamento atualizado.

## <a id="june_changes_12"></a>Versão de pré-visualização de Junho de 2012
A seguinte funcionalidade foi novidade da versão de Novembro do SDK:

* Eliminar entidades
  
    * Objetos IAsset, IAssetFile, ILocator, IAccessPolicy e IContentKey agora são eliminados ao nível do objeto, ou seja, IObject.Delete(), em vez de exigir um eliminar na coleção, ou seja, cloudMediaContext.ObjCollection.Delete(objInstance).
* Localizadores
  
    * Os localizadores agora tem de ser criados utilizando o método CreateLocator. Terão de utilizar os valores de enumeração LocatorType.SAS ou LocatorType.OnDemandOrigin como um argumento para o tipo específico de localizador que pretende criar.
    * Novas propriedades foram adicionadas às localizadores para tornar mais fácil obter os URIs utilizável para o seu conteúdo. Este recriar de localizadores fornece mais flexibilidade para futura extensibilidade de terceiros e aumenta a facilidade de utilização para as aplicações de cliente de suporte de dados.
* Suporte de método assíncronas
  
    * Foi adicionado suporte assíncrono para todos os métodos.

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[fórum MSDN do Azure Media Services]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[referência da API de REST de serviços de suporte de dados do Azure]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[detalhes de preços dos Media Services]: http://azure.microsoft.com/pricing/details/media-services/
[metadados de entrada]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[metadados de saída]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Deliver content]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Index media files with the Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Work with Media Services live streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[Use AES-128 dynamic encryption and the key delivery service]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[Use PlayReady dynamic encryption and the license delivery service]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[descrição geral do modelo de licença PlayReady de serviços de suporte de dados]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Stream storage-encrypted content]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://portal.azure.com
[Empacotamento dinâmico]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin's blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Protect Smooth Streaming with PlayReady]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[repetir lógica em que o SDK de Media Services para .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Grass Valley announces 7 EDIUS de transmissão em fluxo através da nuvem]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Control Media Services Encoder output file names]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Create overlays]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Stitch video segments]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 and 3.0.0.2 releases]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure AD Access Control Service]: http://msdn.microsoft.com/library/hh147631.aspx
[Connect to Media Services with the Media Services SDK for .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Media Services .NET SDK extensions]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure SDK tools]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Manage Media Services assets across multiple Storage accounts]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Handle Media Services job notifications]: http://msdn.microsoft.com/library/azure/dn261241.aspx

