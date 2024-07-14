<head>
  <style>
    :root {
      --color-primary: #794cff;
      --col-span: 8;
      --total-cols: 24;
      --width-per: calc((var(--col-span) / var(--total-cols)) * 100%)
    }
    .row {
      display: flex;
    }
    .col-8 {
      flex: 0 0 var(--width-per)
    }
    .col-10 {
      --col-span: 10;
      flex: 0 0 var(--width-per)
    }
    .form-item-label {
      text-align: end;
    }
    .goodat {
      display: inline-flex;
      align-items: center;
      color: rgba(0,0,0,.88)
    }
    .goodat::after {
      content: ":";
      margin-inline-start: 2px;
      margin-inline-end: 8px;
    }
    .checkbox-wrapper {
      display: inline-flex;
      align-items: baseline;
    }
    .checkbox{
       /* input[type="checkbox"]:chekced {
            background-color:
            colro: 
        }
       */
      input[type="checkbox"] {
        accent-color: var(--color-primary)
      }
    }
    .checkbox + span {
      white-space: nowrap;
    }
  </style>
</head>
<div class="row">
  <div class="col-8 form-item-label"><label class="goodat">目前感兴趣的技术有</label></div>

<div class="col-10">
  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox" checked/> </span>
    <span> Web/移动开发(React/Vue/Nodejs/Spring/iOS/Android) </span>
  </label>
  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox" checked/> </span>
    <span> 服务器/运维/数据库（Go/Java/Python/C++/Docker） </span>
  </label>
  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox"/> </span>
    <span> 算法/大数据/人工智能/机器人/AIoT（MapReduce/TensorFlow/ROS/OpenCV） </span>
  </label>
  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox"/> </span>
    <span> 自动驾驶/智能网联车/新能源（V2X/AUTOSAR/OTA/CAN/FCEV）</span>
  </label>
  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox"/> </span>
    <span> 网络/分布式系统/区块链（HTTP3/libp2p/web3）</span>
  </label>
  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox" checked/> </span>
    <span> 音视频/直播/VR/AR（WebRTC/FFmpeg/RTMP/SRS）</span>
  </label>
  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox" checked/> </span>
    <span> 地理GIS/绘图canvasEcharts/svg/threejs3D/Cesium三维地球和地图可视化（WebGL/WebGIS）</span>
  </label>
  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox" checked/> </span>
    <span> 低代码/ 云原生自动化运维/人工智能,新能源 </span>
  </label>
  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox" checked/> </span>
    <span> react/servelt/vue/solidty.js </span>
  </label>


  <label class="checkbox-wrapper">
    <span class="checkbox"> <input type="checkbox"/> </span>
    <span> 硬件/驱动/系统软件开发（汇编/C语言/Linux/Drivers/KVM/Hypervisor）</span>
  </label>
</div>
</div>
