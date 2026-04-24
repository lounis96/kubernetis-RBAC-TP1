# TP Kubernetes - ServiceAccount

## 1. Création du dossier et du pod

```bash
mkdir TP
cd TP
gedit pod-default.yaml
kubectl apply -f pod-default.yaml
```

## 2. Vérification des pods

```bash
kubectl get po
```

Résultat :

```text
NAME            READY   STATUS             RESTARTS        AGE
alpine          0/1     CrashLoopBackOff   8 (4m30s ago)   3d3h
monitoring      1/1     Running            0               164m
pod-default     1/1     Running            0               10s
private-image   0/1     ImagePullBackOff   0               2d21h
proxy           1/1     Running            1 (28h ago)     43h
```

---

## 3. Accès au pod

```bash
kubectl exec -ti pod-default -- sh
```

Installation de curl :

```bash
apk add --update curl
```

---

## 4. Test API Kubernetes (sans token)

```bash
curl https://kubernetes/api/v1 --insecure
```

Résultat :

```json
{
  "status": "Failure",
  "message": "Unauthorized",
  "code": 401
}
```

---

## 5. Récupération du token

```bash
ls /run/secrets/kubernetes.io/serviceaccount/
cat /run/secrets/kubernetes.io/serviceaccount/token
```

---

## 6. Test API avec token

```bash
TOKEN=$(cat /run/secrets/kubernetes.io/serviceaccount/token)

curl -H "Authorization: Bearer $TOKEN" \
https://kubernetes/api/v1 --insecure
```

Résultat : accès aux ressources API ✅

---

## 7. Test accès aux pods

```bash
curl -H "Authorization: Bearer $TOKEN" \
https://kubernetes/api/v1/namespaces/default/pods/ --insecure
```

Résultat :

```json
{
  "status": "Failure",
  "message": "pods is forbidden",
  "code": 403
}
```

👉 Le serviceAccount **n’a pas les droits**.

---

## 8. Création d’un token manuel

```bash
kubectl create token default
```

---

## 9. Installation de jq (outil JSON)

```bash
sudo apt update
sudo apt install jq
```

---

## ⚠️ Erreurs rencontrées

* `Unauthorized (401)` → pas de token
* `Forbidden (403)` → pas de permissions RBAC
* `Could not resolve host` → DNS hors cluster
* `Filename too long` → mauvaise utilisation de variable TOKEN

---

## 🎯 Conclusion

* Kubernetes utilise un **ServiceAccount + token** pour l’authentification
* Mais il faut aussi des **permissions RBAC** pour accéder aux ressources
* Le pod peut contacter l’API Kubernetes via :

```bash
https://kubernetes/api/v1
```

---

## 💡 Remarque

Toujours utiliser des blocs :

* ```bash → commandes
  ```
* ```json → réponses API
  ```
* ```text → output terminal
  ```

Sinon Markdown casse tout 😄
