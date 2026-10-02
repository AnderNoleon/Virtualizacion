# Tarea 05 - Servicios en Kubernetes

## Descripción
Deployment de Nginx (3 réplicas) expuesto mediante un Service tipo **NodePort**
en el namespace `hw-05`. Toda la configuración está definida como IaC en la carpeta `k8s/`.

## Archivos
- `k8s/namespace.yaml`: namespace `hw-05`
- `k8s/deployment.yaml`: deployment de Nginx (puerto 80)
- `k8s/service.yaml`: service NodePort (80 → 30080)

## Cómo ejecutar
```bash
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl port-forward svc/nginx-service 8080:80 -n hw-05
```
Luego abrir `http://localhost:8080` en el navegador.

## Captura del navegador
![Nginx en el navegador](images/captura1.png)

## Salida de `kubectl get svc -n hw-05`
```
NAME            TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort   10.107.89.20   <none>        80:30080/TCP   3s
```