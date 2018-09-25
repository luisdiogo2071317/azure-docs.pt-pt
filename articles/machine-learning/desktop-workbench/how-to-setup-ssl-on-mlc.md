---
title: Ativar o SSL num cluster do Azure Machine Learning de computação (MLC) | Documentos da Microsoft
description: Obter instruções sobre como configurar SSL para chamadas de classificação num cluster do Azure Machine Learning de computação (MLC)
services: machine-learning
author: SerinaKaye
ms.author: serinak
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/24/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 2a7733468ec082c8954f623f3ebe2cea1fbad561
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46976238"
---
# <a name="enable-ssl-on-an-azure-machine-learning-compute-mlc-cluster"></a>Ativar o SSL num cluster do Azure Machine Learning de computação (MLC) 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


Estas instruções permitem-lhe configurar SSL para chamadas de classificação um cluster de computação de aprendizagem máquina (MLC). 

## <a name="prerequisites"></a>Pré-requisitos 

1. Escolher um CNAME (nome DNS) utilizar ao efetuar chamadas de classificação em tempo real.

2. Criar uma *livres de palavra-passe* certificado com esse CNAME.

3. Se o certificado ainda não estiver no formato PEM, convertê-la PEM com ferramentas, como *openssl*.

Terá dois arquivos depois de concluir os pré-requisitos:

* Um ficheiro para o certificado, por exemplo, `cert.pem`. Certifique-se de que o ficheiro tem a cadeia de certificados completa.
* Um ficheiro para a chave, por exemplo, `key.pem`



## <a name="set-up-an-ssl-certificate-on-a-new-acs-cluster"></a>Configurar um certificado SSL num novo cluster de ACS

Com a CLI do Azure Machine Learning, execute o seguinte comando com o `-c` comutador para criar um cluster de ACS com um certificado SSL anexado:

```
az ml env create -c -g <resource group name> -n <cluster name> --cert-cname <CNAME> --cert-pem <path to cert.pem file> --key-pem <path to key.pem file>
```


## <a name="set-up-an-ssl-certificate-on-an-existing-acs-cluster"></a>Configurar um certificado SSL num cluster do ACS existente

Se estiver a filtrar um cluster que foi criado sem SSL, pode adicionar um certificado utilizando cmdlets do PowerShell do Azure.

Tem de fornecer a chave e o certificado em formato não processado do PEM. Estes podem ser lidos em variáveis do PowerShell:

```
$keyValueInPemFormat = [IO.File]::ReadAllText('<path to key.pem file>')
$certValueInPemFormat = [IO.File]::ReadAllText('<path to cert.pem file>')
```
Adicione o certificado para o cluster: 

```
Set-AzureRmMlOpCluster -ResourceGroupName my-rg -Name my-cluster -SslStatus Enabled -SslCertificate $certValueInPemFormat -SslKey $keyValueInPemFormat -SslCName foo.mycompany.com
```

## <a name="map-the-cname-and-the-ip-address"></a>Mapear o CNAME e o endereço IP

Crie um mapeamento entre o CNAME que selecionou nos pré-requisitos e o endereço IP do (FE) front-end em tempo real. Para detetar o endereço IP do FE, execute o comando abaixo. A saída apresenta um campo chamado "publicIpAddress", que contém o endereço IP do front-end em tempo real do cluster. Consulte as instruções do seu fornecedor DNS para configurar um registo a partir do FQDN utilizado no CNAME para o endereço IP público.



## <a name="auto-detection-of-a-certificate"></a>Deteção automática de um certificado 

Quando cria um serviço web em tempo real através do "`az ml service create realtime`" de comando, Azure Machine Learning Deteta automaticamente o SSL configurado no cluster e configura automaticamente o URL de classificação com o certificado designado. 

Para ver o estado do certificado, execute o seguinte comando. Tenha em atenção o prefixo "https" da pontuação URI e o CNAME no nome do anfitrião. 

``` 
az ml service show realtime -n <service name>
{
                "id" : "<your service id>",
                "name" : "<your service name >",
                "state" : "Succeeded",
                "createdAt" : "<datetime>”,
                "updatedAt" : "< datetime>",
                "scoringUri" : "https://<your CNAME>/api/v1/service/<service name>/score"
}
```
