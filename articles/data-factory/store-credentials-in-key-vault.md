---
title: Armazenar credenciais no Cofre de chaves do Azure | Microsoft Docs
description: Saiba como armazenar as credenciais para os arquivos de dados utilizados um cofre de chaves do Azure do Azure Data Factory pode obter automaticamente no tempo de execução.
services: data-factory
author: linda33wj
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/25/2017
ms.author: jingwang
ms.openlocfilehash: e1be16ec6a7536cedf3a27ffacb9c4dffe42bbef
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052420"
---
# <a name="store-credential-in-azure-key-vault"></a>Armazenar credenciais no Cofre de chaves do Azure

Pode armazenar credenciais para os arquivos de dados e computações num [Cofre de chaves do Azure](../key-vault/key-vault-whatis.md). O Azure Data Factory obtém as credenciais quando executar uma atividade que utiliza o armazenamento de dados/computação.

Atualmente, todos os tipos de atividade, exceto atividade personalizada suportam esta funcionalidade. Para configuração do conector especificamente, consulte a secção "Propriedades do serviço ligado" [cada tópico conector](copy-activity-overview.md#supported-data-stores-and-formats) para obter mais detalhes.

## <a name="prerequisites"></a>Pré-requisitos

Esta funcionalidade baseia-se na identidade de serviço de fábrica de dados. Saiba como funciona do [identidade de serviço do Data factory](data-factory-service-identity.md) e certifique-se a fábrica de dados associado um.

>[!TIP]
>No Cofre de chaves do Azure, quando criar um segredo **put o valor inteiro de uma propriedade secreta ADF serviço ligado pede-lhe (por exemplo, ligação palavra-passe/cadeia/serviço principal chave/etc.)**. Por exemplo, para o Storage do Azure serviço ligado, coloque `DefaultEndpointsProtocol=http;AccountName=myAccount;AccountKey=myKey;` como segredo AKV, em seguida, referência no campo "connectionString" do ADF; para o serviço ligado do Dynamics, colocar `myPassword` como segredo AKV, em seguida, fazer referência a no campo "palavras-passe" do ADF. Consulte a cada artigo conector/computação nos detalhes de propriedade suportados.

## <a name="steps"></a>Passos

Para fazer referência a uma credencial armazenada no Cofre de chaves do Azure, tem de:

1. **Obter a identidade de serviço do data factory** ao copiar o valor de "Serviço de identidade ID da aplicação" gerado juntamente com a fábrica. Se utilizar a criação da IU do ADF, o ID do serviço de identidade será apresentado na janela de criação do serviço ligado do Cofre de chaves do Azure; Também pode obtê-lo no portal do Azure, consulte [obter a identidade de serviço do data factory](data-factory-service-identity.md#retrieve-service-identity).
2. **Conceda o acesso ao serviço de identidade para o seu Cofre de chaves do Azure.** No seu Cofre de chaves -> acesso políticas -> Adicionar novo -> Esta aplicação de identidade de serviço para conceder o ID de pesquisa **obter** permissão na lista pendente de permissões secreta. Permite que esta fábrica designada aceder ao segredo no Cofre de chaves.
3. **Crie um serviço ligado aponta para o seu Cofre de chaves do Azure.** Consulte [serviço ligado do Cofre de chaves do Azure](#azure-key-vault-linked-service).
4. **Crie o serviço ligado do arquivo de dados, no interior de referência de que a chave armazenada no secreta correspondente do cofre.** Consulte [segredo de referência armazenado no Cofre de chaves](#reference-secret-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Serviço ligado de Cofre de chaves do Azure

As seguintes propriedades são suportadas para o serviço Cofre de chaves do Azure ligada:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo tem de ser definida: **AzureKeyVault**. | Sim |
| baseUrl | Especifique o URL do Cofre de chaves do Azure. | Sim |

**Utilizando a IU de criação:**

Clique em **ligações** -> **serviços ligados** -> **+ novo** -> procure "Cofre de chaves do Azure":

![Pesquisa AKV](media/store-credentials-in-key-vault/search-akv.png)

Selecione o Cofre de chaves do Azure aprovisionados onde as suas credenciais são armazenadas. Pode fazê-lo **Testar ligação** para se certificar de que o AKV ligação é válida. 

![Configurar AKV](media/store-credentials-in-key-vault/configure-akv.png)

**Exemplo JSON:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-secret-stored-in-key-vault"></a>Segredo de referência armazenado no Cofre de chaves

As seguintes propriedades são suportadas quando configura um campo no serviço ligado que referencia um segredo do Cofre de chaves:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| tipo | A propriedade de tipo do campo tem de ser definida: **AzureKeyVaultSecret**. | Sim |
| secretName | O nome do segredo no Cofre de chaves do azure. | Sim |
| secretVersion | A versão do segredo no Cofre de chaves do azure.<br/>Se não for especificado, utiliza sempre a versão mais recente do segredo.<br/>Se for especificado, em seguida,-sticks para a versão especificada.| Não |
| arquivo | Refere-se a um serviço Cofre de chaves do Azure ligado que pode utilizar para guardar a credencial. | Sim |

**Utilizando a IU de criação:**

Selecione **Cofre de chaves do Azure** para campos secretos ao criar a ligação ao seu arquivo de dados/computação. Selecione o aprovisionamento chave de cofre serviço ligado do Azure e forneça o **nome secreto**. Opcionalmente, pode fornecer uma versão secreta bem. 

![Configurar o segredo AKV](media/store-credentials-in-key-vault/configure-akv-secret.png)

**Exemplo JSON: (consulte a secção "password")**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Passos Seguintes
Para obter uma lista dos arquivos de dados suportados como origens e sinks pela atividade de cópia no Azure Data Factory, consulte [arquivos de dados suportados](copy-activity-overview.md#supported-data-stores-and-formats).
