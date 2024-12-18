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