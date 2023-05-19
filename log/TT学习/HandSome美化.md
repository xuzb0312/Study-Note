### 一、后台登陆界面美化

**截图**
![image-20220825134954582](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220825134954582.png)
**下载**

**方法**
第一步：用压缩包内的`login.php`文件替换掉`/admin/login.php`文件
第二步：将压缩包内的`style`文件夹上传到`/admin/`文件夹下
第三步：修改`login.php`第35行，把“Xuzb's Blog”替换成自己的信息

### 二、隐藏左侧边栏下方的RSS订阅按钮

![image-20220824092812452](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220824092812452.png)

### 三、去除左侧边栏导航与组成小字

![image-20220824092933000](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220824092933000.png)

![image-20220824092952962](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220824092952962.png)



### 四、添加工具栏

![image-20220824093555371](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220824093555371.png)

**默认关闭下拉框并在新页面打开链接**

```html
<!--左侧下拉框-->
<li>
    <a class="auto">
        <span class="pull-right text-muted">
            <i class="fontello icon-fw fontello-angle-right text"></i>
            <i class="fontello icon-fw fontello-angle-down text-active"></i>
        </span>
        <i class="glyphicon glyphicon-link"></i>
        <span>工具</span></a>
    <ul class="nav nav-sub dk">
        <li class="nav-sub-header">
            <a data-no-instant="">
                <span>工具</span></a>
        </li>
        <!--网站-->
        <li>
            <a href="https://sunpma.com/other/douyin" class="auto" target="_blank">
                <i class="glyphicon glyphicon-link"></i>
                <span>抖音解析下载</span></a>
        </li>
        <li>
            <a href="https://sunpma.com/other/musicss" class="auto" target="_blank">
                <i class="glyphicon glyphicon-link"></i>
                <span>音乐解析下载</span></a>
        </li>
    </ul>
</li>
<!--下拉框结束-->
```

![image-20220824094559617](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220824094559617.png)

### 五、顶部小组件

```html
<li class="music-box hidden-xs hidden-sm" id="handsome_global_player">
    <p class="handsomePlayer-tip-loading"><span></span> <span></span> <span></span> <span></span><span></span></p>
    <div class="handsome_aplayer player-global" data-preload="false" data-autoplay="" data-listmaxheight="200px" data-order="list" data-theme="#8ea9a7" data-listfolded="true" data-fix_position="true">
        <div class="handsome_aplayer_music" data-id="888233349" data-server="tencent" data-type="playlist" data-auth="8f6439c4bf5ba10c5ed555713d05c628"></div>
    </div>
</li>
```

![image-20220824102251404](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220824102251404.png)

![image-20220824102312528](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220824102312528.png)

暂时这样。接下来上传文章

### 六、代码插件

- Highlight插件，智能实现代码高亮
- Markdown 编辑器 [Editor.md](https://pandao.github.io/editor.md/) for Typecho

插件的使用：

![image-20220824134156876](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220824134156876.png)

将插件放入此目录下，之后去网站后台启用插件即可

全部放在服务器家目录下。

### 七、去除顶部博客名称

**修改`/usr/themes/handsome/index.php`文件，位于公告位置下方**
**删除以下代码**

![image-20220825135805690](http://1.117.169.135/usr/uploads/2022/08/HandSome美化/image-20220825135805690.png)

### 八、修复搜索框按钮

在解决访问速度慢问题之后，出现搜索框按钮不能使用的情况。

解决方法：

**打开`/usr/themes/handsome/component/headnav.php`文件**
**搜索`search_submit`然后将整行替换成如下代码**



```php
<span id="search_submit" class="transparent input-group-btn" onclick=jumpForSearch(search_input.value)>
```

**然后在文件的最后面添加如下代码后保存即可**

```php
<!--/开始修复搜索按钮-->
<script type="text/javascript">
function jumpForSearch(search_ct){
  if(search_ct.length>0){
    $.pjax({ 
    url: "https://"+document.domain+'/search/'+search_ct, 
    container: '#content',
    fragment: '#content',
    timeout: 8000
    });
  }
}
</script>
<!--/修复搜索按钮结束-->
```

