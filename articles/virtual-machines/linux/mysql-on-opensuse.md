---
title: Instalar o MySQL numa VM OpenSUSE no Azure | Microsoft Docs
description: "Saiba como instalar o MySQL numa máquina OpenSUSE Linux VMirtual no Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 1594e10e-c314-455a-9efb-a89441de364b
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: cynthn
ms.openlocfilehash: 88bd895cb3a384f1ada0394fe2da206aca86b981
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Instalar o MySQL numa máquina virtual com o OpenSUSE Linux no Azure

[MySQL](http://www.mysql.com) é uma base de dados do SQL Server popular, open source. Este tutorial mostra como criar uma máquina virtual com OpenSUSE Linux, em seguida, instalar o MySQL.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se optar por instalar e utilizar a CLI localmente, terá da CLI do Azure versão 2.0 ou posterior. Para localizar a versão, execute `az --version`. Se precisar de instalar ou atualizar, veja [instalar o Azure CLI 2.0]( /cli/azure/install-azure-cli).

## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Criar uma máquina virtual com OpenSUSE Linux

Em primeiro lugar, crie um grupo de recursos. Neste exemplo, vamos atribuir nomes o grupo de recursos *mySQSUSEResourceGroup* e -lo na criação do *EUA Leste* região.

```azurecli-interactive
az group create --name mySQLSUSEResourceGroup --location eastus
```

Crie a VM. Neste exemplo, vamos atribuir nomes a VM *myVM*. Também iremos utilizar um tamanho VM *Standard_D2s_v3*, mas deve escolher a [tamanho da VM](sizes.md) pensa que mais adequada para a carga de trabalho.

```azurecli-interactive
az vm create --resource-group mySQLSUSEResourceGroup \
   --name myVM \
   --image openSUSE-Leap \
   --size Standard_D2s_v3 \
   --generate-ssh-keys
```

Também terá de adicionar uma regra para o grupo de segurança de rede para permitir o tráfego através da porta 3306 MySQL.

```azurecli-interactive
az vm open-port --port 3306 --resource-group mySQLSUSEResourceGroup --name myVM
```

## <a name="connect-to-the-vm"></a>Ligar à VM

Irá utilizar o SSH para ligar à VM. Neste exemplo, o endereço IP público da VM é *10.111.112.113*. Pode ver o endereço IP no resultado quando criou a VM.

```azurecli-interactive  
ssh 10.111.112.113
```

 
## <a name="update-the-vm"></a>Atualize a VM
 
Depois de se estiver ligado à VM, instale o sistema atualizações e correções de erros. 
   
```bash
sudo zypper update
```

Siga as instruções para atualizar a VM.

## <a name="install-mysql"></a>Instalar MySQL 


Instale o MySQL na VM através de SSH. Responda a pedidos conforme apropriado.

```bash
sudo zypper install mysql
```
 
Definir o MySQL para iniciar quando efetua o arranque do sistema. 

```bash
sudo systemctl enable mysql
```
Certifique-se de que o MySQL está ativado.

```bash
systemctl is-enabled mysql
```

Isto deverá devolver: ativado.


## <a name="mysql-password"></a>Palavra-passe de MySQL

Após a instalação, a palavra-passe de raiz do MySQL está vazia por predefinição. Execute o **mysql\_segura\_instalação** script para proteger o MySQL. O script pede-lhe para alterar a palavra-passe de raiz do MySQL, remova as contas de utilizador anónimo, desativar inícios de sessão remoto raiz, remover bases de dados de teste e recarregar a tabela de privilégios. 


```bash
mysql_secure_installation
```

## <a name="log-in-to-mysql"></a>Inicie sessão no MySQL

Agora que pode iniciar sessão e introduza a linha de comandos da MySQL.

```bash  
mysql -u root -p
```
Isto muda a linha de comandos da MySQL onde pode emitir instruções SQL para interagir com a base de dados.

Agora, crie um novo utilizador MySQL.

```   
CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
A ponto e vírgula (;) no final da linha é fundamental para terminar o comando.


## <a name="create-a-database"></a>Criar uma base de dados


Criar uma base de dados e conceder a `mysqluser` permissões de utilizador.

```   
CREATE DATABASE testdatabase;
GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
```
   
Os nomes de utilizador de base de dados e as palavras-passe só são utilizadas pelos scripts ligar à base de dados.  Os nomes das contas de utilizador de base de dados não representa necessariamente contas de utilizador real no sistema.

Ative início de sessão de outro computador. Neste exemplo, é o endereço IP do computador que queremos para iniciar sessão a partir *10.112.113.114*.

```   
GRANT ALL ON testdatabase.* TO 'mysqluser'@'10.112.113.114' IDENTIFIED BY 'password';
```
   
Para sair do utilitário de administração de base de dados MySQL, escreva:

```    
quit
```


## <a name="next-steps"></a>Passos Seguintes
Para obter detalhes sobre o MySQL, consulte o [MySQL documentação](http://dev.mysql.com/doc/index-topic.html).




