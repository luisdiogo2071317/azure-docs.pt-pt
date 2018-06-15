---
title: Monitorizar e gerir a criação do certificado
description: Cenários que demonstra uma gama de opções para a criação, monitorização e interação com a criação do certificado processam com o Cofre de chaves.
services: key-vault
documentationcenter: ''
author: lleonard-msft
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 0d0995aa-b60d-4811-be12-ba0a45390197
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: alleonar
ms.openlocfilehash: e1ea77304fa59b67e0e28a4c7e0b13633eeeff6f
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012152"
---
# <a name="monitor-and-manage-certificate-creation"></a>Monitorizar e gerir a criação do certificado
Aplica-se a: Azure  

O seguinte 

Os cenários de operações descritas neste artigo são:

- Pedir um certificado de KV com um emissor suportado
- Obter pedido pendente - estado do pedido é "em curso"
- Obter pedido pendente - estado do pedido é "completo"
- Obter pedido - pedido pendente "Cancelado" ou "Falha" de estado pendente
- Obter pedido - pedido pendente de estado é "eliminado" ou "substituído" pendente
- Criar (ou importar) quando pendentes pedido existe - estado é "em curso"
- Intercalação pedido pendente é criada com um emissor (DigiCert, por exemplo)
- Pedir um cancelamento enquanto o estado do pedido pendente é "em curso"
- Eliminar um objeto do pedido pendente
- Criar um certificado de KV manualmente
- Intercalar quando é criado um pedido pendente - a criação manual de certificados

## <a name="request-a-kv-certificate-with-a-supported-issuer"></a>Pedir um certificado de KV com um emissor suportado 

|Método|URI de pedido|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|  

Os exemplos seguintes necessitam de um objeto com o nome "mydigicert" para já estar disponíveis no seu Cofre de chaves com o fornecedor de emissor como DigiCert. Para obter mais informações sobre como trabalhar com emissores, consulte [emissores de certificados](/rest/api/keyvault/certificate-issuers.md).  

### <a name="request"></a>Pedir  

```json
{  
  "policy": {  
    "x509_props": {  
      "subject": "CN=MyCertSubject1"  
    },  
  "issuer": {  
       "name": "mydigicert",  
    "cty": "OV-SSL",  
    }  
  }  
}  

```  

### <a name="response"></a>Resposta  

```  
StatusCode: 202, ReasonPhrase: 'Accepted'  
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "mydigicert"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "InProgress",  
  "status_details": "Pending certificate created. Certificate request is in progress. This may take some time based on the issuer provider. Please check again later",  
  "request_id": "a76827a18b63421c917da80f28e9913d"  
}  

```  

## <a name="get-pending-request---request-status-is-inprogress"></a>Obter pedido pendente - estado do pedido é "em curso"

|Método|URI de pedido|  
|------------|-----------------|  
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>Pedir  
 INTRODUÇÃO `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OU  

 INTRODUÇÃO `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

> [!NOTE]
>  Se *request_id* é especificada na consulta, que funciona como um filtro. Se o *request_id* na consulta e no objeto pendente são diferentes, é devolvido um código de estado de http de 404.  

### <a name="response"></a>Resposta  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "inProgress",  
  "status_details": "…",  
  "request_id": "a76827a18b63421c917da80f28e9913d"  
}  

```  

## <a name="get-pending-request---request-status-is-complete"></a>Obter pedido pendente - estado do pedido é "completo"

### <a name="request"></a>Pedir  

|Método|URI de pedido|  
|------------|-----------------|  
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

 INTRODUÇÃO `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OU  

 INTRODUÇÃO `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

### <a name="response"></a>Resposta  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "completed",  
  "request_id": "a76827a18b63421c917da80f28e9913d",  
   "target": “https://mykeyvault.vault.azure.net/certificates/mycert1?api-version={api-version}"  
}  

```  

## <a name="get-pending-request---pending-request-status-is-canceled-or-failed"></a>Obter pedido - pedido pendente "Cancelado" ou "Falha" de estado pendente  

### <a name="request"></a>Pedir  

|Método|URI de pedido|  
|------------|-----------------|  
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

 INTRODUÇÃO `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OU  

 INTRODUÇÃO  `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

### <a name="response"></a>Resposta  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "failed",  
  "status_details": "",  
  "request_id": "a76827a18b63421c917da80f28e9913d",  
   "error":  
    {  
        "code": "<errorcode>",    
        "message": "<message>"  
    }  
}  

