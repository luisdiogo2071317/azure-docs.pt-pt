---
title: Comprar e configurar um certificado SSL para o serviço de aplicações do Azure | Documentos da Microsoft
description: Saiba como comprar um certificado de serviço de aplicações e vinculá-lo à sua aplicação de serviço de aplicações
services: app-service
documentationcenter: .net
author: cephalin
manager: cfowler
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: apurvajo;cephalin
ms.openlocfilehash: 38f7f82d293409a49c41381cedaa1f7600068cd3
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319409"
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Comprar e Configurar um Certificado SSL para o Serviço de Aplicações do Azure

Este tutorial mostra-lhe como proteger a sua aplicação web ao adquirir um certificado SSL para o seu  **[serviço de aplicações do Azure](http://go.microsoft.com/fwlink/?LinkId=529714)**, de forma segura armazená-los no [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis), e associar a um domínio personalizado.

## <a name="step-1---sign-in-to-azure"></a>Passo 1 - início de sessão no Azure

Inicie sessão no portal do Azure em http://portal.azure.com

## <a name="step-2---place-an-ssl-certificate-order"></a>Passo 2 – um pedido de certificado SSL

Pode colocar um pedido de certificado SSL ao criar um novo [certificado do serviço de aplicações](https://portal.azure.com/#create/Microsoft.SSL) no **portal do Azure**.

![Criação do certificado](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Introduza um amigável **Name** para o SSL de certificado e introduza o **nome de domínio**

> [!NOTE]
> Este passo é uma das partes mais importantes do processo de compra. Certifique-se de introduzir o nome de anfitrião correto (domínio personalizado) que pretende proteger com este certificado. **Isso não é possível** preceder o nome de anfitrião com WWW. 
>

Selecione seu **subscrição**, **grupo de recursos**, e **SKU de certificado**

> [!TIP]
> Certificados de serviço de aplicações pode ser usado para qualquer do Azure ou serviços do Azure e não está limitados a serviços de aplicações. Para tal, terá de criar uma cópia PFX local de um certificado de serviço de aplicações que pode usá-lo em qualquer lugar que desejar. Para obter mais informações, leia [criar uma cópia PFX local de um certificado de serviço de aplicações](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/).
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a>Passo 3 - Store o certificado no Azure Key Vault

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) é um serviço do Azure que ajuda a salvaguardar chaves criptográficas e segredos utilizados pelas aplicações e serviços cloud.
>

Assim que a compra do certificado SSL estiver concluída, tem de abrir o [certificados do serviço de aplicações](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) página.

![Inserir a imagem de pronto para armazenar em KV](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

O estado do certificado é **"Emissão pendente"** porque há mais algumas etapas que precisa para concluir antes de começar a utilizar este certificado.

Clique em **configuração do certificado** dentro as propriedades do certificado de página e clique em **passo 1: Store** para armazenar este certificado no Azure Key Vault.

Do **estado do Key Vault** página, clique em **repositório do Key Vault** para escolher um cofre de chaves existente para armazenar este certificado **ou criar novo Key Vault** criar novo Key Vault dentro do mesmo grupo de recursos e subscrição.

> [!NOTE]
> O Azure Key Vault tem custos mínimo para armazenar este certificado.
> Para obter mais informações, consulte  **[detalhes de preços do Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Assim que tiver selecionado o repositório de Cofre de chaves para armazenar este certificado, o **Store** opção deve mostrar o sucesso.

![Inserir a imagem de sucesso de arquivo no KV](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a>Passo 4 - Verifique se a propriedade do domínio

Da mesma **configuração do certificado** página que utilizou no passo 3, clique em **passo 2: verificar**.

Escolha o método de verificação de domínio preferencial. 

Existem três tipos de verificação de domínio suportados pelo certificados do serviço de aplicações: serviço de aplicações, domínio e verificação Manual. Esses tipos de verificação são explicados em mais detalhes no [Advanced secção](#advanced).

> [!NOTE]
> **Verificação do serviço de aplicações** é a opção mais conveniente, quando o domínio que pretende verificar já está mapeado para uma aplicação de serviço de aplicações na mesma subscrição. Tira partido do fato de que a aplicação de serviço de aplicações já verificou a propriedade do domínio.
>

Clique em **Verifique se** botão para concluir este passo.

![Inserir a imagem de verificação de domínio](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

Depois de clicar em **Verifique se**, utilize o **atualizar** botão até o **Verifique se** opção deve mostrar o sucesso.

![imagem de inserção de verificar o êxito na KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a>Passo 5 - certificado de atribuir a aplicação de serviço de aplicações

> [!NOTE]
> Antes de executar os passos nesta secção, tem de ter associado um nome de domínio personalizado com a sua aplicação. Para obter mais informações, consulte  **[configurar um nome de domínio personalizado para uma aplicação web.](app-service-web-tutorial-custom-domain.md)**
>

Na  **[portal do Azure](https://portal.azure.com/)**, clique nas **serviço de aplicações** opção à esquerda da página.

Clique no nome da aplicação a que pretende atribuir este certificado.

Na **configurações**, clique em **definições de SSL**.

Clique em **certificado do serviço de aplicações de importação** e selecione o certificado que acaba de comprar.

![Inserir a imagem de importar certificado](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

Na **enlaces ssl** secção clique em **adicionar enlaces**e utilize as listas pendentes para selecionar o nome de domínio para proteger com SSL e o certificado a utilizar. Também pode selecionar se pretende utilizar **[indicação de nome de servidor (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** ou SSL baseado em IP.

![Inserir a imagem dos enlaces SSL](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Clique em **Adicionar enlace** para guardar as alterações e ativar o SSL.

> [!NOTE]
> Se tiver selecionado **SSL baseado em IP** e o seu domínio personalizado está configurado com um registo, terá de efetuar os seguintes passos adicionais. Estes são explicados em mais detalhes no [Advanced secção](#Advanced).

Neste ponto, deve ser capaz de visitar a sua aplicação através de `HTTPS://` em vez de `HTTP://` para verificar se o certificado foi configurado corretamente.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Passo 6 – tarefas de gestão

### <a name="azure-cli"></a>CLI do Azure

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a>Avançado

### <a name="verifying-domain-ownership"></a>Verificar a propriedade do domínio

Há dois outros tipos de verificação de domínio suportados pelo certificados do serviço de aplicações: verificação do domínio e a verificação manual.

#### <a name="domain-verification"></a>Verificação do Domínio

Escolha esta opção apenas para [um domínio do serviço de aplicações que comprou do Azure.](custom-dns-web-site-buydomains-web-app.md). O Azure automaticamente adiciona o registo TXT de verificação para e conclui o processo.

#### <a name="manual-verification"></a>Verificação Manual

> [!IMPORTANT]
> Verificação de página da Web HTML (apenas funciona com o padrão de certificado de SKU)
>

1. Crie um ficheiro HTML com o nome **"starfield.html"**

1. O conteúdo deste ficheiro deve ser o nome exato do Token de verificação do domínio. (Pode copiar o token da página de estado da verificação de domínio)

1. Carregar este ficheiro na raiz do servidor web que aloja o seu domínio `/.well-known/pki-validation/starfield.html`

1. Clique em **atualizar** para atualizar o estado de certificado após a verificação foi concluída. Poderá demorar alguns minutos a conclusão da verificação.

> [!TIP]
> Certifique-se num terminal através de `curl -G http://<domain>/.well-known/pki-validation/starfield.html` a resposta deve conter o `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>Verificação de registo TXT de DNS

1. Com o seu Gestor de DNS, crie um registo TXT no `@` subdomínio com valor igual ao Token de verificação do domínio.
1. Clique em **"Atualizar"** para atualizar o estado de certificado após a verificação foi concluída.

> [!TIP]
> Tem de criar um registo TXT num `@.<domain>` com o valor `<verification-token>`.

### <a name="assign-certificate-to-app-service-app"></a>Atribuir o certificado para a aplicação de serviço de aplicações

Se tiver selecionado **SSL baseado em IP** e o seu domínio personalizado está configurado com um registo, terá de efetuar os seguintes passos adicionais:

Depois de ter configurado o enlace SSL baseado em um IP, é atribuído um endereço IP dedicado à sua aplicação. Pode encontrar este endereço IP no **Custom domain** página nas definições da sua aplicação, logo acima do **os nomes de anfitrião** secção. Ele está listado como **endereço IP externo**

![Inserir a imagem de IP SSL](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Este endereço IP é diferente do que o endereço IP virtual utilizado anteriormente para configurar o registo para o seu domínio. Se estiver configurado para utilizar SSL baseado em SNI, ou não estão configurados para utilizar SSL, nenhum endereço está listado para esta entrada.

Usando as ferramentas fornecidas pelo sua entidade de registo de nome de domínio, modificar o registo para o nome de domínio personalizado apontar para o endereço IP do passo anterior.

## <a name="rekey-and-sync-the-certificate"></a>Recodificar e sincronizar o certificado

Se alguma vez precisar de recodificar o certificado, selecione o **recodificação e sincronização** opção a partir do **propriedades do certificado** página.

Clique em **recodificar** botão para iniciar o processo. Este processo pode demorar 1 a 10 minutos a concluir.

![Inserir a imagem de recodificação SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Recodificação do certificado agrega o certificado com um novo certificado emitido a partir da autoridade de certificação.

## <a name="renew-the-certificate"></a>Renovar o certificado

Para ativar a renovação automática do certificado a qualquer altura, clique em **definições de renovação automática** na página de gestão do certificado. Selecione **nos** e clique em **guardar**. Certificados podem iniciar automaticamente a renovar 90 dias antes da expiração, se tiver ativada a renovação automática.

![](./media/app-service-web-purchase-ssl-web-site/auto-renew.png)

Para renovar manualmente o certificado em vez disso, clique em **renovação Manual** em vez disso. Pode pedir para renovar manualmente o certificado 60 dias antes da expiração.

> [!NOTE]
> O certificado renovado não está vinculado automaticamente à sua aplicação, quer renovados manualmente ou ele renovado automaticamente. Associá-lo à sua aplicação, consulte [renovar certificados](./app-service-web-tutorial-custom-ssl.md#renew-certificates). 

## <a name="more-resources"></a>Mais recursos

* [Impor HTTPS](app-service-web-tutorial-custom-ssl.md#enforce-https)
* [Impor TLS 1.1/1.2](app-service-web-tutorial-custom-ssl.md#enforce-tls-versions)
* [Utilizar um certificado SSL no código da aplicação no serviço de aplicações do Azure](app-service-web-ssl-cert-load.md)
* [FAQ: Certificados de serviço de aplicações](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/24/faq-app-service-certificates/)
