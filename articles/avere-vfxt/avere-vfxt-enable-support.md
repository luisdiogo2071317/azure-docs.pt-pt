---
title: Ativar o suporte para Avere vFXT - Azure
description: Carrega de como ativar o suporte de Avere vFXT para o Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: bc21e06b704bfe3d25132092efbbf23f342acb14
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634309"
---
# <a name="enable-support-uploads"></a>Ativar carregamentos do suporte

O vFXT Avere para o Azure automaticamente pode carregar dados de suporte sobre o cluster. Esses carregamentos permitem à equipe de suporte fornecem o melhor serviço de cliente possíveis.

## <a name="steps-to-enable-uploads"></a>Passos para ativar o carregamentos

Siga estes passos do painel de controle Avere para ativar o suporte. (Leia [aceder ao cluster vFXT](avere-vfxt-cluster-gui.md) para aprender a abrir o painel de controlo de Avere.)

1. Navegue para o **definições** separador na parte superior.
1. Clique nas **suporte** associar à esquerda e aceite a política de privacidade.

   ![Captura de ecrã que confirma a aceitação da política de privacidade](media/avere-vfxt-privacy-policy.png)
1. Clique no triângulo à esquerda da vírgula **Customer Info** para expandir a secção.
1. Definir o nome do suporte do cluster no **nome de Cluster exclusivo** -Certifique-se de que identifica exclusivamente o seu cluster para o suporte técnico.
1. As caixas de verificação **estatísticas de monitorização**, **carregar de informações gerais**, e **carregar de informações de falhas**.
1. Clique nas **validar carregamento informações** botão.
1. Clique em **submeter**.
1. Clique no triângulo à esquerda da vírgula **Secure Proativa suporte (SPS)** para expandir a secção.
1. Marque a caixa **ativar a ligação do SPS**.
1. Clique em **submeter**.

   ![Captura de ecrã que contém todos os passos para ativar o suporte](media/avere-vfxt-support-info-steps.png)


## <a name="next-steps"></a>Passos Seguintes

Se precisar de adicionar um sistema de armazenamento no local para o cluster, siga as instruções em [configurar o armazenamento](avere-vfxt-add-storage.md). 

Se estiver pronto para começar a anexar os clientes para o cluster, leio [montar o cluster de vFXT Avere](avere-vfxt-mount-clients.md).