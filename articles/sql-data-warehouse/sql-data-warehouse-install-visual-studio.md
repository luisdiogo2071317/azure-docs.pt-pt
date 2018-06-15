---
title: Instalar Visual Studio e SSDT para SQL Data Warehouse | Microsoft Docs
description: Instalar o Visual Studio e SQL Server Development Tools (SSDT) para o Azure SQL Data Warehouse
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: a2f01424dedb977000d0e4150f4a31c1a9a21cfb
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
ms.locfileid: "31790649"
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Instalar Visual Studio e SSDT para SQL Data Warehouse
Para desenvolver aplicações SQL do armazém de dados, recomendamos que utilize a versão mais recente do Visual Studio com a versão mais recente do SQL Server Data Tools (SSDT).  A Atualização 5 do Visual Studio 2013 com SSDT também é suportada para compatibilidade com versões anteriores.  

Utilizar o Visual Studio com SSDT permite-lhe utilizar o SQL Server Object Explorer para explorar visualmente tabelas, vistas, procedimentos armazenados e muitos mais objetos no SQL Data Warehouse, bem como executar consultas.

> [!NOTE]
> O SQL Data Warehouse ainda não suporta Projetos de Base de Dados do Visual Studio. Para receber atualizações periódicas sobre esta funcionalidade,. votar em [UserVoice].
> 
> 

## <a name="step-1-install-visual-studio"></a>Passo 1: Instalar o Visual Studio
Siga estas ligações para transferir e instalar o Visual Studio. Se já tiver o Visual Studio 2013 ou posterior instalado, pode avançar para o passo 2, instale o SSDT.

1. [Transferir o Visual Studio][].
2. Siga o [instalar o Visual Studio] [ Installing Visual Studio] orientar na MSDN e escolha as configurações predefinidas.

## <a name="step-2-install-ssdt"></a>Passo 2: Instalar o SSDT
Para instalar o SSDT para Visual Studio, verifique primeiro para uma atualização do SSDT a partir do Visual Studio, seguindo estes passos.

1. No Visual Studio, clique em **ferramentas** / **extensões e atualizações...** / **Atualizações**
2. Selecione **Atualizações de Produtos** e, em seguida, procure **Atualização do Microsoft SQL Server para Ferramentas de Base de Dados**

Se não for encontrada uma atualização, deve ter a versão mais recente instalada.  Para confirmar que o SSDT está instalado, clique em **Ajuda** / **Sobre o Microsoft Visual Studio** e procure SQL Server Data Tools na lista. A versão mais recente do SSDT é 14.0.60525.0. Se a opção de instalação não está disponível a partir do Visual Studio, em alternativa, pode visitar o [transferência do SSDT] [ SSDT Download] página para transferir e instalar manualmente o SSDT.

## <a name="next-steps"></a>Passos Seguintes
Agora que tem a versão mais recente do SSDT, está pronto para [ligar] [ connect] para o SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Transferir o Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
