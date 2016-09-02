## EAD  引擎扩展

BPM Rest 接口详情请参考《BPM推广-BPM-TCK-第三方系统流程处理_接口文档》。

### 1、 视图驱动
EAD 平台结合 BMP 定义的视图驱动，主要功能点是组合 BPM 审批界面按钮显示接口动态生成 BPM 详情视图审批动作。

#### 1.1 详情视图驱动

**驱动名称：** WhbpmView 

**驱动 URI：** Whbpm/view

**涉及 BPM 接口：** 
- 基本信息查询接口（RetrieveBaseInfo）;
- 审批界面按钮显示接口（RetrievebtnStatus）;

**驱动功能：**
- 根据 EAD 视图配置获取业务数据对象;
- Rest 方式请求 BPM 基本信息查询接口获取详情数据;
- Rest 方式请求 BPM 审批界面按钮显示接口接口获取当前详情视图显示按钮;
- 根据 BPM 返回的按钮组装视图动作;

>加签的业务步骤不允许再进行加签、转办，但是允许提交和驳回

### 2、 审批动作驱动 

#### 2.1 流程发起驱动

**驱动名称：** WhbpmStart

**驱动 URI：** Whbpm/start

**涉及 BPM 接口：** 流程发起接口（StartProcess）

**驱动功能：**
- 组织 BPM 流程发起接口参数;
- Rest 方式请求 BPM 流程发起接口获取下一环节信息;
- BPM 接口返回成功后更新 bpm_ts_piid、bpm_status（草稿）、bpm_update_time;
- 根据Rainbow Confirm 参数 Rest 方式请求 BPM 流程发起接口进行二次确认流程发起;
- BPM 接口返回成功后更新 bpm_status（发起）、bpm_update_time;

> 流程发起驱动同时支持从草稿和驳回到草稿状态的实例进行流程发起操作。

#### 2.2 待办提交驱动

**驱动名称：** WhbpmSubmit

**驱动 URI：** Whbpm/submit

**涉及 BPM 接口：** 待办提交接口（CompleteTask）

**驱动功能：**
- 接收并解析用户提交的待办提交数据;
- 根据视图定义处理动作表单数据;
- 组织待办提交接口参数;
- Rest 方式请求 BPM 待办提交接口;
- BPM 接口返回成功后更新 bpm_status、bpm_update_time;

#### 2.3 待办驳回驱动

**驱动名称：** WhbpmBack

**驱动 URI：** Whbpm/back

**涉及 BPM 接口：** 待办驳回接口（RejectTask）

**驱动功能：**
- 接收并解析用户提交的待办驳回数据;
- 根据视图定义处理动作表单数据;
- 组织待办驳回接口参数;
- Rest 方式请求 BPM 待办驳回接口;
- BPM 接口返回成功后更新 bpm_status、bpm_update_time;

#### 2.4 待办转办、加签驱动

**驱动名称：** WhbpmTurnToDo

**驱动 URI：** Whbpm/turnToDo

**涉及 BPM 接口：** 待办转办、加签接口（ReassignTask）

**驱动功能：**
- 接收并解析用户提交的待办转办、加签数据;
- 根据视图定义处理动作表单数据;
- 组织待办转办、加签接口参数;
- Rest 方式请求 BPM 待办转办、加签接口;
- BPM 接口返回成功后更新 bpm_status、bpm_update_time;

#### 2.4 注销驱动

**驱动名称：** WhbpmLogout

**驱动 URI：** Whbpm/logout

**涉及 BPM 接口：** 注销接口（CancelProcess）

**驱动功能：**
- 接收并解析用户提交的数据;
- 根据视图定义处理动作表单数据;
- 组织注销接口参数;
- Rest 方式请求 BPM 注销接口;
- BPM 接口返回成功后更新 bpm_status、bpm_update_time;

### 3、 辅助查询驱动

#### 3.1 下一环节以及审批人驱动

**驱动名称：** WhbpmNext

**驱动 URI：** Whbpm/next

**涉及 BPM 接口：** 下一环节以及审批人接口（RetrieveNextActivity）

**驱动功能：**
- 组织下一环节以及审批人参数;
- Rest 方式请求 BPM 下一环节以及审批人接口;
- Rest 风格返回下一环节以及审批人数据（提供给 Rainbow）;

#### 3.2 驳回路径驱动

**驱动名称：** WhbpmBackPaths

**驱动 URI：** Whbpm/backPaths

**涉及 BPM 接口：** 驳回路径接口（RetrieveRejectPaths）

**驱动功能：**
- 组织驳回路径参数;
- Rest 方式请求 BPM 驳回路径接口;
- Rest 风格返回驳回路径数据（提供给 Rainbow）;

#### 3.3 基本信息查询驱动

**驱动名称：** WhbpmInfo

**驱动 URI：** Whbpm/info

**涉及 BPM 接口：** 基本信息查询接口（RetrieveBaseInfo）

**驱动功能：**
- 组织基本信息查询参数;
- Rest 方式请求 BPM 基本信息查询接口;
- Rest 风格返回基本信息数据（提供给 Rainbow）;

#### 3.4 审批历史驱动

**驱动名称：** WhbpmLogs

**驱动 URI：** Whbpm/logs

**涉及 BPM 接口：** 审批历史接口（RetrieveTaskDones）

**驱动功能：**
- 组织审批历史参数;
- Rest 方式请求 BPM 审批历史接口;
- Rest 风格返回审批历史数据（提供给 Rainbow）;


