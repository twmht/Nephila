* 使用以下命令清理无标签的 `<none>` 镜像: docker image prune -f


## Docker 節省硬碟空間的作法

在使用 Docker 時，隨著容器和映像的增長，硬碟空間的使用量可能會迅速增加。以下是一些有效的策略來節省 Docker 的硬碟空間：

**1. 使用 `docker system prune` 命令**

這個命令可以清理所有未使用的資源，包括停止的容器、未使用的映像和網路。執行此命令可以有效釋放大量空間：

```bash
docker system prune
```

如果希望更徹底地清理，可以使用 `-a` 參數，這樣會刪除所有未使用的映像：

```bash
docker system prune -a
```

**2. 刪除未使用的映像和容器**

- 使用 `docker image prune` 刪除未使用的映像：
  
```bash
docker image prune
```

- 使用 `docker container prune` 刪除所有停止的容器：

```bash
docker container prune
```

這些命令可以幫助清理不再需要的資源，從而釋放空間。

**3. 定期清理策略**

建立定期清理的計劃，例如使用 cron job 自動執行上述命令，以確保系統不會因為累積的未使用資源而佔用過多空間。

**4. 使用多階段構建**

在 Dockerfile 中使用多階段構建可以減少最終映像的大小。這種方法允許你在不同的階段中構建應用，並只將最終需要的檔案複製到最終映像中，從而減少不必要的檔案和依賴。

**5. 清理未使用的卷**

Docker 卷用於持久化數據，但如果不再使用，這些卷也會佔用空間。可以使用以下命令刪除未使用的卷：

```bash
docker volume prune
```

**6. 檢查和管理映像大小**

定期檢查映像的大小，並刪除不再需要的映像。可以使用以下命令查看所有映像及其大小：

```bash
docker images
```

**7. 使用 `.dockerignore` 文件**

在構建映像時，使用 `.dockerignore` 文件來排除不必要的檔案和目錄，這樣可以減少映像的大小。

**7. docker gpu dead**

https://github.com/HaveAGitGat/Tdarr/issues/666#issuecomment-1368171781
https://github.com/jellyfin/jellyfin/issues/9287
[Docker NVIDIA HW doesn't work after a few hours - Linux - Emby Community](https://emby.media/community/index.php?/topic/128480-docker-nvidia-hw-doesnt-work-after-a-few-hours/)