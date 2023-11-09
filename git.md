### git

创建分支

```
git checkout -b gh-pages
```

初始化仓库

```
git init
```

添加文件到暂存区

```
git add .
```

将暂存区内容添加到仓库中

```
git commit 
```

连接远端仓库

```
git remote add origin https://github.com/yeshp-gw/gitbook.git
```

本地推送到远端

```
git push -u origin master
```

------

### gitbook部署在VPS

```
cd /var/www/html/     
```

```
mkdir gitbook
```

```
cd gitbook          
```

```
git init
git remote add origin "githu链接"
git pull origin gh-pages                ##拉取html
```

```
vim /etc/nginx/sites-available/default    ##设置nginx
```

```
root  /var/www/html/gitbook；   ## html后面添加gitbook
```

> [!Note|style:flat]

<script src='//player.polyv.net/script/player.js'></script>
<div id='plv_8e43201c92da68fbd02acd58a641acad_8'></div>
<script>
var player = polyvPlayer({
  'wrap':'#plv_8e43201c92da68fbd02acd58a641acad_8',
  'width':'600',
  'height':'338',
  'vid': '8e43201c92da68fbd02acd58a641acad_8',
  'playsafe': '' // 播放加密视频的凭证, 取值参考文档: https://help.polyv.net/index.html#/vod/api/playsafe/token/create_token 
});
</script>

