---
title: Configurar um nome de domínio personalizado para a sua conta de armazenamento do Azure | Documentos da Microsoft
description: Utilize o portal do Azure para mapear o seu próprio nome canónico (CNAME) para o ponto de extremidade de web ou de BLOBs numa conta de armazenamento do Azure.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: tamram
ms.component: blobs
ms.openlocfilehash: 5fd823e9105157f8292d5a9554850b0f4338a392
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398857"
---
# <a name="configure-a-custom-domain-name-for-your-azure-storage-account"></a>Configurar um nome de domínio personalizado para a sua conta de armazenamento do Azure

Pode configurar um domínio personalizado para aceder a dados de BLOBs na sua conta de armazenamento do Azure. O ponto final predefinido para o armazenamento de BLOBs é `<storage-account-name>.blob.core.windows.net`. Também pode utilizar o ponto final web gerado como parte da [funcionalidade de Web sites estáticos (pré-visualização)](storage-blob-static-website.md). Se mapear um domínio personalizado e o subdomínio, como **www.contoso.com** para o ponto final blob ou web para o armazenamento de conta, pode os utilizadores, em seguida, aceder a dados de BLOBs na sua conta de armazenamento a utilizar esse domínio.

> [!IMPORTANT]
> O armazenamento do Azure, mas nativamente suporta HTTPS com domínios personalizados. Pode atualmente [utilizar a CDN do Azure para aceder a blobs com domínios personalizados através de HTTPS](storage-https-custom-domain-cdn.md).
>

> [!NOTE]  
> Contas de armazenamento atualmente suportam apenas um nome de domínio personalizado por conta. Isso significa que não é possível mapear um nome de domínio personalizado para pontos finais de serviço da web e de Blobs.

A tabela seguinte mostra alguns URLs de exemplo de dados de blob localizados numa conta de armazenamento com o nome **mystorageaccount**. O domínio personalizado registado para a conta de armazenamento são **www.contoso.com**:

| Tipo de Recurso | URL predefinido | URL de domínio personalizado |
| --- | --- | --- | --- |
| Conta de armazenamento | http://mystorageaccount.blob.core.windows.net | http://www.contoso.com |
| Blobs |http://mystorageaccount.blob.core.windows.net/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Contentor de raiz | http://mystorageaccount.blob.core.windows.net/myblob ou http://mystorageaccount.blob.core.windows.net/$root/myblob| http://www.contoso.com/myblob ou http://www.contoso.com/$root/myblob |
| Web |  http://mystorageaccount. [zone].web.core.windows.net/$web/[indexdoc] ou http://mystorageaccount. [ Zone].Web.Core.Windows.NET/[indexdoc] ou http://mystorageaccount. [ Zone].Web.Core.Windows.NET/$Web ou http://mystorageaccount. [ Zone].Web.Core.Windows.NET/ | http://www.contoso.com/$web ou http://www.contoso.com/ ou http://www.contoso.com/$web / [indexdoc] ou http://www.contoso.com/[indexdoc] |

> [!NOTE]  
> Todos os exemplos para o ponto de final de serviço de BLOBs abaixo também se aplicam ao ponto de final de serviço da web.

## <a name="direct-vs-intermediary-domain-mapping"></a>Direto vs. o mapeamento de domínio intermediário

Existem duas formas para apontar o seu domínio personalizado para o ponto final do blob para a sua conta de armazenamento: direcionar o CNAME de mapeamento e a utilizar o *asverifiy* subdomínio intermediário.

### <a name="direct-cname-mapping"></a>Direcionar o mapeamento CNAME

A primeira e mais simples, o método é criar um registo de nome canónico (CNAME) que mapeia o seu domínio personalizado e o subdomínio diretamente para o ponto final do blob. Um registo CNAME é uma funcionalidade (DNS sistema) de nome de domínio que mapeia um domínio de origem para um domínio de destino. Neste caso, o domínio de origem é o seu domínio personalizado e o subdomínio, por exemplo *www.contoso.com*. O domínio de destino é o ponto de final do serviço de BLOBs, por exemplo *mystorageaccount.blob.core.windows.net*.

