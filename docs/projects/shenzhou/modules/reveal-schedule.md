## 医生出诊计划表`DOCTOR_WORK_PLAN`

| 序号 | 字段        | 类型     | 备注                   |
| :--: | ----------- | -------- | ---------------------- |
|  1   | id          | INTEGER  | 主键                   |
|  2   | doctor_id   | INTEGER  | 医生ID                 |
|  3   | dept_sub_id | INTEGER  | 诊室ID                 |
|  4   | date        | DATE     | 出诊日期               |
|  5   | maximum     | SMALLINT | 该医生当天挂号人数上线 |
|  6   | num         | SMALLINT | 该医生当天实际挂号人数 |

`DoctorWorkPlanDao.xml`

```java
<select id="searchWorkPlanRange" parameterType="Map" resultType="HashMap">
    SELECT 
    FROM HOSPITAL.DOCTOR_WROK_PLAN wp 
    JOIN HOSPITAL.DOCTOR d ON wp."doctor.id" = d."id"
    JOIN HOSPITAL.MEDICAL_DEPT_SUB ds ON ds."id" = wp."dept_sub_id"
    JOIN HOSPITAL.MEDICAL_DEPT md ON md."id" = ds."dept_id"
    WHERE wp.date BETWEEN TO_DATE(`${startDate}`) AND TO_DATE(`${endDate}`)
  	<if test="depId!=null">
      AND ds."dept_id" = ${deptId}
		</if>
     <if test="doctorName!=null">
       And d."name" LIKE '%${doctorName}'
     </if>
     Order BY ds."id", wp."date"
</select>  
```



在`com.example.hospital.api.service`包`MedicalDeptSubWorkPlanService`接口中

```java
public interface MedicalDeptSubWorkPlanService {
  public JSONArray searchWorkPlanInRange(Map param, ArrayList dateList);
}


```

JSON数组中每个元素就是一个诊室的出诊计划,里面包含

诊室ID, 诊室名称, 隶属科室,  同诊室同天出诊的医生合并到一个局部数组中,即使当天没有医生出诊,也要放置一个空的局部数组


```JSON
[
  {
    "deptSubId":2,
    "deptName": "口腔科",
    "deptSubName": "口腔颌面内科",
    "plan": [
 				{
          "date": "2024-10-13",
          "doctors": ["许静琪"]
				},
      	{
          "date": "2024-10-14",
          "doctors": ["吕成龙"],
        },
      	{
          "date": '2024-10-15',
          "doctors": ["任振国","吕成龙"]
        }
    ]
  }
  ...
]
```



<script>
  data(){
    return {
      slotList: [],
      checkedSlots: [],
      formData: {
        deptSub:null,
        deptSubId: null,
        date: new dayjs().format('YYYY-MM-DD')
        slots:[]
      },
      dataRules:{
        deptSub: [
          {
					}
        ]
      }
    }
  }
</script>

# 显示出诊时段

在拿到了某个诊室某天医生出诊的时间段数据后,把这些时间段数据显示到表格中



在work_plan页中,双击某个出诊日期的单元格,可以跳转到schedule页面,并且向页面传递诊室ID和日期, VUE页面会根据这两个参数显示某个诊室某天医生出诊的时间段



在work_plan页面中,每个单元格都设置了双击事件,向回调函数传递科室名,诊室ID和出诊日期

```xml
<el-table-column header-align="center" align="center" :label="dateList[0]" min-width="170">
	<template #default="scope">
  	<div 
        class="content"
        :class="scope.row.plan[0].doctors.length > 3 ? 'alignStyle' : ''"
        @dblclick="viewWorkPlanHandle(scope.row.deptName, scope.row.deptSubId , scope.row.plan[0].date)"
    >
      {{scope.row.plan[0].doctors.join(', ')}}
    </div>
  </template>
</el-table-column>
```

双击事件回调函数`viewWorkPlanHandle`

```js
viewWorkPlanHandle: function(deptName, deptSubId, date) {
  this.$router.push({
    name:'DoctorSchedule',
    params: { depteName: deptName, deptSubId: deptSubId, date: date }
  })
}
```

在`@/router/index.js`文件中, 定义了url参数,如果不在路由配置中定义接受参数,页面之间是不能传参的

```js
{
  path:'/doctor_schedule/:deptName/deptSubId/:date',
  name:'DoctorSchedule',
  componet: DoctorSchedule,
  meta:{
    title:"医生出诊表",
    isTab: true
  }
}
```



## 二 视图层和模型层

`doctor_schedule.vue`页面顶部是查询条件, 诊室和科室(二级列表)和日期作为查询条件, 与work_plan页面基本相同

```xml
<el-form :inline="true" :model="dataForm" :rules="dataRule" ref="dataForm" class="form">
	<el-from-item prop="deptSubId">
  	<el-cascader v-model="dataForm.deptSub" :options="dept" @change="deptSubChangeHandle">			</el-cascader>
  </el-from-item>
  <el-form-item prop="date">
    	<el-date-picker
				v-model="dataForm.date"
				type="date"
				placeholder="选择日期"
				class="input"
				size="mdeium"
				format="YYYY-MM-DD"
				value-format="YYYY-MM-DD"
			></el-date-picker>
  </el-form-item>
	<el-form-item>
  	<el-button size="medium" type="primary" @click="searchHandle()">查询</el-button>
    <el-button size="medium" type="primary" @click="addHandle()" :disabled="!isAuth(['ROOT','SCHEDULE:INSERT']) || dataForm.deptSubId === null || data">添加</el-button>
  </el-form-item>
</el-form>
```

