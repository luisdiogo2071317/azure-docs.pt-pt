---
title: Suplemento de encaminhamento HTTP aplicação no Azure Kubernetes Service (AKS)
description: Utilize o suplemento de encaminhamento de aplicação de HTTP no Azure Kubernetes Service (AKS).
services: container-service
author: lachie83
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/25/2018
ms.author: laevenso
ms.openlocfilehash: fa9c9416a8a1f80a62a21e5d9041e750b1debb98
ms.sourcegitcommit: f58fc4748053a50c34a56314cf99ec56f33fd616
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48269382"
---
# <a name="http-application-routing"></a>Encaminhamento de aplicações de HTTP

A solução de encaminhamento de aplicações de HTTP torna mais fácil de aceder a aplicações que são implementadas ao seu cluster do Azure Kubernetes Service (AKS). Quando a solução ativada, configura um controlador de entradas no seu cluster do AKS. Como os aplicativos são implantados, a solução também cria acessíveis publicamente nomes DNS para pontos finais da aplicação.

Quando o suplemento estiver ativado, cria uma zona DNS na sua subscrição. Para obter mais informações sobre o custo DNS, consulte [preços de DNS][dns-pricing].

## <a name="http-routing-solution-overview"></a>Descrição geral de solução encaminhamento HTTP

O suplemento implementa dois componentes: um [controlador de entradas de Kubernetes] [ ingress] e um [DNS externo] [ external-dns] controlador.

- **Controlador de entradas**: controlador de entradas a é exposto à internet através de um serviço de Kubernetes do tipo LoadBalancer. O controlador de entrada, observa e implementa [recursos de entrada do Kubernetes][ingress-resource], que cria rotas para pontos finais da aplicação.
- **Controlador de DNS externo**: controla Kubernetes recursos de entrada e cria A DNS de registos na zona DNS específicos do cluster.

## <a name="deploy-http-routing-cli"></a>Implementar o encaminhamento de HTTP: CLI

O suplemento de encaminhamento de aplicativo HTTP pode ser ativado com a CLI do Azure ao implementar um cluster do AKS. Para tal, utilize o [criar az aks] [ az-aks-create] comando com o `--enable-addons` argumento.

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --enable-addons http_application_routing
```

Também pode ativar o encaminhamento de HTTP num existente AKS cluster com o [enable-complementos de az aks] [ az-aks-enable-addons] comando. Para ativar o encaminhamento de HTTP num cluster existente, adicione a `--addons` parâmetro e especificar *http_application_routing* conforme mostrado no exemplo a seguir:

```azurecli
az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing
```

Depois do cluster é implementado ou atualizado, utilize o [show do az aks] [ az-aks-show] comando para obter o nome da zona DNS. Este nome é necessária para implementar aplicações para o cluster do AKS.

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table

Result
-----------------------------------------------------
9f9c1fe7-21a1-416d-99cd-3543bb92e4c3.eastus.aksapp.io
```

## <a name="deploy-http-routing-portal"></a>Implementar o encaminhamento de HTTP: Portal

O suplemento de encaminhamento de aplicativo HTTP pode ser ativado através do portal do Azure ao implementar um cluster do AKS.

![Ativar a funcionalidade de encaminhamento de HTTP](media/http-routing/create.png)

Depois do cluster é implementado, navegue para o grupo de recursos do AKS criada automaticamente e selecione a zona DNS. Anote o nome da zona DNS. Este nome é necessária para implementar aplicações para o cluster do AKS.

![Obter o nome da zona DNS](media/http-routing/dns.png)

## <a name="use-http-routing"></a>Utilizar o encaminhamento de HTTP

A solução de encaminhamento de aplicações de HTTP apenas pode ser acionada em recursos de entrada que são anotados da seguinte forma:

```yaml
annotations:
  kubernetes.io/ingress.class: addon-http-application-routing
```

Crie um ficheiro denominado **exemplos-http-aplicação-routing.yaml** e copie o YAML seguinte. Na linha 43, atualizar `<CLUSTER_SPECIFIC_DNS_ZONE>` com o nome da zona DNS recolhidos no passo anterior deste artigo.


```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: party-clippy
spec:
  template:
    metadata:
      labels:
        app: party-clippy
    spec:
      containers:
      - image: r.j3ss.co/party-clippy
        name: party-clippy
        tty: true
        command: ["party-clippy"]
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: party-clippy
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: party-clippy
  type: ClusterIP
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: party-clippy
  annotations:
    kubernetes.io/ingress.class: addon-http-application-routing
spec:
  rules:
  - host: party-clippy.<CLUSTER_SPECIFIC_DNS_ZONE>
    http:
      paths:
      - backend:
          serviceName: party-clippy
          servicePort: 80
        path: /
```

Utilize o [aplicam-se de kubectl] [ kubectl-apply] comando para criar os recursos.

```bash
$ kubectl apply -f samples-http-application-routing.yaml

deployment "party-clippy" created
service "party-clippy" created
ingress "party-clippy" created
```

Utilizar cURL ou um browser para navegar para o nome do anfitrião especificado na seção de anfitrião do ficheiro de exemplos-http-aplicação-routing.yaml. A aplicação pode demorar até um minuto antes de ser disponibilizado através da internet.

```bash
$ curl party-clippy.471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io

 _________________________________
/ It looks like you're building a \
\ microservice.                   /
 ---------------------------------
 \
  \
     __
    /  \
    |  |
    @  @
    |  |
    || |/
    || ||
    |\_/|
    \___/

```

## <a name="remove-http-routing"></a>Remover o encaminhamento de HTTP

A solução de roteamento de HTTP pode ser removida com a CLI do Azure. Para fazer isso, execute o seguinte comando, substituindo o AKS cluster e o recurso nome do grupo.

