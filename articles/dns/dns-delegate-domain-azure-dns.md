---
title: Tutorial - Alojar o seu domínio e subdomínio no DNS do Azure
description: Este tutorial mostra-lhe como configurar o DNS do Azure para alojar as suas zonas DNS.
services: dns
author: vhorne
manager: jeconnoc
ms.service: dns
ms.topic: tutorial
ms.date: 6/13/2018
ms.author: victorh
ms.openlocfilehash: ea0dc257d691326bc073b4cbff37e847a6990f02
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452306"
---
# <a name="tutorial-host-your-domain-in-azure-dns"></a>Tutorial: Alojar o seu domínio no DNS do Azure

Pode utilizar o DNS do Azure para alojar o seu domínio DNS e gerir os registos DNS. Ao alojar os seus domínios no Azure, pode gerir os recursos DNS com as mesmas credenciais, APIs, ferramentas e faturação dos seus outros serviços do Azure. 

Suponha que compra o domínio contoso.net a partir de uma entidade de registo de domínios e cria uma zona com o nome contoso.net no DNS do Azure. Na qualidade de proprietário do domínio, a sua entidade de registo de domínios oferece-lhe a opção de configurar os registos do servidor de nomes (NS) para o seu domínio. A entidade de registo de domínios armazena os registos NS na zona principal .net. Os utilizadores da Internet em todo o mundo são então direcionados para o seu domínio na sua zona DNS do Azure quando tentam resolver os registos DNS em contoso.net.


Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma zona DNS
> * Obter uma lista dos servidores de nomes
> * Delegar o domínio
> * Certifique-se de que a delegação está a funcionar


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-a-dns-zone"></a>Criar uma zona DNS

1. Inicie sessão no Portal do Azure.
1. No canto superior esquerdo, selecione **Criar um recurso** > **Rede** > **Zona DNS**, para abrir a página **Criar zona DNS**.

   ![Zona DNS](./media/dns-delegate-domain-azure-dns/openzone650.png)

1. Na página **Criar zona DNS**, introduza os valores seguintes e, em seguida, selecione **Criar**:

   | **Definição** | **Valor** | **Detalhes** |
   |---|---|---|
   |**Nome**|[o seu nome de domínio] |O nome de domínio que comprou. Este tutorial utiliza contoso.net como exemplo.|
   |**Subscrição**|[A sua subscrição]|Selecione uma subscrição na que vai criar a zona.|
   |**Grupo de recursos**|**Criar novo:** contosoRG|Crie um grupo de recursos. O nome do grupo de recursos tem de ser exclusivo dentro da subscrição que selecionou. |
   |**Localização**|EUA Leste||

> [!NOTE]
> A localização do grupo de recursos não tem qualquer impacto na zona DNS. A localização da zona DNS é sempre “global” e não está apresentada.

## <a name="retrieve-name-servers"></a>Obter servidores de nomes

Antes de pode delegar a zona DNS ao DNS do Azure, terá de conhecer os servidores de nomes da sua zona. O DNS do Azure aloca servidores de nomes a partir de um conjunto sempre que é criada uma zona.

1. Com a zona DNS criada, no painel **Favoritos** do portal do Azure, selecione **Todos os recursos**. Na página **Todos os recursos**, selecione a zona de DNS. Se a subscrição que selecionou já tiver vários recursos, pode escrever o seu nome de domínio na caixa **Filtrar por nome** para aceder facilmente ao gateway da aplicação. 

1. Obtenha os servidores de nome na página da zona DNS. Neste exemplo, foram atribuídos à zona contoso.net os servidores de nomes *ns1-01.azure-dns.com*, *ns2-01.azure-dns.net*, *ns3-01.azure-dns.org* e *ns4-01.azure-dns.info*:

   ![Lista dos servidores de nomes](./media/dns-delegate-domain-azure-dns/viewzonens500.png)

O DNS do Azure cria automaticamente registos NS autoritativos na sua zona para os servidores de nomes atribuídos.


## <a name="delegate-the-domain"></a>Delegar o domínio

Agora que a zona DNS está criada e que já tem os servidores de nomes, tem de atualizar o domínio principal com os servidores de nomes DNS do Azure. Cada entidade de registo tem as suas próprias ferramentas de gestão de DNS para alterar os registos do servidor de nomes de um domínio. Na página de gestão do DNS da entidade de registo, edite os registos NS e substitua-os pelos servidores de nomes do DNS do Azure.

Quando estiver a delegar um domínio ao DNS do Azure, tem de utilizar os servidores de nomes fornecidos por esse DNS do Azure. Recomendamos que utilize os quatro servidores de nomes, independentemente do seu domínio. A delegação de domínio não necessita que um servidor de nomes utilize o mesmo domínio de nível superior que o seu domínio.

> [!NOTE]
> Quando copiar cada endereço do servidor de nomes, certifique-se de que copia o ponto à direita no fim do endereço. O ponto à direita indica o fim de um nome de domínio completamente qualificado. Algumas entidades de registo poderão anexar o período, se o nome de NS não o tiver no final. No entanto, para ser conforme à a RFC de DNS, deve incluir o ponto à direita, pois não pode pressupor que todas as entidades de registo o façam por si.

As delegações que utilizam servidores de nomes de nome na sua própria zona, por vezes denominados *servidores de nomes personalizados*, não são atualmente suportadas no DNS do Azure.

## <a name="verify-that-the-delegation-is-working"></a>Certifique-se de que a delegação está a funcionar

Depois de concluir a delegação, pode confirmar que está a funcionar ao utilizar uma ferramenta como a *nslookup* para consultar o registo Início da Autoridade (SOA) da sua zona. O registo SOA é criado automaticamente quando a zona é criada. Poderá ter de aguardar 10 minutos ou mais após concluir a delegação antes de poder verificar com êxito que está a funcionar. Pode demorar algum tempo para as alterações serem propagadas em todo o sistema DNS.

Não é necessário especificar os servidores de nomes DNS do Azure. Se a delegação estiver configurada corretamente, o processo de resolução DNS normal localiza os servidores de nomes automaticamente.

A partir de uma linha de comandos, escreva um comando nslookup semelhante ao seguinte:

```
nslookup -type=SOA contoso.net
```

Aqui está uma resposta de exemplo do comando anterior:

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.net
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="clean-up-resources"></a>Limpar recursos

Pode manter o grupo de recursos **contosoRG** se pretende acompanhar o tutorial seguinte. Caso contrário, elimine o grupo de recursos **contosoRG** para eliminar os recursos criados neste tutorial. Para tal, clique no grupo de recursos **contosoRG** e clique em **Eliminar grupo de recursos**. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma zona DNS para o seu domínio e delegou-o ao DNS do Azure. Para saber mais sobre o DNS do Azure e as aplicações Web, avance para o tutorial de aplicações Web.

> [!div class="nextstepaction"]
> [Criar registos DNS para uma aplicação Web num domínio personalizado](./dns-web-sites-custom-domain.md)
