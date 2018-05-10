---
title: Restaurar uma aplicação no Azure
description: Saiba como restaurar a aplicação a partir de um instantâneo.
services: app-service
documentationcenter: ''
author: ahmedelnably
manager: cfowler
editor: ''
ms.assetid: 4164f9b5-f735-41c6-a2bb-71f15cdda417
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: multiple
ms.topic: article
ms.date: 04/04/2018
ms.author: aelnably;nicking
ms.openlocfilehash: e1ae8fcc30323c865aa96937f43054515f293394
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="restore-an-app-in-azure-from-a-snapshot"></a>Restaurar uma aplicação no Azure a partir de um instantâneo
Este artigo mostra como restaurar uma aplicação no [App Service do Azure](../app-service/app-service-web-overview.md) partir de um instantâneo. Pode restaurar a sua aplicação para um estado anterior, com base dos instantâneos da sua aplicação. Não é necessário ativar a cópia de segurança de instantâneos, a plataforma automaticamente guarda um instantâneo de todas as aplicações para fins de recuperação de dados.

Os instantâneos são cópias sombra incremental e oferecem várias vantagens relativamente ao regular [cópias de segurança](web-sites-backup.md):
- Não existem erros de cópia de ficheiros devido a bloqueios de ficheiros.
- Sem limitação de tamanho de armazenamento.
- Nenhuma configuração necessária.

O restauro de instantâneos está disponível para aplicações em execução **Premium** camada ou superior. Para obter informações sobre como aumentar verticalmente a sua aplicação, consulte [aumentar verticalmente a uma aplicação no Azure](web-sites-scale.md).

## <a name="limitations"></a>Limitações

- A funcionalidade está atualmente em pré-visualização.
- Só pode restaurar para a mesma aplicação ou para uma ranhura pertencentes a essa aplicação.
- Serviço de aplicações interrompe a aplicação de destino ou a ranhura de destino durante o restauro.
- Serviço de aplicações mantém três meses vale de instantâneos para fins de recuperação de dados de plataforma.
- Só pode restaurar instantâneos nos últimos 30 dias.
 

## <a name="restore-an-app-from-a-snapshot"></a>Restaurar uma aplicação a partir de um instantâneo

1. No **definições** página da sua aplicação no [portal do Azure](https://portal.azure.com), clique em **cópias de segurança** para apresentar o **cópias de segurança** página. Em seguida, clique em **restaurar** sob o **Snapshot(Preview)** secção.
   
    ![](./media/app-service-web-restore-snapshots/1.png)

2. No **restaurar** página, selecione o instantâneo para restaurar.
   
    ![](./media/app-service-web-restore-snapshots/2.png)
   
3. Especifique o destino para o restauro da aplicação no **o destino de restauro**.
   
    ![](./media/app-service-web-restore-snapshots/3.png)
   
   > [!WARNING]
   > Se optar por **substituir**, todos os dados existentes no sistema de ficheiros atual da sua aplicação apagar e substituídos. Antes de clicar em **OK**, certifique-se de que é que pretende efetuar.
   > 
   > 
      
   > [!Note]
   > Devido a limitações técnicas atuais, só pode restaurar às aplicações a mesma unidade de escala. Esta limitação será removida numa versão futura.
   > 
   > 
   
    Pode selecionar **aplicação existente** restaurar para uma ranhura. Antes de utilizar esta opção, deve já tiver criado uma ranhura na sua aplicação.

4. Pode escolher restaurar a configuração do site.
   
    ![](./media/app-service-web-restore-snapshots/4.png)

5. Clique em **OK**.
