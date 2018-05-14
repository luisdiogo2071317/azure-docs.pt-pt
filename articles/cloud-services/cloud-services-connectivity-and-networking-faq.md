---
title: Problemas de conectividade e de rede para o Microsoft Azure Cloud Services FAQ | Microsoft Docs
description: Este artigo apresenta uma lista de perguntas mais frequentes sobre a conectividade e de rede para serviços de nuvem do Microsoft Azure.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: ab0fa22e9ba776db3d4af301499545f6e0822478
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problemas de conectividade e de rede do Cloud Services do Azure: Perguntas mais frequentes sobre (FAQ)

Este artigo inclui perguntas mais frequentes sobre problemas de conectividade e de rede para [Cloud Services do Azure](https://azure.microsoft.com/services/cloud-services). Para informações de tamanho, consulte o [página de tamanho de VM de serviços em nuvem](cloud-services-sizes-specs.md).

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Posso não é possível reservar um IP num serviço de nuvem a várias VIP.
Em primeiro lugar, certifique-se de que a instância de máquina virtual que tentar reservar o IP está ativada. Segundo, certifique-se de que utiliza IPs reservados para implementações de teste e produção. *Não* alterar as definições ao atualizar de implementação.

## <a name="how-do-i-use-remote-desktop-when-i-have-an-nsg"></a>Como utilizar ambiente de trabalho remoto quando tiver um NSG?
Adicione regras para o NSG que permitam o tráfego em portas **3389** e **20000**. Ambiente de trabalho remoto utiliza a porta **3389**. Instâncias de serviço de nuvem são carga balanceada, pelo diretamente não é possível controlar que instância para ligar a. O *RemoteForwarder* e *RemoteAccess* agentes gerir o tráfego de protocolo RDP (Remote Desktop Protocol) e permitir que o cliente enviar um cookie RDP e especifique uma instância individual para ligar a. O *RemoteForwarder* e *RemoteAccess* agentes requerem porta **20000** para ser aberto, que poderá ser bloqueado se tiver um NSG.

## <a name="can-i-ping-a-cloud-service"></a>Posso ping um serviço em nuvem?

Não, não utilizando o normal "ping" / protocolo ICMP. O protocolo ICMP não é permitido através do Balanceador de carga do Azure.

Para testar a conectividade, recomendamos que efetue um ping de porta. Enquanto Ping.exe utiliza o ICMP, pode utilizar outras ferramentas, como o PSPing, Nmap e telnet, para testar a conectividade à porta TCP específica.

Para obter mais informações, consulte [utilizar pings porta em vez de ICMP para testar a conectividade da VM do Azure](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-might-indicate-a-malicious-attack-to-the-cloud-service"></a>Como impedir a receção milhares de pedidos de endereços IP desconhecidos que possam indicar um ataque malicioso para o serviço em nuvem?
Azure implementa um segurança de rede multicamada para proteger os seus serviços da plataforma contra ataques em distribuída denial of service (DDoS distribuídos). O sistema de defesa Azure DDoS faz parte monitorização processo contínuo do Azure, que é continuamente melhorado através de testes penetração. Este sistema de defesa DDoS foi concebido para conseguir não apenas os ataques de exterior, mas também a partir de outros inquilinos do Azure. Para obter mais informações, consulte [segurança de rede do Azure](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Também pode criar uma tarefa de arranque para bloquear seletivamente alguns endereços IP específicos. Para obter mais informações, consulte [bloquear um endereço IP específico](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-to-rdp-to-my-cloud-service-instance-i-get-the-message-the-user-account-has-expired"></a>Quando tento RDP à minha instância de serviço em nuvem, é apresentada a mensagem "a conta de utilizador expirou."
Pode obter a mensagem de erro "Esta conta de utilizador expirou" quando a ignorar a data de expiração que está configurada nas definições do RDP. Pode alterar a data de expiração do portal, seguindo estes passos:

1. Iniciar sessão para o [portal do Azure](https://portal.azure.com), aceda ao seu serviço de nuvem e selecione o **ambiente de trabalho remoto** separador.

2. Selecione o **produção** ou **transição** ranhura de implementação.

3. Alterar o **expira em** data e, em seguida, guarde a configuração.

Pode agora deve conseguir RDP para a máquina.

## <a name="why-is-azure-load-balancer-not-balancing-traffic-equally"></a>Por que motivo é Balanceador de carga do Azure não balanceamento tráfego equitativamente?
Para obter informações sobre como funciona um balanceador de carga interno, consulte [novo modo de distribuição do Balanceador de carga do Azure](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

O algoritmo de distribuição utilizado é uma 5 cadeias de identificação (IP de origem, porta de origem, IP de destino, porta de destino e tipo de protocolo) hash para mapear o tráfego para servidores disponíveis. Fornece retenções apenas dentro de uma sessão de transporte. Pacotes na mesma sessão TCP ou UDP são direcionados para a mesma instância de IP (DIP) do Centro de dados por trás o ponto final com balanceamento de carga. Quando o cliente fecha e reopens a ligação ou inicia uma nova sessão a partir do mesmo IP de origem, a porta de origem é alterado e faz com que o tráfego Ir para um endpoint diferente do DIP.

## <a name="how-can-i-redirect-incoming-traffic-to-the-default-url-of-my-cloud-service-to-a-custom-url"></a>Como posso redirecionar o tráfego de entrada para o URL predefinido do meu serviço em nuvem para um URL personalizado? 

O módulo IIS URL Rewrite pode ser utilizado para redirecionar o tráfego que é fornecido para o URL predefinido para o serviço em nuvem (por exemplo, \*. cloudapp.net) para algumas nome/URL personalizado. Porque o URL Rewrite module está ativado em funções da web por predefinição e as respetivas regras estão configuradas no Web. config da aplicação, está sempre disponível na VM, independentemente de reinícios/reimages. Para obter mais informações, consulte:

- [Criar regras de reescrever para o URL Rewrite module](https://docs.microsoft.com/iis/extensions/url-rewrite-module/creating-rewrite-rules-for-the-url-rewrite-module)
- [Remover uma ligação de predefinida](https://stackoverflow.com/questions/32286487/azure-website-how-to-remove-default-link?answertab=votes#tab-top)

## <a name="how-can-i-blockdisable-incoming-traffic-to-the-default-url-of-my-cloud-service"></a>Como pode posso bloco/desativar tráfego de entrada para o URL predefinido do meu serviço em nuvem? 

Pode impedir que o tráfego de entrada predefinido/nome de URL do seu serviço de nuvem (por exemplo, \*. cloudapp.net). Defina o cabeçalho de anfitrião para um nome DNS (por exemplo, www.MyCloudService.com) personalizado em configuração de enlace de site no ficheiro de definição (*.csdef) do serviço de nuvem, conforme indicado: 
 

    <?xml version="1.0" encoding="utf-8"?> 
    <ServiceDefinition name="AzureCloudServicesDemo" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6"> 
      <WebRole name="MyWebRole" vmsize="Small"> 
        <Sites> 
          <Site name="Web"> 
            <Bindings> 
              <Binding name="Endpoint1" endpointName="Endpoint1" hostHeader="www.MyCloudService.com" /> 
            </Bindings> 
          </Site> 
        </Sites> 
        <Endpoints> 
          <InputEndpoint name="Endpoint1" protocol="http" port="80" /> 
        </Endpoints> 
        <ConfigurationSettings> 
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" /> 
        </ConfigurationSettings> 
      </WebRole> 
    </ServiceDefinition> 
 
Uma vez que este enlace de cabeçalho de anfitrião é imposta através do ficheiro csdef, o serviço é acessível apenas através de nome personalizado "www.MyCloudService.com." Todos os pedidos recebidos para o "*. cloudapp.net" domínio falhar sempre. Se utilizar uma sonda SLB personalizada ou um balanceador de carga internos no serviço, bloquear a predefinição/nome de URL do serviço pode interferir com o comportamento de pesquisa. 

## <a name="how-can-i-make-sure-the-public-facing-ip-address-of-a-cloud-service-never-changes"></a>Como posso garantir que nunca altera o endereço IP destinado ao público de um serviço em nuvem?

Para certificar-se de que o endereço IP destinado ao público do seu serviço em nuvem (também conhecido como um VIP) nunca é alterado para que possa ser normalmente na lista de permissões por alguns clientes específicos, recomendamos que tem um IP reservado com associados. Caso contrário, o IP virtual fornecido pelo Azure é desalocado da sua subscrição, se eliminar a implementação. Para a operação de alternância de VIP com êxito, terá de IPs reservados individuais para produção e ranhuras de teste. Sem os mesmos, a operação de troca falha. Reservar um endereço IP e associá-lo com o serviço de nuvem, consulte estes artigos:
 
- [O endereço IP de um serviço em nuvem existente de reserva](../virtual-network/virtual-networks-reserved-public-ip.md#reserve-the-ip-address-of-an-existing-cloud-service)
- [Associar um IP reservado para um serviço em nuvem, utilizando um ficheiro de configuração de serviço](../virtual-network/virtual-networks-reserved-public-ip.md#associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file) 

Se tiver mais do que uma instância para as funções, associar o seu serviço em nuvem RIP não deve causar qualquer período de inatividade. Em alternativa, pode lista branca o intervalo de IP do seu centro de dados do Azure. Pode encontrar todos os intervalos de IP do Azure, o [Microsoft Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=41653). 

Este ficheiro contém os intervalos de endereços do IP (incluindo intervalos de computação, armazenamento e SQL) utilizados nos centros de dados do Azure. Um ficheiro atualizado é publicado semanal que reflete os intervalos de implementado e as alterações futuras para os intervalos de IP. Novo intervalos que aparecem no ficheiro de não são utilizados em dos centros de dados de, pelo menos, uma semana. Transfira o ficheiro. XML novo todas as semanas e efetuar as alterações necessárias no seu site para identificar corretamente os serviços em execução no Azure. Utilizadores do Azure ExpressRoute poderão constatar que este ficheiro utilizado para atualizar o anúncio BGP de espaço do Azure na primeira semana de cada mês. 

## <a name="how-can-i-use-azure-resource-manager-virtual-networks-with-cloud-services"></a>Como utilizar redes virtuais do Azure Resource Manager com serviços em nuvem? 

Serviços cloud não não possível colocar em redes virtuais do Azure Resource Manager. Redes virtuais do Gestor de recursos e redes virtuais de implementação clássica, podem ser ligadas através do peering. Para obter mais informações, consulte [peering de rede Virtual](../virtual-network/virtual-network-peering-overview.md).
