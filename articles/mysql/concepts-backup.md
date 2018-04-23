---
title: Cópia de segurança e restauro da base de dados do Azure para MySQL
description: Saiba mais sobre as cópias de segurança automáticas e restaurar a base de dados do Azure para o servidor de MySQL.
services: mysql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 1cc2c6ccb4459c5c942297cab46378502b63c5bc
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="backup-and-restore-in-azure-database-for-mysql"></a>Cópia de segurança e restauro da base de dados do Azure para MySQL

Base de dados do Azure para MySQL cria cópias de segurança do servidor e armazena-os no armazenamento localmente redundante ou georredundante do utilizador configurada automaticamente. As cópias de segurança podem ser utilizadas para restaurar o servidor para um ponto no tempo. Cópia de segurança e restauro são uma parte essencial de qualquer estratégia de continuidade do negócio porque protegem os dados de danos acidentais ou eliminação.

## <a name="backups"></a>Cópias de segurança

Base de dados do Azure para MySQL guia completo, valor diferencial e cópias de segurança de registo de transações. Estas cópias de segurança permitem-lhe restaurar um servidor para qualquer ponto no tempo dentro do período de retenção de cópias de segurança configurado. O período de retenção de cópias de segurança predefinido é de sete dias. Opcionalmente, pode configurá-la cópias de segurança para 35 dias. Todas as cópias de segurança estão encriptadas com a encriptação AES 256 bits.

### <a name="backup-frequency"></a>Frequência de cópia de segurança

Geralmente, as cópias de segurança completas semanal, ocorrem as cópias de segurança diferenciais ocorrem duas vezes por dia, e cópias de segurança de registo de transações ocorrem a cada cinco minutos. A primeira cópia de segurança completa é agendada imediatamente depois de um servidor é criado. A cópia de segurança inicial pode demorar mais tempo num servidor restaurado grande. O ponto mais antigo na hora em que um novo servidor pode ser restaurado para é o tempo em que a cópia de segurança completa inicial está concluída.

### <a name="backup-redundancy-options"></a>Opções de cópia de segurança de redundância

Base de dados do Azure para MySQL fornece a flexibilidade para escolher entre localmente redundante ou georredundante armazenamento de cópia de segurança em camadas de objetivo geral e a otimização de memória. Quando as cópias de segurança são armazenadas no armazenamento de cópia de segurança georredundante, não são apenas armazenadas numa região na qual o servidor está alojado, mas também são replicadas para um [Centro de dados emparelhado](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Isto proporciona uma melhor proteção e capacidade de restaurar o servidor numa região diferente em caso de desastre. A camada básica oferece apenas armazenamento localmente redundante de cópia de segurança.

> [!IMPORTANT]
> Configurar localmente redundante ou georredundante armazenamento de cópia de segurança só é permitida durante a servidor criar. Assim que o servidor é aprovisionado, não é possível alterar a opção de redundância do armazenamento de cópia de segurança.

### <a name="backup-storage-cost"></a>Custo de armazenamento de cópia de segurança

Base de dados do Azure para MySQL fornece até 100% do seu armazenamento de servidor aprovisionado como armazenamento de cópia de segurança sem custos adicionais. Normalmente, isto é adequado para uma retenção de cópias de segurança de sete dias. Qualquer armazenamento de cópia de segurança adicional utilizado é cobrado num mês de GB.

Por exemplo, se aprovisionou um servidor com 250 GB, terá de 250 GB de armazenamento de cópia de segurança, sem encargos adicionais. É cobrado o armazenamento que excedam 250 GB.

## <a name="restore"></a>Restauro

Na base de dados do Azure para MySQL, efetuar um restauro cria um novo servidor de cópias de segurança do servidor original.

Existem dois tipos de restauro disponíveis:

- **Restauro de ponto no tempo** está disponível em qualquer opção de cópia de segurança redundância e cria um novo servidor na mesma região que o servidor original.
- **Georrestauro** está disponível apenas se tiver configurado o servidor para o armazenamento georredundante e permite-lhe restaurar o servidor para uma região diferente.

O tempo estimado de recuperação depende de vários fatores, incluindo os tamanhos de base de dados, o tamanho do registo de transações, a largura de banda de rede e o número total de bases de dados a recuperar na mesma região ao mesmo tempo. O tempo de recuperação é, normalmente, menos de 12 horas.

> [!IMPORTANT]
> Se eliminar o servidor, todas as bases de dados que pertencem ao servidor também são eliminados e não podem ser recuperados. Não é possível restaurar um servidor eliminado.

### <a name="point-in-time-restore"></a>Restauro para um ponto anterior no tempo

Independentemente da opção de redundância de cópia de segurança, pode efetuar um restauro para qualquer ponto no tempo no seu período de retenção de cópias de segurança. É criado um novo servidor na mesma região do Azure como o servidor original. É criado com a configuração do servidor original para o escalão de preço, geração, número de vCores, o tamanho de armazenamento, o período de retenção de cópias de segurança e opção de cópia de segurança de redundância de computação.

Restauro de ponto no tempo é útil em cenários de vários. Por exemplo, quando um utilizador elimina acidentalmente dados, ignora uma tabela importante ou a base de dados, ou se uma aplicação acidentalmente substitui os dados com dados incorretos devido a um defeito da aplicação.

Poderá ter de esperar pela próxima transação registo cópia de segurança a ser executadas antes de pode restaurar para um ponto no tempo nos últimos cinco minutos.

### <a name="geo-restore"></a>Georrestauro

Pode restaurar um servidor a outra região do Azure onde o serviço está disponível se tiver configurado o servidor para cópias de segurança georredundante. Georrestauro é a opção de recuperação predefinida quando o servidor está indisponível devido a um incidente na região onde está alojado o servidor. Se um incidente em grande escala nos resultados da região na indisponibilidade da sua aplicação de base de dados, pode restaurar um servidor de cópias de segurança georredundante para um servidor em qualquer outra região. Não há um atraso entre quando é efetuada uma cópia de segurança e quando são replicado para noutra região. Este atraso pode demorar até uma hora, por isso, se ocorrer um desastre, pode haver cópias de segurança a perda de dados de uma hora.

### <a name="perform-post-restore-tasks"></a>Executar tarefas de pós-restauro

Após um restauro de um mecanismo de recuperação, deve efetuar as seguintes tarefas para obter os seus utilizadores e aplicações fazer uma cópia em execução:

- Se o novo servidor destina-se para substituir o servidor original, redirecionar clientes e aplicações de cliente para o novo servidor
- Certifique-se de regras de firewall ao nível do servidor adequadas no local para os utilizadores ligarem
- Certifique-se de inícios de sessão adequados e permissões ao nível da base de dados no local
- Configurar alertas, conforme adequado

## <a name="next-steps"></a>Passos Seguintes

- Para saber mais sobre a continuidade do negócio, consulte o [descrição geral da continuidade do negócio](concepts-business-continuity.md).
- Restaurar para um ponto no tempo no portal do Azure, consulte [restaurar base de dados para um ponto no tempo no portal do Azure](howto-restore-server-portal.md).
- Para restaurar para um ponto no tempo com a CLI do Azure, consulte o artigo [restaurar base de dados para um ponto no tempo com a CLI](howto-restore-server-cli.md).