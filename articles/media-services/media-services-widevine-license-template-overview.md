---
title: "Descrição geral do modelo de licença Widevine | Microsoft Docs"
description: "Este tópico fornece uma descrição geral de um modelo de licença Widevine que é utilizado para configurar licenças Widevine."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 85de5765975b0c55fafe9bb4c14a1c1f435a6d5c
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="widevine-license-template-overview"></a>Descrição geral do modelo de licença Widevine
Pode utilizar os Media Services do Azure para configurar e pedido licenças Widevine da Google. Quando o leitor tenta reproduzir os conteúdos protegidos de Widevine, é enviado um pedido para o serviço de entrega de licença para obter uma licença. Se o serviço de licenciamento aprova o pedido, o serviço emite a licença. Este é enviada para o cliente e é utilizada para desencriptar e reproduzir o conteúdo especificado.

Um pedido de licença Widevine é formatado como uma mensagem JSON.  

>[!NOTE]
> Pode criar uma mensagem vazia com nenhuma valores, apenas "{}." Em seguida, é criado um modelo de licença com predefinições. A predefinição funciona para a maioria dos casos. Cenários de entrega de licença baseado em Microsoft devem sempre utilizar as predefinições. Se precisar de definir os valores de "content_id" e "fornecedor", um fornecedor tem de corresponder ao Widevine credenciais.

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>Mensagem JSON
| Nome | Valor | Descrição |
| --- | --- | --- |
| Payload |Cadeia com codificação Base64 |O pedido de licença enviado por um cliente. |
| content_id |Cadeia com codificação Base64 |Identificador utilizado para obter o ID de chave e conteúdo chave para cada content_key_specs.track_type. |
| Fornecedor |string |Utilizado para procurar as chaves de conteúdo e políticas. Se a entrega de chave da Microsoft é utilizada para entrega de licença Widevine, este parâmetro é ignorado. |
| policy_name |string |Nome de uma política anteriormente registado. Opcional. |
| allowed_track_types |Enum |SD_ONLY ou SD_HD. Controlos que as chaves de conteúdo estão incluídos numa licença. |
| content_key_specs |Matriz de JSON estruturas, consulte a secção "Conteúdas especificações chaves".  |Um controlo de bloqueio de mensagens em fila na quais as chaves de conteúdo para devolver. Para obter mais informações, consulte a secção "Conteúdas especificações chaves". Pode ser especificado apenas um dos valores allowed_track_types e content_key_specs. |
| use_policy_overrides_exclusively |Valor booleano, true ou false |Utilizar atributos de política especificados por policy_overrides e omitir a todas as políticas anteriormente armazenada. |
| policy_overrides |JSON estrutura, consulte a secção "Substituições de política". |Definições de política para esta licença.  No caso deste recurso tem uma política predefinida, estes valores especificados são utilizados. |
| session_init |JSON estrutura, consulte a secção "Inicialização da sessão." |Dados opcionais são transmitidos para a licença. |
| parse_only |Valor booleano, true ou false |O pedido de licença é analisado, mas nenhuma licença é emitida. No entanto, os valores no pedido de licença são devolvidos na resposta. |

## <a name="content-key-specs"></a>Especificações de chaves de conteúdo
Se existir uma política preexistentes, é necessário especificar qualquer um dos valores na especificação de chave de conteúdo. A política de pré-existentes associada este conteúdo é utilizada para determinar a proteção de saída, tais como largura de banda alta proteção de conteúdos digitais (HDCP) e o sistema cópia gestão geral (CGMS). Se uma política pré-existentes não está registada com o servidor de licenças Widevine, o fornecedor de conteúdo pode inserir os valores para o pedido de licença.   

Cada valor de content_key_specs tem de ser especificado para todas as controla, independentemente da opção de use_policy_overrides_exclusively. 