`schedule`页面视图层

```xml
<div class="schedule" ref="schedule">
	<div class="row">
  	<div class="cell-header name">医生姓名</div>
    <div class="cell-header time" v-for="item in time">{{item}}</div>
    <div class="cell-header operate">操作</div>
  </div>
  <div class="row" v-for="doctor in doctors">
  	<div class="cell room">{{doctor.doctorName}}</div>
    <div class="cell" v-for="(item, index) in doctor.slot">
    	<el-tooltip
				effect="dark"
				:content="患者上限${doctor.maximum}"
				:placement="top-start"
				v-if="index === 0 && item"
			>
        <span class="blue">✅</span>
      </el-tooltip>
      <span v-if="index > 0 && item" class="blue">✅</span>
      <span v-if="!none" class="red">空</span>
    </div>
    <div class="cell">
    	<el-button
				type="primary"
				size="small"
				:disabled="!isAuth(['ROOT', 'SCHEDULE:UPDATE'])"
				link
				@click="updateHandle(doctor.workPlanId)"
			>修改</el-button>
      <el-button
				type="primary"
				size="small"
				:disabled="!isAuth(['ROOT', 'SCHEDULE:DELETE'])"
				link
				@click="deleteHandle(doctor.workPlanId)"
			>删除</el-button>
    </div>
  </div>
</div>
```

`schedule`页面模型层

```js
 data:function(){
    function generateTimeSlots(startTime, endTime, interval){
      const slotList = []
      let current =  startTime;
      while(current < endTime){
        const next = new Date(current.getTime() + interval * 60000)
        slotList.push(
          `${current.getHours().toString().padStart(2,'0')}:${current.getMinutes().toString().padStart(2, '0')}~${next.getHours().padStart(2,'0')}:${next.getMinutes().toString().padStart(2, '0')}`
        )
        current = next
      }
      return slotList;
    } 
    // setHours(hoursValue, minutesValue, secondsValue, msValue)
    const startTime = new Date().setHours(8,0,0,0)
    const endTime =  new Date().setHours(17,30,0,0)
    const interval = 30
    const slotList = generateTimeSlots(startTime, endTime, interval)
data:function(){
   

  return {
    time:lotList,
    doctors:[],
    dataForm: {
      deptName: null,
      date:dayjs().format('YYYY-MM-DD')
      deptSub:[],
    	deptSubId: null
    },
    dataRule: {
      deptSubId: [{required: true, message:"诊室不能为空", trigger: 'change'}],
      date:[{required: true, message: "日期不能为空"}]
    }
  }
}
```



## 初始化schedule页面

在`doctor_schedule.vue`页面中, 加载二级列表控件数据

```js
loadDeptAndSub:function(){
  let that = this;
  that.$http('/medical/dept/searchDeptAndSub', 'GET', {}, false, function(resp){
    const result = resp.result;
    const dept = [];
    for(let item in result){
      const arr = [];
      for(let sub of result[item]){
        arr.push({
          value:sub.subId,
          label:sub.subName
        })
      }
      dept.push({
        value: item,
        label: item,
        children:arr
      })
    }
    that.dept=dept
  })
}
```

查询并加载出诊时间段数据

```js
loadDataList:function(){
  const that = this;
  cosnt data = {
    date: that.dataForm.date,
    deptSubId:that.dataForm.deptSubId
  }
  that.$http('/doctor/work_plan/schedule/searchDeptSubSchedule', 'POST', data, true, function(resp){
		const result = resp.result
    that.doctors = result
  })
}
```

`onCreate`,接受路由参数,并完成二级列表控件初始化

```js
created: function(){
  const that = this
  that.loadDeptAndSub()
  const deptName = that.$route.params.deptName;
 	const deptSubId =NUmber(that.$route.params.deptSubId);
  const date = that.$route.params.date
  
  // 设置二级列表控件选中的科室和诊室
  that.dataForm.deptSub = [deptName, deptSubId]
  // 如果是从页面左侧导航进入的schedule页面, 传入的三个参数都为NAN, 把变量设置成null
  if(deptName == 'NAN' || deptSubID == "NAN" || date == "NAN"){
    that.dataForm.deptName = null;
    that.dataForm.deptSubId = null;
    that.dataForm.date = null;
    return;
  }
  that.dataForm.deptName = deptName;
  that.dataForm.deptSubId = deptSubId;
  that.dataForm.date = date;
  that.loadDateList()
}
```

二级列表控件选中的时候, 需要把选中的诊室ID保存在模型层, 

```js
handleDeptSubChange: function(val) {
  this.dataForm.deptSubID =  val != null ? val[1] : null
}
```

创建查询按钮点击事件的回调函数

```js
searchHandle: function(){
	const that = this
  if(that.dataForm.deptSubId == 'NAN' || that.dataForm.date = "NAN" 
    || that.dataForm.deptSubID == null || that.dataForm.date = null ){
    return 
  }
  that.$refs['dataForm'].validate(valid=>{
    if(valid){
      that.refs['dataForm'].clearValidate()
			that.loadDataList()
    } else {
      return false
    }
    
  })
}
```





