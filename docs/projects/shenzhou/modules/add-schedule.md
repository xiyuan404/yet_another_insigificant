# 添加出诊时段



![7-14 使用RESTful封装更新出诊时段_神州掌上医疗项目.pdf](/Users/ayao/yet_another_insigificant.../docs/projects/shenzhou/modules/images/7-14 使用RESTful封装更新出诊时段_神州掌上医疗项目.pdf.png)





## 一 显示对话框弹窗

当用户不具备相关权限, 甚至没有选择诊室或者日期的时候,添加按钮都是禁用状态

```xml
<el-button
	size="medium"
	type="primary"
	:disabled="!isAuth(['ROOT','SCHEDULE:INSERT']) 
             || dataForm.deptSubId == null || dataForm.date == null"
>
	添加
</el-button>
```

按钮点击事件回调函数`addHandle`

因为弹窗页面既可以添加出诊计划,也可以修改现有出诊计划,

如果是创建出诊计划,就不需要向弹窗传递`workPlanId`参数;

如果是修改出诊计划,就必须传递`workPlanId` 参数, 让弹窗页面查询出诊计划,并显示到弹窗中

```js	
addHandle:function(){
  this.$nextTick(){
    // 因为是添加新出诊计划, 不需要向弹窗页面传递workPlanId去查找已有的出诊计划
    this.$refs.addOrUpdate.init(null, this.dataForm.deptSubId, this.dataForm.date)
  }
}
```



## 二 弹窗视图层和模型层

```xml
<el-dialog
		title="新增"
		v-if="isAuth(['ROOT','SECHEDULE:INSERT','SCHEDULE:UPDATE'])"
		:close-on-click-modal="false"
		v-model="visible"
>
  <el-form :model="dataForm" ref="dataFrom" :rules="dataRules" lable-width="80px">
  	<el-form-item label="出诊医生" prop="doctorId">
    	<el-select v-model="dataForm.doctorId" :disabled="dataForm.workPlanId !== null" >						<el-option v-for="one in doctorList" :label="one.name" :value="one.id"></el-option>
      </el-select>
		</el-form-item>
    <el-form-item label="出诊时间" prop="doctorId">
    	<div style="width: 100%">
      	<el-checkbox v-model="checkAll" @change="checkAllChangeHandle">全选</el-checkbox>
      </div>
      <div style="width: 100%">
        <el-checkbox-group v-model="checkSlot">
        	<el-checkbox
							v-for="(item, index) in checkSlot"
              :label="item"
							:disabled="analyseCheckBoxDisable(index + 1)"
           >
            
          </el-checkbox>
        </el-checkbox-group>
      </div>
		</el-form-item>
    <el-form-item label="时段人数">
    	<el-slider
				v-model="dataForm.slotMaximum"
				:min="1"
				:max="10"
				show-input
				:disabled="dataForm.workPlanId != null"	
       />
    </el-form-item>
  </el-form>
	<template #footer>
  	<span class="dialog-footer">
    	<el-button @click="visible=false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit">确定</el-button>
    </span>
  </template>
</el-dialog>
```

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
  const startTime = new Date().setHours(8,0,0,0)
  const endTime = new Date().setHours(17,30,0,0)
  const interval = 30 // 30 min
  const slotList = generateTimeSlots(startTime, endTime, 30)
  // setHours(hoursValue, minutesValue, secondsValue, msValue)
  return {
    visible:false,
    doctorList:[],
    checkAll: false,
    slotList,
    checkedSlots:[].
    oldSlots: [],
    formData: {
      workPlanId: null,
      deptSubId: null,
      doctorId: null,
      date: new dayjs().format('YYYY-MM-DD')
      slots: []
    }
  	dataRules: {
      doctorId: [{required: true, message: '出诊医生不能为空'}]
    }
  }
}
```

## 三 弹窗页面初始化

查询选中的医生信息

```js
loadDoctorList: function(){
  const that = this;
  const requestData = {
    deptSubId: that.dataForm.deptSubId
  }
  that.$http('/doctor/searchByDeptSubId', 'POST', requestData, true, function(resp){
    that.doctorList = resp.result
  })
}
```

