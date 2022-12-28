# Instalando o Loki no kubernetes
A ideia desse artigo é realizarmos a instalação do Loki em sua forma mais básica, caso queira realizar algumas brincadeiras a mais com helm siga em frente!


# Pré Requisitos
Certifique-se de ter tudo instalado, se não, não irá funcionar.

- [Helm](https://helm.sh/)
- [Kubernetes](https://kubernetes.io/pt-br/)  Pode ser em qualquer cloud, até mesmo o minikube.


# Implante Loki e Promtail em seu cluster
A instalação do loki pode ser realizada através do Helm de forma simplificado.

- Vamos adicionar o repositório no helm.

```
helm repo add loki https://grafana.github.io/loki/charts
```

-  Atualizar os indices
```
helm repo update
```

- Finalizamos a instalação executando o comando abaixo.
```
helm upgrade --install loki loki/loki-stack
```

Pronto! o Loki esta instalado em seu cluster, agora vamos integrar ele com o grafana 💙


# Personalizações com Helm
O helm nos permite realizar personalizações do deployment no momento da instalação. Então no momento que for realizar o comando **--install** adicione as personalizações se necessário.

```
helm upgrade --set ${chave}=${valor} --install loki loki/loki-stack 
```

<table border="1">
  <tr>
    <th>Chave</th>
    <th>Valor</th>
    <th>Para que serve</th>
  </tr>
  <tr>
    <td>--set loki.persistence.enabled</td>
    <td>true|false</td>
    <td>Habilita a persistência de disco</td>
  </tr>
  <tr>
    <td>--set loki.persistence.storageClassName</td>
    <td>default</td>
    <td>Define qual o storageClass do kubernetes a ser usado</td>
  </tr>
  <tr>
    <td>--set loki.persistence.size</td>
    <td>10Gi</td>
    <td>Define qual o tamanho do disco a ser criado</td>
  </tr>
  <tr>
    <td>--namespace</td>
    <td>default</td>
    <td>Define o namespace do kubernetes a ser instalado</td>
  </tr>
</table>

Só um exemplo para te dar uma mãozinha...

```
helm upgrade --set loki.persistence.enabled=true --set loki.persistence.storageClassName=ebs --set loki.persistence.size=30Gi --namespace default --install loki loki/loki-stack
```
