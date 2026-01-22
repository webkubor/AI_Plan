## 数据库mongodb

## 安装

```bat
brew install mongodb
sudo mkdir -p /data/db
sudo chown -R $USER /data/db
mongod
```

## 启动

```bash
brew services start mongodb
mongod
```

## 停止

```git
brew services stop mongodb
```

## 查看进程

```bash
ps -aef | grep mongo
```

## 检查版本

```bash
mongod --version
```

## 实战依赖

再node中,比如egg开发框架,我们通常会用moogoose作为查询工具

数据库配置:

```javascript
exports.mongoose ={
  url:'mongodb://locathost:27017/webkubor',
  options:{
    useunifiedtopology:true
  }
};
```

实例:

```javascript
const res = await ctx.model.GoodsZone.find({ products: { $elemMatch: { goods_zone_id: ctx.params.id, category: ctx.query.category } } });
```