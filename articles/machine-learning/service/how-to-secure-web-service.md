---
title: Proteger serviços da web com SSL
titleSuffix: Azure Machine Learning service
description: Saiba como proteger um serviço web implementado com o serviço Azure Machine Learning. Pode restringir o acesso a serviços da web e proteger os dados submetidos por clientes utilizando as camadas de soquete seguro (SSL) e a autenticação baseada em chave.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 02/05/2019
ms.custom: seodec18
ms.openlocfilehash: 160bc0e67b2686d17357241887a207cb4a03002c
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56098107"
---
# <a name="use-ssl-to-secure-web-services-with-azure-machine-learning-service"></a>Utilizar o SSL para proteger serviços da web com o serviço Azure Machine Learning

Neste artigo, ficará a saber como proteger um serviço web implementado com o serviço Azure Machine Learning. Pode restringir o acesso a serviços da web e proteger os dados submetidos por clientes utilizando as camadas de soquete seguro (SSL) e a autenticação baseada em chave.

> [!Warning]
> Se não ativar SSL, qualquer utilizador na internet será capaz de fazer chamadas para o serviço web.

O SSL criptografa dados enviados entre o cliente e o serviço web. Ele é também utilizado pelo cliente para verificar a identidade do servidor. Autenticação apenas está ativada para os serviços que forneceram um certificado SSL e a chave.  Se ativar o SSL, é necessária uma chave de autenticação quando aceder ao serviço web.

Se implementar um serviço web habilitado com SSL ou ativar SSL para o serviço web implementado existente, os passos são os mesmos:

1. Obtenha um nome de domínio.

2. Obtenha um certificado SSL.

3. Implementar ou atualizar o serviço web com a definição de SSL ativada.

4. Atualize o DNS para apontar para o serviço web.

Existem pequenas diferenças quando protegendo serviços da web entre as [destinos de implementação](how-to-deploy-and-where.md).

## <a name="get-a-domain-name"></a>Obter um nome de domínio

Se já não possui um nome de domínio, pode comprar um de uma __entidade de registo de nome de domínio__. O processo é diferente entre entidades de registo, como faz o custo. A entidade de registo também fornece ferramentas para gerenciar o nome de domínio. Estas ferramentas são utilizadas para mapear um nome de domínio completamente qualificado (como www.contoso.com) para o endereço IP que aloja o serviço da web.

## <a name="get-an-ssl-certificate"></a>Obter um certificado SSL

Existem várias formas de obter um certificado SSL. A mais comum é comprar um de uma __autoridade de certificação__ (AC). Independentemente de onde obter o certificado, terá dos seguintes ficheiros:

* R __certificado__. O certificado tem de conter a cadeia de certificados completa e tem de ser PEM codificado.
* R __chave__. A chave tem de ser PEM codificado.

Quando pedir um certificado, tem de fornecer o nome de domínio completamente qualificado (FQDN) do endereço que pretende utilizar para o serviço web. Por exemplo, www.contoso.com. O endereço marcados para o certificado e o endereço utilizado pelos clientes são comparados ao validar a identidade do serviço web. Se os endereços não corresponderem, os clientes irão receber um erro.

> [!TIP]
> Se a autoridade de certificação não é possível fornecer o certificado e chave, como ficheiros PEM codificado, pode utilizar um utilitário como [OpenSSL](https://www.openssl.org/) para alterar o formato.

> [!WARNING]
> Certificados autoassinados devem ser usados apenas para desenvolvimento. Eles não devem ser usados na produção. Certificados autoassinados podem causar problemas no seu cliente de aplicativos. Para obter mais informações, consulte a documentação para as bibliotecas de rede utilizadas na sua aplicação de cliente.

## <a name="enable-ssl-and-deploy"></a>Ativar o SSL e implementar

Para implementar (ou voltar a implementar) o serviço com SSL ativado, defina o `ssl_enabled` parâmetro `True`, sempre que aplicável. Definir o `ssl_certificate` parâmetro para o valor da __certificado__ ficheiro e o `ssl_key` para o valor da __chave__ ficheiro.

+ **Implementar no serviço Kubernetes do Azure (AKS)**

  Durante o aprovisionamento do cluster do AKS, fornece valores para parâmetros relacionados com SSL, conforme mostrado no fragmento de código:

    ```python
    from azureml.core.compute import AksCompute

    provisioning_config = AksCompute.provisioning_configuration(ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Implementar no Azure Container Instances (ACI)**

  Durante a implementação no ACI, fornece valores para parâmetros relacionados com SSL, conforme mostrado no fragmento de código:

    ```python
    from azureml.core.webservice import AciWebservice

    aci_config = AciWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

+ **Implementar nas matrizes de porta programável por campo (FPGA)**

  Ao implantar FPGA, forneça valores para os parâmetros relacionados com SSL, conforme mostrado no fragmento de código:

    ```python
    from azureml.contrib.brainwave import BrainwaveWebservice

    deployment_config = BrainwaveWebservice.deploy_configuration(ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem")
    ```

## <a name="update-your-dns"></a>Atualizar o seu DNS

Em seguida, tem de atualizar o DNS para apontar para o serviço web.

+ **Para o ACI**:

  Utilize as ferramentas fornecidas pelo sua entidade de registo de nome de domínio para atualizar o registo DNS para o seu nome de domínio. O registo tem de apontar para o endereço IP do serviço.

  Consoante a entidade de registo e a hora para live (TTL) configurada para o nome de domínio, pode demorar vários minutos a várias horas para que os clientes podem resolver o nome de domínio.

+ **Para o AKS**:

  Atualize o DNS no separador "Configuração" das "Endereço IP público" do cluster AKS conforme mostrado na imagem. Pode encontrar o endereço IP público como um dos tipos de recursos criados no grupo de recursos que contém os nós de agente do AKS e outros recursos de rede.

  ![Azure Machine Learning service: Protegendo serviços da web com SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Passos Seguintes
Aprenda a:
+ [Consumir um modelo implementado como um serviço web machine learning](how-to-consume-web-service.md)
+ [Executar com segurança inferência dentro de uma rede Virtual do Azure e de experimentações](how-to-enable-virtual-network.md)
