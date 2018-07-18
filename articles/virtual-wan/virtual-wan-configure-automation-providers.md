---
title: Configurar a automatização de WAN Virtual do Azure - para parceiros Virtual WAN | Documentos da Microsoft
description: Este artigo ajuda os parceiros de soluções de conectividade definida pelo software configurar a automatização de WAN Virtual do Azure.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: cherylmc
Customer intent: As a Virtual WAN software-defined connectivity provider, I want to set up a provisioning environment.
ms.openlocfilehash: a1ff4364e394b3807cf767722ee934ae024399b0
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114349"
---
# <a name="configure-virtual-wan-automation---for-virtual-wan-partners-preview"></a>Configurar a automatização de Virtual WAN - para parceiros Virtual WAN (pré-visualização)

Este artigo ajuda-o a compreender como configurar o ambiente de automatização para se ligar e configurar um dispositivo de ramificação (um dispositivo VPN do cliente no local ou SDWAN) para o WAN Virtual do Azure. Se for um fornecedor que fornece a dispositivos de ramificação que podem acomodar a conectividade VPN através de IPsec/IKEv2, este artigo é para.

Soluções de conectividade definida pelo software utilizam normalmente um controlador ou de um centro de aprovisionamento de dispositivos para gerir os respetivos dispositivos de ramificação. O controlador pode usar APIs do Azure para automatizar a conectividade WAN Virtual do Azure. Este tipo de ligação requer um SDWAN ou VPN dispositivos localizados no local que tenha um endereço IP público com acesso exterior atribuído a ele.

##  <a name="access"></a>Controlo de acesso

Os clientes tem de ser capazes de configurar o controlo de acesso apropriados para o Virtual WAN na IU do dispositivo. Isto é recomendado utilizar um Principal de serviço do Azure. O acesso de baseada em principais de serviço fornece a autenticação adequada de controlador de dispositivo para carregar as informações do ramo.

##  <a name="site"></a>Carregar informações do ramo

