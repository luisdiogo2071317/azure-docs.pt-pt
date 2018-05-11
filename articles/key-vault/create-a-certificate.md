---
title: Métodos de criação do certificado
description: Formas de criar um certificado no Cofre de chaves.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e17b4c9b-4ff3-472f-8c9d-d130eb443968
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: 7b71c6a8daa97300ecf3b37ec6ab47207fece98e
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="certificate-creation-methods"></a>Métodos de criação do certificado

 Um certificado do Cofre de chave (KV) pode ser criado ou importado para um cofre de chaves. Quando é criado um certificado de KV a chave privada é criada no Cofre de chaves e nunca exposta ao proprietário do certificado. Seguem-se formas de criar um certificado no Cofre de chaves:  

-   **Criar um certificado autoassinado:** este procedimento cria um par de chaves públicas-privadas e associá-la a um certificado. O certificado será assinado pela sua própria chave.  

-    **Criar manualmente um novo certificado:** este procedimento cria um par de chaves públicas-privadas e gerar um pedido de assinatura de certificado x. 509. O pedido de assinatura pode ser assinado por sua autoridade de registo ou a autoridade de certificação. O certificado pode ser intercalado com a chave pendente de x509 assinado emparelhe para concluir o certificado de KV no Cofre de chaves. Embora este método requer mais passos,-lhe fornecer maior segurança porque a chave privada é criada no e restringida ao Cofre de chaves. Isto é explicado no diagrama abaixo.  

![Criar um certificado com a sua própria autoridade de certificação](media/certificate-authority-1.png)  

As seguintes descrições correspondem aos passos lettered verdes no diagrama anterior.

1. No diagrama acima, a aplicação está a criar um certificado que internamente começa por criar uma chave no seu Cofre de chaves.
2. O Cofre de chaves devolve à sua aplicação, um certificado de assinatura do pedido (CSR)
3. A aplicação transmite o CSR para a AC de escolhida.
4. A escolhida AC responde com uma um X509 certificado.
5. A aplicação concluir a criação de certificado novo com uma fusão do X509 certificado da AC.

-   **Criar um certificado com um fornecedor de emissor conhecidos:** este método requer que efetue uma única tarefa de criação de um objeto de emissor. Assim que for criado um objeto de emissor no seu Cofre de chaves, o respetivo nome pode ser referenciado na política do certificado KV. Um pedido para criar um certificado de KV irá criar um par de chaves no cofre e comunicar com o serviço do fornecedor de emissor utilizando as informações no objeto referenciado emissor para obter um x509 certificado. Criação de certificados x509 é obtido a partir do serviço de emissor de certificado e intercalado com o par de chaves para concluir a KV.  

![Criar um certificado com uma autoridade de certificação do Cofre de chaves parcerias](media/certificate-authority-2.png)  

As seguintes descrições correspondem aos passos lettered verdes no diagrama anterior.

1. No diagrama acima, a aplicação está a criar um certificado que internamente começa por criar uma chave no seu Cofre de chaves.
2. Envia o Cofre de chaves e pedido de certificado de SSL à AC.
3. A aplicação consulta, num processo de ciclo e aguarde, para o seu Cofre de chaves para conclusão de certificado. A criação do certificado está concluída quando o Cofre de chaves recebe a resposta da AC com x509 certificado.
4. A AC responde ao pedido de certificado de SSL do Cofre de chaves com um X509 certificado SSL.
5. Concluir a criação do novo certificado com o fusão do X509 certificado da AC.

## <a name="asynchronous-process"></a>Processo assíncrono
A criação do certificado KV é um processo assíncrono. Esta operação irá criar um pedido de certificado KV e devolver um código de estado de http de 202 (aceite). O estado do pedido pode ser controlado através da consulta o objeto pendente criado por esta operação. É devolvido o URI completo do objeto pendente no cabeçalho de localização.  

Quando um pedido para criar um certificado de KV estiver concluída, o estado do objeto pendente será alterado para "concluída" de "em curso" e será criada uma nova versão do certificado KV. Isto irá tornar-se a versão atual.  

## <a name="first-creation"></a>Criação do primeiro
 Quando um certificado de KV é criado pela primeira vez, uma chave endereçável e o segredo também é criado com o mesmo nome que o certificado. Se o nome já está a ser utilizado, em seguida, a operação irá falhar com um código de estado de http de 409 (conflito).
A chave endereçável e o segredo obtêm os atributos de certificado KV respetivos atributos. A chave endereçável e o segredo criadas desta forma estão marcados como geridas chaves e segredos, cuja duração é gerida pelo Cofre de chaves. Gerido chaves e segredos são só de leitura. Nota: Se um certificado de KV expira ou está desativado, a chave correspondente e o segredo irão tornar-se inoperáveis.  

 Se esta for a primeira operação para criar um certificado de KV é necessária uma política.  Também pode ser fornecida uma política com sucessivas criar operações para substituir o recurso de política. Se uma política não for fornecida, o recurso de política do serviço é utilizado para criar uma versão seguinte do certificado KV. Tenha em atenção que enquanto um pedido de criação de uma versão seguinte está em curso, o certificado KV atual e chave endereçável correspondente e o segredo, permanecem inalterados.  

## <a name="self-issued-certificate"></a>Certificados emitidos
 Para criar um certificado emitido, defina o nome do emissor como "Automática" na política de certificado, conforme mostrado no seguinte fragmento de política de certificado.  

```  
"issuer": {  
       "name": "Self"  
    }  

```  

 Se o nome do emissor não for especificado, em seguida, o nome do emissor estiver definido como "Desconhecido". Quando utilizar o emissor como "Desconhecido", o proprietário do certificado tem de obter manualmente um x509 certificado do emissor à sua escolha, em seguida, certificado de intercalação de x509 público com o certificado do Cofre de chaves pendentes objeto para concluir a criação do certificado.

```  
"issuer": {  
       "name": "Unknown"  
    }  

```  

## <a name="partnered-ca-providers"></a>Fornecedores de AC parcerias
A criação do certificado pode ser emissor concluída manualmente ou utilizando um "automática". O Cofre de chaves também parceiros com determinadas fornecedores de emissor para simplificar a criação de certificados. Os seguintes tipos de certificados podem ser ordenados para o Cofre de chaves com estes fornecedores de emissor de parceiro.  

|Fornecedor|Tipo de certificado|  
|--------------|----------------------|  
|DigiCert|O Cofre de chaves oferece OV ou EV SSL certificados com DigiCert|
|GlobalCert|O Cofre de chaves oferece OV ou EV SSL certificados com GlobalSign|

 Para obter mais informações, incluindo disponibilidade geográfica destes fornecedores de emissor, consulte [emissores de certificados](/rest/api/keyvault/certificate-issuers.md).

Tenha em atenção que, quando é colocada uma ordem com o fornecedor de emissor, podem honrar ou extensões de certificado de substituição de x509 e período de validade do certificado com base no tipo de certificado.  

 Autorização: Necessita de permissão Criar/certificados.

 ## <a name="see-also"></a>Consultar Também
 - [Sobre chaves, os segredos e certificados](about-keys-secrets-and-certificates.md)
 - [Monitorizar e gerir a criação do certificado](create-certificate-scenarios.md)
