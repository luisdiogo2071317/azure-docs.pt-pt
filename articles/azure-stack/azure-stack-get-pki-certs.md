---
title: "Gerar os certificados de infraestrutura de chave pública do Azure pilha para a implementação de sistemas de pilha do Azure integrado | Microsoft Docs"
description: "Descreve os sistemas de pilha do Azure integrado processfor de implementação de certificados de PKI de pilha do Azure."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: e7edbc1c4aa6e3cb1026d493886ef7ca704b9131
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="generate-pki-certificates-for-azure-stack-deployment"></a>Gerar os certificados PKI para a implementação de pilha do Azure
Agora que já sabe [os requisitos de certificado PKI](azure-stack-pki-certs.md) para implementações de pilha do Azure, terá de obter esses certificados a partir da autoridade de certificação (CA) à sua escolha. 

## <a name="request-certificates-using-an-inf-file"></a>Solicitar certificados utilizando um ficheiro INF
É uma forma para pedir certificados de uma AC pública ou uma AC interna, utilizando um ficheiro INF. O utilitário de certreq.exe incorporado do Windows pode utilizar um ficheiro INF especificar detalhes do certificado, para gerar um ficheiro de pedido, conforme descrito nesta secção. 

### <a name="sample-inf-file"></a>Ficheiro INF de exemplo 
O ficheiro de INF de pedido de certificado de exemplo pode ser utilizado para criar um ficheiro de pedido de certificado offline para submissão para uma AC (interno ou público). INF abrange todos os pontos finais necessários (incluindo os serviços de PaaS opcionais) num certificado de caráter universal único. 

O ficheiro INF do exemplo assume que nessa região é igual a **sea** e o valor FQDN externo é **sea &#46;contoso &#46; com**. Alterar os valores para corresponder ao seu ambiente antes de gerar um. Ficheiro INF para a sua implementação. 

    
    [Version] 
    Signature="$Windows NT$"

    [NewRequest] 
    Subject = "C=US, O=Microsoft, L=Redmond, ST=Washington, CN=portal.sea.contoso.com"

    Exportable = TRUE                   ; Private key is not exportable 
    KeyLength = 2048                    ; Common key sizes: 512, 1024, 2048, 4096, 8192, 16384 
    KeySpec = 1                         ; AT_KEYEXCHANGE 
    KeyUsage = 0xA0                     ; Digital Signature, Key Encipherment 
    MachineKeySet = True                ; The key belongs to the local computer account 
    ProviderName = "Microsoft RSA SChannel Cryptographic Provider" 
    ProviderType = 12 
    SMIME = FALSE 
    RequestType = PKCS10
    HashAlgorithm = SHA256

    ; At least certreq.exe shipping with Windows Vista/Server 2008 is required to interpret the [Strings] and [Extensions] sections below

    [Strings] 
    szOID_SUBJECT_ALT_NAME2 = "2.5.29.17" 
    szOID_ENHANCED_KEY_USAGE = "2.5.29.37" 
    szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1" 
    szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

    [Extensions] 
    %szOID_SUBJECT_ALT_NAME2% = "{text}dns=*.sea.contoso.com&dns=*.blob.sea.contoso.com&dns=*.queue.sea.contoso.com&dns=*.table.sea.contoso.com&dns=*.vault.sea.contoso.com&dns=*.adminvault.sea.contoso.com&dns=*.dbadapter.sea.contoso.com&dns=*.appservice.sea.contoso.com&dns=*.scm.appservice.sea.contoso.com&dns=api.appservice.sea.contoso.com&dns=ftp.appservice.sea.contoso.com&dns=sso.appservice.sea.contoso.com&dns=adminportal.sea.contoso.com&dns=management.sea.contoso.com&dns=adminmanagement.sea.contoso.com" 
    %szOID_ENHANCED_KEY_USAGE% = "{text}%szOID_PKIX_KP_SERVER_AUTH%,%szOID_PKIX_KP_CLIENT_AUTH%"

    [RequestAttributes]
    

## <a name="generate-and-submit-request-to-the-ca"></a>Gerar e submeter pedido para a AC
O fluxo de trabalho seguinte descreve como pode personalizar e utilize o ficheiro INF de exemplo gerado anteriormente para pedir um certificado a partir de uma AC:

1. **Editar e guardar o ficheiro INF**. Copiar o exemplo fornecido e guardá-lo para um novo ficheiro de texto. Substitua o nome do requerente e o FQDN externo com os valores que correspondem à sua implementação e guarde o ficheiro como um. Ficheiro INF.
2. **Gerar um pedido com certreq**. Utilizar um computador Windows, inicie uma linha de comandos como administrador e execute o seguinte comando para gerar um ficheiro de pedido (.req): `certreq -new <yourinffile>.inf <yourreqfilename>.req`.
3. **Submeter a AC**. Submeter o. Ficheiro de pedidos gerado a sua AC (pode ser interno ou público).
4. **Importe. CER**. Devolve a AC um. Ficheiro CER. Utilizar o mesmo. o computador Windows a partir do qual gerou o ficheiro de pedido, importar o. Ficheiro CER devolvido para o arquivo do computador/pessoal. 
5. **Exportar e copiar. PFX para pastas de implementação**. Exportar o certificado (incluindo a chave privada) como um. PFX ficheiro e copie o. Ficheiro PFX para as pastas de implementação descrita no [requisitos do Azure pilha implementação PKI](azure-stack-pki-certs.md).

