---
title: Azure pilha integração do Centro de dados - publicar pontos finais
description: Saiba como publicar pontos finais de pilha do Azure no seu centro de dados
services: azure-stack
author: jeffgilb
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 03/21/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: ''
ms.openlocfilehash: 694b8604949406c1c3064fd91bf1fe38f83ee946
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure pilha integração do Centro de dados - publicar pontos finais
Pilha do Azure configura endereços IP virtuais (VIPs) para as respetivas funções de infraestrutura. Estes VIPs são atribuídos de conjunto de endereços IP público. Cada VIP está protegido com uma lista de controlo de acesso (ACL) na camada de rede definidas por software. As ACLs também são utilizadas em todos os comutadores físicos (TORs e BMC) para proteger ainda mais a solução. Uma entrada DNS é criada para cada ponto final na zona DNS externa que especificado no momento da implementação.


O diagrama da arquitetura seguinte mostra as camadas de rede diferente e ACLs:

![Imagem estrutural](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Portas e protocolos (entrada)

Um conjunto de infraestrutura VIPs são necessários para publicação do Azure pilha pontos finais para redes externas. O *ponto final (VIP)* tabela mostra cada ponto final, a porta necessária e o protocolo. Consulte a documentação de implementação do fornecedor de recursos específico para pontos finais que necessitam de fornecedores de recursos adicionais, tais como o fornecedor de recursos do SQL Server.

Infra-estrutura interna que vips não estão listados porque não forem necessários para publicação pilha do Azure.

> [!NOTE]
> Utilizador VIPs são dinâmicos, definidas pelos próprios utilizadores com nenhum controlo pela operadora de rede de pilha do Azure.


|Ponto final (VIP)|Um registo de anfitrião de DNS|Protocolo|Portas|
|---------|---------|---------|---------|
|AD FS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portal (administrador)|Adminportal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|O Azure Resource Manager (administrador)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portal (utilizador)|Portal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
|O Azure Resource Manager (utilizador)|Management.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Graph|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Lista de revogação de certificados|Crl.*&lt;region>.&lt;fqdn>*|HTTP|80|
|DNS|&#42;.*&lt;region>.&lt;fqdn>*|TCP E UDP|53|
|Cofre de chaves (utilizador)|&#42;.vault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Cofre de chaves (administrador)|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Fila de Armazenamento|&#42;.queue.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Tabela de armazenamento|&#42;.table.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Blob de armazenamento|&#42;.blob.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Fornecedor de recursos SQL|sqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|Fornecedor de recursos de MySQL|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|Serviço de Aplicações|&#42;.appservice.*&lt;region>.&lt;fqdn>*|TCP|80 (HTTP)<br>443 (HTTPS)<br>8172 (MSDeploy)|
|  |&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)|
|  |api.appservice.*&lt;region>.&lt;fqdn>*|TCP|443 (HTTPS)<br>44300 (Gestor de recursos do azure)|
|  |ftp.appservice.*&lt;region>.&lt;fqdn>*|TCP, UDP|21, 1021, 10001-101000 (FTP)<br>990 (FTPS)|

## <a name="ports-and-urls-outbound"></a>Portas e URLs (saídos)

Pilha do Azure suporta apenas servidores de proxy transparente. Numa implementação em que um uplinks de proxy transparentes para um servidor proxy tradicionais, tem de permitir os URLs e portas seguintes para comunicação de saída:


|Objetivo|do IdP|Protocolo|Portas|
|---------|---------|---------|---------|
|Identidade|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net|HTTP<br>HTTPS|80<br>443|
|Sindicação do Marketplace|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Patch & atualização|https://&#42;.azureedge.net|HTTPS|443|
|Registo|https://management.azure.com|HTTPS|443|
|Utilização|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.com|HTTPS|443|


## <a name="next-steps"></a>Passos Seguintes

[Requisitos de PKI de pilha do Azure](azure-stack-pki-certs.md)