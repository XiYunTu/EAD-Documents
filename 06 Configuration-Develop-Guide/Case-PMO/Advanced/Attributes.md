## 高级属性配置

### 模型属性

登录开发者中心，依次点击 “开发 > 模型”菜单，选定 “pmo_project” 点击进入模型详情视图，选择“模型属性”子视图。

#### 虚拟属性

按照如下内容添加虚拟模型属性：
- 收款进度
    - 名称：```receive_progress```
    - 别名：```收款进度```
    - 类型：```直接量```
    - 数据类型：```number```
    - 虚拟属性：```是```
    - 描述：```收款进度```
    - 排序：```111```
- 超时工单数量
    - 名称：```timeout_task_count```
    - 别名：```超时工单数```
    - 类型：```直接量```
    - 数据类型：```integer```
    - 虚拟属性：```是```
    - 描述：```超时工单数```
    - 排序：```112```
- 工单总数量
    - 名称：```task_count```
    - 别名：```工单总数```
    - 类型：```直接量```
    - 数据类型：```integer```
    - 虚拟属性：```是```
    - 描述：```工单总数```
    - 排序：```113```

#### 数据字典设置

按照如下内容修改数据字典：
- 所属客户
    - 名称：```customer_id```
    - 数据字典：```客户信息数据字典```

> 进入“项目信息”视图详情视图，设置“所属客户”视图属性为“可写”。

#### 子视图

- 收款记录
    - 名称：```receive```
    - 别名：```收款```
    - 类型：```模型集合```
    - 数据类型：```pmo_project_receive```
    - 描述：```收款```
    - 外键：```project_id```
    - 排序：```114```
- 任务工单
    - 名称：```tasks```
    - 别名：```工单```
    - 类型：```模型集合```
    - 数据类型：```pmo_project_task```
    - 描述：```工单```
    - 外键：```project_id```
    - 排序：```115```

#### 视图属性

此处主要陈述说明视图（项目信息）和子视图（收款和工单子视图）的关联过程。  

- 在构建项目信息模型时，将子视图的**类型设置为模型集合，数据类型设置为对应的模型，并且外键为视图的主键（这是视图和子视图连接的关键）**  
- 在构建子视图模型时，要有视图的主键属性，即project_id，正规来讲，要将其设为id或者wordbook类型，其实来讲，string也是可以的。当在子视图中CRUD时，系统会根据外键自动将相关属性（这里指项目信息视图的product_id）填入到子视图的对应属性中，完成关联。  
- 要想在项目信息中显示出对应的子视图，要在项目信息中添加子视图属性，并将其绑定视图选为子视图（系统自动添加） 

#### Session 过滤器  

Session过滤器通过比对Session中的相关信息，将过滤后的信息展示给用户。Session过滤器一般为隐藏属性。
#### 表单联动  
表单联动用于表单B随表单A的改变而发生改变，具体内容位于[表单联动](http://10.10.102.154/06-Configuration-Develop-Guide/Advanced%20Settings/Form.html)，此处我们将根据项目类型不同选择不同开发平台。  
项目类型：```develop:开发,maintain:运维```  
平台类型枚举：```windows:Windows,linux:Linux,unix:Unix,j2ee:J2EE,dotnet:.NET,ead:EAD,bpm:BPM```  
视图属性参数：
```
{"control":
	{"events":
		{"change:project_type":"swift|maintain:linux,windows;develop:dotnet,j2ee,ead,bpm"}
	}
}
```  
> 需要注意的是被联动的也要在枚举列表里将所有枚举录入；且视图属性参数中的是枚举列表里的K值

#### 属性格式化  
视图属性里的Format用于对属性进行格式化处理，此处格式化为对数据的最终处理，不信可以填入任何内容，最终显示都是你填写的内容。我们可以对项目信息中的合同金额进行格式化处理  
```
function(value, row){
var style = 'default';
        value = row['contract_account'];
style = value >=10000  ? 'success' : style;
style = value > 5000 && value <10000 ? 'warning' : style;
        style = value < 5000 ? 'danger' : style;
return value !== null ? '<span class="status-round status-round-' +style+ '"></span>' : '';
}
```  
如上代码可以根据金额大小展示不同颜色的圆点
#### 视图选择器  

视图选择器用于复杂情况的内容选择，它相较于列表形式，展示的是一个视图，可以在视图上完成内容的选择，几点说明  

- 相对应的模型字段选为类型选为WORDBOOK（WORDBOOK指代所有视图，不一定非得有字典），视图中对应的属性选择视图选择器。  
- 视图中字典选择对应的视图或者字典（这其实与视图参数中的json中的URL没有对应，只要二者指向同一个模型即可）
- 视图中对应属性参数配置为  


```
{"control":{"displayValue":"","realValue":"","url":"demo/api/pmo_customer-pmo_customer_info-pmo_customer_info"}}
```  
其中**URL拼接规则为：应用编码/api/应用中各级菜单用-拼接**。  
为方便，我们使用客户作为工单责任人，应用编码为：demo，应用中到客户信息各级编码为pmo_customer，pmo_customer_info，pmo_customer_info，最终拼接为pmo_customer-pmo_customer_info-pmo_customer_info，所以最终的URL为
```
demo/api/pmo_customer-pmo_customer_info-pmo_customer_info
```  
  
  

有人可能会问：那如果一个视图没有在应用资源里，那该如何去拼接这个视图的URL呢？  
答：所有的视图只有存在于应用的资源中时，才相当于被实例化，才能被访问，仅仅存在于标准视图中是不能被访问的，这是对视图的一种权限管理。只有应用才能访问视图。  

#### 日历与排程  

