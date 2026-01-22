 docker run -d --name=maxkb --restart=always -p 8080:8080 -v ~/.maxkb:/opt/maxkb 1panel/maxkb

  说明：
   - 端口映射：8080:8080 (确保本地 8080 端口未被占用)
   - 数据挂载：~/.maxkb:/opt/maxkb (数据将持久化保存在用户目录下的 .maxkb 文件夹)
   - 默认账号：admin
   - 默认密码：MaxKB@123.. // webkubor@163.com