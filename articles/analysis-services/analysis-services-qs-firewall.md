---
title: Início Rápido – Configurar uma firewall para um servidor do Analysis Services no Azure | Microsoft Docs
description: Saiba como configurar uma firewall para uma instância de servidor do Analysis Services no Azure.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 0351ab3213ac75559cc025f2ef71e2f098a7504c
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52993187"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Início Rápido: Configurar a firewall do servidor – Portal

Este início rápido ajuda-o a configurar uma firewall para o servidor do Analysis Services do Azure. Ativar uma firewall e configurar os intervalos dos endereços IP apenas para os computadores que acedem ao seu servidor são uma parte importante para proteger o servidor e os dados.

## <a name="prerequisites"></a>Pré-requisitos

- Um servidor do Analysis Services na subscrição. Para obter mais informações, veja [Início Rápido: Criar um servidor – Portal](analysis-services-create-server.md) ou [Início Rápido: Criar um servidor – PowerShell](analysis-services-create-powershell.md)
- Intervalos de endereços IP de um ou mais computadores cliente (se necessário).

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure 

[Inicie sessão no portal](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Configurar uma firewall

1. Clique no servidor para abrir a página Descrição geral. 
2. Em **DEFINIÇÕES** > **Firewall** > **Ativar firewall**, clique em **Ativar**.
3. Para permitir o acesso do DirectQuery a partir do serviço Power BI, em **Permitir acesso a partir do Power BI**, clique em **Ativar**.  
4. (Opcional) Especifique um ou mais intervalos de endereços IP. Introduza um nome, um endereço IP de início e de fim para cada intervalo. 
5. Clique em **Guardar**.

     ![Definições de firewall](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não forem necessários, elimine os intervalos de endereços IP ou desative a firewall.

## <a name="next-steps"></a>Passos Seguintes
Neste guia de início rápido, aprendeu a configurar uma firewall para o servidor. Agora que tem o servidor protegido com uma firewall, pode adicionar um modelo de dados básicos de exemplo a partir do portal. Ter um modelo de exemplo é útil para saber como configurar as funções de base de dados do modelo e testar as ligações de cliente. Para obter mais informações, avance para o tutorial para adicionar um modelo de exemplo.

> [!div class="nextstepaction"]
> [Tutorial: Adicionar um modelo de exemplo ao servidor](analysis-services-create-sample-model.md)
