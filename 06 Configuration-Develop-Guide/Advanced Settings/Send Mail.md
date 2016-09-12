### Send Mail

第一步：配置视图动作

属性名称 | 属性值 | 说明
------- | ----- | ---
名称 | xxx | 自定义
编码 | xxx | 自定义
HTTP方法 | POST | --
驱动 | remindNotice | 后端自定义驱动，需要首先在驱动表中进行注册
动作参数 | {"msg":["{templateName}"]} | 邮件发送模板名称
属性列表 | -- | --
前端事件 | e.send-mail | --
记录动态 | 默认 | --
前端参数 | -- | --
图标 | glyphicon-bell | --
选择模式 | 单选 | --
主要 | x | 自定义
分组 | x | 自定义
描述 | xxx | 自定义
操作视图 | -- | --
工作流模型 | -- | --
显示模式 | -- | --
序号 | xxx | 自定义

第二步：后端自定义扩展驱动

邮件发送后端扩展驱动伪代码：

```
 public Object remindNotice(ActionContext context) throws Exception {
    Object returnObj = null;
    //获取当前选中的数据
    Map<String, Object> object = context.getData();
    //获取数据主键
    String dataId = object.get(context.getEadModel().getIdName()).toString();
    //获取是否确认发送的标识
    String confirm = ParamsDeal.getConfirmFlag(object);
    //获取模板参数数据（注意：所有模板参数数据都必须在以下Map中涵盖）
    Map<String, Object> mailTemplateParamData = getTemplateParamsData(dataId);
    Map<String,Object> params = new HashMap<>();
    //设置应用标识，如固废项目，则设置为：hwms
    params.put("app_key", "{appKey}}");
    //获取模板并发送邮件，可以配置多个模板，进行邮件发送
    if (StringKit.isNotBlank(context.getEadAction().getActionParam())) {
      JSONObject actionParam = JSON.parseObject(context.getEadAction().getActionParam());
      JSONArray templates = actionParam.getJSONArray("msg");
      if (templates != null && templates.size() > 0) {
        for (int i = 0; i < templates.size() ; i++) {
          returnObj = sysNoticeService.sendEmail(dataId, templates.getString(i), 
              mailTemplateParamData, context.getEadAction(), context.getSession(), confirm, params,"");
        }
      }
    }
    //若发送邮件后，需要更新数据状态，则需要执行以下语句，并且，需要在视图动作参数中配置更新需要的参数，详细见：基本配置 —> 视图动作配置
    commonAction.updateBySetting(context);
    return returnObj;
  }
```

第三步：前端自定义扩展驱动

邮件发送前端扩展驱动伪代码：

```
define(function(require){
	var Modal = require('app/utility/modal');
	var action = function(){
		var _this = this;
		var model = this.view.getActiveModel();
		var url = this.view.model.url + '/' + this.model.get('key');
		var method = this.model.get('method') ? this.model.get('method') : 'POST';
		var data = model.toJSON();
		var confirmData;
		var id = model.get(this.view.idName);

		//Modal Render 
		var modal = new Modal({
			model: new Backbone.Model({
				title: this.model.get('legend') ? this.model.get('legend') : this.model.get('name'),
				type:'modal-sm'
			}),
			content:'',
			tpl:'#tpl-modal-form-mail'
		});
		modal.render();

		//Submit Event
		var $btn = modal.$('.rb-submit:not(.disabled)');
		$btn.on('click', function(e){
			
			var data = {
      	email:modal.$('input[name=email]').val()
      };

      data[_this.view.idName] = model.get(_this.view.idName);

			$.ajax(url, {
	      data:JSON.stringify(data),
	      dataType: 'json',
				contentType: 'application/json; charset=UTF-8',
				type: method
	    })
	    .success(function(response, options) {
       	_this.trigger('success');
       	_this.view.model.request();
       	_this.details && _this.details.exit();
	    })
	    .error(function(response, options) {
       	rainbow.errorMsg(response.responseJSON.content);
				_this.trigger('error');
	    })
	    .complete(function(){
	    	modal.close();
	    });
		});
	};
	return action;
});

```





