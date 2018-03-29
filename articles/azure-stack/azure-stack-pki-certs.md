---
title: Requisitos de certificados de infraestrutura de chaves públicas de pilha do Azure para a pilha do Azure integrado sistemas | Microsoft Docs
description: Descreve os requisitos de implementação de certificados PKI de pilha do Azure para sistemas de pilha do Azure integrado.
services: azure-stack
documentationcenter: ''
author: mabriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: a5712e556d7b3bdcce38b8b8d39a08414ce0fd2f
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="azure-stack-public-key-infrastructure-certificate-requirements"></a>Requisitos de certificados de infraestrutura de chaves públicas de pilha do Azure
Pilha do Azure tem uma rede de infraestrutura público com externamente acessíveis endereços IP públicos atribuídos a um pequeno conjunto de serviços de pilha do Azure e, possivelmente, VMs inquilinas. São necessários certificados PKI com os nomes DNS adequados para estes pontos finais públicos de infraestrutura de pilha do Azure durante a implementação de pilha do Azure. Este artigo fornece informações sobre:

- Os certificados são necessários para implementar a pilha do Azure
- O processo de obtenção de certificados correspondentes essas especificações
- Como preparar, validar e utilizar os certificados em questão durante a implementação

> [!NOTE]
> Durante a implementação tem de copiar certificados para a pasta de implementação que corresponda ao fornecedor de identidade que está a implementar contra (Azure AD ou AD FS). Se utilizar um certificado único para todos os pontos finais, tem de copiar esse ficheiro de certificado para cada pasta de implementação, conforme descrito nas tabelas abaixo. A estrutura da pasta é criada previamente na máquina virtual implementação e podem ser encontrada em: C:\CloudDeployment\Setup\Certificates. 

## <a name="certificate-requirements"></a>Requisitos de certificado
A lista seguinte descreve os requisitos de certificados que são necessários para implementar a pilha do Azure: 
- Certificados tem de ser emitidos a partir de uma autoridade de certificação interna ou uma autoridade de certificação pública. Se for utilizada uma autoridade de certificação pública, tem de ser incluído na imagem base do sistema operativo como parte do programa autoridade de raiz fidedigna Microsoft. Pode encontrar a lista completa aqui: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca 
- A infraestrutura de pilha do Azure tem de ter acesso de rede para a autoridade de certificado utilizado para assinar os certificados
- Quando a rotação dos certificados, os certificados devem estar a emitido na mesma autoridade de certificado interno utilizado para assinar certificados fornecidos na implementação ou qualquer autoridade de certificação pública de acima
- A utilização de certificados autoassinados não são suportados
- O certificado pode ser um certificado de caráter universal único que abrangem a todos os espaços de nomes no campo do nome de alternativo do requerente (SAN). Em alternativa, pode utilizar certificados individuais utilizando os carateres universais para pontos finais, tais como acs e o Cofre de chaves, onde são necessárias. 
- O algoritmo de assinatura de certificado não pode ser SHA1, como deve ser mais forte. 
- O formato de certificado tem de ser PFX, como as chaves públicas e privadas são necessárias para a instalação de pilha do Azure. 
- Os ficheiros pfx do certificado tem um valor "Assinatura Digital" e "KeyEncipherment" do respetivo campo "Utilização de chave".
- Os ficheiros pfx do certificado tem de ter valores "Autenticação de servidor (1.3.6.1.5.5.7.3.1)" e "Autenticação de cliente (1.3.6.1.5.5.7.3.2)" no campo "Utilização de chave avançada".
- O certificado "emitido para:" campo não tem de ser igual ao respetivo "emitido por:" campo.
- As palavras-passe para todos os ficheiros pfx de certificado devem ser o mesmo no momento da implementação
- Certifique-se de que o se os nomes de requerente e do requerente alternativo nomes de todos os certificados coincidem com as especificações descritas neste artigo para evitar a implementação falhou.

> [!NOTE]
> Self-Signed certificados não são suportados.

> [!NOTE]
> A presença de autoridades de certificação intermediário no está da cadeia de fidedignidades de um certificado suportado. 

## <a name="mandatory-certificates"></a>Certificados obrigatórios
A tabela nesta secção descreve os certificados PKI do ponto final público de pilha do Azure que são necessários para ambas do Azure AD e implementações de pilha do AD FS do Azure. Requisitos de certificado são agrupados por área, bem como os espaços de nomes utilizados e os certificados que são necessárias para cada espaço de nomes. A tabela também descreve a pasta na qual o seu fornecedor de solução copia diferentes certificados por ponto final público. 

São necessários certificados com os nomes DNS adequados para cada ponto final público da infraestrutura de pilha do Azure. Nome DNS de cada ponto final é expresso no formato:  *&lt;prefixo >.&lt; Região >. &lt;fqdn >*. 

Para a sua implementação, a [Região] [externalfqdn] valores tem de coincidir com a região e nomes de domínio externo que escolheu para o seu sistema de pilha do Azure. Por exemplo, se o nome de região era *Redmond* e o nome de domínio externo era *contoso.com*, os nomes DNS teria o formato *&lt;prefixo >. redmond.contoso.com*. O  *&lt;prefixo >* valores são predesignated pela Microsoft para descrever o ponto final protegido pelo certificado. Além disso, o  *&lt;prefixo >* valores dos pontos finais externos infraestrutura dependem do serviço de pilha do Azure que utiliza o ponto final específico. 

