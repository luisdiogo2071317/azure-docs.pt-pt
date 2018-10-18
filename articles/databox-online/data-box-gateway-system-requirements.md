---
title: Requisitos de sistema do Gateway de caixa de dados do Microsoft Azure | Documentos da Microsoft
description: Saiba mais sobre o software e os requisitos de rede para o Gateway de caixa de dados do Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 10/17/2018
ms.author: alkohli
ms.openlocfilehash: f70cd5de29cc3050142dfff905acae2008dab115
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49379127"
---
# <a name="azure-data-box-gateway-system-requirements-preview"></a>Requisitos de sistema de Gateway de caixa de dados do Azure (pré-visualização)

Este artigo descreve os requisitos de sistema importante para a sua solução de Gateway de caixa de dados do Microsoft Azure e para os clientes ligar ao Gateway de caixa de dados do Azure. Recomendamos que reveja as informações cuidadosamente antes de implementar o Gateway de caixa de dados e, em seguida, regressar mais tarde a ela conforme necessário durante a implantação e operação subseqüente.

Os requisitos de sistema para o dispositivo virtual de Gateway de caixa de dados incluem:

- **Requisitos de software para anfitriões** -descreve as plataformas suportadas, browsers para a configuração local da interface do Usuário, os clientes do SMB e quaisquer requisitos adicionais para os anfitriões que se ligam ao dispositivo.
- **Requisitos de rede para o dispositivo** -fornece informações sobre quaisquer requisitos de rede para a operação do dispositivo virtual.

> [!IMPORTANT]
> O Data Box Gateway está em Pré-visualização. Reveja os [termos de utilização da pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solução. 

## <a name="supported-os-for-clients-connected-to-device"></a>SO suportado para os clientes ligados ao dispositivo

Aqui está uma lista de sistemas operativos suportados para os clientes ou hosts ligadas ao Gateway de caixa de dados.

| **Sistema operativo/plataforma** | **Versões** |
| --- | --- |
| Windows Server |2012 R2 <br> 2016 |
| Windows |8, 10 |
| SUSE Linux |Enterprise Server 12 (x86_64)|
| Ubuntu |16.04.3 LTS|
| CentOS | 7.0 |

## <a name="supported-protocols-for-clients-accessing-device"></a>Protocolos suportados para os clientes a aceder ao dispositivo

|**Protocolo** |**Versões**   |**Notas**  |
|---------|---------|---------|
|SMB    | 2.X, 3.X      | 1 de SMB não é suportada.|
|NFS     | V3 e V4        |         |

## <a name="supported-virtualization-platforms-for-device"></a>Plataformas de Virtualização suportado para o dispositivo

| **Sistema operativo/plataforma**  |**Versões**   |**Notas**  |
|---------|---------|---------|
|Hyper-V  |  2012 R2 <br> 2016  |         |
|VMware ESXi     | 6.0 <br> 6.5        |As ferramentas VMware não são suportadas.         |


## <a name="supported-storage-accounts"></a>Contas de armazenamento suportadas

Aqui está uma lista dos tipos de armazenamento suportadas para o Gateway de caixa de dados.

| **Conta de armazenamento** | **Notas** |
| --- | --- |
| Clássica | Standard |
| Fins Gerais  |Standard; ambos o V1 e V2 são suportadas. São suportadas camadas frequentes e esporádica. |


## <a name="supported-storage-types"></a>Tipos de armazenamento suportadas

Aqui está uma lista dos tipos de armazenamento suportadas para o Gateway de caixa de dados.

| **Formato de ficheiro** | **Notas** |
| --- | --- |
| Blob de blocos do Azure | |
| BLOBs de páginas do Azure  | |
| Ficheiros do Azure | |

## <a name="supported-browsers-for-local-web-ui"></a>Browsers suportados para a IU web local

Aqui está uma lista dos browsers suportados para a web local da interface do Usuário para o dispositivo virtual.

|Browser  |Versões  |Notas/requisitos adicionais  |
|---------|---------|---------|
|Google Chrome   |Versão mais recente         |         |
|Microsoft Edge    | Versão mais recente        |         |
|Internet Explorer     | Versão mais recente        |         |
|FireFox    |Versão mais recente         |         |


## <a name="networking-requirements"></a>Requisitos de rede

A tabela seguinte lista as portas que têm de ser aberta na firewall para permitir o tráfego de gestão, na cloud ou SMB. Nesta tabela, *no* ou *entrada* refere-se para a direção do que acesso de pedidos de cliente recebidos para o seu dispositivo. *Out* ou *saída* refere-se para a direção em que o seu dispositivo de Gateway de caixa de dados envia dados externamente, além da implementação: por exemplo, a saída à Internet.

| Porta não.| Dentro ou para fora | Âmbito de porta| Necessário|   Notas                                                             |                                                                                     |
|--------|---------|----------|--------------|----------------------|---------------|
| TCP 80 (HTTP)|Saída|WAN |Não|Porta de saída é utilizada para acesso à Internet para obter atualizações. <br>O proxy da web de saída é configurável de utilizador. |                          
| TCP 443 (HTTPS)|Saída|WAN|Sim|Porta de saída é utilizada para aceder aos dados na cloud.<br>O proxy da web de saída é configurável de utilizador.|   
| UDP 53 (DNS)|Saída|WAN|Em alguns casos<br>Consulte as notas|Esta porta é necessária apenas se estiver a utilizar um servidor DNS baseado na Internet.<br>Recomendamos que utilize o servidor DNS local. |
| UDP 123 (NTP)|Saída|WAN|Em alguns casos<br>Consulte as notas|Esta porta é necessária apenas se estiver a utilizar um servidor NTP baseado na Internet.  |
| UDP 67 (DHCP)|Saída|WAN|Em alguns casos<br>Consulte as notas|Esta porta é necessária apenas se estiver a utilizar um servidor DHCP.  |
| TCP 80 (HTTP)|Em|LAN|Sim|Esta é a porta de entrada para a interface do Usuário local no dispositivo para gestão local. <br>Acessar a interface do Usuário local através de HTTP será automaticamente redirecionada para HTTPS.  | 
| TCP 443 (HTTPS)|Em|LAN|Sim|Esta é a porta de entrada para a interface do Usuário local no dispositivo para gestão local. | 

## <a name="url-patterns-for-firewall-rules"></a>Padrões de URL para regras de firewall

Os administradores de rede, muitas vezes, podem configurar regras de firewall avançado com base nos padrões de URL para filtrar a entrada e o tráfego de saída. O dispositivo de Gateway de caixa de dados e o serviço de Gateway de caixa de dados dependem de outras aplicações da Microsoft, como o Azure Service Bus, controlo de acesso do Azure Active Directory, contas de armazenamento e servidores do Microsoft Update. Os padrões de URL associados a estas aplicações podem ser utilizados para configurar regras de firewall. É importante compreender que os padrões de URL associados a estas aplicações podem alterar. Isso por sua vez, irá exigir o administrador de rede monitorizar e atualizar as regras de firewall para o seu Gateway de caixa de dados como e quando necessário.

Recomendamos que defina as regras de firewall para tráfego de saída, com base no Gateway de caixa de dados fixa livremente os endereços IP, na maioria dos casos. No entanto, pode utilizar as informações abaixo para definir as regras de firewall avançado que são necessários para criar ambientes seguros.

> [!NOTE]
> - O dispositivo (origem) IPs deve ser sempre definido para todas as interfaces de rede com capacidade de nuvem.
> - O destino IPs deve ser definido como [intervalos IP do datacenter do Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653).

|     Padrão de URL                                                                                                                                                                                                                                                                                                                                                                                                                                       |     Componente/funcionalidade                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
|    https://*.databoxedge.azure.com/*<br>https://*.servicebus.windows.net/*<br>https://login.windows.net                                                                                                                                                                                                                                                                                                        |    Serviço de Gateway de caixa de dados do Azure<br>Service Bus do Azure<br>Serviço de Autenticação    |
|    http://*.backup.windowsazure.com                                                                                                                                                                                                                                                                                                                                                                                                                   |    Ativação do dispositivo                                                                                    |
|    http://crl.microsoft.com/pki/*   http://www.microsoft.com/pki/*                                                                                                                                                                                                                                                                                                                                                                                    |    Revogação de certificados                                                                               |
|    https://*.core.windows.net/* https://*. data.microsoft.com http://*. msftncsi.com                                                                                                                                                                                                                                                                                                                                                                |    Contas de armazenamento do Azure e monitorização                                                                |
|    http://windowsupdate.microsoft.com<br>http://*. windowsupdate.microsoft.com<br>https://*. windowsupdate.microsoft.com<br>http://*. update.microsoft.com<br>https://*. update.microsoft.com<br>http://*. windowsupdate.com<br>http://download.microsoft.com<br>http://*. download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://*. ws.microsoft.com<br>https://*. ws.microsoft.com<br>http://*.MP.microsoft.com        |    Servidores do Microsoft Update                                                                             |
|    http://*.Deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                             |    Akamai CDN                                                                                           |
|    https://*.partners.extranet.microsoft.com/*                                                                                                                                                                                                                                                                                                                                                                                                        |    Pacote de suporte                                                                                      |
|    http://*.data.microsoft.com                                                                                                                                                                                                                                                                                                                                                                                                                        |    O serviço de telemetria no Windows, consulte a atualização para a experiência do cliente e a telemetria de diagnóstico      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                                                                         |



## <a name="internet-bandwidth"></a>Largura de banda de Internet

Os seguintes requisitos aplicam-se para a Internet largura de banda mínima disponível para os seus dispositivos de Gateway de caixa de dados.

- O Data Box Gateway tem uma largura de banda de Internet de 20 Mbps (ou mais) disponível sempre. Esta largura de banda não deve ser partilhada com outras aplicações. 
- O Gateway de caixa de dados tem uma largura de banda Mbps Internet dedicado 32 (ou mais) ao utilizar a limitação de rede.

## <a name="next-step"></a>Passo seguinte

* [Implementar o Gateway de caixa de dados do Azure](data-box-gateway-deploy-prep.md)

