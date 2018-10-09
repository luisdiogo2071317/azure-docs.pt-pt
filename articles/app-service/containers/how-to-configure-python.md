---
title: Configurar a imagem de Python incorporada no Serviço de Aplicações do Azure
description: Este tutorial descreve as opções para a criação e a configuração de uma aplicação Python no Serviço de Aplicações do Azure, com a imagem de Python incorporada.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 09/25/2018
ms.author: astay;cephalin
ms.custom: mvc
ms.openlocfilehash: 9316805993b81e4d2511e833e0cc8f240807a1f9
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47228557"
---
# <a name="configure-built-in-python-image-in-azure-app-service-preview"></a>Configurar a imagem de Python incorporada no Serviço de Aplicações do Azure (Pré-visualização)

Este artigo mostra como configurar a imagem de Python incorporada no [Serviço de Aplicações no Linux](app-service-linux-intro.md) para executar as suas aplicações Python.

## <a name="python-version"></a>Versão de Python

O runtime de Python no Serviço de Aplicações no Linux utiliza a versão `python-3.7.0`.

## <a name="supported-frameworks"></a>Estruturas suportadas

São suportadas todas as versões de estruturas Web em conformidade com a Web Server Gateway Interface (WSGI) e compatíveis com o runtime `python-3.7`.

## <a name="package-management"></a>Gestão de pacotes

Durante a publicação de Git, o motor do Kudu procura o ficheiro [requirements.txt](https://pip.pypa.io/en/stable/user_guide/#requirements-files) na raiz do repositório e instala automaticamente os pacotes no Azure ao utilizar o `pip`.

Para gerar este ficheiro antes da publicação, navegue até à raiz do repositório e execute o seguinte comando no seu ambiente de Python:

```bash
pip freeze > requirements.txt
```

## <a name="configure-your-python-app"></a>Configurar a sua aplicação Python

A imagem de Python incorporada no Serviço de Aplicações utiliza o servidor [Gunicorn](http://gunicorn.org/) para executar a aplicação Python. O Gunicorn é um servidor de HTTP da WSGI de Python para UNIX. O Serviço de Aplicações configura o Gunicorn automaticamente para projetos Django e Flask.

### <a name="django-app"></a>Aplicação Django

Se publicar um projeto Django que contenha um módulo `wsgi.py`, o Azure chama automaticamente o Gunicorn com o seguinte comando:

```bash
gunicorn <path_to_wsgi>
```

### <a name="flask-app"></a>Aplicação Flask

Se estiver a publicar uma aplicação Flask e o ponto de entrada estiver num módulo `application.py` ou `app.py`, o Azure chama automaticamente o Gunicorn com um dos seguintes comandos, respetivamente:

```bash
gunicorn application:app
```

Ou 

```bash
gunicorn app:app
```

### <a name="customize-start-up"></a>Personalizar o arranque

Para definir um ponto de entrada personalizado para a sua aplicação,  crie primeiro um ficheiro _.txt_ com um comando do Gunicorn personalizado e coloque-o na raiz do seu projeto. Por exemplo, para iniciar o servidor com o módulo _helloworld.py_ e a variável `app`, crie um ficheiro _startup.txt_ com o seguinte conteúdo:

```bash
gunicorn helloworld:app
```

Na página [Definições da aplicação](../web-sites-configure.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), selecione **Python|3.7** como a **Pilha de Tempo de Execução** e forneça o nome do seu **Ficheiro de Arranque** do passo anterior. Por exemplo, _startup.txt_.
