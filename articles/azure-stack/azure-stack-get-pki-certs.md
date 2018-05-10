---
title: Gerar os certificados de infraestrutura de chave pública do Azure pilha para a implementação de sistemas de pilha do Azure integrado | Microsoft Docs
description: Descreve o processo de implementação do certificado PKI de pilha do Azure para sistemas de pilha do Azure integrado.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 17737c2b272f2a123df3d58c62c471b3da5bebe1
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="azure-stack-certificates-signing-request-generation"></a>Certificados de pilha do Azure, a geração de pedido de assinatura

A ferramenta do Verificador de preparação de pilha do Azure descrita neste artigo está disponível [da galeria do PowerShell](https://aka.ms/AzsReadinessChecker). A ferramenta cria pedidos de assinatura de certificado (CSRs) adequado para uma implementação de pilha do Azure. Certificados devem ser solicitados, gerados e validados com tempo suficiente para testar antes da implementação.

A ferramenta do Verificador de preparação de pilha do Azure (AzsReadinessChecker) efetua os seguintes pedidos de certificado:

 - **Pedidos de certificado padrão**  
    Pedido de acordo com [gerar os certificados PKI para a implementação de pilha do Azure](azure-stack-get-pki-certs.md).
 - **Tipo de pedido**  
    Especifica se pretende ou não a solicitação de assinatura de certificado será pedido único ou vários pedidos.
 - **Plataforma-como-um-serviço**  
    Pedido, opcionalmente, os nomes de (PaaS) de plataforma-como-um-serviço certificados conforme especificado no [requisitos de certificados de infraestrutura de chave pública do Azure pilha - certificados PaaS opcionais](azure-stack-pki-certs.md#optional-paas-certificates).



## <a name="prerequisites"></a>Pré-requisitos

O sistema deve cumprir os seguintes pré-requisitos antes de gerar CSR(s) para certificados PKI para uma implementação de pilha do Azure:

 - Verificador de preparação de pilha do Microsoft Azure
 - Atributos de certificado:
    - Nome da região
    - Nome de domínio completamente qualificado (FQDN) externo
    - Assunto
 - Windows 10 ou Windows Server 2016
 
  > [!NOTE]
  > Quando receber os certificados de fazer uma cópia da sua autoridade de certificação os passos em [certificados PKI de pilha de Azure preparar](azure-stack-prepare-pki-certs.md) terão de ser concluídos no mesmo sistema!

## <a name="generate-certificate-signing-requests"></a>Gerar pedidos de assinatura de certificado

Utilize estes passos para preparar e validar os certificados PKI de pilha do Azure: 

1.  Instale AzsReadinessChecker a partir de uma linha de comandos PowerShell (5.1 ou acima), executando o seguinte cmdlet:

    ````PowerShell  
        Install-Module Microsoft.AzureStack.ReadinessChecker
    ````

2.  Declarar o **requerente** como um dicionário ordenado. Por exemplo: 

    ````PowerShell  
    $subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"} 
    ````
    > [!note]  
    > Se for fornecido um nome comum (CN) esta será substituída pelo nome DNS do primeiro do pedido de certificado.

3.  Declare um diretório de saída que já exista:

    ````PowerShell  
    $outputDirectory = "$ENV:USERNAME\Documents\AzureStackCSR" 
    ````
4.  Declarar identificar sistema

    Azure Active Directory

    ```PowerShell
    $IdentitySystem = "AAD"
    ````

    Serviços de Federação do Active Directory

    ```PowerShell
    $IdentitySystem = "ADFS"
    ````

5. Declarar **nome de região** e um **FQDN externo** pretendido para a implementação de pilha do Azure.

    ```PowerShell
    $regionName = 'east'
    $externalFQDN = 'azurestack.contoso.com'
    ````

    > [!note]  
    > `<regionName>.<externalFQDN>` compõe a base no qual todos os nomes DNS externos na pilha do Azure são criados, neste exemplo, o portal seria `portal.east.azurestack.contoso.com`.

6. Para gerar um pedido de certificado único com vários nomes de alternativo do requerente:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType SingleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Para incluir serviços PaaS especificar o comutador ```-IncludePaaS```

7. Para gerar pedidos para cada nome DNS de assinatura de certificado individuais:

    ```PowerShell  
    Start-AzsReadinessChecker -RegionName $regionName -FQDN $externalFQDN -subject $subjectHash -RequestType MultipleCSR -OutputRequestPath $OutputDirectory -IdentitySystem $IdentitySystem
    ````

    Para incluir serviços PaaS especificar o comutador ```-IncludePaaS```

8. Reveja o resultado:

    ````PowerShell  
    AzsReadinessChecker v1.1803.405.3 started
    Starting Certificate Request Generation

    CSR generating for following SAN(s): dns=*.east.azurestack.contoso.com&dns=*.blob.east.azurestack.contoso.com&dns=*.queue.east.azurestack.contoso.com&dns=*.table.east.azurestack.cont
    oso.com&dns=*.vault.east.azurestack.contoso.com&dns=*.adminvault.east.azurestack.contoso.com&dns=portal.east.azurestack.contoso.com&dns=adminportal.east.azurestack.contoso.com&dns=ma
    nagement.east.azurestack.contoso.com&dns=adminmanagement.east.azurestack.contoso.com
    Present this CSR to your Certificate Authority for Certificate Generation: C:\Users\username\Documents\AzureStackCSR\wildcard_east_azurestack_contoso_com_CertRequest_20180405233530.req
    Certreq.exe output: CertReq: Request Created

    Finished Certificate Request Generation

    AzsReadinessChecker Log location: C:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\1.1803.405.3\AzsReadinessChecker.log
    AzsReadinessChecker Completed
    ````

9.  Submeter o **. REQ** ficheiro gerado a sua AC (interno ou público).  O diretório de saída do **início AzsReadinessChecker** contém CSR(s) necessários para submeter a uma autoridade de certificação.  Também contém um diretório de subordinados que contém os ficheiros INF utilizados durante a geração de pedido de certificado, como uma referência. Certifique-se de que a sua AC gera certificados utilizando o seu pedido gerado que cumprem os [requisitos do Azure pilha PKI](azure-stack-pki-certs.md).

## <a name="next-steps"></a>Passos Seguintes

[Preparar os certificados PKI de pilha do Azure](azure-stack-prepare-pki-certs.md)
