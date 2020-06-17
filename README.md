# 用helm安装minio

## 需要先拉取镜像

```shell
docker pull minio/minio 
```

再用helm安装

```shell
helm install stable/minio --version 5.0.30 --generate-name
```

得到

```

NAME: minio-1592405296
LAST DEPLOYED: Wed Jun 17 22:48:27 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Minio can be accessed via port 9000 on the following DNS name from within your cluster:
minio-1592405296.default.svc.cluster.local

To access Minio from localhost, run the below commands:

  1. export POD_NAME=$(kubectl get pods --namespace default -l "release=minio-1592405296" -o jsonpath="{.items[0].metadata.name}")

  2. kubectl port-forward $POD_NAME 9000 --namespace default

Read more about port forwarding here: http://kubernetes.io/docs/user-guide/kubectl/kubectl_port-forward/

You can now access Minio server on http://localhost:9000. Follow the below steps to connect to Minio server with mc client:

  1. Download the Minio mc client - https://docs.minio.io/docs/minio-client-quickstart-guide

  2. mc config host add minio-1592405296-local http://localhost:9000 AKIAIOSFODNN7EXAMPLE wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY S3v4

  3. mc ls minio-1592405296-local

Alternately, you can use your browser or the Minio SDK to access the server - https://docs.minio.io/categories/17
```

因此执行

```shell
export POD_NAME=$(kubectl get pods --namespace default -l "release=minio-1592405296" -o jsonpath="{.items[0].metadata.name}")

kubectl port-forward $POD_NAME 9000 --namespace default
```

打开网页 http://127.0.0.1:8080/

![login](https://raw.githubusercontent.com/zknyy/helm-minio-install/master/login.png)

## 获取登陆key

执行

```shell
helm ls
```

获得

```shell
NAME            	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART                 	APP VERSION  
minio-1592405296	default  	1       	2020-06-17 22:48:27.965039 +0800 CST	deployed	minio-5.0.30          	master  
```

执行

```shell
helm get all minio-1592405296
```

得到

```shell
accessKey: AKIAIOSFODNN7EXAMPLE
secretKey: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

> 这两个key按字母顺序排序，所以离的比较远。
>
> 也可以根据最后的信息推断出来

```shell
  2. mc config host add minio-1592405296-local http://localhost:9000 AKIAIOSFODNN7EXAMPLE wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY S3v4
```

填入Key然后登陆

![blank-list](https://raw.githubusercontent.com/zknyy/helm-minio-install/master/blank-list.png)

## 安装客户端mc

打开网页 https://docs.minio.io/docs/minio-client-quickstart-guide

执行

```shell
brew install minio/stable/mc
```

然后执行

```shell
mc config host add minio-1592405296-local http://localhost:9000 AKIAIOSFODNN7EXAMPLE wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

得到

```shell
Added `minio-1592405296-local` successfully.
```

