---
title: incluir ficheiro
description: incluir ficheiro
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/10/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: cdb60ffb3ba3c31c336c8b74c46621792c707f74
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/11/2018
ms.locfileid: "40046824"
---
### <a name="delete-local-resources"></a>Eliminar recursos locais

Se quiser remover o runtime do IoT Edge e os recursos relacionados do seu dispositivo, utilize os comandos adequados para o sistema operativo do mesmo. 

#### <a name="windows"></a>Windows

Desinstale o runtime do IoT Edge.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Uninstall-SecurityDaemon
   ```

Quando o runtime do IoT Edge é removido, os contentores que ele criou são parados, mas continuam no seu dispositivo. Veja todos os contentores.

   ```powershell
   docker ps -a
   ```

Elimine os contentores de runtime criados no seu dispositivo.

   ```powershell
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Veja os nomes dos contentores para eliminar os contentores adicionais que foram apresentados na saída `docker ps`. 

#### <a name="linux"></a>Linux

Remova o runtime do IoT Edge.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Quando o runtime do IoT Edge é removido, os contentores que ele criou são parados, mas continuam no seu dispositivo. Veja todos os contentores.

   ```bash
   sudo docker ps -a
   ```

Elimine os contentores de runtime criados no seu dispositivo.

   ```bash
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Veja os nomes dos contentores para eliminar os contentores adicionais que foram apresentados na saída `docker ps`. 

Remova o runtime do contentor.

   ```bash
   sudo apt-get remove --purge moby
   ```