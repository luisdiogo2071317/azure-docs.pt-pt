---
title: Ferramentas para utilizar Ansible com o Azure
description: Instalar e utilizar ferramentas individuais para Ansible com o Azure
ms.service: ansible
keywords: ansible do azure, devops, ferramentas, o vs code, código do visual studio, a extensão
author: tomarcher
manager: routlaw
ms.author: tarcher
ms.date: 01/14/2018
ms.topic: article
ms.openlocfilehash: 73d31054bb0c40dd08bf4b4a93c31c59354b5bed
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
ms.locfileid: "28212051"
---
# <a name="tools-for-using-ansible-with-azure"></a>Ferramentas para utilizar Ansible com o Azure

As ferramentas seguintes tornam a trabalhar com Ansible e o Azure mais fácil e mais eficiente.

## <a name="visual-studio-code-extension-for-ansible"></a>Visual Studio Code extensão para Ansible

O [extensão do Visual Studio Code para Ansible](https://marketplace.visualstudio.com/items?itemName=vscoss.vscode-ansible) fornece várias funcionalidades para utilizar Ansible do Visual Studio Code:

- Tipo de conclusão automática de diretivas Ansible, módulos e plug-ins na documentação do Ansible conforme.
- Apresentação de fragmentos de código, quando clicar em &lt;Ctrl >&lt;espaço > ao seu playbooks Ansible de criação.
- Realce da sintaxe apresenta o código do manual de comunicação social Ansible em diferentes cores e tipos de letra em conformidade com a sintaxe YAML.
- Ansible playbooks podem ser executadas a partir da janela de Terminal de código do Visual Studio.
    - (Apenas Windows) Ansible pode ser executada a partir de um contentor de Docker.
    - (Linux e macOS) Ansible pode ser executada a partir de um contentor de Docker ou de uma instalação de Ansible local. 
- Ansible playbooks podem ser executadas a partir da Shell de nuvem do Azure.