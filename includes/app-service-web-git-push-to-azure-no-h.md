---
title: incluir ficheiro
description: incluir ficheiro
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 9f865897ee478f25a44fe876d44aec253e84eb62
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36319714"
---
Regresse à _janela de terminal local_ e adicione um remoto do Azure ao seu repositório Git local. Substitua _&lt;deploymentLocalGitUrl-from-create-step>_ pelo URL do Git remoto que guardou em [Criar uma aplicação Web](#create).

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Envie para o remoto do Azure para implementar a sua aplicação com o comando seguinte. Quando lhe forem pedidas credenciais pelo Gestor de Credenciais do Git, confirme que introduz as credenciais que criou em [Configurar um utilizador de implementação](#configure-a-deployment-user) e não as que utilizou para iniciar sessão no portal do Azure.

```bash
git push azure master
```

Este comando pode demorar alguns minutos a ser executado. Ao executar, apresenta informações semelhantes ao exemplo seguinte:
