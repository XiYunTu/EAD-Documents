## 通用视图动作驱动

### 前端事件
- one、batch、ever、download，link  

one是单条数据操作，batch是多条数据操作，ever是不包含任何参数的再次请求操作，download是用于动态下载文件，link用于下载静态文件（暂时只有这一个功能,地址放在描述中 0.0）。download中形成的动态文档率先存在MongoDB中，并在缓存中存放地址，待下载成功后删除缓存中的地址，但是MongoDB中的文档不会删除。  
**Download例子**  
使用驱动excelExportAll以Excel格式输出当前页面所有的信息，设置前端事件为download。  
link  
我们将文件“程序使用说明.txt”放在路径"ead-app>ead>apache-tomcat>webapps_pros>ROOT>WEB-INF>classes>static>templates"路径下，按要求设置后，即可点击下载。注意，txt格式默认在浏览器中自动展示。 

### 按条件更新  
仅用于更新修改选项，驱动选择“updatebysetting”,并且在动作参数中填入过滤条件，以实现更新前的过滤。  
此处需要注意的是为何不将验证放在前端，而是放在后端增加了服务器的压力。这是因为前段可能会人为修改参数以符合过滤条件，所以为了安全要放在后端。
### 附件下载  

本例中在收款清单里有上传文件的行为，可以在此处加入下载的行为。文件统一保存到MongoDB数据库中，在插入或者提取只需要提供路径和ID即可查找出来，在收款清单视图的文件属性的Format中添加如下代码:  
```
function(value, row){
  if (row.file && row.file != 'undefined') {
    var url = '/demo/api/project-project_info-pmo%E9%A1%B9%E7%9B%AE%E4%BF%A1%E6%81%AF/0156c091e69b8a8ae61a56bbb67c0426/collection_record/'  + row.record_id + '?_mode=attachment&bust=' + (new Date()).getTime();
    return '联单附件：<a href="' + url +  '" target="_blank">点击下载</a>';
  } else {
    return '未上传';
  }
}
```  
也可以是  
```
function(value, row){
  if (row.file && row.file != 'undefined') {
    var url = '/demo/api/project-project_info-pmo%E9%A1%B9%E7%9B%AE%E4%BF%A1%E6%81%AF/0156c091e69b8a8ae61a56bbb67c0426/collection_record/'  + row.record_id + '?_mode=attachment&bust=' + (new Date()).getTime();
    return '<img src="' + url +  '" target="_blank"/>';
  } else {
    return '未上传';
  }
}
```  
其中，URL指的是项目信息的详情页面，在CHROME的检查中取出来即可；row.record_id取的是收款信息一行的主键而非file字段的内容是因为；mode有两种：attachment和inline，前者所有都作为附件进行下载，如果模式为attachment，则即使展示的是图片，对图片进行“在新标签页中打开”时，也是作为附件下载，inline方式为内部嵌入方式，即使是点击下载样式，也是在新标签页进行图片展示而不下载。  
通过URL的row.record_id找到某行数据，然后系统会**自动**去查找这一行的file字段通过file字段找到对应的图片。*现在还不能支持同一行数据传入两张及以上的图片*。


### 文件上传  
文件上传简单，不再赘述。

### 复制选定数据  
具体复制的相关配置在[配置页面](http://10.10.102.154/06-Configuration-Develop-Guide/Base%20Settings/View%20Action%20Settings.html)。需要说明的几点是前端事件选one时只能选取一条数据，选batch可以选择多条数据。不能用“one/batch”这种形式。



### 日历或者排程  

日历或者排程两个关键点：一是要将视图的显示设置为日历或者排程，二是视图模型中要有start_time和end_time两个字段，用于选定排程或者日历的区间。具体效果如demo所示。