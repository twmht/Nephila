### Docker

1. 要先到 console enable 一個 permission [Enable and disable the service  |  Artifact Registry documentation  |  Google Cloud](https://cloud.google.com/artifact-registry/docs/enable-service)
2. 之後照著官方流程下載 gcloud
3. 設定 docker 
```bash
gcloud auth configure-docker
```
4. 在自己的 local 上 build 自己要的 image
5. 然後下 tag
```bash
 docker tag eed53890f6dd asia.gcr.io/aic-addvalue-dev/asr-streaming-server:1.1.0
```
6.  push image
```
 docker push asia.gcr.io/aic-addvalue-dev/asr-streaming-server:1.1.0
```
7. 修改 /etc/docker/daemon.json
```
{
    "runtimes": {
        "nvidia": {
            "args": [],
            "path": "nvidia-container-runtime"
        }
    },
    "default-runtime": "nvidia"
}
```
9. sudo docker service restart
8. 給他跑起來
```bash
`docker run -d -p 43007:43007 --name asr-streaming-server-container --restart always asia.gcr.io/aic-addvalue-dev/asr-streaming-server:1.1.0`
```
9. GCP 上整個流程
```
docker pull asia.gcr.io/aic-addvalue-dev/asr-streaming-server:latest
docker pull asia.gcr.io/aic-addvalue-dev/asr-offline-server:latest
docker run -d -p 43007:43007 --name asr-streaming-server-container --restart always asia.gcr.io/aic-addvalue-dev/asr-streaming-server:latest
docker run -d -p 8000:8000 --name asr-offline-server-container --restart always asia.gcr.io/aic-addvalue-dev/asr-offline-server:latest
```
10. GCP registry: [Digests for asr-streaming-server – Artifact Registry – aic-addvalue-dev – Google Cloud console](https://console.cloud.google.com/artifacts/docker/aic-addvalue-dev/asia/asia.gcr.io/asr-streaming-server?invt=AbkrVA&project=aic-addvalue-dev)