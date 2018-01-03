
O código para todas as funções numa aplicação de função específico está localizado na pasta raiz que contenha um ficheiro de configuração de anfitrião e uma ou mais subpastas. Cada subpasta contém o código para uma função separada, como no exemplo seguinte:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

O ficheiro host.json contém algumas configurações específicas do tempo de execução e se encontra na pasta raiz da aplicação de função. Para obter informações sobre as definições que estão disponíveis, consulte o [host.json referência](../articles/azure-functions/functions-host-json.md).

Cada função tem uma pasta que contém um ou mais ficheiros de código, a configuração de function.json e outras dependências.

