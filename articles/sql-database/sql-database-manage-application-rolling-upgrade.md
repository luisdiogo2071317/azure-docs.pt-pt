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
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 02/13/2019
ms.openlocfilehash: 748a9f4d7c2ec47a2ed9470789a4443bffdc0eba
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301745"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>Gerenciamento de atualizações sem interrupção das aplicações na cloud com a base de dados SQL georreplicação ativa

Aprenda a usar [georreplicação ativa](sql-database-auto-failover-group.md) na base de dados SQL para ativar a implementação de atualizações da sua aplicação na cloud. Como a atualização é uma operação de interrupção, deve ser parte de seu planejamento de continuidade de negócios e design. Neste artigo Vamos observar dois métodos diferentes para orquestrar o processo de atualização e discutem as vantagens e desvantagens de cada opção. Para os fins deste artigo, utilizamos uma aplicação que consiste num web site ligado a uma base de dados como sua camada de dados. Nosso objetivo é atualizar a versão 1 do aplicativo para a versão 2 sem qualquer impacto significativo sobre a experiência do utilizador final.

Ao avaliar as opções de atualização, deve considerar os seguintes fatores:

* Impacto na disponibilidade de aplicações durante as atualizações. O tempo que a função de aplicação pode estar limitada ou degradada.
* Capacidade de reversão se atualização falhar.
* Vulnerabilidade do aplicativo ocorra uma falha catastrófica não relacionada durante a atualização.
* Dólar de total de custos.  Isto inclui a proporcionar uma redundância adicional e os custos incrementais dos componentes temporários utilizados pelo processo de atualização.

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Atualização dos aplicativos que dependem de cópias de segurança da base de dados para recuperação após desastre

Se seu aplicativo depende de cópias de segurança automáticas da base de dados e utiliza o restauro geográfico para recuperação após desastre, é implementada numa única região do Azure. Para minimizar a interrupção do utilizador final, irá criar um ambiente de teste nessa região com todos os componentes de aplicação envolvidos na atualização. O diagrama seguinte ilustra o ambiente operacional antes do processo de atualização. O ponto final `contoso.azurewebsites.net` representa um bloco de produção do aplicativo web. Para ativar a capacidade de reverter a atualização, precisa criar um bloco de teste com uma cópia totalmente sincronizada da base de dados. Os passos seguintes, irão criar um ambiente de teste para a atualização:

1. Crie uma base de dados secundário na mesma região do Azure. Monitorize o secundário para ver se o processo de propagação é concluída (1).
2. Crie um novo bloco de implementação para a sua aplicação web chamado "Teste". Ele será registado no DNS com o URL `contoso-staging.azurewebsites.net` (2).

> [!NOTE]
> Tenha em atenção os passos de preparação não irão afetar o bloco de produção e que possa funcionar no modo de acesso total.
>  

![Configuração de Go-replicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/option1-1.png)

Depois de concluir os passos de preparação, a aplicação está pronta para a atualização real. O diagrama seguinte ilustra as etapas envolvidas no processo de atualização.

1. Defina a base de dados principal para o modo só de leitura (3). Este modo garantirá que o bloco de produção do aplicativo web (V1) irá permanecer somente-leitura durante a atualização, impedindo a potencial divergência de dados entre as instâncias de base de dados V1 e V2.  
2. Desligar a base de dados secundária, usando o modo de terminação planeada (4). Ele criará uma cópia totalmente sincronizada independente da base de dados primário. Esta base de dados será atualizado.
3. Ativar a base de dados secundária para o modo de leitura / escrita e execute o script de atualização (5).

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/option1-2.png)

Se a atualização foi concluída com êxito, está agora pronto para mudar do utilizador final para a cópia atualizada do aplicativo. Ele agora se tornará um bloco de produção.  A mudança envolve mais algumas etapas conforme ilustrado no diagrama seguinte.

1. Ative uma operação de troca entre a produção e blocos de teste do aplicativo web (6). Ele muda os URLs das duas ranhuras. Agora `contoso.azurewebsites.net` apontará para a versão V2 do web site e a base de dados (ambiente de produção).  
2. Se já não precisar da versão de V1, que se tornou uma cópia de teste após a troca, pode desativar o ambiente de teste (7).

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/option1-3.png)

