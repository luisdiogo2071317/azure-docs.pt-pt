---
title: Notas de versão do Azure ML Workbench para sprint 3 de Janeiro de 2018
description: Este documento fornece detalhes sobre as atualizações para a versão de sprint 3 do Azure ML
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/22/2018
ms.openlocfilehash: fa209ba2259ae82796556fc1229cd6944c7a2ae1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/27/2018
---
# <a name="sprint-3---january-2018"></a>Sprint 3 – Janeiro de 2018 

#### <a name="version-number-01171218263"></a>Número de versão: 0.1.1712.18263

>Eis como pode [determinar o número de versão](known-issues-and-troubleshooting-guide.md).

Bem-vindo à actualização quarta do Workbench do Azure Machine Learning. Seguem-se as atualizações e melhoramentos neste sprint. Muitas destas atualizações que são efetuadas como resultado direto de comentários do utilizador. 

## <a name="notable-new-features-and-changes"></a>Acentuadas novas funcionalidades e alterações
- As atualizações à pilha de autenticação força o início de sessão e a conta de seleção no arranque

## <a name="detailed-updates"></a>Atualizações de detalhado
Segue-se uma lista das atualizações de detalhado cada área de componente do Azure Machine Learning neste sprint.

### <a name="workbench"></a>Workbench
- Capacidade de instalar/desinstalar a aplicação a partir de adicionar/remover programas
- As atualizações à pilha de autenticação força o início de sessão e a conta de seleção no arranque
- Experiência melhorada de único início de sessão (SSO) no Windows
- Os utilizadores que pertencem a vários inquilinos com credenciais diferentes agora conseguirão iniciar sessão em Workbench

#### <a name="ui"></a>IU
- Melhoramentos gerais e correções de erros

### <a name="notebooks"></a>Blocos de notas
- Melhoramentos gerais e correções de erros

### <a name="data-preparation"></a>Preparação de dados 
- Sugestões de automática melhoradas ao efetuar transformações de exemplo por
- Algoritmo melhorado para inspector frequência padrão
- Capacidade de enviar dados de exemplo e comentários ao efetuar transformações de exemplo por ![derivar de imagem de ligação de comentários de envio na transformação de coluna](media/release-notes-sprint-3/SendFeedbackFromDeriveColumn.png)
- Melhoramentos de tempo de execução do Spark
- Scala substituiu Pyspark
- Incapacidade fixa para fechar a dados não é aplicável para o Inspector de séries de tempo 
- Corrigido hora hang de execução de preparação de dados para HDI

### <a name="model-management-cli-updates"></a>Modelo de CLI de gestão de atualizações 
  - Propriedade da subscrição já não é necessária para o aprovisionamento de recursos. Acesso de contribuinte ao grupo de recursos serão suficiente para configurar o ambiente de implementação.
  - Ambiente local ativada gratuitamente configurar subscrições 