```  

> [!NOTE]
> O valor da *errorcode* pode ser "Error de emissor do certificado" ou "Pedido rejeitado" com base no erro de emissor ou utilizador, respetivamente.  

## <a name="get-pending-request---pending-request-status-is-deleted-or-overwritten"></a>Obter pedido - pedido pendente de estado é "eliminado" ou "substituído" pendente  
 Um objeto pendente pode ser eliminado ou substituído por uma operação de criação/importação quando não o respetivo estado é "em curso."

|Método|URI de pedido|  
|------------|-----------------|  
|GET|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>Pedir  
 INTRODUÇÃO `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OU  

 INTRODUÇÃO `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

### <a name="response"></a>Resposta  

```  
StatusCode: 404, ReasonPhrase: 'Not Found'  
{  
  "error":  
    {  
         "code": "PendingCertificateNotFound",  
        "message": "…"  
    }  
}  

```  

## <a name="create-or-import-when-pending-request-exists---status-is-inprogress"></a>Criar (ou importar) quando pendentes pedido existe - estado é "em curso"
 Um objeto pendente tem quatro Estados possíveis; "em curso", "Cancelado", "Falha" ou "concluída".

 Quando o estado de um pedido pendente é "em curso", criar (e importar) operações irão falhar com um código de estado de http de 409 (conflito).  

 Para resolver um conflito de:  

 - Se o certificado está a ser criado manualmente, pode concluir o certificado de KV efetuando uma intercalação ou eliminar o objeto pendente.  

 - Se o certificado está a ser criado com um emissor, pode aguardar até que o certificado é concluído, falha ou foi cancelado. Em alternativa, pode eliminar o objeto pendente.

> [!NOTE]
> Eliminar um objeto pendente pode ou não pode cancelar o x509 pedido de certificado com o fornecedor.  

|Método|URI de pedido|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|  

### <a name="request"></a>Pedir  

```json
{  
  "policy": {  
    "x509_props": {  
      "subject": "CN=MyCertSubject1"  
    },  
  "issuer": {  
       "name": "mydigicert"  
    }  
  }  
}  

```  

### <a name="response"></a>Resposta  

```  
StatusCode: 409, ReasonPhrase: 'Conflict'  
{  
  "error":  
    {  
        "code": "Forbidden",  
        "message": "A new key vault certificate can not be created or imported while a pending key vault certificate's status is inProgress."  
    }  
}  

```  

## <a name="merge-when-pending-request-is-created-with-an-issuer"></a>É criada com um emissor a intercalação pedido pendente
 Intercalação não é permitida quando um objeto pendente é criado com um emissor, mas é permitido quando o estado é "em curso." 

 Se o pedido de criação de x509 certificado falha ou cancela por algum motivo, e se um certificado pode ser obtido através de meios de fora de banda de x509, uma operação de intercalação pode ser feita para concluir o certificado de KV.  

|Método|URI de pedido|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|  

### <a name="request"></a>Pedir  

```json
{  
  "x5c": [  "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8="  
  ]  
}  

```  

### <a name="response"></a>Resposta  

```json
StatusCode: 403, ReasonPhrase: 'Forbidden'  
{  
  "error":  
    {  
       "code": "Forbidden",  
       "message": "Merge is forbidden on pending object created with issuer : <issuer-name> while it is in progess."  
    }  
}  

```  

## <a name="request-a-cancellation-while-the-pending-request-status-is-inprogress"></a>Pedir um cancelamento enquanto o estado do pedido pendente é "em curso"  
 Só pode ser pedido um cancelamento.  Um pedido pode ou não pode ser cancelado. Se um pedido não é "em curso", é devolvido um Estado de http de 400 (pedido incorreto).  

|Método|URI de pedido|  
|------------|-----------------|  
|PATCH|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>Pedir  
 PATCH `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OU  

 PATCH `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

```json
{  
  "cancellation_requested": true  
}  

```  

### <a name="response"></a>Resposta  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": true,  
  "status": "inProgress",  
  "status_details": "…",  
  "request_id": "a76827a18b63421c917da80f28e9913d"  
}  

```  

## <a name="delete-a-pending-request-object"></a>Eliminar um objeto do pedido pendente  

> [!NOTE]
> Ao eliminar o objeto pendente pode ou não pode cancelar o x509 pedido de certificado com o fornecedor.  

|Método|URI de pedido|  
|------------|-----------------|  
|DELETE|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}`|  

### <a name="request"></a>Pedir  
 ELIMINAR `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"`  

 OU  

 ELIMINAR `“https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}"`  

