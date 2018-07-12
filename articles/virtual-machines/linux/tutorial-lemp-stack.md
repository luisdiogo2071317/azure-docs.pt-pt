---
title: Tutorial – Implementar a LEMP numa máquina virtual do Linux no Azure | Microsoft Docs
description: Neste tutorial, vai aprender a instalar a pilha LEMP numa máquina virtual do Linux no Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: danlep
ms.openlocfilehash: 1f4c74ab96fc9494d1b3c7dd626265516533330b
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38606352"
---
# <a name="tutorial-install-a-lemp-web-server-on-a-linux-virtual-machine-in-azure"></a>Tutorial: Instalar um servidor Web da LEMP numa máquina virtual do Linux no Azure

Este artigo explica como implementar um servidor Web NGINX, o MySQL e o PHP (pilha LEMP) numa VM do Ubuntu no Azure. A pilha LEMP é uma alternativa à popular [pilha LAMP](tutorial-lamp-stack.md), que também pode instalar no Azure. Para ver o servidor LEMP em ação, opcionalmente, pode instalar e configurar um site do WordPress. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma VM do Ubuntu (o “L” na pilha LEMP)
> * Abrir a porta 80 para o tráfego da Web
> * Instalar o NGINX, o MySQL e o PHP
> * Verificar a instalação e a configuração
> * Instalar o WordPress no servidor LEMP

Esta configuração é para testes rápidos ou uma prova de conceito.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, este tutorial requer que execute uma versão da CLI do Azure que seja a 2.0.30 ou posterior. Executar `az --version` para localizar a versão. Se precisar de instalar ou atualizar, veja [instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli).

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>Instalar o NGINX, o MySQL e o PHP

Execute os seguintes comandos para atualizar as origens do pacote Ubuntu e instalar o NGINX, o MySQL e o PHP. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

É-lhe pedido para instalar os pacotes e outras dependências. Quando lhe for pedido, defina uma palavra-passe de raiz para o MySQL e, em seguida, clique em [Enter] para continuar. Siga as instruções restantes. Este processo instala as extensões PHP mínimas necessárias para utilizar o PHP com o MySQL. 

![Página de palavra-passe de raiz do MySQL][1]

## <a name="verify-installation-and-configuration"></a>Verificar a instalação e a configuração


### <a name="nginx"></a>NGINX

Verifique a versão do NGINX com o seguinte comando:
```bash
nginx -v
```

Com o NGINX instalado e a porta 80 aberta para a VM, torna-se possível aceder ao servidor Web a partir da Internet. Para ver a página de boas-vindas do NGINX, abra um browser e introduza o endereço IP público da VM. Utilize o endereço IP público que utilizou para encaminhar o SSH para a VM:

![Página predefinida do NGINX][3]


### <a name="mysql"></a>MySQL

Verifique a versão do MySQL com o seguinte comando (tenha em atenção o parâmetro `V` em maiúscula):

```bash
mysql -V
```

Para ajudar a proteger a instalação do MySQL, execute o script `mysql_secure_installation`. Se estiver apenas a configurar um servidor temporário, pode ignorar este passo. 

```bash
mysql_secure_installation
```

Introduza uma palavra-passe de raiz para o MySQL e configure as definições de segurança para o seu ambiente.

Se pretender experimentar funcionalidades do MySQL (criar uma base de dados MySQL, adicionar utilizadores ou alterar as definições de configuração), inicie sessão no MySQL. Este passo não é necessário para concluir este tutorial. 


```bash
mysql -u root -p
```

Quando terminar, sair da linha de comandos do mysql, escrevendo `\q`.

### <a name="php"></a>PHP

Verifique a versão do PHP com o seguinte comando:

```bash
php -v
```

Configure o NGINX para utilizar o Gestor de Processos PHP FastCGI (PHP-FPM). Execute os comandos seguintes para fazer cópia de segurança do ficheiro de configuração do bloco de servidor NGINX original e, em seguida, edite o ficheiro original num editor à sua escolha:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

No editor, substitua os conteúdos do `/etc/nginx/sites-available/default` pelo seguinte. Veja os comentários para obter uma explicação sobre as definições. Substitua o endereço IP público da sua VM de *yourPublicIPAddress*e deixe as restantes definições. Em seguida, guarde o ficheiro.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

Verifique se existem erro de sintaxe na configuração NGINX:

```bash
sudo nginx -t
```

Se a sintaxe estiver correta, reinicie o NGINX com o seguinte comando:

```bash
sudo service nginx restart
```

Se pretender mais testes, crie uma página de informações do PHP rápida, para a ver num browser. O comando seguinte cria a página de informações do PHP:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Agora pode verificar a página de informações do PHP que criou. Abra um browser e aceda a `http://yourPublicIPAddress/info.php`. Substitua o endereço IP público da sua VM. Deve ter um aspeto semelhante a esta imagem.

![Página de informações do PHP][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, implementou um servidor LEMP no Azure. Aprendeu a:

> [!div class="checklist"]
> * Criar uma VM do Ubuntu
> * Abrir a porta 80 para o tráfego da Web
> * Instalar o NGINX, o MySQL e o PHP
> * Verificar a instalação e a configuração
> * Instalar o WordPress na pilha LEMP

Avance para o tutorial seguinte para aprender a proteger os servidores Web com certificados SSL.

> [!div class="nextstepaction"]
> [Proteger o servidor Web com SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
