---
title: Delegar um subdomínio de DNS do Azure
description: Aprenda a delegar um subdomínio de DNS do Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 1/22/2019
ms.author: victorh
ms.openlocfilehash: 87a80703c473245660a850645ca3fef21bbd80f6
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452722"
---
# <a name="delegate-an-azure-dns-subdomain"></a>Delegar um subdomínio de DNS do Azure

Pode utilizar o portal do Azure para delegar um subdomínio DNS. Por exemplo, se for o proprietário do domínio contoso.com, pode delegar um subdomínio chamado *engenharia* à zona de outro e separada que pode ser administrada em separado da zona contoso.com.

## <a name="prerequisites"></a>Pré-requisitos

Delegar um subdomínio de DNS do Azure, primeiro tem de delegar o seu domínio público ao DNS do Azure. Ver [delegar um domínio ao DNS do Azure](./dns-delegate-domain-azure-dns.md) para obter instruções sobre como configurar os servidores de nome para a delegação. Assim que o seu domínio é delegado à sua zona DNS do Azure, pode delegar o seu subdomínio.

Os exemplos neste artigo utilizam o domínio contoso.com. Deve substituir seu próprio domínio quando utilizar estes procedimentos.

## <a name="create-a-zone-for-your-subdomain"></a>Criar uma zona para o subdomínio

Em primeiro lugar, crie a zona para o **engenharia** subdomínio.

1. A partir do portal do Azure, selecione **criar um recurso**.
2. Na caixa de pesquisa, escreva **DNS**e selecione **zona DNS**.
3. Selecione **Criar**.
4. Na **criar zona DNS** painel, escreva **engineering.contoso.com** no **nome** caixa de texto.
5. Selecione o grupo de recursos para a sua zona. Pode querer utilizar o mesmo grupo de recursos como zona principal para manter os recursos semelhantes em conjunto.
6. Clique em **Criar**.
7. Após a implementação com êxito, vá para a nova zona.

## <a name="note-the-name-servers"></a>Tenha em atenção os servidores de nomes

Em seguida, copie os quatro servidores de nomes para o subdomínio.

1. Sobre o **engenharia** zona painel, tenha em atenção os quatro servidores de nomes para a zona. Irá utilizar estes servidores de nomes mais tarde.
2. Criar uma **A** registo para utilizar em testes. Por exemplo, criar um **www** A gravar e configurá-lo com um **10.10.10.10** endereço IP.

## <a name="create-an-ns-record"></a>Criar um registo NS

Em seguida, crie um registo de servidor (NS) de nome para o **engenharia** zona.

1. Navegue para a zona de domínio principal.
2. Selecione **+ Conjunto de registos**.
3. Sobre o **adicionar conjunto de registos** painel, escreva **engenharia** no **nome** caixa de texto.
4. Para **tipo**, selecione **NS**.
5. Sob **servidor de nomes**, introduza os quatro servidores de nomes que registou anteriormente dos **engenharia** zona.
6. Clique em **OK**.

## <a name="test-the-delegation"></a>Testar a delegação

Use o nslookup para testar a delegação.

1. Abra uma janela do PowerShell.
2. No prompt de comando, digite `nslookup www.engineering.<your domain name>.`
3. Deverá receber uma resposta não autoritativa, que mostra o endereço **10.10.10.10**.



## <a name="next-steps"></a>Passos Seguintes

Saiba como [configurar DNS inverso para os serviços alojados no Azure](dns-reverse-dns-for-azure-services.md).