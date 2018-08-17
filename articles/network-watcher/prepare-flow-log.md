---
title: Alterações de registo de fluxo de grupo de segurança de rede do Azure | Documentos da Microsoft
description: Saiba mais sobre as alterações de registo de fluxo de grupo de segurança de rede do Azure.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2018
ms.author: jdial
ms.openlocfilehash: b3a5ca929c83f70c31d667c2d9c811623691cff8
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180905"
---
# <a name="network-security-group-flow-logs--version-2--upcoming-changes"></a>Alterações futuras - versão 2 – registos de fluxo do grupo de segurança de rede

O formato de registos de fluxo de grupo de segurança de rede está mudando a partir de 15 de Setembro de 2018. Foram adicionados campos fornecem informações sobre o fluxo de contagens de estado e incrementais de bytes e pacotes transferidos em cada direção. As aplicações que analisem os registos de fluxo serão afetadas por esta alteração e tem de ser atualizado em conformidade. Este artigo descreve os detalhes da alteração.

## <a name="what-is-changing"></a>O que está a mudar?

A versão de registos de fluxo de grupo de segurança de rede será alterado de "Versão": 1 para "Versão": 2, com campos adicionais adicionados para o *flowTuples* propriedade nos registos. São fornecidos detalhes sobre o formato nas [como um fluxo é modelado na versão 2](#how-is-a-flow-modeled-in-version-2).

### <a name="version-1-flow-tuple-format"></a>Formato de cadeia de identificação de fluxo de versão 1

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D"
```

### <a name="version-2-flow-tuple-format"></a>Formato de cadeia de identificação de fluxo de versão 2

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186"
```

## <a name="when-will-this-change-take-place"></a>Quando esta alteração terá lugar?

Esta alteração irá entrar em início efeito **15 de Setembro de 2018** na região e.u.a. Centro-Oeste. Implementação da alteração nas regiões da cloud pública será concluído por 31 de Novembro de 2018, após o qual apenas a versão 2 regista estará disponível.

## <a name="am-i-affected-by-the-change"></a>Estou afetado pela alteração?

Poderá ser afetada por esta alteração se usar um aplicativo que processa dados de registo de fluxo de grupo de segurança de rede ou uma compilação.

### <a name="if-i-use-traffic-analytics"></a>Se utilizar a análise de tráfego?

Não. Análise de tráfego não vai ser afetada durante a transição da versão 1 para o registo do fluxo de versão 2. Aprimoramentos em breve irão tirar partido das informações adicionais de largura de banda e de sessão fornecidas nos registos de fluxo 2 da versão.

### <a name="if-im-using-third-party-tooling"></a>Se estiver a utilizar de terceiros de ferramentas?

A alteração no formato de registo foi comunicada antecipadamente com todos os [parceiros do observador de rede](https://azure.microsoft.com/services/network-watcher). Contacte o seu fornecedor para obter detalhes.

### <a name="if-i-have-built-a-custom-application-or-integration"></a>Se já criei uma aplicação personalizada ou integração?

**Sim**, terá de modificar a sua aplicação para processar registos de fluxo de NSG no novo formato.

## <a name="what-is-the-new-flow-logging-format"></a>O que é o novo formato de registo de fluxo?

Versão de registos de fluxo 2 contém cadeias de identificação de fluxo no seguinte formato:

```
"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1,103,1,186".
```

A tabela que se segue fornece os nomes das propriedades e as descrições para a tupla de fluxo 2 versão de grupo de segurança de rede. Registos de exemplo completo que podem ser encontrados na [evento de exemplo da versão 2](#version2sampleevent).

| Nome da propriedade                        | Valor           | Descrição                                                                                                                                                 |
| ------------------------------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Carimbo de data/hora                           | 1493763938      | Timestamp correspondente para a entrada de fluxo. Formato do UNIX "Epoch" é utilizado.                                                                                       |
| Endereço IP de origem                    | 185.170.185.105 |                                                                                                                                                             |
| Endereço IP de destino               | 10.2.0.4        |                                                                                                                                                             |
| Porta de origem                          | 35370           |                                                                                                                                                             |
| Porta de destino                     | 23              |                                                                                                                                                             |
| Protocolo de tráfego                     | T               | **T**: TCP e **U**: UDP.                                                                                                                                  |
| Direção do fluxo de tráfego               | I               | **Eu**: tráfego de entrada e **s**: tráfego de saída.                                                                                                         |
| Decisão de tráfego                     | A               | **R**: fluxo foi permitido e **1!d**: fluxo foi negado.                                                                                                         |
| Estado do fluxo                           | C               | **B**: Begin, quando um fluxo é criado. Estatísticas não são fornecidas. **C**: continuar para um fluxo em curso. As estatísticas são fornecidas em intervalos de 5 minutos. **E**: final, quando um fluxo é terminado. As estatísticas são fornecidas.                                                                                                                                                        |
| Pacotes - origem para destino      | 1               | O número total de pacotes TCP e UDP enviados da origem para destino, desde a última atualização.                                                                  |
| Bytes enviados - origem para destino   | 103             | O número total de bytes de pacotes TCP e UDP enviados da origem para destino, desde a última atualização. Bytes de pacote incluem o cabeçalho de pacote e o payload.         |
| Pacotes enviados - destino para origem | 1               | O número total de pacotes TCP e UDP enviadas do destino para origem, desde a última atualização.                                                                  |
| Bytes enviados - destino para origem   | 186             | O número total de bytes de pacotes TCP e UDP enviadas do destino para origem, desde a última atualização. Bytes de pacote incluem o cabeçalho de pacote e o payload.             |

## <a name="how-is-a-flow-modeled-in-version-2"></a>Como um fluxo é modelado na versão 2?

Versão 2 dos logs de apresenta o estado do fluxo. Estado do fluxo *B* é registado quando um fluxo é iniciado. Estado do fluxo *C* e o estado do fluxo *i* são Estados que marcam a continuação de um fluxo e a terminação de fluxo, respectivamente. Ambos *C* e *i* Estados contêm informações de largura de banda do tráfego.

**Exemplo**: fluxo de cadeias de identificação de uma conversa de TCP entre 185.170.185.105:35370 e 10.2.0.4:23:

"1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,A,B,,," "1493695838,185.170.185.105,10.2.0.4,35370,23,T,I,A,C,1021,588096,8005,4610880" "1493696138,185.170.185.105,10.2.0.4,35370,23,T,I,A,E,52,29952,47,27072"

### <a name="how-does-the-format-differ-from-version-1"></a>O formato difere da versão 1?

Na versão 1, uma cadeia de identificação do fluxo ["1493763938,185.170.185.105,10.2.0.4,35370,23,T,I,D"] é criada sempre que um fluxo é estabelecido ou tentou ser estabelecida (negar caso).

### <a name="how-are-bytes-and-packets-accounted-for"></a>Como são bytes e pacotes presentes?

Para continuação *C* e de fim *i* Estados de fluxo, as contagens de byte e pacotes são agregadas contagens desde o momento do registo anterior para a cadeia de identificação de fluxo. O número total de pacotes transferidos referenciar a conversação do exemplo anterior, é 1021 + 52 + 8005 + 47 = 9125. É o número total de bytes transferidos 588096 + 29952 + 4610880 + 27072 = 5256000.

### <a name="if-my-application-doesnt-understand-the-traffic-bandwidth-fields-how-does-version-2-affect-the-application"></a>Se meu aplicativo não entende os campos de largura de banda do tráfego, como a versão 2 afeta o aplicativo?

Fluxo de registo, normalmente, contagem de que o número de fluxos exclusivos do grupo de aplicações com segurança de rede de versão 1. Se nenhuma alteração na análise é feita para levar em conta o estado de fluxo, poderá ver um aumento incorreto no número de fluxos comunicado. Contagem de fluxos exclusivos pode ser feito ao ignorar cadeias de identificação de fluxo no *C* e *i* Estados de fluxo.

## <a name="can-i-control-the-version-format-i-receive"></a>Pode controlar o formato de versão que recebo?

Não. Ativar e desativar os registos de fluxo não afetarão o formato de saída dos registos. A alteração da versão 1 para registos de versão 2 irá ocorrer em cada região a região. Quando uma região está a atualizar da versão 1 para a versão 2, poderá ver os registos de fluxo NSG em ambos os formatos. Depois de concluída a implementação, apenas os registos de versão 2 vão estar disponíveis.

## <a name="while-the-change-occurs-can-i-see-both-formats-for-the-same-network-security-group"></a>Enquanto a alteração ocorre, posso ver os dois formatos para o mesmo grupo de segurança de rede?

Sim. Dentro de uma região, a transição irá ocorrer numa base por mac endereço. Uma vez que um grupo de segurança de rede pode ser aplicado a várias máquinas, poderá ver os registos em ambos os formatos escritos no armazenamento. Registos será versão 1 ou versão 2.

## <a name="will-i-see-duplicate-data"></a>Irá ver dados duplicados?

Não haverá duplicação de fluxo de registo de dados em formatos. Um fluxo será gravado no formato de versão 1 ou versão 2, enquanto a alteração ocorre.

## <a name="how-can-i-test-the-new-format-ahead-of-time"></a>Como posso testar o novo formato de antecedência?

Sim. Pode [transferir](https://aka.ms/nsgflowlogsv2blobsample) um ficheiro de registo do fluxo do versão 2 do exemplo para utilizar em testes de sua solução.

## <a name="are-there-changes-to-configuration-and-management-of-network-security-group-flow-logging"></a>Existem alterações à configuração e gestão de registo de fluxo do grupo de segurança de rede?

Não. Suporte para contas de armazenamento do Azure nas subscrições que partilham o mesmo inquilino do Azure Active Directory foi [lançado](https://azure.microsoft.com/blog/new-azure-network-watcher-integrations-and-network-security-group-flow-logging-updates/) início deste ano. Esta alteração ajuda a reduzir o número de contas de armazenamento necessário ao gerir os registos de fluxo de grupo de segurança de rede.

## <a name="questions-or-feedback"></a>Questões ou comentários?

Estamos ansiosos por ouvir a sua experiência com registos de fluxo de grupo de segurança de rede e o observador de rede. Pode fornecer comentários ou sugestões online ou por e-mail em AzureNetworkWatcher@microsoft.com.

## <a name="version-2-sample"></a>Exemplo de versão 2

```json
{

"records": [

{

"time": "2018-08-03T17:00:54.5657764Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315592,10.1.1.6,204.79.197.200,62375,80,T,O,A,B,,,,",

"1533315595,10.1.1.6,204.79.197.200,62373,80,T,O,A,E,30,1784,92,123631",

"1533315597,10.1.1.6,204.79.197.200,62376,80,T,O,A,B,,,,",

"1533315601,10.1.1.6,204.79.197.200,62374,80,T,O,A,E,13,866,87,123079",

"1533315603,10.1.1.6,204.79.197.200,62377,80,T,O,A,B,,,,",

"1533315604,10.1.1.6,204.79.197.200,62375,80,T,O,A,E,33,1946,90,123247",

"1533315608,10.1.1.6,204.79.197.200,62378,80,T,O,A,B,,,,",

"1533315610,10.1.1.6,204.79.197.200,62376,80,T,O,A,E,20,1244,92,123355",

"1533315613,10.1.1.6,204.79.197.200,62379,80,T,O,A,B,,,,",

"1533315616,10.1.1.6,204.79.197.200,62377,80,T,O,A,E,24,1460,92,123355",

"1533315619,10.1.1.6,204.79.197.200,62380,80,T,O,A,B,,,,",

"1533315622,10.1.1.6,204.79.197.200,62378,80,T,O,A,E,23,1406,93,123409",

"1533315624,10.1.1.6,204.79.197.200,62381,80,T,O,A,B,,,,",

"1533315628,10.1.1.6,204.79.197.200,62379,80,T,O,A,E,16,1028,88,123133",

"1533315630,10.1.1.6,204.79.197.200,62382,80,T,O,A,B,,,,",

"1533315631,10.1.1.6,204.79.197.200,62380,80,T,O,A,E,13,866,87,123079",

"1533315635,10.1.1.6,204.79.197.200,62384,80,T,O,A,B,,,,",

"1533315637,10.1.1.6,204.79.197.200,62381,80,T,O,A,E,15,974,86,123025",

"1533315640,10.1.1.6,204.79.197.200,62385,80,T,O,A,B,,,,",

"1533315643,10.1.1.6,204.79.197.200,62382,80,T,O,A,E,16,1028,88,123139",

"1533315646,10.1.1.6,204.79.197.200,62386,80,T,O,A,B,,,,",

"1533315649,10.1.1.6,204.79.197.200,62384,80,T,O,A,E,21,1298,92,123355",

"1533315651,10.1.1.6,204.79.197.200,62387,80,T,O,A,B,,,,"

]

}

]

}

]

}

},

{

"time": "2018-08-03T17:01:54.5668880Z",

"systemId": "bbd48473-8ce0-4834-99bb-2e13b9b23ff8",

"category": "NetworkSecurityGroupFlowEvent",

"resourceId":
"/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FLOWLOGSV2DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/MULTITIERAPP2-NSG",

"operationName": "NetworkSecurityGroupFlowEvents",

"properties": {

"Version": 2,

"flows": [

{

"rule": "DefaultRule_DenyAllInBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315685,80.211.83.37,10.1.1.6,45321,81,T,I,D,B,,,,"

]

}

]

},

{

"rule": "DefaultRule_AllowInternetOutBound",

"flows": [

{

"mac": "002248034CC3",

"flowTuples": [

"1533315655,10.1.1.6,204.79.197.200,62385,80,T,O,A,E,20,1244,87,123079",

"1533315657,10.1.1.6,204.79.197.200,62388,80,T,O,A,B,,,,",

"1533315658,10.1.1.6,204.79.197.200,62386,80,T,O,A,E,26,1568,92,123355",

"1533315662,10.1.1.6,204.79.197.200,62389,80,T,O,A,B,,,,",

"1533315664,10.1.1.6,204.79.197.200,62387,80,T,O,A,E,15,974,88,123133",

"1533315667,10.1.1.6,204.79.197.200,62390,80,T,O,A,B,,,,",

"1533315670,10.1.1.6,204.79.197.200,62388,80,T,O,A,E,18,1136,88,123139",

"1533315673,10.1.1.6,204.79.197.200,62391,80,T,O,A,B,,,,",

"1533315676,10.1.1.6,204.79.197.200,62389,80,T,O,A,E,14,920,87,123079",

"1533315678,10.1.1.6,204.79.197.200,62392,80,T,O,A,B,,,,",

"1533315679,10.1.1.6,204.79.197.200,62390,80,T,O,A,E,31,1838,94,123739",

"1533315684,10.1.1.6,204.79.197.200,62393,80,T,O,A,B,,,,",

"1533315685,10.1.1.6,204.79.197.200,62391,80,T,O,A,E,28,1676,101,141199",

"1533315689,10.1.1.6,204.79.197.200,62394,80,T,O,A,B,,,,",

"1533315691,10.1.1.6,204.79.197.200,62392,80,T,O,A,E,21,1298,93,123409",

"1533315694,10.1.1.6,204.79.197.200,62395,80,T,O,A,B,,,,",

"1533315697,10.1.1.6,204.79.197.200,62393,80,T,O,A,E,26,1568,91,123393",

"1533315700,10.1.1.6,204.79.197.200,62396,80,T,O,A,B,,,,",

"1533315703,10.1.1.6,204.79.197.200,62394,80,T,O,A,E,14,920,89,123187",

"1533315705,10.1.1.6,204.79.197.200,62397,80,T,O,A,B,,,,",

"1533315706,10.1.1.6,204.79.197.200,62395,80,T,O,A,E,13,866,87,123079",

"1533315711,10.1.1.6,204.79.197.200,62398,80,T,O,A,B,,,,"

]

}

]

}

]

}

}

]

}
```

## <a name="version-1-sample"></a>Exemplo de versão 1

```json
{ 

    "records": [ 

        { 

            "time": "2017-02-16T22:00:32.8950000Z", 

            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434", 

            "category": "NetworkSecurityGroupFlowEvent", 

            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG", 

            "operationName": "NetworkSecurityGroupFlowEvents", 

            "properties": { 

                "Version": 1, 

                "flows": [ 

                    { 

                        "rule": "DefaultRule_DenyAllInBound", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D" 

                                ] 

                            } 

                        ] 

                    }, 

                    { 

                        "rule": "UserRule_default-allow-rdp", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A", 

                                    "1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A", 

                                    "1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A", 

                                    "1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A" 

                                ] 

                            } 

                        ] 

                    } 

                ] 

            } 

        }, 

        { 

            "time": "2017-02-16T22:01:32.8960000Z", 

            "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434", 

            "category": "NetworkSecurityGroupFlowEvent", 

            "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG", 

            "operationName": "NetworkSecurityGroupFlowEvents", 

            "properties": { 

                "Version": 1, 

                "flows": [ 

                    { 

                        "rule": "DefaultRule_DenyAllInBound", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D" 

                                ] 

                            } 

                        ] 

                    }, 

                    { 

                        "rule": "UserRule_default-allow-rdp", 

                        "flows": [ 

                            { 

                                "mac": "000D3AF8801A", 

                                "flowTuples": [ 

                                    "1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A",

                                    "1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A",

                                    "1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"

                                ] 

                            } 

                        ] 

                    } 

                ] 

            } 

        } 

    ] 
}
```