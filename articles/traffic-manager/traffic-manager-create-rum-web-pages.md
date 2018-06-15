---
title: Medidas de utilizador reais para Traffic Manager do Azure com páginas web | Microsoft Docs
description: Configurar as suas páginas web para enviar medidas de utilizador reais para o Gestor de tráfego
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 71661d8640dbe14082292cb9637964adac9fa803
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30180627"
---
# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Como enviar medidas de utilizador reais para Traffic Manager do Azure utilizando as páginas web

Pode configurar as suas páginas web para enviar para o Gestor de tráfego de medidas de utilizador reais por obter uma chave de medidas de utilizador reais (RUM) e ao incorporar o código gerado para a página web.

## <a name="obtain-a-real-user-measurements-key"></a>Obter uma chave de medidas de utilizador reais

Os valores demorar e enviar para o Gestor de tráfego a partir da sua aplicação de cliente são identificados pelo serviço através de uma cadeia exclusiva, denominada o **chave medidas de utilizador reais (RUM)**. Pode obter uma chave RUM no portal do Azure, uma API REST, ou utilizando o PowerShell ou a CLI do Azure.

Para obter a chave de RUM através do portal do Azure:
1. Num browser, inicie sessão no portal do Azure. Se ainda não tiver uma conta, pode inscrever-se numa avaliação gratuita de um mês.
2. Na barra de pesquisa do portal, procure o nome de perfil do Gestor de tráfego que pretende modificar e, em seguida, clique no perfil de Gestor de tráfego nos resultados de que o apresentados.
3. No painel de perfil do Traffic Manager, clique em **medidas de utilizador reais** em **definições**.
4. Clique em **gerar chave** para criar uma nova chave de RUM.
 
  ![Gerar a chave de medidas de utilizador reais](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Figura 1: Geração de chaves de medidas de utilizador reais**

5. O painel apresenta agora gerar a chave de RUM e um fragmento de código JavaScript que tem de ser incorporado na sua página HTML.
 
    ![No código JavaScript para a chave de medidas de utilizador reais](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)

    **Figura 2: Utilizador Real medidas chave e medida JavaScript**
 
6.  Clique em de **cópia** botão para copiar o código JavaScript. 

>[!IMPORTANT]
> Utilize o JavaScript gerado para a funcionalidade de medidas de utilizador reais para funcionar corretamente. Quaisquer alterações a este script ou os scripts utilizados por valores reais de utilizador podem levar a um comportamento imprevisível.

## <a name="embed-the-code-to-an-html-web-page"></a>Incorporar o código para uma página web HTML

Após obter a chave RUM, o passo seguinte é incorporar esta JavaScript copiado para uma página HTML que os utilizadores finais visitar. Edição de HTML pode ser feito de várias maneiras e utilizando ferramentas diferentes e fluxos de trabalho. Este exemplo mostra como atualizar uma página HTML para adicionar este script. Pode utilizar esta orientação para adaptá-lo para o fluxo de trabalho de gestão de origem HTML.

1.  Abra a página HTML num editor de texto
2.  Cole o código JavaScript tinha copiou no passo anterior para a secção de corpo de HTML (o código de copiados é linha 8 & 9, consulte a figura 3).
 
    ![Incorporar o código Javascript na página web para Real medidas de utilizador](./media/traffic-manager-create-rum-web-pages/real-user-measurement-embed-script.png)  

    **Figura 3: HTML Simple com incorporados JavaScript de medidas de utilizador reais**

3.  Guarde o ficheiro HTML e o anfitrião-lo num webserver ligado à internet. 
4. Próxima vez que esta página é composta no web browser, o JavaScript referenciado é transferido e o script será executado a medição e elaboração de relatórios de operações.


## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [Real medidas de utilizador](traffic-manager-rum-overview.md)
- Saiba [como funciona o Gestor de tráfego](traffic-manager-overview.md)
- Saiba mais sobre o [métodos de encaminhamento de tráfego](traffic-manager-routing-methods.md) suportada pelo Gestor de tráfego
- Saiba como [criar um perfil de Gestor de tráfego](traffic-manager-create-profile.md)