|Pasta de implementação|Assunto do certificado necessário e os nomes alternativos do requerente (SAN)|Âmbito (por região)|Espaço de nomes de subdomínios|
|-----|-----|-----|-----|
|Portal público|portal.*&lt;region>.&lt;fqdn>*|Portais|*&lt;region>.&lt;fqdn>*|
|Portal de administração|adminportal.*&lt;region>.&lt;fqdn>*|Portais|*&lt;region>.&lt;fqdn>*|
|Público de Gestor de recursos do Azure|management.*&lt;region>.&lt;fqdn>*|Azure Resource Manager|*&lt;region>.&lt;fqdn>*|
|Administrador do Gestor de recursos do Azure|adminmanagement.*&lt;region>.&lt;fqdn>*|Azure Resource Manager|*&lt;region>.&lt;fqdn>*|
|ACS<sup>1</sup>|Um certificado de caráter universal de várias subdomínio com nomes alternativos do requerente para:<br>&#42;.blob.*&lt;region>.&lt;fqdn>*<br>&#42;.queue.*&lt;region>.&lt;fqdn>*<br>&#42;.table.*&lt;region>.&lt;fqdn>*|Armazenamento|blob.*&lt;region>.&lt;fqdn>*<br>table.*&lt;region>.&lt;fqdn>*<br>queue.*&lt;region>.&lt;fqdn>*|
|KeyVault|&#42;.vault.*&lt;region>.&lt;fqdn>*<br>(Certificado de SSL de caráter universal)|Cofre de Chaves|vault.*&lt;region>.&lt;fqdn>*|
|KeyVaultInternal|&#42;.adminvault.*&lt;region>.&lt;fqdn>*<br>(Certificado de SSL de caráter universal)|Keyvault interno|adminvault.*&lt;region>.&lt;fqdn>*|
|
<sup>1</sup> certificados de ACS o requer três SANs de caráter universal num certificado único. Caráter universal vários SANs num único certificado poderá não ser suportado por todas as autoridades de certificação pública. 

Se implementar pilha do Azure utilizando o modo de implementação do Azure AD, apenas terá de pedir certificados listados na tabela anterior. No entanto, se implementar pilha do Azure utilizando o modo de implementação do AD FS, tem também de pedir certificados descritos na seguinte tabela:

|Pasta de implementação|Assunto do certificado necessário e os nomes alternativos do requerente (SAN)|Âmbito (por região)|Espaço de nomes de subdomínios|
|-----|-----|-----|-----|
|ADFS|adfs.*&lt;region>.&lt;fqdn>*<br>(Certificado SSL)|ADFS|*&lt;region>.&lt;fqdn>*|
|Graph|graph.*&lt;region>.&lt;fqdn>*<br>(Certificado SSL)|Graph|*&lt;region>.&lt;fqdn>*|
|

> [!IMPORTANT]
> Todos os certificados indicados nesta secção tem de ter a mesma palavra-passe. 

## <a name="optional-paas-certificates"></a>Certificados de PaaS opcionais
Se estiver a planear implementar os serviços do Azure pilha PaaS adicionais (SQL Server, MySQL e do serviço de aplicações) após a pilha do Azure foi implementado e configurado, terá de pedir certificados adicionais para cobrir os pontos finais dos serviços de PaaS. 

> [!IMPORTANT]
> Os certificados que utiliza para fornecedores de recursos do serviço de aplicações, o SQL Server e o MySQL tem de ter a mesma autoridade de raiz como são utilizados para os pontos finais de pilha do Azure global. 

A tabela seguinte descreve os pontos finais e certificados necessários para os adaptadores de SQL e o MySQL e para o serviço de aplicações. Não precisa de copiar estes certificados para a pasta de implementação de pilha do Azure. Em vez disso, forneça estes certificados quando instala os fornecedores de recursos adicionais. 

|Âmbito (por região)|Certificado|Assunto do certificado necessário e os nomes de alternativo do requerente (SANs)|Espaço de nomes de subdomínios|
|-----|-----|-----|-----|
|SQL, MySQL|SQL Server e o MySQL|&#42;.dbadapter.*&lt;region>.&lt;fqdn>*<br>(Certificado de SSL de caráter universal)|dbadapter.*&lt;region>.&lt;fqdn>*|
|Serviço de Aplicações|Certificado SSL do Web tráfego predefinido|&#42;.appservice.*&lt;region>.&lt;fqdn>*<br>&#42;.scm.appservice.*&lt;region>.&lt;fqdn>*<br>(Certificado de SSL de caráter universal de domínio de várias<sup>1</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|Serviço de Aplicações|API|api.appservice.*&lt;region>.&lt;fqdn>*<br>(Certificado SSL<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|Serviço de Aplicações|FTP|ftp.appservice.*&lt;region>.&lt;fqdn>*<br>(Certificado SSL<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|
|Serviço de Aplicações|SSO|sso.appservice.*&lt;region>.&lt;fqdn>*<br>(Certificado SSL<sup>2</sup>)|appservice.*&lt;region>.&lt;fqdn>*<br>scm.appservice.*&lt;region>.&lt;fqdn>*|

<sup>1</sup> requer um certificado com vários nomes alternativos para o caráter universal do requerente. Caráter universal vários SANs num único certificado poderá não ser suportado por todas as autoridades de certificação pública 

<sup>2</sup> A &#42;.appservice. *&lt;região >. &lt;fqdn >* certificado de caráter universal não pode ser utilizado em vez destas três certificados (api.appservice. *&lt;região >. &lt;fqdn >*, ftp.appservice. *&lt;região >. &lt;fqdn >*e sso.appservice. *&lt;região >. &lt;fqdn >*. Serviço de aplicações requer explicitamente a utilização de certificados separados para estes pontos finais. 

## <a name="learn-more"></a>Saiba mais
Saiba como [gerar os certificados PKI para a implementação da pilha de Azure](azure-stack-get-pki-certs.md). 

## <a name="next-steps"></a>Passos Seguintes
[Integração de identidade](azure-stack-integrate-identity.md)

