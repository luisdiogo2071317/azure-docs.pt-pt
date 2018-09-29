---
title: Configurar o método de encaminhamento de tráfego de round robin ponderado utilizando o Gestor de tráfego do Azure | Documentos da Microsoft
description: Este artigo explica como balancear carga de tráfego através de um método de round robin no Gestor de tráfego
services: traffic-manager
documentationcenter: ''
author: kumudd
manager: timlt
editor: ''
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
ms.openlocfilehash: 6637132481ee33d43ec2b747ba89a56983205ff2
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/28/2018
ms.locfileid: "47432448"
---
# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Configurar o método de encaminhamento de tráfego de ponderado no Gestor de tráfego

É um padrão comum para o tráfego encaminhamento método fornecer um conjunto de pontos finais idênticos, que incluem os serviços cloud e Web sites, e enviar o tráfego para cada um de uma forma round robin. Os passos seguintes descrevem como configurar este tipo de método de encaminhamento de tráfego.

> [!NOTE]
> Aplicação Web do Azure já fornece a funcionalidade para Web sites dentro de uma região do Azure (contém vários centros de dados) de balanceamento de carga round robin. O Gestor de tráfego permite-lhe especificar o método de encaminhamento de tráfego round robin para sites em datacenters diferentes.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>Para configurar o método de encaminhamento de tráfego ponderada

1. Num browser, inicie sessão no [portal do Azure](http://portal.azure.com). Se ainda não tiver uma conta, pode inscrever-se para obter uma [avaliação gratuita durante um mês](https://azure.microsoft.com/free/). 
2. Na barra de pesquisa do portal, procure o **perfis do Gestor de tráfego** e, em seguida, clique no nome de perfil que pretende configurar o método de encaminhamento para.
3. Na **perfil do Traffic Manager** painel, certifique-se de que os serviços cloud e de Web sites que pretende incluir na sua configuração estão presentes.
4. Na **configurações** secção, clique em **configuração**e, no **configuração** painel, faça o seguinte:
    1. Para **definições do método de encaminhamento de tráfego**, certifique-se de que o método de encaminhamento de tráfego é **ponderado**. Se não estiver, clique em **ponderado** na lista pendente.
    2. Definir o **definições do monitor de ponto final** idêntica para todas as cada ponto final dentro deste perfil da seguinte forma:
        1. Selecione o adequado **protocolo**e especifique a **porta** número. 
        2. Para **caminho** escreva uma barra */*. Para monitorizar os pontos finais, tem de especificar um caminho e nome de ficheiro. A reencaminhar barra "/" é uma entrada válida para o caminho relativo e indica que o ficheiro está no diretório de raiz (predefinição).
        3. Na parte superior da página, clique em **guardar**.
5. Teste as alterações na configuração da seguinte forma:
    1.  Na barra de pesquisa do portal, procure o nome de perfil do Gestor de tráfego e clique no perfil do Gestor de tráfego nos resultados que apresentados.
    2.  Na **Gestor de tráfego** painel de perfil, clique em **descrição geral**.
    3.  O **perfil do Traffic Manager** painel apresenta o nome DNS do perfil do Traffic Manager criado recentemente. Isto pode ser utilizado por todos os clientes (por exemplo, ao navegar para ele usando um navegador da web) obter encaminhado para o ponto de extremidade direita, como determinado pelo tipo de encaminhamento. Neste caso todos os pedidos são encaminhados cada ponto de extremidade de uma forma round robin.
6. Assim que o perfil do Traffic Manager está a funcionar, edite o registo DNS no seu servidor DNS autoritativo para apontar o nome de domínio da empresa para o nome de domínio do Gestor de tráfego.

![Configurar o método de encaminhamento de tráfego ponderada utilizando o Gestor de tráfego][1]

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [método de encaminhamento de tráfego de prioridade](traffic-manager-configure-priority-routing-method.md).
- Saiba mais sobre [método de encaminhamento de tráfego de desempenho](traffic-manager-configure-performance-routing-method.md).
- Saiba mais sobre [método de encaminhamento geográfico](traffic-manager-configure-geographic-routing-method.md).
- Saiba como [testar as definições do Gestor de tráfego](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png
