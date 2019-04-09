## 个人搭建博客说明

#### *hexo*基本命令

```bash
# 安装与升级
npm install hexo -g
npm update hexp -g

# hexo初始化
hexo init

# 创建页面categories与tags
hexo new page categories  
hexo new page tags

# 创建新文章
hexo new "title"

# 发布部署，生成静态网页，开始部署
hexo g  # or generate
hexo g --watch # 监视文件变动
hexo d  # or deploy

# 启动服务预览，监视文件变动并自发更新，无须重启
hexo s # or server
hexo s -s # 静态模式
hexo s -p 5000 # 更改端口
hexo s -i 192.168.1.1 # 自定义IP

# 清除缓存
hexo clean

# list type=[page,post,route,tag,category]
hexo list type

# 查看hexo信息
hexo version

# hexo选项
hexo --safe # 安全模式，不会载入插件和脚本
hexo --debug # 调试模式，输出debug.log
hexo --silent # 简介模式，隐藏终端信息
hexo --config custom.yml # 自定义配置文件路径，替代_config.yml
hexo --draft # 显示草稿
```


#### 参考文献
> - [Hexo NexT主题中集成gitalk评论系统](https://asdfv1929.github.io/2018/01/20/gitalk/)
> - [Hexo+Next个人博客主题优化](https://www.jianshu.com/p/efbeddc5eb19)
> - [Next官方文档](http://theme-next.iissnan.com/)
