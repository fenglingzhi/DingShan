<template>
  <!--<div class="navheader">人员清点</div>-->
  <el-row class="menu_title_wrap home">
    <el-col :span="1" style="height:10px"></el-col>
    <el-col :span="22">
      <div class="li1_parts">
        <div class="tabHead">
          <div :class="['tab', { tabing: isB1}]" v-on:click="toggle1()">{{buttonText}}（{{outCriminalList.length}}人）</div>
          <div :class="['tab', { tabing: isB2}]" v-on:click="toggle2()">留监（{{inCriminalList.length}}人）</div>
        </div>
        <div class="partsBody" v-show="isShow1">
          <div class="bodyCon">

            <el-col :span="2" v-for="(outCrimina,index)  in outCriminalList.slice(outA-1,outB)" :key="1">
              <div class="criminal">
                <div style="height: 91px;width:102px;">
                <img :src="outCrimina.Photo" width="98%" height="85" alt=""/>
                </div>
                <span class="criminalName">{{outCrimina.CriminalName}}</span>
              </div>
            </el-col>
          </div>
          <el-row >
            <el-col :span="8" style="height: 10px"></el-col>
            <el-col :span="8" >
              <div class="pages">
                <span class="pageControl"><img src="../../assets/q1.png" v-on:click="outBack()" alt=""/></span>
                <span class="pagesText">{{outNowPage}}/{{outPages}}</span>
                <span class="pageControl"><img src="../../assets/q2.png" v-on:click="outGo()" alt=""/></span>
              </div>
            </el-col>
            <el-col :span="8" style="height: 10px"></el-col>
          </el-row>

        </div>
        <div class="partsBody" v-show="isShow2">
          <div class="bodyCon">
            <el-col :span="2" v-for="(inCriminal,index) in inCriminalList.slice(inA-1,inB)" :key="1">
              <div class="criminal">
                <div style="height: 91px;width: 102px;">
                <img :src="inCriminal.Photo" width="98%" height="85" alt=""/>
                </div>
                <span class="criminalName">{{inCriminal.CriminalName}}</span>
              </div>
            </el-col>
          </div>
          <el-row >
            <el-col :span="8" style="height: 10px"></el-col>
            <el-col :span="8" >
              <div class="pages">
                <span class="pageControl"><img src="../../assets/q1.png" v-on:click="inBack()" alt=""/></span>
                <span class="pagesText">{{inNowPage}}/{{inPages}}</span>
                <span class="pageControl"><img src="../../assets/q2.png" v-on:click="inGo()" alt=""/></span>
              </div>
            </el-col>
            <el-col :span="8" style="height: 10px"></el-col>
          </el-row>

        </div>
        <div class="partsFoot">
          <div class="alertText">{{alertText}}</div>
          <div class="sure" v-on:click="sub()">{{buttonText}}</div>
        </div>
      </div>
    </el-col>
    <el-col :span="1"  style="height:10px"></el-col>
  </el-row>




</template>