Crie a experiência do usuário para carregar informações do ramo (site no local) para o Azure. [REST APIs](https://docs.microsoft.com/rest/api/virtualwan/vpnsites) para **VPNSite** pode ser utilizado para criar as informações do site na Virtual WAN. Pode fornecer todos os dispositivos SDWAN/VPN ramo ou selecionar personalizações de dispositivo conforme apropriado.

##  <a name="hub"></a>Hub e serviços

Depois do dispositivo de ramificação é carregado para o Azure, o cliente certificar, normalmente, as seleções de região do hub e/ou serviços no portal do Azure, que invoca um conjunto de operações para criar rede virtual do concentrador e o ponto de final VPN no concentrador. O gateway de VPN é um gateway dimensionável que os tamanhos adequadamente, com base na largura de banda e ligação necessidades.

## <a name="device"></a>Configuração do dispositivo

Neste passo, faria um cliente que não está a utilizar um fornecedor manualmente transferir a configuração do Azure e aplicá-la para o respetivo dispositivo SDWAN/VPN no local. Como um fornecedor, pode automatizar este passo. O controlador pode chamar **GetVpnConfiguration** API de REST para transferir a configuração do Azure, que, normalmente, terá um aspeto semelhante para o arquivo a seguir.

**Notas de configuração**

  * Se as VNets do Azure estão ligados ao concentrador virtual, serão apresentados como ConnectedSubnets.
  * Conectividade VPN utiliza a configuração baseada em rota e IKEv2.

### <a name="understanding-the-device-configuration-file"></a>Noções básicas sobre o ficheiro de configuração do dispositivo

O ficheiro de configuração do dispositivo contém as definições a utilizar quando configurar o dispositivo VPN no local. Quando vê esse arquivo, tenha em atenção as seguintes informações:

* **vpnSiteConfiguration -** esta secção indica os detalhes do dispositivo definido como um site de ligar para o WAN virtual. Ele inclui o nome e endereço ip público do dispositivo ramo.
* **vpnSiteConnections -** Esta seção fornece informações sobre o seguinte:

    * **Espaço de endereços** do virtual no hub ou hubs VNet.<br>Exemplo:
 
        ```
        "AddressSpace":"10.1.0.0/24"
        ```
    * **Espaço de endereços** das VNets que estão ligadas ao hub.<br>Exemplo:

         ```
        "ConnectedSubnets":["10.2.0.0/16","10.30.0.0/16"]
         ```
    * **Endereços IP** do vpngateway virtual hub. Como o vpngateway tem que cada ligação incluem de 2 túneis numa configuração ativa-ativa, verá os dois endereços IP listados neste ficheiro. Neste exemplo, vê "Instance0" e "Instance1" para cada site.<br>Exemplo:

        ``` 
        "Instance0":"104.45.18.186"
        "Instance1":"104.45.13.195"
        ```
    * **Detalhes de configuração de ligação de Vpngateway** , como o protocolo BGP, pré-partilhada chave etc. O PSK é a chave pré-partilhada, que é gerada automaticamente para. Pode sempre editar a ligação na página de descrição geral para uma PSK personalizado.
  
### <a name="example-device-configuration-file"></a>Ficheiro de configuração do dispositivo de exemplo

  ```
  { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"r403583d-9c82-4cb8-8570-1cbbcd9983b5"
      },
      "vpnSiteConfiguration":{ 
         "Name":"testsite1",
         "IPAddress":"73.239.3.208"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe",
               "ConnectedSubnets":[ 
                  "10.2.0.0/16",
                  "10.30.0.0/16"
               ]
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.186",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"bkOWe5dPPqkx0DfFE3tyuP7y3oYqAEbI",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"1f33f891-e1ab-42b8-8d8c-c024d337bcac"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite2",
         "IPAddress":"66.193.205.122"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"XzODPyAYQqFs4ai9WzrJour0qLzeg7Qg",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   },
   { 
      "configurationVersion":{ 
         "LastUpdatedTime":"2018-07-03T18:29:49.8405161Z",
         "Version":"cd1e4a23-96bd-43a9-93b5-b51c2a945c7"
      },
      "vpnSiteConfiguration":{ 
         "Name":" testsite3",
         "IPAddress":"182.71.123.228"
      },
      "vpnSiteConnections":[ 
         { 
            "hubConfiguration":{ 
               "AddressSpace":"10.1.0.0/24",
               "Region":"West Europe"
            },
            "gatewayConfiguration":{ 
               "IpAddresses":{ 
                  "Instance0":"104.45.18.187",
                  "Instance1":"104.45.13.195"
               }
            },
            "connectionConfiguration":{ 
               "IsBgpEnabled":false,
               "PSK":"YLkSdSYd4wjjEThR3aIxaXaqNdxUwSo9",
               "IPsecParameters":{ 
                  "SADataSizeInKilobytes":102400000,
                  "SALifeTimeInSeconds":3600
               }
            }
         }
      ]
   }
  ```

## <a name="default"></a>Políticas predefinidas

### <a name="initiator"></a>Iniciador

As secções seguintes listam as combinações de política suportadas quando o Azure é o iniciador para o túnel.

**Fase 1**

* DH_GROUP_2 AES_256, SHA1,
* DH_GROUP_2 AES_256, SHA_256,
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* DH_GROUP_2 3DES, SHA1,
* DH_GROUP_2 3DES, SHA_256,

**Fase 2**

* GCM_AES_256 GCM_AES_256, PFS_NONE
* AES_256 SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE


### <a name="responder"></a>Dispositivo de resposta

As secções seguintes listam as combinações de política suportadas quando o Azure é o dispositivo de resposta para o túnel.

**Fase 1**

* DH_GROUP_2 AES_256, SHA1,
* DH_GROUP_2 AES_256, SHA_256,
* AES_128, SHA1, DH_GROUP_2
* AES_128, SHA_256, DH_GROUP_2
* DH_GROUP_2 3DES, SHA1,
* DH_GROUP_2 3DES, SHA_256,

**Fase 2**

* GCM_AES_256 GCM_AES_256, PFS_NONE
* AES_256 SHA_1, PFS_NONE
* CBC_3DES, SHA_1, PFS_NONE
* AES_256, SHA_256, PFS_NONE
* AES_128, SHA_1, PFS_NONE
* CBC_3DES, SHA_256, PFS_NONE
* PFS_NONE CBC_DES, SHA_1, 
* PFS_1 AES_256, SHA_1,
* PFS_2 AES_256, SHA_1,
* PFS_14 AES_256, SHA_1,
* AES_128, SHA_1, PFS_1
* AES_128, SHA_1, PFS_2
* AES_128, SHA_1, PFS_14
* PFS_1 CBC_3DES, SHA_1,
* PFS_2 CBC_3DES, SHA_1,
* PFS_2 CBC_3DES, SHA_256,
* PFS_1 AES_256, SHA_256,
* PFS_2 AES_256, SHA_256,
* PFS_14 AES_256, SHA_256,
* PFS_24 AES_256, SHA_1,
* PFS_24 AES_256, SHA_256,
* AES_128, SHA_256, PFS_NONE
* AES_128, SHA_256, PFS_1
* AES_128, SHA_256, PFS_2
* AES_128, SHA_256, PFS_14
* PFS_14 CBC_3DES, SHA_1,

### <a name="does-everything-need-to-match-between-the-virtual-hub-vpngateway-policy-and-my-on-premises-sdwanvpn-device-or-sd-wan-configuration"></a>Tudo o que precisa corresponder entre a política do virtual hub vpngateway e meu dispositivo SDWAN/VPN no local ou de configuração SD-WAN?

O dispositivo SDWAN/VPN no local ou SD-WAN configuração tem de corresponder ou conter os seguintes algoritmos e parâmetros, que pode especificar na política de IPsec/IKE do Azure. A duração do SA é apenas a especificação local e não têm de corresponder.

* Algoritmo de encriptação do IKE
* Algoritmo de integridade do IKE
* Grupo DH
* Algoritmo de encriptação do IPsec
* Algoritmo de integridade do IPsec
* Grupo PFS

## <a name="feedback"></a>Comentários de pré-visualização

Agradecemos os seus comentários. Envie um e-mail para <azurevirtualwan@microsoft.com> para reportar quaisquer problemas ou para fornecer comentários (positivo ou negativo) para o Virtual WAN. Inclua o nome da sua empresa em "[]" na linha de assunto. Também incluem o seu ID de subscrição se está a relatar um problema.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Virtual WAN, consulte [sobre a Azure Virtual WAN](virtual-wan-about.md) e o [FAQ de WAN Virtual do Azure](virtual-wan-faq.md).
