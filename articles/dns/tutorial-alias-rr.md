---
title: Tutorial – Criar um registo de alias do DNS do Azure para fazer referência a um registo de recursos na zona.
description: Este tutorial mostra como configurar um registo de alias do DNS do Azure para fazer referência a um registo de recursos na zona.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 09c1768602fede51d7ff2b23f48278a1d0b0cd2a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990846"
---
# <a name="tutorial-create-an-alias-record-to-refer-to-a-zone-resource-record"></a>Tutorial: Criar um registo de alias para fazer referência a um registo de recursos na zona

Os registos de alias podem fazer referência a outros conjuntos de registos do mesmo tipo. Por exemplo, pode ter um conjunto de registos CNAME do DNS que funciona como alias de outro conjunto de registos CNAME do mesmo tipo. Tal será útil se quiser ter alguns conjuntos de registos como aliases e alguns como não aliases em termos de comportamento.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um registo de alias para um registo de recursos na zona
> * Testar o registo de alias


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Deve ter um nome de domínio disponível que possa alojar no DNS do Azure para testar. Deve ter controlo total sobre este domínio, incluindo a capacidade de definir os registos do servidor de nomes (NS) do domínio.

Para obter instruções sobre como alojar o seu domínio no DNS do Azure, veja [Tutorial: Alojar o seu domínio no DNS do Azure](dns-delegate-domain-azure-dns.md).


## <a name="create-an-alias-record"></a>Criar um registo de alias

Crie um registo de alias que esteja associado a um registo de recursos na zona.

### <a name="create-the-target-resource-record"></a>Criar um registo de recursos de destino
1. Clique na sua zona DNS do Azure para abrir a zona.
2. Clique em **Conjunto de registos**.
3. Na caixa de texto **Nome**, escreva **servidor**.
4. Em **Tipo**, selecione **A**.
5. Na caixa de texto **ENDEREÇO IP**, escreva **10.10.10.10**.
6. Clique em **OK**.

### <a name="create-the-alias-record"></a>Criar o registo de alias
1. Clique na sua zona DNS do Azure para abrir a zona.
2. Clique em **Conjunto de registos**.
3. Na caixa de texto **Nome**, escreva **teste**.
4. Em **Tipo**, selecione **A**.
5. Clique em **Sim** na caixa de verificação **Conjunto de Registos de Alias** e selecione a opção **Conjunto de registos da zona**.
6. No **Conjunto de registos da zona**, selecione o registo **servidor**.
7. Clique em **OK**.

## <a name="test-the-alias-record"></a>Testar o registo de alias

1. Inicie a sua ferramenta nslookup favorita. Uma das opções consiste em navegar até [https://network-tools.com/nslook](https://network-tools.com/nslook).
2. Defina o tipo de consulta para os registos A e procure **teste.\<o nome de domínio\>**. Obterá **10.10.10.10** como resposta.
3. No portal do Azure, altere o registo A do **servidor** para **10.11.11.11**.
4. Aguarde alguns minutos e, em seguida, utilize novamente nslookup para **testar** o registo.
5. Obterá **10.11.11.11** como resposta.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode eliminar o **servidor** e **testar** os registos de recursos na sua zona.


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou um registo de alias para fazer referência a um registo de recursos na zona. Para saber mais sobre o DNS do Azure e as aplicações Web, avance para o tutorial de aplicações Web.

> [!div class="nextstepaction"]
> [Criar registos DNS para uma aplicação Web num domínio personalizado](./dns-web-sites-custom-domain.md)
