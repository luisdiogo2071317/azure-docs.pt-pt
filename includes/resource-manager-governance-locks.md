---
title: incluir ficheiro
description: incluir ficheiro
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 0cb3de7d893ccfe638468110b1b6f5fb61b2bc7c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
Bloqueios de recurso impedem os utilizadores na sua organização acidentalmente eliminem ou modificar a recursos críticos. Ao contrário do controlo de acesso baseado em funções, bloqueios de recurso aplicam-se uma restrição em todos os utilizadores e funções. 

Pode definir o bloqueio para **CanNotDelete** ou **ReadOnly**. No portal, os níveis de bloqueios são apresentados como **eliminar** e **só de leitura** respetivamente.

* **CanNotDelete** significa que os utilizadores autorizados, podem ainda ler e modificar um recurso, mas não é possível eliminar o recurso. 
* **Só de leitura** significa que os utilizadores autorizados podem ler um recurso, mas não é possível eliminar ou atualizar o recurso. Aplicar esta bloqueio é semelhante ao restringir todos os utilizadores autorizados para as permissões concedidas à **leitor** função. 

> [!TIP]
> Seja cuidadoso ao aplicar um **ReadOnly** bloqueio. Algumas operações que parecem como ler operações, na verdade, necessitam de ações adicionais. Por exemplo, um **ReadOnly** bloqueio numa conta de armazenamento impede que todos os utilizadores listar as chaves. A lista de operação de chaves é processada através de um pedido POST porque as chaves devolvidas estão disponíveis para operações de escrita. A **ReadOnly** bloqueio de um recurso de serviço de aplicações impede o Explorador de servidores do Visual Studio a apresentação de ficheiros para o recurso porque esse interação requer acesso de escrita.

Ao aplicar um bloqueio num âmbito principal, todos os recursos desse âmbito herdam o bloqueio do mesmo. Recursos mesmo que adicionar mais tarde herdam o bloqueio do principal. O bloqueio mais restritivo na herança tem precedência.

Bloqueios de Gestor de recursos são aplicadas apenas a operações acontecer na plane a gestão, que consiste em operações de envio para `https://management.azure.com`. Os bloqueios não restringir como recursos processam as suas próprias funções. Alterações de recurso estão limitadas, mas as operações de recurso não são restritas. Por exemplo, um bloqueio de só de leitura de uma base de dados do SQL Server impede-o de eliminar ou modificar a base de dados. Não impedi-lo de criar, atualizar ou eliminar dados na base de dados. Transações de dados são permitidas porque essas operações não são enviadas para `https://management.azure.com`.

### <a name="who-can-create-or-delete-locks-in-your-organization"></a>Quem pode criar ou eliminar as bloqueios na sua organização
Para criar ou eliminar as bloqueios de gestão, tem de ter acesso a `Microsoft.Authorization/locks/*` ações. Das funções incorporadas, apenas **proprietário** e **administrador de acesso de utilizador** concedidas essas ações.
