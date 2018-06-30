---
title: Começar a utilizar certificados de Cofre de chaves
description: Os cenários seguintes realçam vários das utilizações de principais de serviço de gestão de certificados do Cofre de chaves, incluindo os passos adicionais necessários para criar o primeiro certificado no seu Cofre de chaves.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a788b958-3acb-4bb6-9c94-4776852aeea1
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: f1a1a2fa083dd1bf02132e08981d736a17a2c58f
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109489"
---
# <a name="get-started-with-key-vault-certificates"></a>Começar a utilizar certificados de Cofre de chaves
Os cenários seguintes realçam vários das utilizações de principais de serviço de gestão de certificados do Cofre de chaves, incluindo os passos adicionais necessários para criar o primeiro certificado no seu Cofre de chaves.

Está delineado do seguinte:
- Criar o primeiro certificado do Cofre de chaves
- Criar um certificado com uma autoridade de certificação que é parcerias com o Cofre de chaves
- Criar um certificado com uma autoridade de certificação que não é parcerias com o Cofre de chaves
- Importar um certificado

## <a name="certificates-are-complex-objects"></a>Os certificados são os objetos complexos
Certificados são compostas por três recursos interrelated ligados em conjunto como um certificado do Cofre de chaves; metadados do certificado, uma chave e um segredo.


![Os certificados são complexos](media/azure-key-vault.png)


## <a name="creating-your-first-key-vault-certificate"></a>Criar o primeiro certificado do Cofre de chaves  
 Antes de pode criar um certificado num cofre de chave (KV), os passos dos pré-requisitos 1 e 2 devem ser efetuados com êxito e tem de existir um cofre de chaves para este utilizador / organização.  

**Passo 1** -fornecedores de autoridade (AC) de certificado  
-   Dependency como o administrador de TI, administração PKI ou qualquer pessoa gerir contas com as ACs, para uma empresa especificada (ex. A Contoso) é um pré-requisito para utilizar certificados de Cofre de chaves.  
    As seguintes ACs são os fornecedores partnered atuais com o Cofre de chaves:  
    -   DigiCert - Cofre de chaves oferece OV SSL certificados com DigiCert.  
    -   GlobalSign - Cofre de chaves oferece OV SSL certificados com GlobalSign  
    -   WoSign - ofertas do Cofre de chaves OV SSL ou certificados SSL de EV com WoSign com base na definição configurada pelo cliente na respetiva conta WoSign no portal do WoSign.  

**Passo 2** -administrador de conta para um fornecedor de AC cria as credenciais a serem utilizadas pelo Cofre de chaves para se inscrever, renove e utilizar certificados SSL através do Cofre de chaves.

**Passo 3** -admin A Contoso, juntamente com um funcionário de Contoso (utilizador do Cofre de chaves) que é proprietário de certificados, dependendo da AC, pode obter um certificado do administrador ou diretamente a partir da conta com a AC.  

