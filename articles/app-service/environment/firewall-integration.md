---
title: Bloquear o tráfego de saída do ambiente de serviço de aplicações do Azure
description: Descreve como integrar o Firewall do Azure para proteger o tráfego de saída
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 955a4d84-94ca-418d-aa79-b57a5eb8cb85
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: ccompy
ms.openlocfilehash: bcd5cb15c003ea146263a6e71a280f7da42ba830
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47168024"
---
# <a name="app-service-environment-certificates-overview"></a>Descrição geral de certificados do ambiente de serviço de aplicações 

O ambiente de serviço de aplicações (ASE) tem um número de dependências externas, que requer acesso para funcionar corretamente. O ASE mora no cliente de rede Virtual do Azure (VNet). Os clientes têm de permitir o tráfego de dependência do ASE, o que é um problema para os clientes que pretende bloquear todos os saída da sua VNet.

Há um número de dependências de entrada que tenha um ASE. Não é possível enviar o tráfego de entrada de gestão através de um dispositivo de firewall. Os endereços de origem para este tráfego são conhecidos e são publicados na [endereços de gestão do ambiente de serviço de aplicações](https://docs.microsoft.com/azure/app-service/environment/management-addresses) documento. Pode criar regras do grupo de segurança de rede com essas informações para proteger o tráfego de entrada.

As dependências de saída do ASE quase que totalmente são definidas com FQDNs, que não têm endereços estáticos por trás delas. A falta de endereços estáticos significa que grupos de segurança de rede (NSGs) não pode ser utilizados para bloquear o tráfego de saída do ASE. Os endereços são alterados com frequência suficiente uma não é possível configurar regras com base na resolução atual e usá-lo para criar NSGs. 

A solução para proteger os endereços de saída está em utilização de um dispositivo de firewall que pode controlar o tráfego de saída com base nos nomes de domínio. A equipe de rede do Azure tem de colocar um novo dispositivo de rede em pré-visualização, chamado de Firewall do Azure. O Firewall do Azure é capaz de restrição de saída HTTP e o tráfego HTTPS com base no nome DNS de destino.  

## <a name="configuring-azure-firewall-with-your-ase"></a>Configurar a Firewall do Azure com o seu ASE 

Os passos para bloquear a saída do ASE com o Firewall do Azure são:

1. Crie uma Firewall do Azure na VNet em que o seu ASE está ou será. [Documentação do Azure do Firewall](https://docs.microsoft.com/azure/firewall/)
2. Na IU de Firewall do Azure, selecione a etiqueta de FQDN de ambiente de serviço de aplicações
3. Criar uma tabela de rotas com os endereços de gestão do [endereços de gestão do ambiente de serviço de aplicações]( https://docs.microsoft.com/azure/app-service/environment/management-addresses) com um salto seguinte da Internet. As entradas de tabela de rota são necessárias para evitar problemas de encaminhamento assimétricos. 
4. Adicione rotas para as dependências de endereço IP indicadas abaixo nas dependências de endereço IP com um salto seguinte da Internet. 
5. Adicionar uma rota à sua tabela de rota para 0.0.0.0/0 com o salto seguinte a ser a sua aplicação de rede de Firewall do Azure
6. Criar pontos finais de serviço para o SQL do Azure e o armazenamento do Azure da sub-rede do ASE
7. Atribuir a tabela de rotas que criou para a sub-rede do ASE  

## <a name="application-traffic"></a>Tráfego de aplicativo 

Os passos acima irão permitir que o ASE para operar sem problemas. Ainda tem de configurar as coisas para acomodar as necessidades da sua aplicação. Há dois problemas para aplicações num ASE que está configurado com a Firewall do Azure.  

- Dependência de aplicação FQDNs tem de ser adicionada para o Firewall do Azure ou a tabela de rotas
- As rotas têm de ser criadas para os endereços que serão provenientes tráfego para evitar problemas de encaminhamento assimétrico

Se as aplicações têm dependências, terá de ser adicionado à Firewall do Azure. Crie regras de aplicações para permitir tráfego HTTP/HTTPS e regras de rede para todo o resto. 

Se sabe o intervalo de endereços que o tráfego de pedido de aplicação serão provenientes, pode adicionar que à tabela de rotas que está atribuído a sub-rede do ASE. Se o intervalo de endereços for grande ou não especificado, em seguida, pode utilizar uma aplicação de rede, como o Gateway de aplicação para que tenha um endereço para adicionar à sua tabela de rota. Para obter detalhes sobre como configurar um Gateway de aplicação com o ASE de ILB, leia [integrar o ASE de ILB com um Gateway de aplicação](https://docs.microsoft.com/azure/app-service/environment/integrate-with-application-gateway)


## <a name="dependencies"></a>Dependências

O serviço de aplicações do Azure tem um número de dependências externas. Eles podem ser categoricamente divididos em várias áreas principais:

- Ponto final de serviço devem ser configurados serviços compatíveis com pontos finais de serviço com se desejar bloquear o tráfego de rede de saída.
- Pontos finais do endereço IP não são abordados com um nome de domínio. Isso pode ser um problema para os dispositivos de firewall que esperar todo o tráfego HTTPS para utilizar nomes de domínio. Os pontos de extremidade de endereço IP devem ser adicionados à tabela de rotas definida na sub-rede do ASE.
- Pontos finais HTTP/HTTPS de FQDN podem ser colocados no seu dispositivo de firewall.
- Pontos finais HTTP/HTTPS de caráter universal são dependências que podem variar com o seu ASE com base num número de qualificadores. 
- Dependências de Linux são apenas uma preocupação, se estiver a implementar aplicações do Linux em ASE. Se não estiver a implementar aplicações do Linux em ASE, estes endereços não é necessário ser adicionado à firewall. 


#### <a name="service-endpoint-capable-dependencies"></a>Dependências com capacidade de ponto final de serviço 

| Ponto Final |
|----------|
| SQL do Azure |
| Storage do Azure |
| Cofre de chaves do Azure |


#### <a name="ip-address-dependencies"></a>Dependências de endereço IP 

| Ponto Final |
|----------|
| 40.77.24.27:443 |
| 13.82.184.151:443 |
| 13.68.109.212:443 |
| 13.90.249.229:443 |
| 13.91.102.27:443 |
| 104.45.230.69:443 |
| 168.62.226.198:12000 |


#### <a name="fqdn-httphttps-dependencies"></a>Dependências de FQDN HTTP/HTTPS 

| Ponto Final |
|----------|
|graph.windows.net:443 |
|login.live.com:443 |
|login.Windows.com:443 |
|login.Windows.NET:443 |
|login.microsoftonline.com:443 |
|Client.WNS.Windows.com:443 |
|definitionupdates.microsoft.com:443 |
|go.microsoft.com:80 |
|go.microsoft.com:443 |
|www.microsoft.com:80 |
|www.microsoft.com:443 |
|wdcpalt.microsoft.com:443 |
|wdcp.microsoft.com:443 |
|OCSP.msocsp.com:443 |
|mscrl.microsoft.com:443 |
|mscrl.microsoft.com:80 |
|CRL.microsoft.com:443 |
|crl.microsoft.com:80 |
|www.Thawte.com:443 |
|crl3.digicert.com:80 |
|OCSP.digicert.com:80 |
|csc3-2009-2.crl.verisign.com:80 |
|CRL.VeriSign.com:80 |
|OCSP.VeriSign.com:80 |
|azperfcounters1.blob.Core.Windows .net: 443 |
|azurewatsonanalysis prod.core.windows.net:443 |
|global.Metrics.nsatc.NET:80   |
|AZ prod.metrics.nsatc.net:443 |
|antares.Metrics.nsatc.NET:443 |
|azglobal black.azglobal.metrics.nsatc.net:443 |
|azglobal red.azglobal.metrics.nsatc.net:443 |
|antares black.antares.metrics.nsatc.net:443 |
|antares red.antares.metrics.nsatc.net:443 |
|maupdateaccount.blob.Core.Windows.NET:443 |
|clientconfig.Passport.NET:443 |
|Packages.microsoft.com:443 |
|schemas.microsoft.com:80 |
|schemas.microsoft.com:443 |
|Management.Core.Windows.NET:443 |
|Management.Core.Windows.NET:80 |
|www.msftconnecttest.com:80 |
|shavamanifestcdnprod1.azureedge .net: 443 |
|validação v2.sls.microsoft.com:443 |
|flighting.CP.WD.microsoft.com:443 |
|DMD.metaservices.microsoft.com:80 |
|Admin.Core.Windows.NET:443 |
|azureprofileruploads.blob.Core.Windows.NET:443 |
|azureprofileruploads2.blob.Core.Windows .net: 443 |
|azureprofileruploads3.blob.Core.Windows .net: 443 |
|azureprofileruploads4.blob.Core.Windows .net: 443 |
|azureprofileruploads5.blob.Core.Windows .net: 443 |

#### <a name="wildcard-httphttps-dependencies"></a>Dependências HTTP/HTTPS de caráter universal 

| Ponto Final |
|----------|
|Gr-Prod -\*. cloudapp.net:443 |
| \*. management.azure.com:443 |
| \*. update.microsoft.com:443 |
| \*. windowsupdate.microsoft.com:443 |
|grmdsprod\*mini\*. servicebus.windows.net:443 |
|grmdsprod\*lini\*. servicebus.windows.net:443 |
|grsecprod\*mini\*. servicebus.windows.net:443 |
|grsecprod\*lini\*. servicebus.windows.net:443 |
|graudprod\*mini\*. servicebus.windows.net:443 |
|graudprod\*lini\*. servicebus.windows.net:443 |

#### <a name="linux-dependencies"></a>Dependências do Linux 

| Ponto Final |
|----------|
|wawsinfraprodbay063.blob.Core.Windows .net: 443 |
|registo 1.docker.io:443 |
|auth.docker.IO:443 |
|Production.cloudflare.docker.com:443 |
|download.docker.com:443 |
|US.Archive.ubuntu.com:80 |
|download.mono project.com:80 |
|Packages.treasuredata.com:80|
|Security.ubuntu.com:80 |

