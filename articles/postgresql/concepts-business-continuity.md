---
title: Descrição geral da continuidade do negócio com a base de dados do Azure para PostgreSQL
description: Descrição geral da continuidade do negócio com a base de dados do Azure para PostgreSQL.
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 1b981b650d75556f4521aaf0f089443bb88d064a
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="overview-of-business-continuity-with-azure-database-for-postgresql"></a>Descrição geral da continuidade do negócio com a base de dados do Azure para PostgreSQL

Esta descrição geral descreve as capacidades que a base de dados do Azure para PostgreSQL fornece continuidade do negócio e recuperação de desastre. Saiba mais sobre as opções para recuperar a partir de eventos acontece que podem causar a perda de dados ou fazer com que a base de dados e a aplicação fique indisponível. Saiba o que fazer quando um erro de utilizador ou aplicação afeta a integridade dos dados, uma região do Azure tem uma interrupção ou a aplicação necessita de manutenção.

## <a name="features-that-you-can-use-to-provide-business-continuity"></a>Funcionalidades que pode utilizar para fornecer a continuidade do negócio

Base de dados do Azure para PostgreSQL fornece funcionalidades de continuidade empresarial, que incluem as cópias de segurança automatizadas e a capacidade para os utilizadores iniciem georrestauro. Cada um tem características diferentes para o tempo estimado de recuperação (ERT) e potencial perda de dados. Assim que compreender estas opções, pode escolher entre eles e utilizá-los em conjunto para diferentes cenários. Tal como desenvolver o seu plano de continuidade do negócio, tem de compreender o tempo máximo de aceitável, antes da aplicação recupera totalmente após o evento problemático – este é o objetivo de tempo de recuperação (RTO). Também precisa de compreender a quantidade máxima de dados recentes atualizações (intervalo de tempo) da aplicação pode tolerar o perder ao recuperar após o evento problemático – este é o objetivo de ponto de recuperação (RPO).

A tabela seguinte compara as ERT e o RPO para as funcionalidades disponíveis:

| **Capacidade** | **Básica** | **Fins gerais** | **Com otimização de memória** |
| :------------: | :-------: | :-----------------: | :------------------: |
| Restauro para um Ponto Anterior no Tempo a partir de cópia de segurança | Qualquer ponto de restauro dentro do período de retenção | Qualquer ponto de restauro dentro do período de retenção | Qualquer ponto de restauro dentro do período de retenção |
| Georrestauro de cópias de segurança georreplicação | Não suportado | ERT < 12 h<br/>RPO < 1 h | ERT < 12 h<br/>RPO < 1 h |

> [!IMPORTANT]
> Se eliminar o servidor, todas as bases de dados que pertencem ao servidor também são eliminados e não podem ser recuperados. Não é possível restaurar um servidor eliminado.

## <a name="recover-a-server-after-a-user-or-application-error"></a>Recuperar um servidor após um erro de utilizador ou aplicação

Pode utilizar cópias de segurança do serviço para recuperar um servidor a partir de vários eventos acontece. Um utilizador pode elimina acidentalmente alguns dados, remova inadvertidamente uma tabela importante ou até remover uma base de dados completa. Uma aplicação poderá inadvertidamente substituir os dados com dados incorretos devido a um defeito da aplicação e assim sucessivamente.

Pode efetuar um ponto-na-tempo-restauro para criar uma cópia do seu servidor para um ponto de boa conhecido no tempo. Este ponto no tempo tem de estar dentro do período de retenção de cópias de segurança que configurou para o servidor. Após o restauro de dados para o novo servidor, pode substituir o servidor original com o servidor recentemente restaurado ou copie os dados necessários do servidor restaurado para o servidor original.

## <a name="recover-from-an-azure-regional-data-center-outage"></a>Recuperar a partir de uma falha do Centro de dados regionais do Azure

Embora seja raro, um centro de dados do Azure pode ficar indisponível. Quando ocorre uma falha, faz com que uma interrupção de negócio que poderá da última apenas alguns minutos, mas foi a última durante horas.

Uma opção é a aguardar para o servidor fique novamente online quando a interrupção do Centro de dados está acima. Isto funciona para as aplicações que podem suportar para que o servidor offline para um determinado período de tempo, por exemplo um ambiente de desenvolvimento. Quando o Centro de dados tem uma falha, não souber quanto a interrupção poderá última, pelo que esta opção só funciona se não precisa do servidor de tempo.

A outra opção consiste em utilizar a base de dados do Azure para a funcionalidade de georrestauro do PostgreSQL que restaura o servidor através de cópias de segurança georredundante. Estas cópias de segurança estão acessíveis, mesmo quando a região que o servidor estiver alojado num está offline. Pode restaurar a partir destas cópias de segurança para qualquer outra região e colocar o servidor novamente online.

> [!IMPORTANT]
> Georrestauro só é possível se aprovisionar o servidor com o armazenamento de cópia de segurança georredundante.

## <a name="next-steps"></a>Passos Seguintes
- Para saber mais sobre as cópias de segurança automatizadas, consulte o artigo [cópias de segurança na base de dados do Azure para PostgreSQL](concepts-backup.md). 
- Restaurar para um ponto no tempo no portal do Azure, consulte [restaurar base de dados para um ponto no tempo no portal do Azure](howto-restore-server-portal.md).
- Para restaurar para um ponto no tempo com a CLI do Azure, consulte o artigo [restaurar base de dados para um ponto no tempo com a CLI](howto-restore-server-cli.md).