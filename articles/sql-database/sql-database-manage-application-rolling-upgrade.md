---
title: Implementar atualizações de aplicações - base de dados SQL do Azure | Documentos da Microsoft
description: Saiba como utilizar georreplicação de base de dados do Azure SQL para oferecer suporte a atualizações online da sua aplicação na cloud.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 08/23/2018
ms.openlocfilehash: 7031617dea4b7eb45309fd003242a8ee32d797ed
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53272019"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Gerenciamento de atualizações sem interrupção das aplicações na cloud com a base de dados SQL georreplicação ativa

Aprenda a usar [georreplicação ativa](sql-database-auto-failover-group.md) na base de dados SQL para ativar a implementação de atualizações da sua aplicação na cloud. Como a atualização é uma operação de interrupção, deve ser parte de seu planejamento de continuidade de negócios e design. Neste artigo Vamos observar dois métodos diferentes para orquestrar o processo de atualização e discutem as vantagens e desvantagens de cada opção. Para os fins deste artigo, nós usaremos um aplicativo simples que consiste num web site ligado a uma base de dados como sua camada de dados. Nosso objetivo é atualizar a versão 1 do aplicativo para a versão 2 sem qualquer impacto significativo sobre a experiência do utilizador final.

Ao avaliar as opções de atualização deve considerar os seguintes fatores:

* Impacto na disponibilidade de aplicações durante as atualizações. O tempo que a função de aplicação pode estar limitada ou degradada.
* Recurso de reversão em caso de uma falha de atualização.
* Vulnerabilidade do aplicativo ocorra uma falha catastrófica não relacionada durante a atualização.
* Dólar de total de custos.  Isto inclui a proporcionar uma redundância adicional e os custos incrementais dos componentes temporários utilizados pelo processo de atualização.

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Atualização dos aplicativos que dependem de cópias de segurança da base de dados para recuperação após desastre

Se seu aplicativo depende de cópias de segurança automáticas da base de dados e utiliza o restauro geográfico para recuperação após desastre, geralmente é implementado numa única região do Azure. Neste caso o processo de atualização envolve a criação de uma implementação de cópia de segurança de todos os componentes da aplicação envolvidos na atualização. Para minimizar a interrupção do utilizador final aproveitará o Gestor de tráfego do Azure (ATM) com o perfil de ativação pós-falha.  O diagrama seguinte ilustra o ambiente operacional antes do processo de atualização. O ponto final `contoso-1.azurewebsites.net` representa um bloco de produção do aplicativo que precisa ser atualizado. Para ativar a capacidade de reverter a atualização, tem de criar uma ranhura de fase com uma cópia totalmente sincronizada do aplicativo. Os seguintes passos são necessários para preparar a aplicação para a atualização:

1. Crie uma ranhura de fase para a atualização. Para fazer isso, crie uma base de dados secundária (1) e implemente um web site idênticos na mesma região do Azure. Monitorize o secundário para ver se o processo de propagação é concluído.
2. Criar um perfil de ativação pós-falha no ATM com `contoso-1.azurewebsites.net` como ponto de extremidade online e `contoso-2.azurewebsites.net` como offline.

> [!NOTE]
> Tenha em atenção os passos de preparação não irão afetar o aplicativo no bloco de produção e que possa funcionar no modo de acesso total.
>  

