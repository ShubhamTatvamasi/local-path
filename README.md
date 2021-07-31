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

kubectl patch storageclasses local-path \
  --patch='{"volumeBindingMode": "Immediate"}'

kubectl patch storageclass local-path \
  --patch='{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```