O método direto é abordado [Registre-se um domínio personalizado](#register-a-custom-domain).

### <a name="intermediary-mapping-with-asverify"></a>Mapeamento de intermediário com *asverifiy*

O segundo método também usa os registos CNAME, mas primeiro emprega um subdomínio especial reconhecido pelo Azure para evitar o tempo de inatividade: **asverifiy**.

O processo de mapeamento de seu domínio personalizado para um ponto final do blob pode resultar num breve período de tempo de inatividade para o domínio enquanto estiver a registá-lo na [portal do Azure](https://portal.azure.com). Se o seu domínio personalizado suporte no momento um aplicativo com um contrato de nível de serviço (SLA) que precisa de nenhum tempo de inatividade, então pode utilizar o Azure *asverifiy* subdomínio como passo intermédio de registo. Este passo intermédio garante que os utilizadores são capazes de aceder ao seu domínio, enquanto o mapeamento de DNS está em curso.

O método intermediário é abordado [Registre-se de um domínio personalizado, utilizando o *asverifiy* subdomínio](#register-a-custom-domain-using-the-asverify-subdomain).

## <a name="register-a-custom-domain"></a>Registe-se um domínio personalizado
Utilize este procedimento para registar o seu domínio personalizado se tiver sem preocupações sobre do domínio rapidamente indisponível para seus usuários, ou se o seu domínio personalizado não aloja atualmente um aplicativo. Pode utilizar o DNS do Azure para configurar um nome DNS personalizado para seu armazenamento de Blobs do Azure. Para obter mais informações, veja [Utilizar o DNS do Azure para oferecer definições de domínio personalizado para um serviço do Azure](https://docs.microsoft.com/azure/dns/dns-custom-domain#blob-storage).

Se o seu domínio personalizado suporte no momento um aplicativo que não pode ter qualquer período de inatividade, siga o procedimento descrito em [Registre-se de um domínio personalizado, utilizando o *asverifiy* subdomínio](#register-a-custom-domain-using-the-asverify-subdomain).

Para configurar um nome de domínio personalizado, tem de criar um novo registo CNAME no DNS. O registo CNAME Especifica um alias para um nome de domínio. Neste caso, ele mapeia o endereço do seu domínio personalizado para o ponto de final de armazenamento de BLOBs para a sua conta de armazenamento.

Normalmente, pode gerir as definições de DNS do seu domínio no site da entidade de domínio. Cada entidade de registo tem um método semelhante, mas ligeiramente diferente de especificar um registo CNAME, mas o conceito é o mesmo. Alguns pacotes de registo de domínio básico não oferecem a configuração de DNS, por isso terá de atualizar o seu pacote de registo do domínio antes de poder criar o registo CNAME.

1. Navegue até à sua conta de armazenamento na [portal do Azure](https://portal.azure.com).
1. Sob **serviço BLOB** no painel de menu, selecione **Custom domain** para abrir o *Custom domain* painel.
1. Inicie sessão no site da entidade de domínio e vá para a página para a gestão de DNS. Poderá encontrar isto numa secção como **Nome de Domínio**, **DNS** ou **Gestão de Nome de Servidor**.
1. Encontre a secção para gerir CNAMEs. Poderá ter de voltar a uma página de definições avançadas e procurar as palavras **CNAME**, **Alias**, ou **subdomínios**.
1. Criar um novo registo CNAME e fornecer um alias de subdomínio, tal como **www** ou **fotos**. Em seguida, forneça um nome de anfitrião, o que é o serviço ponto final Blob, no formato **mystorageaccount.blob.core.windows.net** (em que *mystorageaccount* é o nome da conta de armazenamento). O nome de anfitrião a utilizar é apresentado no item #1 do *Custom domain* painel no [portal do Azure](https://portal.azure.com).
1. Na caixa de texto a *Custom domain* painel no [portal do Azure](https://portal.azure.com), introduza o nome do seu domínio personalizado, incluindo o subdomínio. Por exemplo, se o seu domínio for **contoso.com** e é o seu alias de subdomínio **www**, introduza **www.contoso.com**. Se for o subdomínio **fotografias**, introduza **photos.contoso.com**. É o subdomínio *necessário*.
1. Selecione **salvar** sobre o *Custom domain* painel para registar o seu domínio personalizado. Se o registo for bem sucedido, verá uma notificação do portal que sua conta de armazenamento foi atualizada com êxito.

Assim que o seu novo registo CNAME ter sido propagada através de DNS, os utilizadores podem ver dados de Blobs com o seu domínio personalizado, desde que eles têm as permissões adequadas.

## <a name="register-a-custom-domain-using-the-asverify-subdomain"></a>Registe-se de um domínio personalizado, utilizando o *asverifiy* subdomínio
Utilize este procedimento para registar a sua personalizado domínio não se o seu domínio personalizado suporte no momento um aplicativo com um SLA que tal seja necessário haver nenhum tempo de inatividade. Ao criar um CNAME que aponte do `asverify.<subdomain>.<customdomain>` para `asverify.<storageaccount>.blob.core.windows.net`, pode registrar previamente seu domínio com o Azure. Em seguida, pode criar um segundo CNAME que aponte do `<subdomain>.<customdomain>` para `<storageaccount>.blob.core.windows.net`, altura em que o tráfego para o seu domínio personalizado será direcionado para o ponto final blob.

O **asverifiy** subdomínio é um subdomínio especial reconhecido pelo Azure. Ao início `asverify` a sua própria subdomínio, permitir que o Azure reconheça seu domínio personalizado sem modificar o registo DNS para o domínio. Ao modificar o registo DNS para o domínio, ele será mapeado para o ponto final do blob sem períodos de indisponibilidade.

1. Navegue até à sua conta de armazenamento na [portal do Azure](https://portal.azure.com).
1. Sob **serviço BLOB** no painel de menu, selecione **Custom domain** para abrir o *Custom domain* painel.
1. Inicie sessão no Web site do seu fornecedor de DNS e vá para a página para a gestão de DNS. Poderá encontrar isto numa secção como **Nome de Domínio**, **DNS** ou **Gestão de Nome de Servidor**.
1. Encontre a secção para gerir CNAMEs. Poderá ter de voltar a uma página de definições avançadas e procurar as palavras **CNAME**, **Alias**, ou **subdomínios**.
1. Criar um novo registo CNAME e fornecer um alias de subdomínio que inclui a *asverifiy* subdomínio. Por exemplo, **asverify.www** ou **asverify.photos**. Em seguida, forneça um nome de anfitrião, o que é o serviço ponto final Blob, no formato **asverify.mystorageaccount.blob.core.windows.net** (em que **mystorageaccount** é o nome da conta de armazenamento). O nome de anfitrião a utilizar é apresentado no item #2 do *Custom domain* painel no [portal do Azure](https://portal.azure.com).
1. Na caixa de texto a *Custom domain* painel no [portal do Azure](https://portal.azure.com), introduza o nome do seu domínio personalizado, incluindo o subdomínio. Não inclua *asverifiy*. Por exemplo, se o seu domínio for **contoso.com** e é o seu alias de subdomínio **www**, introduza **www.contoso.com**. Se for o subdomínio **fotografias**, introduza **photos.contoso.com**. O subdomínio é necessário.
1. Selecione o **utilizam a validação de CNAME indireta** caixa de verificação.
1. Selecione **salvar** sobre o *Custom domain* painel para registar o seu domínio personalizado. Se o registo for bem sucedido, verá uma notificação do portal que indica que a conta de armazenamento foi atualizada com êxito. Neste momento, o seu domínio personalizado foi verificado pelo Azure, mas o tráfego para o seu domínio ainda não é a ser encaminhado para a sua conta de armazenamento.
1. Voltar ao site do seu fornecedor de DNS e crie outro registo CNAME que mapeia o subdomínio para o ponto final de serviço de Blobs. Por exemplo, especifique o subdomínio como **www** ou **fotos** (sem o *asverifiy*) e o nome do anfitrião como **mystorageaccount.blob.core.windows.net**  (onde **mystorageaccount** é o nome da conta de armazenamento). Neste passo, o registo do seu domínio personalizado está concluído.
1. Por fim, pode eliminar o registo CNAME que criou, que contém o **asverifiy** subdomínio, pois era necessário apenas como uma etapa intermediária.

Assim que o seu novo registo CNAME ter sido propagada através de DNS, os utilizadores podem ver dados de Blobs com o seu domínio personalizado, desde que eles têm as permissões adequadas.

## <a name="test-your-custom-domain"></a>Testar o seu domínio personalizado

Para confirmar o que seu domínio personalizado, de fato, é mapeado para o ponto de final de serviço Blob, crie um blob num contentor público na sua conta de armazenamento. Em seguida, num navegador da web, utilize um URI no seguinte formato para aceder ao blob:

`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Por exemplo, poderá utilizar o URI seguinte para aceder a um formulário da web na **myforms** contentor no **photos.contoso.com** subdomínio personalizado:

`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Anular o registo um domínio personalizado

Para anular o registo um domínio personalizado para o ponto final de armazenamento de BLOBs, utilize um dos seguintes procedimentos.

### <a name="azure-portal"></a>Portal do Azure

Execute o seguinte no portal do Azure para remover a definição de domínio personalizado:

1. Navegue até à sua conta de armazenamento na [portal do Azure](https://portal.azure.com).
1. Sob **serviço BLOB** no painel de menu, selecione **Custom domain** para abrir o *Custom domain* painel.
1. Limpe o conteúdo da caixa de texto que contém o nome de domínio personalizado.
1. Selecione o botão **Guardar**.

Quando o domínio personalizado foi removido com êxito, verá uma notificação do portal que indica que a conta de armazenamento foi atualizada com êxito.

### <a name="azure-cli-20"></a>CLI 2.0 do Azure

Utilize o [atualização da conta de armazenamento az](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_update) CLI de comandos e especificar uma cadeia vazia (`""`) para o `--custom-domain` valor do argumento para remover um registo de domínio personalizado.

* Formato do comando:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Exemplo de comando:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

Utilize o [Set-AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) cmdlet do PowerShell e especificar uma cadeia vazia (`""`) para o `-CustomDomainName` valor do argumento para remover um registo de domínio personalizado.

* Formato do comando:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Exemplo de comando:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Passos Seguintes
* [Mapear um domínio personalizado para um ponto de extremidade de rede de entrega conteúdo (CDN)](../../cdn/cdn-map-content-to-custom-domain.md)
* [Utilizar a CDN do Azure para aceder a blobs com domínios personalizados através de HTTPS](storage-https-custom-domain-cdn.md)
* [Alojar o Web site estático no armazenamento de Blobs do Azure (pré-visualização)](storage-blob-static-website.md)