-   Iniciar uma operação de credencial de adição para um cofre de chaves ao criar um [emissor do certificado](https://docs.microsoft.com/rest/api/keyvault/certificate-issuers) recursos. 
    -   Ex. MyDigiCertIssuer  
        -   Fornecedor  
        -   Credenciais – as credenciais da conta de AC. Cada AC tem os seus próprios dados específicos.  

     Para obter mais informações sobre a criação de contas com fornecedores de AC, consulte a publicação relacionada no [blogue do Cofre de chaves](http://aka.ms/kvcertsblog).  

**Passo 3.1** - configure [contactos de certificado](https://docs.microsoft.com/rest/api/keyvault/certificate-contacts) para notificações. Este é o contacto para o utilizador do Cofre de chaves. O Cofre de chaves não impõe este passo.  

Nota - Este processo, através do passo 3.1, é uma operação onetime.  

## <a name="creating-a-certificate-with-a-ca-partnered-with-key-vault"></a>Criar um certificado com uma AC parceria com o Cofre de chaves

![Criar um certificado com uma autoridade de certificação do Cofre de chaves parcerias](media/certificate-authority-2.png)

**Passo 4** -as seguintes descrições correspondem aos passos numerados verdes no diagrama anterior.  
  (1) - no diagrama acima, a aplicação está a criar um certificado que internamente começa por criar uma chave no seu Cofre de chaves.  
  (2) - Cofre de chaves envia um pedido de certificado de SSL para a AC.  
  (3) - a aplicação consulta, num processo de ciclo e aguarde, para o seu Cofre de chaves para conclusão de certificado. A criação do certificado está concluída quando o Cofre de chaves recebe a resposta da AC com x509 certificado.  
  (4) - AC responde ao pedido de certificado de SSL do Cofre de chaves com um X509 certificado SSL.  
  (5) - conclui a criação do novo certificado com o fusão do X509 certificado da AC.  

  Utilizador do Cofre de chaves – cria um certificado ao especificar uma política

  -   Repita conforme necessário  
  -   Restrições de políticas  
      -   X509 propriedades  
      -   Propriedades de chave  
      -   Referência de fornecedor - > ex. MyDigiCertIssure  
      -   As informações de renovação - > ex. 90 dias antes da expiração  

  - Um processo de criação do certificado é normalmente um processo assíncrono e envolve o seu Cofre de chaves para o estado da operação de certificado a criação de consulta.  
[Obter a operação de certificado](https://docs.microsoft.com/en-us/rest/api/keyvault/getcertificateoperation)  
      -   Estado: ou foi concluída, falhou com informações de erro, canceladas  
      -   Devido a atraso para criar, pode ser iniciada uma operação de cancelamento. A cancelar pode ou não estar em vigor.  

## <a name="import-a-certificate"></a>Importar um certificado  
 Em alternativa – um certificado pode ser importado para o Cofre de chaves – PFX ou PEM.  

 Para obter mais informações sobre o formato PEM, consulte a secção de certificados de [sobre certificados, chaves e segredos](about-keys-secrets-and-certificates.md).  

 Importar certificado – requer um PEM ou. PFX no disco e ter uma chave privada. 
-   Tem de especificar: cofre nome e o nome do certificado (política é opcional)

-   PEM / ficheiros PFX contém atributos que KV pode analisar e utilizar para preencher a política de certificado. Se uma política de certificado já for especificada, KV irá tentar corresponder dados PFX / ficheiro PEM.  

-   Assim que a importação estiver final, operações subsequentes utilizarão a nova política (novas versões).  

-   Se não existirem, não existem operações adicionais, é a primeira coisa que o Cofre de chaves a enviar um aviso de expiração. 

-   Além disso, o utilizador pode editar a política, que está funcional no momento da importação mas, contém as predefinições quando não existem informações foi especificadas na importação. Ex. Não existem informações do emissor  

## <a name="creating-a-certificate-with-a-ca-not-partnered-with-key-vault"></a>Criar um certificado com uma AC não parceria com o Cofre de chaves  
 Este método permite trabalhar com outras ACs ao parcerias fornecedores Cofre de chaves, que significa que a sua organização pode trabalhar com uma AC da sua preferência.  

![Criar um certificado com a sua própria autoridade de certificação](media/certificate-authority-1.png)  

 As seguintes descrições de passo correspondem aos passos lettered verdes no diagrama anterior.  

  (1) - no diagrama acima, a aplicação está a criar um certificado, que começa internamente através da criação de uma chave no seu Cofre de chaves.  

  (2) - Cofre de chaves devolve à sua aplicação, um certificado de assinatura do pedido (CSR).  

  (3) - a aplicação passa a CSR para a AC de escolhida.  

  (4) - a escolhida AC responde com uma X509 certificado.  

  (5) - a aplicação concluir a criação de certificado novo com uma fusão do X509 certificado da AC.

## <a name="see-also"></a>Consultar Também
- [Operações de certificado](/rest/api/keyvault/certificate-operations)
- [Sobre chaves, segredos e certificados](about-keys-secrets-and-certificates.md)
