---
title: Configurar a entrada com o cluster do Azure Kubernetes Service (AKS)
description: Instalar e configurar um controlador de entrada do NGINX num cluster do Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/25/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bd223e9eebac495d7336c618b831528505c30959
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857400"
---
# <a name="https-ingress-on-azure-kubernetes-service-aks"></a>HTTPS de entrada no serviço Kubernetes do Azure (AKS)

Um controlador de entrada é uma parte do software que fornece o proxy inverso, encaminhamento de tráfego configurável e terminação de TLS para serviços do Kubernetes. Recursos de entrada do Kubernetes são utilizados para configurar as regras de entrada e as rotas para serviços individuais do Kubernetes. Utilizar um controlador de entrada e de regras de entrada, um único endereço externo pode servir-se para encaminhar o tráfego para vários serviços num cluster do Kubernetes.

Este documento explica como através de uma implementação de exemplo dos [controlador de entradas NGINX] [ nginx-ingress] num cluster do Azure Kubernetes Service (AKS). Além disso, o [Gestor de cert] [ cert-manager] projeto é utilizado para gerar e configurar automaticamente [vamos encriptar] [ lets-encrypt] certificados. Por fim, vários aplicativos são executados no cluster do AKS, cada um deles está acessível através de um único endereço.

## <a name="install-an-ingress-controller"></a>Instalar um controlador de entrada

Utilize o Helm para instalar o controlador de entrada do NGINX. Consulte o controlador de entrada do NGINX [documentação] [ nginx-ingress] para informações de implementação detalhados.

Este exemplo instala o controlador no `kube-system` espaço de nomes, isso pode ser modificado para um espaço de nomes da sua preferência. Se o cluster do AKS não RBAC ativado, adicione `--set rbac.create=false` ao comando. Para obter mais informações, consulte a [gráfico de entrada do nginx][nginx-ingress].

```bash
helm install stable/nginx-ingress --namespace kube-system
```

Durante a instalação, um endereço IP público do Azure é criado para o controlador de entrada. Para obter o endereço IP público, utilize o comando de serviço do kubectl get. Pode demorar algum tempo para o endereço IP a ser atribuída ao serviço.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Uma vez que não existem regras de entrada foram criadas, se navegar até o endereço IP público, são encaminhados para a entrada controladores página predefinida do NGINX 404.

![Back-end do padrão NGINX](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>Configurar o nome DNS

Uma vez que forem utilizados certificados HTTPS, terá de configurar um nome FQDN para o endereço IP de controladores de entrada. Neste exemplo, um FQDN do Azure é criado com a CLI do Azure. Atualize o script com o endereço IP do controlador de entrada e o nome que pretende utilizar no FQDN.

```bash
#!/bin/bash

# Public IP address
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with dns name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

O controlador de entrada deve agora estar acessível através do FQDN.

## <a name="install-cert-manager"></a>Instalar o Gestor de certificado

O controlador de entrada do NGINX oferece suporte a terminação de TLS. Embora haja várias formas de obter e configurar certificados para HTTPS, este documento demonstra como utilizar [Gestor de cert][cert-manager], que fornece automática [permite encriptar] [ lets-encrypt] funcionalidade de geração e gestão de certificados.

Para instalar o controlador de Gestor de certificado, utilize o seguinte comando de instalação do Helm.

```bash
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-prod --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Se o cluster não for RBAC ativado, utilize este comando.

```bash
helm install stable/cert-manager \
  --set ingressShim.defaultIssuerName=letsencrypt-prod \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set rbac.create=false \
  --set serviceAccount.create=false
```

Para obter mais informações sobre a configuração do Gestor de certificados, consulte a [cert-Gestor projeto][cert-manager].

## <a name="create-ca-cluster-issuer"></a>Criar o emissor de cluster de AC

Antes dos certificados podem ser emitidos, Gestor de certificados requer um [emissor] [ cert-manager-issuer] ou [ClusterIssuer] [ cert-manager-cluster-issuer] recursos. Os recursos são idênticos em funcionalidade entretanto `Issuer` funciona num único espaço de nomes onde `ClusterIssuer` funciona em todos os espaços de nomes. Para obter mais informações, consulte a [cert-manager emissor] [ cert-manager-issuer] documentação.

Crie um emissor de cluster utilizando o manifesto do seguinte. Atualize o endereço de e-mail com um endereço válido da sua organização.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-prod
    http01: {}
```

## <a name="create-certificate-object"></a>Criar o objeto de certificado

Em seguida, um recurso de certificado tem de ser criado. O recurso de certificado define o certificado X.509 desejado. Para obter mais informações, consulte, [cert-Gestor certificados][cert-manager-certificates].

Crie o recurso de certificado com o manifesto do seguinte.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
```

## <a name="run-application"></a>Executar aplicação

Neste momento, um controlador de entrada e de uma solução de gestão de certificado foram configuradas. Agora, execute alguns aplicativos no seu cluster do AKS.

Neste exemplo, o Helm é utilizada para executar várias instâncias de um aplicativo do mundo Olá simples.

Antes de executar a aplicação, adicione o repositório Helm de exemplos do Azure no sistema de desenvolvimento.

```bash
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Execute o gráfico do AKS Olá mundo com o seguinte comando:

```bash
helm install azure-samples/aks-helloworld
```

Agora a instalar uma segunda instância da aplicação hello world.

Para a segunda instância, especifique um novo título para que as duas aplicações são visualmente distintas. Também tem de especificar um nome de serviço único. Estas configurações podem ser vistas no comando seguinte.

```bash
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Criar rota de entrada

Ambas as aplicações estão agora em execução no seu cluster do Kubernetes, no entanto foram configuradas com um serviço do tipo `ClusterIP`. Como tal, os aplicativos não são acessíveis a partir da internet. Para que fiquem disponíveis, crie um recurso de entrada do Kubernetes. O recurso de entrada configura as regras que encaminham o tráfego para uma das duas aplicações.

Criar um nome de ficheiro `hello-world-ingress.yaml` e copie o YAML seguinte.

Observe que o tráfego para o endereço `https://demo-aks-ingress.eastus.cloudapp.azure.com/` é encaminhado para o serviço com o nome `aks-helloworld`. O tráfego para o endereço `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` é encaminhado para o `ingress-demo` serviço.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-prod
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Criar o recurso de entrada com o `kubectl apply` comando.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Testar a configuração de entrada

Navegue para o FQDN do seu controlador de entrada do Kubernetes, deverá ver a aplicação hello world.

![Um exemplo de aplicativo](media/ingress/app-one.png)

Agora, navegue para o FQDN do controlador de entrada com o `/hello-world-two` caminho, deve ver a aplicação hello world com o título personalizado.

![Exemplo de aplicativo dois](media/ingress/app-two.png)

Observe também que a conexão seja criptografada e que é utilizado um certificado emitido por vamos encriptar.

![Permite encriptar o certificado](media/ingress/certificate.png)

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre o software demonstrado neste documento.

- [Helm CLI][helm-cli]
- [Controlador de entradas do NGINX][nginx-ingress]
- [Gestor de certificado][cert-manager]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
