# local-path

delete standard storageclasse or remove it as default:
```bash
kubectl delete storageclasses standard

kubectl patch storageclass standard \
  --patch='{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"false"}}}'
```

install local-path storageclasse:
```bash
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml

kubectl patch storageclass local-path \
  --patch='{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

doesn't work:
```bash
kubectl patch storageclasses local-path \
  --patch='{"volumeBindingMode": "Immediate"}'
```

### Test PVC Setup

setup pvc and pod:
```bash
curl -sL https://k8s.io/examples/pods/storage/pv-claim.yaml \
  | sed 's/manual/local-path/' | kubectl apply -f -

kubectl apply -f https://k8s.io/examples/pods/storage/pv-pod.yaml
```

test pvc with pod:
```bash
kubectl exec task-pv-pod -- bash -c "echo 'Hello World' > /usr/share/nginx/html/index.html"

kubectl exec task-pv-pod -- cat /usr/share/nginx/html/index.html
```

Delete the pod and recreate it again: 
```bash
kubectl delete po/task-pv-pod
```

Once checked that file still exist delete everything:
```bash
kubectl delete po/task-pv-pod pvc/task-pv-claim
```