Se o processo de atualização não for bem-sucedida, por exemplo devido a um erro no script de atualização, o bloco de estágio deve ser considerado comprometido. Reverter o aplicativo para o estado de pré-atualização, reverter o aplicativo no bloco de produção para acesso total. Os passos envolvidos são apresentados no diagrama seguinte.

1. Defina a cópia da base de dados para o modo de leitura / escrita (8). Ele irá restaurar o V1 completa funcionalmente da cópia de produção.
2. Executar a análise de causa raiz e desativar o ambiente de teste (9).

Neste momento, o aplicativo é totalmente funcional e os passos de atualização podem ser repetidos.

> [!NOTE]
> A reversão não requer alterações DNS como ainda não tiver sido executada uma operação de troca.

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/option1-4.png)

A chave **partido** desta opção é que pode atualizar uma aplicação numa única região usando um conjunto de passos simples. O custo de dólar da atualização é relativamente baixo. Os principais **compensação** é que, se ocorrer uma falha catastrófica durante a atualização a recuperação para o estado de pré-atualização envolverá reimplantação do aplicativo numa região diferente e restaurar a base de dados de cópia de segurança a utilizar restauro geográfico. Este processo irá resultar em períodos de indisponibilidade significativos.

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Atualização dos aplicativos que dependem de georreplicação da base de dados para recuperação após desastre

Se a sua aplicação tira partido da georreplicação ativa ou grupos de ativação pós-falha para continuidade do negócio, ele é implementado, pelo menos, duas regiões diferentes, com uma base de dados primária ativa na região primária e uma só de leitura secundária da base de dados na região de cópia de segurança. Além dos fatores mencionados anteriormente, o processo de atualização tem de garantir que:

* O aplicativo permanece protegido contra falhas catastróficas durante todo o tempo durante o processo de atualização
* Os componentes com redundância geográfica do aplicativo são atualizados em paralelo com os componentes do Active Directory

Para alcançar estes objetivos, além de usar as ranhuras de implementação de aplicação Web, aproveitará o Gestor de tráfego do Azure (ATM) a utilizar um perfil de ativação pós-falha com um ativo e pontos de extremidade uma cópia de segurança.  O diagrama seguinte ilustra o ambiente operacional antes do processo de atualização. Os web sites `contoso-1.azurewebsites.net` e `contoso-dr.azurewebsites.net` representam um ambiente de produção do aplicativo com redundância geográfica completa. O ambiente de produção inclui os seguintes componentes:

1. Bloco de produção do aplicativo web `contoso-1.azurewebsites.net` na região primária (1)
2. Base de dados primária na região primária (2) 
3. Uma instância de reserva do aplicativo web na região de cópia de segurança (3)
4. Georreplicado base de dados secundária numa região de cópia de segurança (4)
5. Perfil de desempenho do Gestor de tráfego do Azure com o ponto final online `contoso-1.azurewebsites.net` e ponto final offline `contoso-dr.azurewebsites.net`

Para ativar a capacidade de reverter a atualização, tem de criar um ambiente de teste com uma cópia totalmente sincronizada do aplicativo. Uma vez que precisa garantir que o aplicativo pode recuperar rapidamente no caso de ocorrer uma falha catastrófica durante o processo de atualização, o ambiente de teste tem de ser georredundante também. Os seguintes passos são necessários para criar um ambiente de teste para a atualização:

1. Implementar um bloco de teste do aplicativo web na região primária (6)
2. Crie uma base de dados secundária na região primária do Azure (7). Configure o bloco de teste do aplicativo web para ligar ao mesmo. 
3. Criar outra com redundância geográfica secundária da base de dados na região de cópia de segurança através da replicação de base de dados secundária numa região primária (isso é chamado de "em cadeia de georreplicação") (8).
3. Implementar um bloco de teste da instância da aplicação web na região de cópia de segurança (9) e configurá-lo para ligar a geo-secundária que criou no passo (9).


> [!NOTE]
> Tenha em atenção os passos de preparação não irão afetar o aplicativo no bloco de produção e irá permanecer totalmente funcional no modo de leitura / escrita.

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/option2-1.png)

Depois de concluir os passos de preparação, ambiente de teste está pronto para a atualização. O diagrama seguinte ilustra as etapas de atualização.

