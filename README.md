# Assessment 06 – GitOps con Argo CD

Se instaló **Argo CD** en el clúster de Minikube de las actividades anteriores (con Traefik y MetalLB) para desplegar las 4 aplicaciones del namespace `parcial-aonhl` con el enfoque **GitOps**: el repositorio de GitHub (rama `assessment-06`) es la única fuente de verdad, y Argo CD mantiene el clúster sincronizado con él.

| Elemento | Valor |
|---|---|
| Namespace de Argo CD | `argocd-aonhl` |
| Instalación | Helm, chart `argo/argo-cd` (`argocd/values.yaml`) |
| Dominio | `argocd.midominio.local` |
| Exposición | IngressRoute de Traefik, con la IP del LoadBalancer asignada por MetalLB |
| Applications | `parcial-aonhl-app1` … `parcial-aonhl-app4` |
| Namespace destino | `parcial-aonhl` |

```
 GitHub (rama assessment-06) ──pull cada 60s──► Argo CD (argocd-aonhl) ──apply──► parcial-aonhl
                                                     ▲
 Navegador ──► argocd.midominio.local ──► Traefik (IP MetalLB) ──► argocd-server:80
```

## Estructura

```
argocd/
├── namespace.yaml                  # Namespace argocd-aonhl
├── values.yaml                     # values de Helm de Argo CD
├── repo-secret.example.yaml        # plantilla de credenciales (solo si el repo es privado)
├── traefik/
│   └── ingressroute-argocd.yaml    # exposición por Traefik
└── applications/
    ├── app1.yaml                   # Application de Argo CD por cada aplicación
    ├── app2.yaml
    ├── app3.yaml
    └── app4.yaml
apps/                               # manifiestos que Argo CD despliega en parcial-aonhl
├── nginx/nginx.yaml                # app1
├── apache/apache.yaml              # app2
├── hello/hello.yaml                # app3
└── whoami/whoami.yaml              # app4
screenshots/
```

## 0. Rama

```bash
git checkout main && git pull
git checkout -b assessment-06
git push -u origin assessment-06
```

La rama debe existir en GitHub antes de crear las Applications, porque Argo CD la usa como `targetRevision`.

## 1. Instalación de Argo CD

Se usa Helm porque el chart permite instalar Argo CD en un namespace con cualquier nombre. El `install.yaml` oficial tiene el namespace `argocd` fijo en sus ClusterRoleBindings.

```bash
kubectl apply -f argocd/namespace.yaml

helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
helm install argocd argo/argo-cd -n argocd-aonhl -f argocd/values.yaml

kubectl get pods -n argocd-aonhl
```

Configuración en `values.yaml`:
- `global.domain`: dominio con el que se accede a la interfaz.
- `configs.params.server.insecure: true`: Traefik atiende por HTTP (entrypoint `web`), por lo que `argocd-server` sirve sin TLS. Sin esto, el servidor redirige a HTTPS en bucle.
- `configs.cm.timeout.reconciliation: 60s`: Argo CD revisa el repositorio cada minuto en lugar de cada 3.
- Dex (SSO) y notifications deshabilitados, porque no se usan y así se ahorran recursos en Minikube.

Contraseña inicial del usuario `admin`:
```bash
kubectl -n argocd-aonhl get secret argocd-initial-admin-secret \
  -o jsonpath="{.data.password}" | base64 -d; echo
```

## 2. Exposición por Traefik y DNS

```bash
kubectl apply -f argocd/traefik/ingressroute-argocd.yaml
kubectl get svc -A | grep traefik      # EXTERNAL-IP asignada por MetalLB
```

En `/etc/hosts`:
```
<IP_LOADBALANCER_TRAEFIK>  argocd.midominio.local
```

Acceso: `http://argocd.midominio.local` con el usuario `admin`.

## 3. Conexión con el repositorio

- **Repositorio público:** no requiere configuración; Argo CD clona la URL indicada en cada Application.
- **Repositorio privado:** copiar `repo-secret.example.yaml` a `repo-secret.yaml`, colocar un Personal Access Token de GitHub con permiso de lectura y aplicarlo con `kubectl apply -f argocd/repo-secret.yaml`. El archivo con el token está en `.gitignore` y no se sube al repositorio.

La conexión se verifica en **Settings → Repositories**, donde el repositorio debe aparecer con estado *Successful*.

## 4. Creación de las Applications

Cada aplicación del namespace `parcial-aonhl` tiene su propia Application declarada en YAML (`argocd/applications/`):

| Campo | Valor |
|---|---|
| `source.repoURL` | URL del repositorio en GitHub |
| `source.targetRevision` | `assessment-06` |
| `source.path` | carpeta con los manifiestos de la aplicación |
| `destination.namespace` | `parcial-aonhl` |
| `syncPolicy.automated` | `prune: true`, `selfHeal: true` |
| `syncOptions` | `CreateNamespace=true` |

```bash
kubectl apply -f argocd/applications/
kubectl get applications -n argocd-aonhl
```

Con la sincronización automática, Argo CD aplica los manifiestos sin intervención manual. `prune` elimina del clúster los recursos que se borren del repositorio y `selfHeal` revierte los cambios hechos a mano en el clúster. Si los recursos ya existían de actividades anteriores, Argo CD los adopta y los administra desde ese momento.

## 5. Sincronización de un cambio

1. Estado inicial: las 4 Applications en **Synced** y **Healthy** (captura *antes*).
2. Cambio en el repositorio: se modificó `replicas` de `1` a `3` en el Deployment de la aplicación 1.
   ```bash
   git commit -am "feat(app1): aumentar réplicas de 1 a 3"
   git push
   ```
3. En un minuto como máximo (o al presionar **Refresh**), Argo CD detecta el nuevo commit y sincroniza automáticamente.
4. Verificación:
   ```bash
   kubectl get deploy -n parcial-aonhl
   ```
   El Deployment muestra `3/3`, y en la interfaz la Application vuelve a **Synced** y **Healthy** con el nuevo commit (captura *después*).

## Evidencias

| Evidencia | Archivo |
|---|---|
| Argo CD desde el navegador por dominio | `screenshots/argocd-login.png` |
| Repositorio conectado | `screenshots/argocd-repo.png` |
| Applications Synced/Healthy antes del cambio | `screenshots/apps-antes.png` |
| Applications Synced/Healthy después del cambio | `screenshots/apps-despues.png` |
| Detalle con 3 réplicas | `screenshots/app1-replicas.png` |