```azurecli
az aks disable-addons --addons http_application_routing --name myAKSCluster --resource-group myResourceGroup --no-wait
```

## <a name="troubleshoot"></a>Resolução de problemas

Utilize o [registos de kubectl] [ kubectl-logs] comando para ver os registos da aplicação para a aplicação de DNS externo. Os registos devem confirmar que um registo A e TXT DNS foram criados com êxito.

```
$ kubectl logs -f deploy/addon-http-application-routing-external-dns -n kube-system

time="2018-04-26T20:36:19Z" level=info msg="Updating A record named 'party-clippy' to '52.242.28.189' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
time="2018-04-26T20:36:21Z" level=info msg="Updating TXT record named 'party-clippy' to '"heritage=external-dns,external-dns/owner=default"' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
```

Estes registos também podem ser vistos no recurso de zona DNS no portal do Azure.

![Obter os registos DNS](media/http-routing/clippy.png)

Utilize o [registos de kubectl] [ kubectl-logs] comando para ver os registos da aplicação para o controlador de entrada do Nginx. Os registos devem confirmar o `CREATE` de um recurso de entrada e o recarregamento do controlador. Todas as atividades HTTP é registada.

```bash
$ kubectl logs -f deploy/addon-http-application-routing-nginx-ingress-controller -n kube-system

-------------------------------------------------------------------------------
NGINX Ingress controller
  Release:    0.13.0
  Build:      git-4bc943a
  Repository: https://github.com/kubernetes/ingress-nginx
-------------------------------------------------------------------------------

I0426 20:30:12.212936       9 flags.go:162] Watching for ingress class: addon-http-application-routing
W0426 20:30:12.213041       9 flags.go:165] only Ingress with class "addon-http-application-routing" will be processed by this ingress controller
W0426 20:30:12.213505       9 client_config.go:533] Neither --kubeconfig nor --master was specified.  Using the inClusterConfig.  This might not work.
I0426 20:30:12.213752       9 main.go:181] Creating API client for https://10.0.0.1:443
I0426 20:30:12.287928       9 main.go:225] Running in Kubernetes Cluster version v1.8 (v1.8.11) - git (clean) commit 1df6a8381669a6c753f79cb31ca2e3d57ee7c8a3 - platform linux/amd64
I0426 20:30:12.290988       9 main.go:84] validated kube-system/addon-http-application-routing-default-http-backend as the default backend
I0426 20:30:12.294314       9 main.go:105] service kube-system/addon-http-application-routing-nginx-ingress validated as source of Ingress status
I0426 20:30:12.426443       9 stat_collector.go:77] starting new nginx stats collector for Ingress controller running in namespace  (class addon-http-application-routing)
I0426 20:30:12.426509       9 stat_collector.go:78] collector extracting information from port 18080
I0426 20:30:12.448779       9 nginx.go:281] starting Ingress controller
I0426 20:30:12.463585       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-nginx-configuration", UID:"2588536c-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"559", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-nginx-configuration
I0426 20:30:12.466945       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-tcp-services", UID:"258ca065-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"561", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-tcp-services
I0426 20:30:12.467053       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-udp-services", UID:"259023bc-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"562", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-udp-services
I0426 20:30:13.649195       9 nginx.go:302] starting NGINX process...
I0426 20:30:13.649347       9 leaderelection.go:175] attempting to acquire leader lease  kube-system/ingress-controller-leader-addon-http-application-routing...
I0426 20:30:13.649776       9 controller.go:170] backend reload required
I0426 20:30:13.649800       9 stat_collector.go:34] changing prometheus collector from  to default
I0426 20:30:13.662191       9 leaderelection.go:184] successfully acquired lease kube-system/ingress-controller-leader-addon-http-application-routing
I0426 20:30:13.662292       9 status.go:196] new leader elected: addon-http-application-routing-nginx-ingress-controller-5cxntd6
I0426 20:30:13.763362       9 controller.go:179] ingress backend successfully reloaded...
I0426 21:51:55.249327       9 event.go:218] Event(v1.ObjectReference{Kind:"Ingress", Namespace:"default", Name:"party-clippy", UID:"092c9599-499c-11e8-a5e1-0a58ac1f0ef2", APIVersion:"extensions", ResourceVersion:"7346", FieldPath:""}): type: 'Normal' reason: 'CREATE' Ingress default/party-clippy
W0426 21:51:57.908771       9 controller.go:775] service default/party-clippy does not have any active endpoints
I0426 21:51:57.908951       9 controller.go:170] backend reload required
I0426 21:51:58.042932       9 controller.go:179] ingress backend successfully reloaded...
167.220.24.46 - [167.220.24.46] - - [26/Apr/2018:21:53:20 +0000] "GET / HTTP/1.1" 200 234 "" "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)" 197 0.001 [default-party-clippy-80] 10.244.0.13:8080 234 0.004 200
```

## <a name="clean-up"></a>Limpeza

Remova os objetos de Kubernetes associados criados neste artigo.

```bash
$ kubectl delete -f samples-http-application-routing.yaml

deployment "party-clippy" deleted
service "party-clippy" deleted
ingress "party-clippy" deleted
```

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre como instalar um controlador de entrada HTTPS seguro no AKS, consulte [HTTPS de entrada no Azure Kubernetes Service (AKS)][ingress-https].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-show]: /cli/azure/aks?view=azure-cli-latest#az-aks-show
[ingress-https]: ./ingress-tls.md
[az-aks-enable-addons]: /cli/azure/aks#az-aks-enable-addons


<!-- LINKS - external -->
[dns-pricing]: https://azure.microsoft.com/pricing/details/dns/
[external-dns]: https://github.com/kubernetes-incubator/external-dns
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[ingress]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[ingress-resource]: https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource
