<h1> Kubernetes + microk8s </h1>

Kubernetes **version**
```{bash}
$ kubectl version
```

Kubernetes **config**
```{bash}
$ kubectl config view
```

Výpis **clusterů**
```{bash}
$ kubectl get nodes
```

Vypíše **využití zdrojů** u clusteru (musí být povolený **metrics-server** pro microk8s, jinak bude chyba)
```{bash}
$ kubectl top nodes
```
 **Nasazení** souboru z yamlu

```{bash}
$ kubectl apply -f (. všechny soubory)
```

<h2> Namespace - NS </h2>

List **namespace** 

```{bash}
$ kubectl get ns /namespace
```

**Vytvoření** nového NS

```{bash}
$ kubectl create ns <název>
```

Možnost vytvoření nového **kontextu** pro jiný NS

```{bash}
$ kubectl config set-context --current --namespace=<název NS>   
```

**Výpis** všech NS - nutno doinstalovat

```{bash}
$ kubens   
```

**Přepnutí** na jiný NS

```{bash}
$ kubens <název>  
```

<h2> Pod </h2>

**Výpis** podu v daném NS 

```{bash}
$ kubectl get pod
```

**Delete** pod

```{bash}
$ kubectl delete pod <název>
```

**Výpis** podu pro určitý **NS**

```{bash}
$ kubectl get po --namespace/-n <název> 
```

**Výpis** podu pro určitý **NS v json**

```{bash}
$ kubectl get po -n <název> -o json 
```

**Vytvoření** nového podu  /v jiném NS

```{bash}
$ kubectl run <název nového podu> --image <název image> --port 80  /--namespace <název>
```
<h4>Štítky</h4>

Vypíše **pody se štítky**

```{bash}
$ kubectl get po --show-labels
```

**Filtrování** štítků pro pody podle velikosti (hodnoty)

```{bash}
$ kubectl get po --selector "color=blue,misohu/size in (small, big)"
```

**Mapování** portu na port 80

```{bash}
$ kubectl port-forward <název podu> 8080:80  
```

Dodatečné **přidání štítku**

```{bash}
$ kubectl label pod <název podu> "misohu/size=big" 
```

**Smazání** štítku

```{bash}
kubectl label pod <název podu> <štítek-> 
```

<h3> Debug </h3>

**Logy** z podu (-f v reálném čase)
```{bash}
$ kubectl logs -f <název podu>
```

Napojení **terminálu** u podu
```{bash}
$ kubectl exec -ti <název podu> -- bash
```

**Výpis** podu v yamlu/jsonu
```{bash}
$ kubectl get pod <název podu> -o yaml
```

<h2> Deployment (Deploy) </h2>

**Výpis** deploy

```{bash}
$ kubectl get deploy  
```
**Editace** pro novou aktulizaci 

```{bash}
$ kubectl edit deploy <název deploy> 
```

**Aktulizace** image na novou verzi

```{bash}
$ kubectl set image deploy <název deploy> <název image>=<název podu>:1.22 
```

**Změna** počtu replik

```{bash}
$ kubectl scale deploy <název> --replicas=2  
```

**Výpis** poslední aktulizace na EtCD

```{bash}
$ kubectl rollout history deploy <název>
```

**Vrátí** předchozí verzi deploymentu 

```{bash}
$ kubectl rollout undo deploy <název> 
```

<h2> ReplikaSet (rs) & Autoscaling (hpa)</h2>

**Výpis** replikací

```{bash}
$ kubectl get rs /replikaset 
```
**Smazání** konkrétní repliky

```{bash}
$ kubectl delete rs <název repliky>  
```

**Výpis** autoškálování 

```{bash}
$ kubectl get hpa 
```

**Vytvoření yaml** souboru pro autoškálování
```{bash}
$ kubectl get hpa <název hpa> -o yaml > hpa.yaml (název souboru) 
```

**Smazání** autoškálování, nikoliv deploymentu 
```{bash}
$ kubectl delete hpa <název>
```

**Nastavení auto škál.** při využití více jak **50%** cpu (potom je možné vygenerovat yaml)
```{bash}
$ kubectl autoscale deploy <název deploy> --cpu-percent=50 --min=1 --max=10
```
<h2> Service (svc) </h2>

**Výpis** services (služeb)
```{bash}
$ kubectl get svc 
```

**Vytvoření services** pro deploy
```{bash}
$ kubectl expose deploy 'název' --port 8080 --target-port 80
```

<h2> Config map (cm) & Secret (sc) </h2>

**Výpis** config maps
```{bash}
$ kubectl get cm (configmap) 
```

**Vytvoří** novou cm s uvedenými proměnnými
```{bash}
$ kubectl create cm <název cm> --from-literal <proměnná1=hodnota> --from-literal <proměnná2=hodnota> 
```
**Vytvoření** generického secretu s určením proměnné + připojení souboru s proměnnými popložkami

```{bash}
$ kubectl create secret generic <název secret> --from-literal username=<proměnná> --from-file <soubor> 
```

<h2> Microk8s </h2>

**Stažení** microk8s (poslední verze)
```{bash}
$ sudo snap install microk8s --classic  
```

**Status** microk8s 
```{bash}
$ sudo microk8s status  
```

Zkopírování **configu**  pro kubernretes 
```{bash}
$ microk8s config > ~/.kube/config/
```

**Povolení** jednotlivých funkcích v microk8s
```{bash}
$ microk8s enable dns rbac 
```
- metrics-server - výpis využitých zdrojů u clusteru (povolit pro každý cluster)
- metallb rozsah IP - pro Loadbalancer
- dns - pro využívání IP a komunikaci mezi služby
- storage 
- ingress - pro zobrazení více služeb pod jedním service