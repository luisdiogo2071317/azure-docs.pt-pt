---
title: Estrutura de modelo do Azure Resource Manager e sintaxe | Microsoft Docs
description: Descreve a estrutura e propriedades de modelos Azure Resource Manager utilizando a sintaxe declarativa de JSON.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2017
ms.author: tomfitz
ms.openlocfilehash: 89e4b52e7d306bd495c426bcf775f59d0f30eb55
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/14/2017
---
# <a name="resources-section-of-azure-resource-manager-templates"></a>Secção de recursos de modelos Azure Resource Manager

Na secção de recursos, é possível definir os recursos que são implementados ou atualizados. Nesta secção pode obter complicada porque tem de compreender os tipos que está a implementar para fornecer os valores corretos.

## <a name="available-properties"></a>Propriedades disponíveis

Definir recursos com a estrutura seguinte:

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Nome do elemento | Necessário | Descrição |
|:--- |:--- |:--- |
| condição | Não | Valor booleano que indica se o recurso é implementado. |
| apiVersion |Sim |Versão da API REST para utilizar para criar o recurso. |
| tipo |Sim |Tipo de recurso. Este valor é uma combinação do espaço de nomes do fornecedor de recursos e o tipo de recurso (tais como **Storage/storageaccounts**). |
| nome |Sim |Nome do recurso. O nome tem de seguir as restrições de componente URI definidas no RFC3986. Além disso, os serviços do Azure que expõem o nome do recurso para fora entidades validar o nome para se certificar de não é uma tentativa de spoof de identidade de outra. |
| localização |varia |Georreplicação-localizações suportadas do recurso fornecido. Pode selecionar qualquer uma das localizações disponíveis, mas normalmente faz sentido para escolher aquele que está próximo os seus utilizadores. Normalmente, também faz sentido colocar recursos que interagem entre si na mesma região. A maioria dos tipos de recursos necessitam de uma localização, mas alguns tipos (por exemplo, uma atribuição de função) não necessitam de uma localização. |
| etiquetas |Não |Etiquetas que estão associadas o recursos. Aplica etiquetas para organizar logicamente os recursos na sua subscrição. |
| comentários |Não |As notas para documentar recursos no seu modelo |
| copiar |Não |Se for necessário mais do que uma instância, o número de recursos para criar. O modo predefinido é paralelo. Especifique o modo de série quando não pretender que todos os ou os recursos a implementar em simultâneo. Para obter mais informações, consulte [criar várias instâncias de recursos no Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Não |Recursos a que tem de ser implementados antes de implementar este recurso. O Resource Manager avalia as dependências entre os recursos e implementa-los na ordem correta. Quando os recursos não são dependentes entre si, que são implementados em paralelo. O valor pode ser uma lista separada por vírgulas de um recurso nomes ou identificadores exclusivos de recursos. Apenas lista os recursos que são implementados neste modelo. Recursos que não estão definidos neste modelo tem de existir. Evite adicionar dependências desnecessárias à medida que estes possam lenta a implementação e criar dependências circulares. Para obter orientações sobre as dependências de definição, consulte [definir dependências nos modelos Azure Resource Manager](resource-group-define-dependencies.md). |
| propriedades |Não |Definições de configuração específicas do recurso. Os valores para as propriedades são os mesmos que os valores que fornecem no corpo do pedido para a operação de REST API (método PUT) criar o recurso. Também pode especificar uma matriz de cópia para criar várias instâncias de uma propriedade. |
| recursos |Não |Recursos subordinados que dependem do recurso que está a ser definido. Fornece apenas tipos de recursos que são permitidos pelo esquema do recurso principal. O tipo completamente qualificado do recurso subordinado inclui o tipo de recurso principal, tal como **Microsoft.Web/sites/extensions**. Dependência no recurso principal não está implícita. Tem de definir explicitamente que dependência. |

## <a name="resource-specific-values"></a>Valores de recurso específico

O **apiVersion**, **tipo**, e **propriedades** são diferentes para cada tipo de recurso. Para determinar os valores para estas propriedades, consulte [referência ao modelo](/azure/templates/).

## <a name="resource-names"></a>Nomes de recursos
Geralmente, trabalha com três tipos de nomes de recursos no Gestor de recursos:

* Nomes de recursos que têm de ser exclusivos.
* Os nomes de recursos que não têm de ser exclusivo, mas optar por fornecer um nome que pode ajudar a identificar o recurso.
* Nomes de recursos que podem ser genéricos.

### <a name="unique-resource-names"></a>Nomes exclusivos de recursos
Tem de fornecer um nome exclusivo do recurso para qualquer tipo de recurso que tenha um ponto final de acesso de dados. Alguns tipos de recursos comuns que necessitam de um nome exclusivo incluem:

* Armazenamento do Azure<sup>1</sup> 
* Funcionalidade de Aplicações Web no Serviço de Aplicações do Azure
* SQL Server
* Azure Key Vault
* Cache de Redis do Azure
* Azure Batch
* Traffic Manager do Azure
* Azure Search
* O Azure HDInsight

<sup>1</sup> os nomes das contas de armazenamento também tem de ser em minúsculas, 24 carateres ou menos e não têm qualquer hífenes.

Quando o nome da definição, pode manualmente criar um nome exclusivo ou utilizar o [uniqueString()](resource-group-template-functions-string.md#uniquestring) função para gerar um nome. Também poderá pretender adicionar um prefixo ou sufixo para o **uniqueString** resultado. Modificar o nome exclusivo pode ajudá-lo mais fácil identificar o tipo de recurso do nome. Por exemplo, pode gerar um nome exclusivo para uma conta de armazenamento utilizando a seguinte variável:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Nomes de recursos para identificação
Alguns tipos de recursos, que pode querer nome, mas os respetivos nomes tem de ser exclusivo. Estes tipos de recurso, pode fornecer um nome que identifique o contexto de recursos e o tipo de recurso.

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

### <a name="generic-resource-names"></a>Nomes de recursos genérico
Tipos de recurso que principalmente aceder através de um recurso diferente, pode utilizar um nome genérico que é "hard-coded" no modelo. Por exemplo, pode definir um nome genérico padrão das regras de firewall num SQL server:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="location"></a>Localização
Quando implementar um modelo, tem de fornecer uma localização para cada recurso. São suportados tipos de recursos diferentes em diferentes localizações. Para ver uma lista de localizações que estão disponíveis para a sua subscrição para um tipo de recurso específico, utilize o Azure PowerShell ou a CLI do Azure. 

O exemplo seguinte utiliza o PowerShell para obter as localizações para o `Microsoft.Web\sites` tipo de recurso:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

O exemplo seguinte utiliza o Azure CLI 2.0 para obter as localizações para o `Microsoft.Web\sites` tipo de recurso:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

Após determinar as localizações suportadas para os seus recursos, defina essa localização no seu modelo. A forma mais fácil para definir este valor é criar um grupo de recursos numa localização que suporta os tipos de recursos e como cada localização `[resourceGroup().location]`. Pode implementar novamente o modelo para grupos de recursos em localizações diferentes e não alterar quaisquer valores no modelo ou de parâmetros. 

O exemplo seguinte mostra uma conta de armazenamento que é implementada a mesma localização que o grupo de recursos:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Se precisar que codifique os a localização no seu modelo, forneça o nome de uma das regiões suportadas. O exemplo seguinte mostra uma conta de armazenamento sempre implementar Norte Central-na:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="tags"></a>Etiquetas
[!INCLUDE [resource-manager-tag-introduction](../../includes/resource-manager-tag-introduction.md)]

### <a name="add-tags-to-your-template"></a>Adicionar etiquetas ao seu modelo

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="child-resources"></a>Recursos subordinados

Dentro de alguns tipos de recursos, também pode definir uma matriz de recursos subordinados. Recursos subordinados são recursos que só existem no contexto de outro recurso. Por exemplo, uma base de dados do SQL Server não pode existir sem um SQL server para a base de dados é um elemento subordinado do servidor. Pode definir a base de dados a definição para o servidor.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

Quando aninhados, o tipo está definido como `databases` , mas o respetivo tipo de recurso completo é `Microsoft.Sql/servers/databases`. Não fornecem `Microsoft.Sql/servers/` porque é suposto o principal do tipo de recurso. O nome do recurso subordinado está definido como `exampledatabase` , mas o nome completo inclui o nome do principal. Não fornecem `exampleserver` porque é suposto do recurso principal.

O formato do tipo de recurso subordinado é:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`

O formato do nome de recurso subordinado é:`{parent-resource-name}/{child-resource-name}`

No entanto, não terá de definir a base de dados no servidor. Pode definir os recursos subordinados no nível superior. Poderá utilizar esta abordagem se o recurso principal não está implementado no mesmo modelo ou se pretende utilizar `copy` criar subordinados vários recursos. Com esta abordagem, tem de fornecer o tipo de recurso completo e incluir o nome do recurso principal no nome de recurso do subordinado.

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

Ao construir uma referência a um recurso completamente qualificada, a ordem combinar segmentos do tipo e o nome não é simplesmente uma concatenação dos dois.  Em vez disso, após o espaço de nomes, utilize uma sequência de *nome do tipo* pares de específicos, pelo menos, mais específico para:

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

Por exemplo:

`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`está correto `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` não está correto

## <a name="recommendations"></a>Recomendações
As seguintes informações podem ser úteis quando trabalhar com recursos:

* Para ajudar outros contribuintes compreender a finalidade do recurso, especifique **comentários** para cada recurso no modelo:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Se utilizar um *ponto final público* no seu modelo (por exemplo, um Blob do Azure armazenamento ponto final público), *efetue não codificar* o espaço de nomes. Utilize o **referência** função dinamicamente obter o espaço de nomes. Pode utilizar esta abordagem para implementar o modelo para ambientes de espaço de nomes pública diferente, sem alterar manualmente o ponto final no modelo. Defina a versão da API para a mesma versão que está a utilizar para a conta de armazenamento no seu modelo:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Se a conta de armazenamento é implementada no mesmo modelo que está a criar, não terá de especificar o espaço de nomes do fornecedor ao referenciam o recurso. O exemplo seguinte mostra a sintaxe simplificada:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Se tiver outros valores no modelo que estão configurados para utilizar um espaço de nomes público, alterar estes valores para refletir o mesmo **referência** função. Por exemplo, pode definir o **storageUri** propriedade do perfil de diagnóstico de máquina virtual:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Também pode referenciar uma conta de armazenamento existente que esteja no grupo de recursos diferente:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Atribua endereços IP públicos para uma máquina virtual, apenas quando uma aplicação exige. Para ligar a uma máquina virtual (VM) de depuração, ou para fins administrativos ou de gestão, utilize regras NAT de entrada, um gateway de rede virtual ou um jumpbox.
   
     Para obter mais informações sobre como ligar a máquinas virtuais, consulte:
   
   * [Executar as VMs para uma arquitetura de N camadas no Azure](../guidance/guidance-compute-n-tier-vm.md)
   * [Configurar o acesso de WinRM para VMs no Azure Resource Manager](../virtual-machines/windows/winrm.md)
   * [Permitir o acesso externo à VM através do portal do Azure](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Permitir o acesso externo à sua VM utilizando o PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Permitir o acesso externo para a VM com Linux utilizando a CLI do Azure](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* O **domainNameLabel** propriedade para endereços IP públicos têm de ser exclusiva. O **domainNameLabel** valor tem de ter entre 3 e 63 carateres de comprimento e siga as regras especificadas por esta expressão regular: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Porque o **uniqueString** função gera uma cadeia que é de 13 carateres de comprimento, a **dnsPrefixString** parâmetro está limitado a 50 carateres:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Quando adiciona uma palavra-passe para uma extensão de script personalizado, utilize o **commandToExecute** propriedade no **protectedSettings** propriedade:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > Para garantir que os segredos são encriptados quando estes são transmitidos como parâmetros para VMs e as extensões, utilize o **protectedSettings** propriedade das extensões relevantes.
   > 
   > 


## <a name="next-steps"></a>Passos seguintes
* Para ver modelos completos para vários tipos de soluções, veja os [Modelos de Início Rápido do Azure](https://azure.microsoft.com/documentation/templates/).
* Para obter detalhes sobre as funções que pode utilizar a partir de um modelo, consulte [funções de modelo do Azure Resource Manager](resource-group-template-functions.md).
* Combinar vários modelos durante a implementação, consulte [utilizar modelos ligados com o Azure Resource Manager](resource-group-linked-templates.md).
* Terá de utilizar os recursos que existe dentro de um grupo de recursos diferente. Este cenário é comum quando trabalhar com as contas de armazenamento e as redes virtuais que são partilhadas em vários grupos de recursos. Para obter mais informações, consulte o [resourceId função](resource-group-template-functions-resource.md#resourceid).
* Para obter informações sobre as restrições de nome de recurso, consulte [recomendado convenções de nomenclatura para recursos do Azure](../guidance/guidance-naming-conventions.md).