<script>
  import { BasicUrl,IMG,ajaxUrl} from '../../config'

  export default {
    name: 'navheader',
    props:[
      'SocketAllData','receiveDataMsgType25','criminalList'
    ],
    data () {
      return {
        isShow1: true,
        isShow2: false,
        isB1: true,
        isB2: false,
        MoveType:2601,
        areaCriminalList:[],
        outCriminalList:[],
        inCriminalList:[],
        inPages:1,//留监总页数
        inNowPage:1,//留监当前页
        inListAll:0,//留监总数
        inA:1,
        inB:48,
        outPages:1,//外出总页数
        outNowPage:1,//外出当前页
        outListAll:0,//外出总数
        outA:1,
        outB:48,
        isSuccess:0,
        alertText:"",
        buttonText:""

      }
    },
    methods: {
      toggle1: function () {
        this.isShow1 = true
        this.isShow2 =false
        this.isB1 =  true
        this.isB2 = false
        this.recordPage=0
      },
      toggle2: function () {
        this.isShow1 = false
        this.isShow2 = true
        this.isB1 = false
        this.isB2 = true
      },
      inGo:function () {
        if(this.inNowPage<this.inPages){
          this.inNowPage=this.inNowPage+1
          this.inA=this.inA+48
          this.inB=this.inB+48
        }else {
//          alert("已经最后一页了")
        }
      },
      inBack:function () {
        if(this.inNowPage==1){
//          alert("已经是第一页了")
        }else {
          this.inNowPage=this.inNowPage-1
          this.inA=this.inA-48
          this.inB=this.inB-48
        }

      },
      outGo:function () {
        if(this.outNowPage<this.outPages){
          this.outNowPage=this.outNowPage+1
          this.outA=this.outA+48
          this.outB=this.outB+48
        }else {
//          alert("已经最后一页了")
        }
      },
      outBack:function () {
        if(this.outNowPage==1){
//          alert("已经是第一页了")
        }else {
          this.outNowPage=this.outNowPage-1
          this.outA=this.outA-48
          this.outB=this.outB-48
        }

      },
      sub:function () {
        var vm=this
        localStorage.setItem("placemanID","0")

        vm.$emit('openLogin',true)
        var subSetInterval=setInterval(function () {
          if(localStorage.getItem("placemanID")==0){

          }else {
              clearInterval(subSetInterval)
            var Polices=localStorage.getItem("placemanID");
            var Reason;
            if(vm.MoveType==2601){
              Reason="出工"
            }else {
              Reason="收工"
            }
            var workSend = {
              Header: {
                MsgID:"201501260000000031",
                MsgType:23
              },
              Body: JSON.stringify({
                OrgID : localStorage.getItem('OrgID'),
                DoorID : localStorage.getItem('DoorID'),
                Polices:Polices,
                Reason:Reason,
              })
            }
            //发送数据
            $.ajax({
              type: "get",
              contentType: "application/json; charset=utf-8",
              dataType: "jsonp",
              jsonp: "callback",
              async: false,
              url: ajaxUrl,
              data:JSON.stringify(workSend),
              success: function (result) {
                localStorage.setItem("moveTypes","0")
                if(result.RET==1){
                  vm.alertText="提交成功"
                  setTimeout(function () {
                    vm.alertText=""
                    vm.$router.push({ path: '/' })
                  },2000)
                }else {
                  vm.alertText="提交失败"
                  setTimeout(function () {
                    vm.alertText=""
                    vm.$router.push({ path: '/' })
                  },2000)
                }
              },
              complete: function (XHR, TS) {
                XHR = null;  //回收资源
              }
            })

          }

        },1000)


      },


      firstWs:function () {
        var vm=this
        var send1 = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:20
          },
          Body: JSON.stringify({
            OrgID : localStorage.getItem('OrgID'),
            DoorID : localStorage.getItem('DoorID'),
            AreaID : localStorage.getItem('AreaID'),
            RegType:vm.MoveType
          })
        }
        //发送数据
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(send1),
          success: function (result) {
            if(result.RET==1){
              vm.isSuccess=1
              localStorage.setItem("placemanID","0")
            }else {
              vm.isSuccess=0
            }
          },
          complete: function (XHR, TS) {
            XHR = null;  //回收资源
          }
        })

      }
    },
    mounted(){
      var vm = this
      if(localStorage.getItem("AreaType")==1){
        vm.buttonText="结束"
        vm.MoveType="2602"
      }else {
        vm.buttonText="出工"
        vm.MoveType="2601"
      }
      localStorage.setItem("placemanID","0")
      var outWork= setInterval(function () {
        if(localStorage.getItem("placemanID")==0){
        }else {
          localStorage.setItem("moveTypes","1")//1为进出工，2为临时外出登记

          vm.firstWs()

          clearInterval(outWork)
        }
      },500)
//      发送内容
      var personnel_distribution = {
        Header: {
          MsgID:"201501260000000001",
          MsgType:25
        },
        Body: JSON.stringify({
          OrgID : localStorage.getItem('OrgID'),
          MoveType : vm.MoveType
        })
      }
      setInterval(function () {
        if(vm.isSuccess==1){
          if(vm.ws.readyState == WebSocket.OPEN){
            vm.ws.send(JSON.stringify(personnel_distribution))
          }
          var  receiveDataMsgType25 = vm.receiveDataMsgType25
          var getCriminalLists=[]
          if(receiveDataMsgType25!=""&&receiveDataMsgType25!=null){
            vm.inPages=Math.ceil(vm.inCriminalList.length/48)==0?1:Math.ceil(vm.inCriminalList.length/48)
            for(var i=0;i<receiveDataMsgType25.length;i++){
              var getCriminalID = receiveDataMsgType25[i]["CriminalID"]
              var getCriminalList=receiveDataMsgType25[i]
              getCriminalList["ischoose"]=false
              getCriminalList["CriminalName"]=vm.criminalList[0][getCriminalID]["CriminalName"]
              getCriminalList["Photo"]=vm.criminalList[0][getCriminalID]["Photo"]
              getCriminalLists.push(getCriminalList)
              vm.outCriminalList=getCriminalLists
              for(var k=0;k<vm.inCriminalList.length;k++){
                if(vm.inCriminalList[k]["FlnkID"]==getCriminalID){
                  vm.inCriminalList.splice(k,1)
                }
              }
            }
          }
          vm.outPages=Math.ceil(vm.outCriminalList.length/48)==0?1:Math.ceil(vm.outCriminalList.length/48)
        }
      },500)


//      获取当前监区罪犯集合
      $.ajax({
        type: "get",
        contentType: "application/json; charset=utf-8",
        dataType: "jsonp",
        jsonp: "callback",
        async: false,
        data:{"OrgID":localStorage.getItem("OrgID")},
        url: BasicUrl+'CriminalCnt/GetCriminalList' + "?callback=?",
        success: function (result) {
          if(result!=""&&result!=null){
            vm.inListAll=result.length
            for (var i=0;i<result.length;i++){
              result[i]["ischoose"]=false
            }
            vm.areaCriminalList=result
            vm.inCriminalList=result
            vm.inPages=Math.ceil(result.length/48)==0?1:Math.ceil(result.length/48)
          }
        },
        error: function (err) {
//            alert("请求异常")
        },
        complete: function (XHR, TS) {
          XHR = null;  //回收资源
        }
      });
    }
  }