![Configuração de Go-replicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

Depois de concluir os passos de preparação a aplicação está pronta para a atualização real. O diagrama seguinte ilustra as etapas envolvidas no processo de atualização.

1. Defina a base de dados primária no bloco de produção para o modo só de leitura (3). Tal irá garantir que a instância de produção do aplicativo (V1) irá permanecer somente-leitura durante a atualização, impedindo a potencial divergência de dados entre as instâncias de base de dados V1 e V2.  
2. Desligar a base de dados secundária, usando o modo de terminação planeada (4). Ele criará uma cópia totalmente sincronizada independente da base de dados primário. Esta base de dados será atualizado.
3. Ativar a base de dados principal para o modo de leitura / escrita e execute o script de actualização na ranhura de fase (5).

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Se a atualização foi concluída com êxito agora está pronto para mudar do utilizador final para a cópia faseada do aplicativo. Ele agora se tornará o bloco de produção do aplicativo.  Isso envolve mais algumas etapas conforme ilustrado no diagrama seguinte.

1. Mudar o ponto de extremidade online no perfil de ATM sejam `contoso-2.azurewebsites.net`, que aponta para a versão V2 do web site (6). Ele se torna o bloco de produção com a aplicação V2 e o tráfego de utilizador final é direcionado ao mesmo.  
2. Se precisar dos componentes de aplicações V1 já não é por isso, pode com segurança removê-los (7).

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Se o processo de atualização não for bem-sucedida, por exemplo devido a um erro no script de atualização, o bloco de estágio deve ser considerado comprometido. Para reverter o aplicativo para o estado de pré-atualização simplesmente reverter o aplicativo no bloco de produção para acesso total. Os passos envolvidos são apresentados no diagrama seguinte.

1. Defina a cópia da base de dados para o modo de leitura / escrita (8). Esta ação irá restaurar o V1 completa funcionalmente no bloco de produção.
2. Executar a análise de causa raiz e remover os componentes comprometidos na ranhura de fase (9).

Neste momento, o aplicativo é totalmente funcional e os passos de atualização podem ser repetidos.

> [!NOTE]
> A reversão não requer alterações no perfil de ATM como ele já aponte para `contoso-1.azurewebsites.net` como o ponto final do Active Directory.

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

A chave **partido** desta opção é que pode atualizar uma aplicação numa única região usando um conjunto de passos simples. O custo de dólar da atualização é relativamente baixo. Os principais **compensação** é que, se ocorrer uma falha catastrófica durante a atualização a recuperação para o estado de pré-atualização envolverá reavaliação de implementação da aplicação numa região diferente e restaurar a base de dados de cópia de segurança a utilizar restauro geográfico. Este processo irá resultar em períodos de indisponibilidade significativos.

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Atualização dos aplicativos que dependem de georreplicação da base de dados para recuperação após desastre

Se a sua aplicação tira partido da georreplicação para continuidade do negócio, ele é implementado, pelo menos, duas regiões diferentes, com uma implementação ativa na região primária e uma implementação em espera na região de cópia de segurança. Além dos fatores mencionados anteriormente, o processo de atualização tem de garantir que:

* O aplicativo permanece protegido contra falhas catastróficas durante todo o tempo durante o processo de atualização
* Os componentes com redundância geográfica do aplicativo são atualizados em paralelo com os componentes do Active Directory

Para atingir esses objetivos aproveitará o Gestor de tráfego do Azure (ATM) com o perfil de ativação pós-falha com uma ativa e três pontos de extremidade de cópia de segurança.  O diagrama seguinte ilustra o ambiente operacional antes do processo de atualização. Os web sites `contoso-1.azurewebsites.net` e `contoso-dr.azurewebsites.net` representam um bloco de produção do aplicativo com redundância geográfica completa. Para ativar a capacidade de reverter a atualização, tem de criar uma ranhura de fase com uma cópia totalmente sincronizada do aplicativo. Uma vez que precisa garantir que o aplicativo pode recuperar rapidamente no caso de ocorrer uma falha catastrófica durante o processo de atualização, o bloco de fase tem de ser georredundante também. Os seguintes passos são necessários para preparar a aplicação para a atualização:

1. Crie uma ranhura de fase para a atualização. Para fazer que criar uma base de dados secundária (1) e implementar uma cópia idêntica do web site na mesma região do Azure. Monitorize o secundário para ver se o processo de propagação é concluído.
2. Crie uma base de dados secundária com redundância geográfica na ranhura de fase georreplicação a base de dados secundária para a região de cópia de segurança (isso é chamado de "em cadeia de georreplicação"). Monitorize a cópia de segurança secundária para ver se o processo de propagação é concluída (3).
3. Criar uma cópia em espera do web site na região de cópia de segurança e ligá-lo para o secundário com redundância geográfica (4).  
4. Adicionar os pontos finais adicionais `contoso-2.azurewebsites.net` e `contoso-3.azurewebsites.net` para o perfil de ativação pós-falha em ATM como pontos finais offline (5).

> [!NOTE]
> Tenha em atenção os passos de preparação não irão afetar o aplicativo no bloco de produção e que possa funcionar no modo de acesso total.

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

Depois de concluir os passos de preparação, o bloco de fase está pronto para a atualização. O diagrama seguinte ilustra as etapas de atualização.

1. Defina a base de dados primária no bloco de produção para o modo só de leitura (6). Tal irá garantir que a instância de produção do aplicativo (V1) irá permanecer somente-leitura durante a atualização, impedindo a potencial divergência de dados entre as instâncias de base de dados V1 e V2.  
2. Desligar a base de dados secundário na mesma região com o modo de terminação planeada (7). Ele criará uma cópia totalmente sincronizada independente da base de dados primária, que se tornará automaticamente um site primário após a finalização. Esta base de dados será atualizado.
3. Ativar a base de dados primária na ranhura de fase para o modo de leitura / escrita e execute o script de atualização (8).

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Se a atualização foi concluída com êxito agora está pronto para mudar do utilizador final para a versão V2 do aplicativo. O diagrama seguinte ilustra as etapas envolvidas.

1. Mudar o ponto final do Active Directory no perfil de ATM sejam `contoso-2.azurewebsites.net`, que agora aponta para a versão V2 do web site (9). Agora será um bloco de produção com o aplicativo V2 e o tráfego de utilizador final é direcionado ao mesmo.
2. Se precisar da aplicação V1 já não é por isso, pode com segurança removê-lo (10 e 11).  

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Se o processo de atualização não for bem-sucedida, por exemplo devido a um erro no script de atualização, o bloco de estágio deve ser considerado comprometido. Para reverter o aplicativo para o estado de pré-atualização simplesmente reverter de usando o aplicativo no bloco de produção com acesso total. Os passos envolvidos são apresentados no diagrama seguinte.

1. Defina a cópia da base de dados primária no bloco de produção para o modo de leitura / escrita (12). Esta ação irá restaurar o V1 completa funcionalmente no bloco de produção.
2. Executar a análise de causa raiz e remover os componentes comprometidos na ranhura de fase (13 e 14).

Neste momento, o aplicativo é totalmente funcional e os passos de atualização podem ser repetidos.

> [!NOTE]
> A reversão não requer alterações no perfil de ATM como ele já aponte para `contoso-1.azurewebsites.net` como o ponto final do Active Directory.

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

A chave **partido** desta opção é que pode atualizar o aplicativo e sua cópia georredundante em paralelo sem comprometer a continuidade do negócio durante a atualização. Os principais **compensação** é que ele requer redundância dupla de cada componente da aplicação e, portanto, incorre em custo mais elevado de dólar. Ela também envolve um fluxo de trabalho mais complicado.

## <a name="summary"></a>Resumo

Os dois métodos de atualização descritos no artigo diferem em complexidade e o dólar de custos, mas eles ambas se concentrar em minimizar o tempo quando o utilizador final está limitado a operações só de leitura. Esse tempo é definido diretamente pela duração do script de atualização. Ele não depende do tamanho de base de dados, a camada de serviços que escolheu, a configuração do site da web e outros fatores que não pode controlar facilmente. Isso ocorre porque todos os passos de preparação estão desassociados dos passos de atualização e podem ser feitos sem afetar o aplicativo de produção. A eficiência do script de atualização é o fator principal que determina a experiência de utilizador final durante as atualizações. Portanto, a melhor maneira de melhorá-lo é ao concentrar-se seus esforços em tornar o script de atualização mais eficiente possível.  

## <a name="next-steps"></a>Passos Seguintes

* Para uma visão geral de continuidade de negócio e cenários, consulte [descrição geral da continuidade de negócio](sql-database-business-continuity.md).
* Para saber mais sobre SQL do Azure, base de dados automatizada de cópias de segurança, consulte [cópias de segurança automatizadas de base de dados SQL](sql-database-automated-backups.md).
* Para saber mais sobre a utilização de cópias de segurança automatizadas para recuperação, veja [restaurar uma base de dados a partir de cópias de segurança automáticas](sql-database-recovery-using-backups.md).
