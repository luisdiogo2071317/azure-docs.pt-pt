---
title: "Ativar o SSL num cluster do Azure Machine Learning de computação (MLC) | Microsoft Docs"
description: "Obter as instruções para configurar o SSL para chamadas de classificação num cluster do Azure Machine Learning de computação (MLC)"
services: machine-learning
author: SerinaKaye
ms.author: serinak
manager: hjerez
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/24/2018
ms.openlocfilehash: b76fe7c0caa4a9aca76a9a3f50d1fced0ab67cba
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="enable-ssl-on-an-azure-machine-learning-compute-mlc-cluster"></a>Ativar o SSL num cluster do Azure Machine Learning de computação (MLC) 

Estas instruções permitem-lhe configurar o SSL para chamadas de classificação num cluster de computação de aprendizagem máquina (MLC). 

## <a name="prerequisites"></a>Pré-requisitos 

1. Decida sobre um CNAME (nome DNS) utilizar quando efetuar chamadas de classificação em tempo real.

2. Criar um *livre de palavra-passe* certificado com esse CNAME.

3. Se o certificado ainda não estiver no formato PEM, convertê-la PEM através de ferramentas, como *openssl*.

Terá dois ficheiros depois de concluir os pré-requisitos:

* Um ficheiro para o certificado, por exemplo, `cert.pem`
* Um ficheiro para a chave, por exemplo, `key.pem`



## <a name="set-up-an-ssl-certificate-on-a-new-acs-cluster"></a>Configurar um certificado SSL no novo cluster de ACS

Utilizar a CLI do Azure Machine Learning, execute o seguinte comando com o `-c` comutador para criar um cluster de ACS com um certificado SSL anexado:

```
az ml env create -c -g <resource group name> -n <cluster name> --cert-cname <CNAME> --cert-pem <path to cert.pem file> --key-pem <path to key.pem file>
```


## <a name="set-up-an-ssl-certificate-on-an-existing-acs-cluster"></a>Configurar um certificado SSL num cluster existente dos ACS

Se estiver a filtrar um cluster que foi criado sem SSL, pode adicionar um certificado utilizando cmdlets do Azure PowerShell: 

```
Set-AzureRmMlOpCluster -ResourceGroupName my-rg -Name my-cluster -SslStatus Enabled -SslCertificate $certValueInPemFormat -SslKey $keyValueInPemFormat -SslCName foo.mycompany.com
```

## <a name="map-the-cname-and-the-ip-address"></a>Mapear o CNAME e o endereço IP

Crie um mapeamento entre o CNAME que selecionou nos pré-requisitos e o endereço IP a (FE) front-end em tempo real. Para detetar o endereço IP do FE, execute o comando abaixo. A saída apresenta um campo chamado "publicIpAddress" que contém o endereço IP do cluster em tempo real front-end. Consulte as instruções do seu fornecedor DNS para configurar um registo CNAME.



## <a name="auto-detection-of-a-certificate"></a>Deteção automática de um certificado 

Quando cria um serviço web em tempo real, utilizando o "`az ml service create realtime`" de comandos, do Azure Machine Learning Deteta automaticamente o SSL configurado no cluster e configura automaticamente o URL de classificação com o certificado designado. 

Para ver o estado de certificado, execute o seguinte comando. Tenha em atenção o prefixo "https" da classificação de URI e o CNAME no nome do anfitrião. 

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