| Nome | Valor | Descrição |
| --- | --- | --- |
| content_key_specs. track_type |string |Um nome de tipo de controlar. Se content_key_specs é especificado no pedido de licença, certifique-se de que especifica que todos os tipos de controlar explicitamente. Falha ao fazê-lo resulta numa falha reproduzir após 10 segundos. |
| content_key_specs  <br/> security_level |UInt32 |Define os requisitos do cliente robustez para reprodução. <br/> -É necessária software baseados em criptografia de caixa em branco. <br/> -Criptografia de software e um descodificador oculto são necessários. <br/> -As operações de criptografia e material de chaves têm de ser executadas num ambiente de hardware de segurança execução fidedigna. <br/> -A criptografia e descodificar de conteúdo devem ser executados num ambiente de hardware de segurança execução fidedigna.  <br/> -A criptografia, descodificar e todos os processamento do suporte de dados (comprimidos e descomprimidos) tem de ser processada num ambiente de hardware de segurança execução fidedigna. |
| content_key_specs <br/> required_output_protection.hDC |cadeia, um dos HDCP_V2 HDCP_NONE, HDCP_V1, |Indica se HDCP é necessária. |
| content_key_specs <br/>key |Base64-<br/>cadeia codificada |Chave de conteúdo a utilizar para este controlar. Se for especificado, é necessário o track_type ou key_id. O fornecedor de conteúdo pode utilizar esta opção para inserir a chave de conteúdo para este registo em vez de permitir que o servidor de licenças Widevine gerar ou procurar uma chave. |
| content_key_specs.key_id |Binário de cadeia com codificação Base64, 16 bytes |Identificador exclusivo para a chave. |

## <a name="policy-overrides"></a>Substituições de política
| Nome | Valor | Descrição |
| --- | --- | --- |
| policy_overrides. can_play |Valor booleano, true ou false |Indica que reprodução do conteúdo é permitida. Predefinição é falsa. |
| policy_overrides. can_persist |Valor booleano, true ou false |Indica que a licença pode ser persistentes para o armazenamento não volátil para utilização offline. Predefinição é falsa. |
| policy_overrides. can_renew |Valor booleano, true ou false |Indica que a renovação esta licença é permitida. Se for VERDADEIRO, a duração da licença de pode ser expandida por heartbeat. Predefinição é falsa. |
| policy_overrides. license_duration_seconds |Int64 |Indica a janela de tempo para este licenças específico. Um valor de 0 indica que não há nenhum limite para a duração. Predefinição é 0. |
| policy_overrides. rental_duration_seconds |Int64 |Indica a janela de tempo enquanto reprodução é permitida. Um valor de 0 indica que não há nenhum limite para a duração. Predefinição é 0. |
| policy_overrides. playback_duration_seconds |Int64 |A janela de visualização de tempo depois de reprodução começar dentro da duração de licença. Um valor de 0 indica que não há nenhum limite para a duração. Predefinição é 0. |
| policy_overrides. renewal_server_url |string |Todos os pedidos de heartbeat (renovação) para esta licença são direcionados para o URL especificado. Este campo só é utilizado se can_renew é verdadeiro. |
| policy_overrides. renewal_delay_seconds |Int64 |Quantos segundos após license_start_time antes de renovação pela primeira vez é tentada. Este campo só é utilizado se can_renew é verdadeiro. Predefinição é 0. |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Especifica o atraso em segundos entre pedidos de renovação de licença subsequentes, em caso de falha. Este campo só é utilizado se can_renew é verdadeiro. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |A janela de tempo no qual reprodução pode permanecer durante a renovação é tentada, mas sem êxito devido a problemas de back-end com o servidor de licenças. Um valor de 0 indica que não há nenhum limite para a duração. Este campo só é utilizado se can_renew é verdadeiro. |
| policy_overrides. renew_with_usage |Valor booleano, true ou false |Indica que a licença é enviada para a renovação quando é iniciado de utilização. Este campo só é utilizado se can_renew é verdadeiro. |

## <a name="session-initialization"></a>Inicialização de sessão
| Nome | Valor | Descrição |
| --- | --- | --- |
| provider_session_token |Cadeia com codificação Base64 |Este token de sessão é passada de volta na licença de e não existe no renovações subsequentes. O token de sessão não mantêm-se para além de sessões. |
| provider_client_token |Cadeia com codificação Base64 |Token do cliente para enviar novamente na resposta licença. Se o pedido de licença contém um token do cliente, este valor é ignorado. O token do cliente mantém-se para além de sessões de licença. |
| override_provider_client_token |Valor booleano, true ou false |Se false e o pedido de licença contém um token do cliente, utilize o token do pedido, mesmo se um token do cliente foi especificado nesta estrutura. Se for VERDADEIRO, utilize sempre o token especificado nesta estrutura. |

## <a name="configure-your-widevine-licenses-by-using-net-types"></a>Configurar as suas licenças Widevine utilizando tipos de .NET
Os Media Services fornecem APIs .NET que pode utilizar para configurar as suas licenças Widevine. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>Classes, tal como definido no SDK do .NET de Media Services
As seguintes classes se as definições dos seguintes tipos:

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>Exemplo
O exemplo seguinte mostra como utilizar os APIs de .NET para configurar uma licença Widevine simple:

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Consulte também
[Utilizar encriptação comum dinâmica PlayReady e/ou Widevine](media-services-protect-with-playready-widevine.md)

