Вроде как работающий конфиг:
```shell
minikube start \
--cpus=4 \
--memory=8g \
--cni=flannel \
--kubernetes-version="v1.19.0" \
--driver=hyperkit
```
**ВНИМАНИЕ** миникуб не включается с недефолтным драйвером если включена VPN!

