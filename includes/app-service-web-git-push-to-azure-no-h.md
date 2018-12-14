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
ms.openlocfilehash: 026540290710d039dbc06c394ab538ebe2d7c12f
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344674"
---
Regresse à _janela de terminal local_ e adicione um remoto do Azure ao seu repositório Git local. Substitua _&lt;deploymentLocalGitUrl-from-create-step>_ pelo URL do Git remoto que guardou em [Criar uma aplicação Web](#create-a-web-app).

```bash
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

Envie para o remoto do Azure para implementar a sua aplicação com o comando seguinte. Quando lhe forem pedidas credenciais pelo Gestor de Credenciais do Git, confirme que introduz as credenciais que criou em [Configurar um utilizador de implementação](#configure-a-deployment-user) e não as que utilizou para iniciar sessão no portal do Azure.

```bash
git push azure master
```

Este comando pode demorar alguns minutos a ser executado. Ao executar, apresenta informações semelhantes ao exemplo seguinte:
