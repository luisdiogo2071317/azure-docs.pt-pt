---
title: Ver e modificar os nomes de anfitrião | Microsoft Docs
description: Como ver e alterar os nomes de anfitrião para máquinas virtuais do Azure, web e funções de trabalho para resolução de nomes
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2018
ms.author: genli
ms.openlocfilehash: f4c602368368e8ef36581d3f035ff3943a8f0d8f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34657286"
---
# <a name="viewing-and-modifying-hostnames"></a>Ver e modificar os nomes de anfitrião
Para permitir que as instâncias da função ser referenciado por nome de anfitrião, tem de definir o valor para o nome de anfitrião no ficheiro de configuração do serviço para cada função. Fazê-lo ao adicionar o nome de anfitrião pretendido para o **vmName** atributo o **função** elemento. O valor da **vmName** atributo é utilizado como base para o nome de anfitrião de cada instância de função. Por exemplo, se **vmName** é *webrole* e existem três instâncias de função, os nomes de anfitrião das instâncias será *webrole0*, *webrole1*, e *webrole2*. Não tem de especificar um nome de anfitrião para máquinas virtuais no ficheiro de configuração, porque o nome de anfitrião para uma máquina virtual é preenchido com base no nome da máquina virtual. Para obter mais informações sobre como configurar um serviço do Microsoft Azure, consulte [esquema de configuração de serviço do Azure (. cscfg ficheiro)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Ver os nomes de anfitrião
Pode ver os nomes de anfitrião de máquinas virtuais e instâncias de função no serviço em nuvem, utilizando qualquer uma das ferramentas abaixo.

### <a name="service-configuration-file"></a>Ficheiro de configuração de serviço
Pode transferir o ficheiro de configuração de serviço para um serviço implementado o **configurar** painel do serviço no portal do Azure. Em seguida, pode procurar o **vmName** atributo para o **nome da função** elemento para ver o nome de anfitrião. Tenha em atenção que este nome de anfitrião é utilizado como base para o nome de anfitrião de cada instância de função. Por exemplo, se **vmName** é *webrole* e existem três instâncias de função, os nomes de anfitrião das instâncias será *webrole0*, *webrole1*, e *webrole2*.

### <a name="remote-desktop"></a>Ambiente de Trabalho Remoto
Depois de ativar o ambiente de trabalho remoto (Windows), sistema de interação remota do Windows PowerShell (Windows) ou ligações SSH (Linux e Windows) a máquinas virtuais ou instâncias de função, pode ver o nome de anfitrião a partir de uma ligação de ambiente de trabalho remoto Active Directory de várias formas:

* Escreva o nome do anfitrião na linha de comandos ou do terminal SSH.
* Escreva o ipconfig/todas as no comando (apenas Windows) de linha de comandos.
* Ver o nome do computador em que as definições do sistema (apenas Windows).

### <a name="azure-service-management-rest-api"></a>REST API de gestão de serviço do Azure
De um cliente REST, siga estas instruções:

1. Certifique-se de que tem um certificado de cliente para ligar ao portal do Azure. Para obter um certificado de cliente, siga os passos apresentados na [como: transferir e importar definições de publicação e informações de subscrição](https://msdn.microsoft.com/library/dn385850.aspx). 
2. Defina uma entrada de cabeçalho x-ms-version com um valor de 2013-11-01 com o nome.
3. Envie um pedido no seguinte formato: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<service-name\>?embed-detail=true
4. Procure o **HostName** elemento para cada **RoleInstance** elemento.

> [!WARNING]
> Também pode ver o sufixo de domínio interno do serviço em nuvem da resposta de chamada REST, verificando o **InternalDnsSuffix** elemento, ou executando o ipconfig/todas as numa linha de comandos numa sessão de ambiente de trabalho remoto (Windows) ou executando cat /etc/resolv.conf de um terminal SSH (Linux).
> 
> 

## <a name="modifying-a-hostname"></a>Modificar um nome de anfitrião
Pode modificar o nome de anfitrião para qualquer máquina virtual ou instância de função através do carregamento de um ficheiro de configuração do serviço modificado ou mudar o nome do computador a partir de uma sessão de ambiente de trabalho remoto.

## <a name="next-steps"></a>Passos Seguintes
[Resolução de nomes (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Esquema de configuração do serviço do Azure (. cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Esquema de configuração de rede Virtual do Azure](http://go.microsoft.com/fwlink/?LinkId=248093)

[Especifique as definições de DNS a utilizar ficheiros de configuração de rede](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)