</script>

<style>

  .li1_parts{
    height: 740px;
    margin: 0px auto;
  }
  .li1_parts .tabHead{
    width: 100%;
    height: 40px;
  }
  .li1_parts .tab{
    width: 155px;
    height: 40px;
    background: #004bdc;
    font-size: 18px;
    text-align: center;
    float: left;
    color:white;
    line-height: 38px;
  }
  .li1_parts .tabing{
    background: white;
    font-size: 18px;
    color: #004bdc;
    text-align: center;
  }
  .li1_parts .partsBody{
    width: 100%;
    height: 614px;
    background: white;

  }
  .li1_parts .partsFoot{
    width: 100%;
    height: 84px;
    background: #c5cfdb;

  }
  .li1_parts .bodyCon{
    height: 522px;
    padding: 20px;
  }
  .li1_parts .pages{
    width: 273px;
    height: 50px;
    margin: 0px auto;
  }
  .li1_parts .pageControl{

  }
  .li1_parts .pagesText {
    font-size: 24px;
    display: inline-block;
    width: 155px;
    text-align: center;
  }
  .li1_parts .sure{
    width: 126px;
    height: 40px;
    background: #004bdc;
    text-align: center;
    color: white;
    font-size: 20px;
    line-height: 36px;
    letter-spacing: 9px;
    float: right;
    margin: 24px 95px;
  }



  .li1_parts .criminal{
    /*width: 64px;*/
    height: 110px;
    /*float: left;*/
    background: #0048d9;
    margin: 6px 6px;
    text-align: center;
    padding: 2px;
  }
  .li1_parts .criminal img{
    border: 1px solid white;
  }
  .li1_parts .criminalName{
    font-size: 12px;
    color: white;
    display: inline-block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    width: 99%;
  }

  .navheader{
    height: 122px;
    color: #fff;
    font-size: 20px;
  }
</style>