### <a name="response"></a>Resposta  

```  
StatusCode: 200, ReasonPhrase: 'OK'  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "{issuer-name}"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "cancellation_requested": false,  
  "status": "inProgress",  
  "request_id": "a76827a18b63421c917da80f28e9913d",  
}  
```  

## <a name="create-a-kv-certificate-manually"></a>Criar um certificado de KV manualmente  
 Pode criar um certificado emitido com uma AC à sua escolha através de um processo de criação manual. Definir o nome do emissor como "Desconhecido" ou especifique o campo do emissor.  

|Método|URI de pedido|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/create?api-version={api-version}`|  

### <a name="request"></a>Pedir  

```json
{  
  "policy": {  
    "x509_props": {  
      "subject": "CN=MyCertSubject1"  
    }  
  "issuer": {  
       "name": "Unknown"  
    }  
  }  
}  

```  

### <a name="response"></a>Resposta  

```  
StatusCode: 202, ReasonPhrase: 'Accepted'  
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1/pending?api-version={api-version}&request_id=a76827a18b63421c917da80f28e9913d"  
{  
  "id": “https://mykeyvault.vault.azure.net/certificates/mycert1/pending",  
  "issuer": {  
    "name": "Unknown"  
  },  
  "csr": "MIICq......DD5Lp5cqXg==",  
  "status": "inProgress",  
  "status_details": "Pending certificate created. Please Perform Merge to complete the request.",  
  "request_id": "a76827a18b63421c917da80f28e9913d"  
}  

```  

## <a name="merge-when-a-pending-request-is-created---manual-certificate-creation"></a>Intercalar quando é criado um pedido pendente - a criação manual de certificados  

|Método|URI de pedido|  
|------------|-----------------|  
|POST|`https://mykeyvault.vault.azure.net/certificates/mycert1/pending/merge?api-version={api-version}`|  

### <a name="request"></a>Pedir  

```json
{  
  "x5c": [  "MIICxTCCAbi………………………trimmed for brevitiy……………………………………………EPAQj8="  
  ]  
}  

```  

|Nome do elemento|Necessário|Tipo|Versão|Descrição|  
|------------------|--------------|----------|-------------|-----------------|  
|x5c|Sim|array|\<Introdução ao versão >|Cadeia de certificados X509 como base matriz de cadeia 64.|  

### <a name="response"></a>Resposta  

```  
StatusCode: 201, ReasonPhrase: 'Created'  
Location: “https://mykeyvault.vault.azure.net/certificates/mycert1?api-version={api-version}"  
{  
"id": "https mykeyvault.vault.azure.net/certificates/mycert1/f366e1a9dd774288ad84a45a5f620352",  
    "kid": "https:// mykeyvault.vault.azure.net/keys/mycert1/f366e1a9dd774288ad84a45a5f620352",  
    "sid": " mykeyvault.vault.azure.net/secrets/mycert1/f366e1a9dd774288ad84a45a5f620352",  
    "cer": "……de34534……",  
    "x5t": "n14q2wbvyXr71Pcb58NivuiwJKk",  
    "attributes": {  
        "enabled": true,  
        "exp": 1530394215,  
        "nbf": 1435699215,  
        "created": 1435699919,  
        "updated": 1435699919  
    },  
    "pending": {  
        "id": "https:// mykeyvault.vault.azure.net/certificates/mycert1/pending"  
    },  
    "policy": {  
        "id": "https:// mykeyvault.vault.azure.net/certificates/mycert1/policy",  
        "key_props": {  
            "exportable": false,  
            "kty": "RSA",  
            "key_size": 2048,  
            "reuse_key": false  
        },  
        "secret_props": {  
            "contentType": "application/x-pkcs12"  
        },  
        "x509_props": {  
            "subject": "CN=Mycert1",  
            "ekus": ["1.3.6.1.5.5.7.3.1", "1.3.6.1.5.5.7.3.2"],  
                       "validity_months":12  
        },  
        "lifetime_actions": [{  
            "trigger": {  
                "lifetime_percentage": 80  
            },  
            "action": {  
                "action_type": "EmailContacts"  
            }  
        }],  
        "issuer": {  
            "name": "Unknown"  
        },  
        "attributes": {  
            "enabled": true,  
            "created": 1435699811,  
            "updated": 1435699811  
        }  
    }  
}  

```

## <a name="see-also"></a>Consultar Também
- [Sobre chaves, segredos e certificados](about-keys-secrets-and-certificates.md)
