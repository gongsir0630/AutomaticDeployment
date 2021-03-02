### AutoMaticDeployment---自动部署

<hr/>

#### 项目简介

基于GitHub提供的WebHooks勾子实现静态项目自动部署，流程如下：

- 本地编码
- 提交代码到GitHub，触发WebHooks
- 请求url
- 执行脚本，自动部署

#### 技术栈
- Java
- SpringBoot

#### 使用说明

1、克隆代码到本地或者服务器，编辑`src/main/resources`下的`application.properties`文件，修改端口号，然后使用maven命令编译打包:
```shell
mvn clean install -Dmaven.test.skip=true
```

2、后台运行项目：  
```shell
nohup java -jar AutomaticDeployment.jar > AutomaticDeployment.out 2>&1 &
```

3、访问`http://{your_website}:{port}/hello`,显示“hello”表示部署成功

4、配置触发WebHooks（以GitHub为例）。在项目的settings页面，点击左侧webhook选项，点击new新建webhooks，填写url，并在url拼接需要执行的shell脚本的位置：
![图片](https://cdn.gongsir.club/blog/20200402/it9QRShppxXu.png?imageslim)

配置url：`http://{your_website}:{port}/linux/exec?cmd={cmd}&secret={secret}`

参数说明：其中`cmd`表示需要执行的shell脚本的位置：/root/xxx/update.sh：

```bash
echo "========== 开始执行home.sh脚本 =========="
echo "进入blog所在目录"
cd /usr/local/nginx/html/blog
## 拉取最新代码
echo "从github拉取最新代码"
git pull
## 重启nginx
echo "重启nginx"
../../sbin/nginx -s reload
## 打印提示语句
echo "========== 网站更新完成 =========="
```

secret表示自定义密码，这里需要和代码一致（默认：gongsir0630），以此验证用户身份，如需修改，请编辑`src/main/java/club.gongsir.linux.controller.DemoController`中exec方法的secret字符串：
![图片](http://cdn.gongsir.club/blog/20200402/3cHCd4NlLvvN.png?imageslim)

5、保存webhooks配置即可，这样当仓库的代码更新之后，就会自动发post请求以触发shell脚本的执行。

6、执行成功返回：  
![图片](http://cdn.gongsir.club/blog/20200402/5q8TSaYGcooC.png?imageslim)

#### 后期更新
使用github的secret签名完成用户身份验证

#### 联系我
个人主页（含联系方式）：https://www.gongsir.club