1. Defina a base de dados primária no bloco de produção para o modo só de leitura (10). Este modo garantirá que a base de dados de produção (V1) não será alterado durante a atualização, impedindo a potencial divergência de dados entre as instâncias de base de dados V1 e V2.  
2. Desligar a base de dados secundário na mesma região com o modo de terminação planeada (11). Ele criará uma cópia totalmente sincronizada mas independente da base de dados de produção. Esta base de dados será atualizado.
3. Execute o script de atualização em relação a `contoso-1-staging.azurewebsites.net`, `contoso-dr-staging.azurewebsites.net` e o teste principal da base de dados (12). As alterações de base de dados serão replicadas automaticamente para o teste secundário 

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/option2-2.png)

Se a atualização foi concluída com êxito, está agora pronto para mudar do utilizador final para a versão V2 do aplicativo. O diagrama seguinte ilustra as etapas envolvidas.

1. Ative uma operação de troca entre a produção e blocos de teste do aplicativo web na região primária (13) e na região de cópia de segurança (14). V2 do aplicativo agora se torna um bloco de produção com uma cópia redundante na região de cópia de segurança.
2. Pode desativar o ambiente de teste se já não precisar da aplicação V1 (15 e 16).  

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/option2-3.png)

Se o processo de atualização não for bem-sucedida, por exemplo devido a um erro no script de atualização, o ambiente de teste ser considerado em estado inconsistente. Reverter o aplicativo para o estado de pré-atualização reverter para utilizar a V1 do aplicativo no ambiente de produção. Os passos necessários são apresentados no diagrama seguinte.

1. Defina a cópia da base de dados primária no bloco de produção para o modo de leitura / escrita (17). Ele irá restaurar o V1 completa funcionalmente no bloco de produção.
2. Efetuar a análise da causa raiz e reparar ou remover o ambiente de teste (18 e 19).

Neste momento, o aplicativo é totalmente funcional e os passos de atualização podem ser repetidos.

> [!NOTE]
> A reversão não requer alterações DNS porque não tiver sido executada uma operação de troca.

![Configuração de georreplicação de base de dados SQL. Recuperação de desastres em nuvem.](media/sql-database-manage-application-rolling-upgrade/option2-4.png)

A chave **partido** desta opção é que pode atualizar o aplicativo e sua cópia georredundante em paralelo sem comprometer a continuidade do negócio durante a atualização. Os principais **compensação** é que ele requer redundância dupla de cada componente da aplicação e, portanto, incorre em custo mais elevado de dólar. Ela também envolve um fluxo de trabalho mais complicado.

## <a name="summary"></a>Resumo

Os dois métodos de atualização descritos no artigo diferem em complexidade e o dólar de custos, mas eles ambas se concentrar em minimizar o tempo quando o utilizador final está limitado a operações só de leitura. Esse tempo é definido diretamente pela duração do script de atualização. Ele não depende do tamanho de base de dados, a camada de serviços que escolheu, a configuração do site da web e outros fatores que não pode controlar facilmente. Todos os passos de preparação estão desassociados dos passos de atualização e não afetam o aplicativo de produção. A eficiência do script de atualização é um fator-chave que determina a experiência de utilizador final durante as atualizações. Portanto, a melhor maneira de melhorá-lo é ao concentrar-se seus esforços em tornar o script de atualização mais eficiente possível.  

## <a name="next-steps"></a>Passos Seguintes

* Para uma visão geral de continuidade de negócio e cenários, consulte [descrição geral da continuidade de negócio](sql-database-business-continuity.md).
* Para saber mais sobre a georreplicação ativa do Azure SQL da base de dados, veja [criar legíveis bases de dados secundárias com georreplicação ativa](sql-database-active-geo-replication.md).
* Para saber mais sobre grupos de ativação pós-falha de base de dados do SQL Azure, veja [utilizar grupos de ativação pós-falha automática para ativar a ativação pós-falha transparente e coordenada de várias bases de dados](sql-database-auto-failover-group.md).
* Para saber mais sobre as ranhuras de implementação e ambiente de teste no serviço de aplicações do Azure, veja [configurar ambientes de teste no serviço de aplicações do Azure](../app-service/deploy-staging-slots.md).  
* Para obter os perfis do Gestor de tráfego do Azure, veja [gerir um perfil do Gestor de tráfego do Azure](../traffic-manager/traffic-manager-manage-profiles.md).  


