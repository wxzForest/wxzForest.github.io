# 搭建hexo


# 步骤

1. 安装hexo

   ```html
   npm install -g hexo-cli
   ```

2. 初始化hexo

   ```html
   #会生成一个myblog文件夹，文件夹内生成相关配置文件
   hexo init myblog
   
   #下载node_modules
   npm install
   
   #新建完成后，指定文件夹目录下有：
   	node_modules: 依赖包
   	public：存放生成的页面
   	scaffolds：生成文章的一些模板
   	source：用来存放你的文章
   	themes：主题
   	** _config.yml: 博客的配置文件**
   ```

3. 启动hexo服务

   ```html
   hexo g
   hexo server
   
   #命令说明
       hexo n "我的博客" == hexo new "我的博客" #新建文章
       hexo g == hexo generate #生成
       hexo s == hexo server #启动服务预览
       hexo d == hexo deploy #部署
       hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
       hexo server -s #静态模式
       hexo server -p 5000 #更改端口
       hexo server -i 192.168.1.1 #自定义 IP
       hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令
   ```

   

# 踩的坑


   ```html
   wxz@DESKTOP-RFS74RI  ~\Desktop\wxzBlog [14:12]
   ❯ hexo g
   ERROR Cannot find module 'C:\Users\wxz\Desktop\wxzBlog\node_modules\bluebird\js\release\bluebird.js'. Please verify that
   ERROR Local hexo loading failed in ~\Desktop\wxzBlog
   ERROR Try running: 'rm -rf node_modules && npm install --force'
   ```
 1. hexo依赖包下载失败导致的报错，删除node_modules、package-lock.json然后重新执行 npm install。


