<template>
  <!--<div class="navheader">人员清点</div>-->
  <el-row class="menu_title_wrap home">
    <el-col :span="1" style="height:10px"></el-col>
    <el-col :span="22">
      <div class="li2_parts">
        <div class="tabHead">
          <div  :class="['tab', { tabing: isB1}]"  v-on:click="toggle1()">人员清点</div>
          <div  :class="['tab', { tabing: isB2}]"  v-on:click="toggle2()">清点记录</div>
        </div>
        <div class="tab1" v-show="isShow1">
          <div class="partsBody" style="height:392px;">
            <div class="bodyHead">
              <div class="title">监内未点人员{{inCriminals.length}}人</div>
              <div class="titleDescribe">（本监区总人数：{{orgCriminalCount}}人，<span @click="$emit('hasCheaked')" style="color: #1443cd">已点人数{{hascelled}}人</span>）</div>
            </div>
            <div class="bodyCon">
              <el-col :span="2"  v-for="(criminal,index) in inCriminals.slice(inA-1,inB)" :key="1">
                <div  :class="['criminal', {choosedcriminal: criminal.ischoose}]" v-on:click="chooseIn(index)" >
                  <div style="height: 91px;width: 90px;">
                  <img :src="criminal.Photo" width="98%" height="85" alt=""/>
                  </div>
                  <span class="criminalName">{{ criminal.CriminalName }}</span>
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
          <div class="partsBody" style="height:260px;">
            <div class="bodyHead">
              <div class="title">外出未点人员{{outCriminals.length}}人</div>
            </div>
            <div class="bodyCon" style="height: 135px;">
              <el-col :span="2"  v-for="(criminal,index) in outCriminals.slice(outA-1,outB)" :key="2">
                <div  :class="['criminal', {choosedcriminal: criminal.ischoose}]" v-on:click="chooseOut(index)" >
                  <div style="height: 91px;width: 90px;">
                       <img :src="criminal.Photo" width="98%" height="85" alt=""/>
                  </div>
                  <span class="criminalName">{{ criminal.CriminalName}}</span>
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
          <div class="partsFoot">
            <div class="alertText">{{alertText}}</div>
            <div style="margin: 13px 2px;float: right">
              <div class="sure" v-on:click="submitCriminal()">手动确定</div>
              <div class="sure" v-on:click="cancel()">手动结束</div>
            </div>
          </div>
        </div>
        <div class="tab2" v-show="isShow2">
          <div class="partsBody" style="height:697px;">

            <div class="bodyCon">

              <table  border="1" cellspacing="0" cellpadding="0" width="100%">
                <tr>
                  <th>清点类型</th>
                  <th>清点时间</th>
                  <th>应点总数</th>
                  <th>已点总数</th>
                  <th>未点数量</th>
                  <th>外出数量</th>
                  <th>清点人姓名</th>
                  <th>清点状态</th>
                </tr>
                <tr v-for="record in records" :key="1">
                  <td>{{record.CountTypeName}}</td>
                  <td>{{(record.CountTime==""||record.CountTime==null)?"":record.CountTime.replace("T"," ")}}</td>
                  <td>{{record.ShouldCount}}</td>
                  <td>{{record.RealCount}}</td>
                  <td>{{record.UnCount}}</td>
                  <td>{{record.OutterCount}}</td>
                  <td>{{record.PoliceName}}</td>
                  <td>{{record.StatusName}}</td>
                </tr>
              </table>

            </div>
            <el-row >
              <el-col :span="8" style="height: 10px"></el-col>
              <el-col :span="8" >
                <div class="pages">
                  <span class="pageControl"><img src="../../assets/q1.png" v-on:click="getRecordback()" alt=""/></span>
                  <span class="pagesText">{{recordPage+1}}/{{Math.ceil(recordCount/20)==0?1:Math.ceil(recordCount/20)}}</span>
                  <span class="pageControl"><img src="../../assets/q2.png" v-on:click="getRecordGo()" alt=""/></span>
                </div>
              </el-col>
              <el-col :span="8" style="height: 10px"></el-col>
            </el-row>
          </div>
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
      'SocketAllData','criminalList','receiveDataMsgType31','receiveDataMsgType30','receiveDataMsgType33'

    ],
    beforeCreate () {
      /* Coding By YanM */

      /* Coding By YanM */
      /*Coding By Qianjf */


      /*Coding By Qianjf */

    },
    data () {
      return {
        isShow1: true,
        isShow2: false,
        isB1: true,
        isB2: false,
        records:[],
        recordPage:0,
        recordIsLastPage:false,
        recordCount:0,
        hascelled:0,
        orgCriminalCount:0,

        inCriminals: [],
        inPages:1,//监内未点总页数
        inNowPage:1,//监内未点当前页
        inListAll:0,//监内未点总数
        inChoose:[],//监内选中人员
        inA:1,
        inB:24,

        outCriminals: [],
        outPages:1,//外出未点总页数
        outNowPage:1,//外出未点当前页
        outListAll:0,//外出未点总数
        outChoose:[],//外出选中人员
        outA:1,
        outB:12,
        alertText:""
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
          this.inA=this.inA+24
          this.inB=this.inB+24
        }else {
//          alert("已经最后一页了")
        }
      },
      inBack:function () {
        if(this.inNowPage==1){
//          alert("已经是第一页了")
        }else {
          this.inNowPage=this.inNowPage-1
          this.inA=this.inA-24
          this.inB=this.inB-24
        }
      },
      chooseIn:function (index){
        var PersonID=this.inCriminals[index+this.inA-1]["PersonID"]
//        this.inCriminals[index+this.inA-1].ischoose=true
//        this.inChoose.push(PersonID)
//        alert(this.inChoose)
        if(this.inChoose.indexOf(PersonID)==-1){
          this.inChoose.push(PersonID)
          this.inCriminals[index+this.inA-1].ischoose=true
        }else {
          this.inCriminals[index+this.inA-1].ischoose=false
          this.inChoose.splice(this.inChoose.indexOf(PersonID),1)
        }
      }
      ,

      outGo:function () {
        if(this.outNowPage<this.outPages){
          this.outNowPage=this.outNowPage+1
          this.outA=this.outA+12
          this.outB=this.outB+12
        }else {
//          alert("已经最后一页了")
        }
      },
      outBack:function () {
        if(this.outNowPage==1){
//          alert("已经是第一页了")
        }else {
          this.outNowPage=this.outNowPage-1
          this.outA=this.outA-12
          this.outB=this.outB-12
        }
      },
      chooseOut:function (index){
        var PersonID=this.outCriminals[index+this.outA-1]["PersonID"]
        if(this.outChoose.indexOf(PersonID)==-1){
          this.outChoose.push(PersonID)
          this.outCriminals[index+this.outA-1].ischoose=true
        }else {
          this.outCriminals[index+this.outA-1].ischoose=false
          this.outChoose.splice(this.outChoose.indexOf(PersonID),1)
        }
      }
      ,
      getRecordGo:function () {
        var vm = this
        if(!vm.recordIsLastPage){
          vm.recordPage=vm.recordPage+1
          $.ajax({
            type: "get",
            contentType: "application/json; charset=utf-8",
            dataType: "jsonp",
            jsonp: "callback",
            async: false,
            data:{"OrgID":localStorage.getItem("OrgID"),"PageIndex":vm.recordPage,"PageSize":20},
            url:  BasicUrl+'CriminalCnt/GetCriminalCntRecord' + "?callback=?",
            success: function (result) {
              if(result.length!=20){
                vm.recordIsLastPage=true
              }else {
                vm.recordIsLastPage=false

              }
              vm.records=result
            },
            error: function (err) {
//              alert("请求异常")
            },
            complete: function (XHR, TS) {
              XHR = null;  //回收资源
            }
          });

          //      获取总条数
          $.ajax({
            type: "get",
            contentType: "application/json; charset=utf-8",
            dataType: "jsonp",
            jsonp: "callback",
            async: false,
            data:{"OrgID":localStorage.getItem("OrgID")},
            url:  BasicUrl+'CriminalCnt/GetCriminalCntRecordsCount' + "?callback=?",
            success: function (result) {
              vm.recordCount=result
            },
            error: function (err) {
//              alert("请求异常")
            },
            complete: function (XHR, TS) {
              XHR = null;  //回收资源
            }
          });
        }else {
//          alert("已经到了最后一页了")
        }



      },
      getRecordback:function () {
        var vm = this
        if(vm.recordPage==0){
//          alert("已经是第一页了")
        }else {
          vm.recordPage=vm.recordPage-1
//          localStorage.setItem("OrgID","43368189-CE77-4721-BAA7-1545BB3E5A42")
          $.ajax({
            type: "get",
            contentType: "application/json; charset=utf-8",
            dataType: "jsonp",
            jsonp: "callback",
            async: false,
            data:{"OrgID":localStorage.getItem("OrgID"),"PageIndex":vm.recordPage,"PageSize":20},
            url:  BasicUrl+'CriminalCnt/GetCriminalCntRecord' + "?callback=?",
            success: function (result) {
              if(result.length!=20){
                vm.recordIsLastPage=true
              }else {
                vm.recordIsLastPage=false
              }
              vm.records=result
            },
            error: function (err) {
//              alert("请求异常")
            },
            complete: function (XHR, TS) {
              XHR = null;  //回收资源
            }
          });
          //      获取总条数
          $.ajax({
            type: "get",
            contentType: "application/json; charset=utf-8",
            dataType: "jsonp",
            jsonp: "callback",
            async: false,
            data:{"OrgID":localStorage.getItem("OrgID")},
            url:  BasicUrl+'CriminalCnt/GetCriminalCntRecordsCount' + "?callback=?",
            success: function (result) {
              vm.recordCount=result
            },
            error: function (err) {
//              alert("请求异常")
            },
            complete: function (XHR, TS) {
              XHR = null;  //回收资源
            }
          });
        }
      },
      submitCriminal:function () {
        var vm=this
        var allCriminals=this.outChoose.concat(this.inChoose);
        var subCriminals=""
        for (var i=0;i<allCriminals.length;i++){
            if(i==0){
              subCriminals=allCriminals[i]
            }else {
              subCriminals=subCriminals+","+allCriminals[i]
            }
        }
        var send = {
          Header: {
            MsgID:"201501260000000034",
            MsgType:30
          },
          Body: JSON.stringify({
            OrgID : localStorage.getItem('OrgID'),
            Type:2,
            ObjectID:subCriminals
          })
        }
        console.log("人员清点",send)
        vm.$emit('openLogin',true)
        var submitCriminalSet=setInterval(function () {
          if(localStorage.getItem("placemanID")==0){

          }else {
            clearInterval(submitCriminalSet)
            if(subCriminals==[]||subCriminals==''){
              vm.alertText="还没选人"
              setTimeout(function () {
                vm.alertText=""
              },2000)
            }else {
              //发送数据
              $.ajax({
                type: "get",
                contentType: "application/json; charset=utf-8",
                dataType: "jsonp",
                jsonp: "callback",
                async: false,
                url: ajaxUrl,
                data:JSON.stringify(send),
                success: function (result) {
                  if(result.RET==1){
                  vm.outChoose.splice(0,vm.outChoose.length)
                  vm.inChoose.splice(0,vm.inChoose.length)
                    localStorage.setItem("placemanID","0")
                    vm.alertText="手动确定成功"
                    setTimeout(function () {
                      vm.alertText=""
//                    vm.$router.push({ path: '/' })
                    },2000)

                  }else {
                    vm.alertText="手动确定失败"
                    localStorage.setItem("placemanID","0")
                    setTimeout(function () {
                      vm.alertText=""
                    },2000)
                  }
                },
                complete: function (XHR, TS) {
                  XHR = null;  //回收资源
                }
              })
            }
          }
        },1000)

      },
      cancel:function () {
        var vm=this
        vm.$emit('openLogin',true)
        var send = {
          Header: {
            MsgID:"201501260000000034",
            MsgType:33
          },
          Body: JSON.stringify({
            OrgID : localStorage.getItem('OrgID'),
            CountType:2
          })
        }
        var cancelSet=setInterval(function () {
          if(localStorage.getItem("placemanID")==0){

          }else {
              clearInterval(cancelSet)
            //发送数据
            $.ajax({
              type: "get",
              contentType: "application/json; charset=utf-8",
              dataType: "jsonp",
              jsonp: "callback",
              async: false,
              url: ajaxUrl,
              data:JSON.stringify(send),
              success: function (result) {
                if(result.RET==1){

                    vm.inChoose=[]
                    vm.outChoose=[]

                  vm.alertText="手动结束成功"
                  localStorage.setItem("placemanID","0")
                  vm.inCriminals=[]
                  vm.outCriminals=[]
                  setTimeout(function () {
                    vm.alertText=""

//                vm.$router.push({ path: '/' })
                  },2000)

                }else {
                  localStorage.setItem("placemanID","0")

                  vm.alertText="手动结束失败"
                  setTimeout(function () {
                    vm.alertText=""
                  },2000)
                }
              },
              complete: function (XHR, TS) {
                XHR = null;  //回收资源
              }
            })
          }
        },500)


      }

    },
    destroyed: function () {
      localStorage.setItem("crimalCheckClean","0")

    },
    mounted () {
      /* Coding By YanM */

      /* Coding By YanM */
      /* Coding By Qianjf */
      localStorage.setItem("crimalCheckClean","1")
      localStorage.setItem("placemanID","0")

      var vm = this
      var send = {
        Header: {
          MsgID:"201501260000000003",
          MsgType:31
        },
        Body: JSON.stringify({
          OrgID : localStorage.getItem('OrgID'),
        })
      }
      localStorage.setItem("placemanID","0")

    var crimalCheckClean1= setInterval(function () {
      if(localStorage.getItem("crimalCheckClean")==0){
        clearInterval(crimalCheckClean1)
      }else {
        //发送数据
        if(vm.ws.readyState == WebSocket.OPEN){
          vm.ws.send(JSON.stringify(send))
        }
        //接收数据
        var  receiveData = vm.receiveDataMsgType31
        if(receiveData!=""&&receiveData!=null){
          var hasNotCall=[] //监内未点2402
          var outHasNotCall=[] //外出未点2403
          for (var i=0;i<receiveData.length;i++){
            if(receiveData[i].Status=="2402"){
              var notCall=receiveData[i]
              notCall["ischoose"]=false
              notCall["CriminalName"]=vm.criminalList[0][notCall["PersonID"]]["CriminalName"]
              notCall["Photo"]=vm.criminalList[0][notCall["PersonID"]]["Photo"]
              for (var m=0;m<vm.inChoose.length;m++){
                if(vm.inChoose[m]==notCall["PersonID"]){
                  notCall["ischoose"]=true
                }
              }
              hasNotCall.push(notCall)
              vm.inCriminals=hasNotCall
              vm.inPages=Math.ceil(vm.inCriminals.length/24)==0?1:Math.ceil(vm.inCriminals.length/24)

            }else if(receiveData[i].Status=="2403"){

              var outNotCall=receiveData[i]
              outNotCall["ischoose"]=false
              outNotCall["CriminalName"]=vm.criminalList[0][outNotCall["PersonID"]]["CriminalName"]
              outNotCall["Photo"]=vm.criminalList[0][outNotCall["PersonID"]]["Photo"]
              for (var n=0;n<vm.outChoose.length;n++){
                if(vm.outChoose[n]==outNotCall["PersonID"]){
                  outNotCall["ischoose"]=true
                }
              }

              outHasNotCall.push(outNotCall)
              vm.outCriminals=outHasNotCall
              vm.outPages=Math.ceil(vm.outCriminals.length/12)==0?1:Math.ceil(vm.outCriminals.length/12)

            }
          }
          if(hasNotCall.length==0){
            vm.inCriminals=[]
          }
          if(outHasNotCall.length==0){
            vm.outCriminals=[]
          }
        }


      }

      },500)

//      获取第一页记录数据
      $.ajax({
        type: "get",
        contentType: "application/json; charset=utf-8",
        dataType: "jsonp",
        jsonp: "callback",
        async: false,
        data:{"OrgID":localStorage.getItem("OrgID"),"PageIndex":0,"PageSize":20},
        url: BasicUrl+'CriminalCnt/GetCriminalCntRecord' + "?callback=?",
        success: function (result) {
          if(result.length!=20){
            vm.recordIsLastPage=true
          }else {
            vm.recordIsLastPage=false
          }
          vm.records=result
        },
        error: function (err) {
//          alert("请求异常")
        },
        complete: function (XHR, TS) {
          XHR = null;  //回收资源
        }
      });
//      获取清点记录总条数
      $.ajax({
        type: "get",
        contentType: "application/json; charset=utf-8",
        dataType: "jsonp",
        jsonp: "callback",
        async: false,
        data:{"OrgID":localStorage.getItem("OrgID")},
        url: BasicUrl+'CriminalCnt/GetCriminalCntRecordsCount' + "?callback=?",
        success: function (result) {
          vm.recordCount=result
        },
        error: function (err) {
//          alert("请求异常")
        },
        complete: function (XHR, TS) {
          XHR = null;  //回收资源
        }
      });
//      获取已点人员总数,本监区总人数
      var crimalCheckClean2= setInterval(function () {
        if(localStorage.getItem("crimalCheckClean")==0){
          clearInterval(crimalCheckClean2)
        }else {
          $.ajax({
            type: "get",
            contentType: "application/json; charset=utf-8",
            dataType: "jsonp",
            jsonp: "callback",
            async: false,
            data:{"OrgID":localStorage.getItem("OrgID")},
            url:  BasicUrl+'CriminalCnt/GetCriminalCalledCount' + "?callback=?",
            success: function (result) {
              vm.hascelled=result
            },
            error: function (err) {
//            alert("请求异常")
            },
            complete: function (XHR, TS) {
              XHR = null;  //回收资源
            }
          });
          $.ajax({
            type: "get",
            contentType: "application/json; charset=utf-8",
            dataType: "jsonp",
            jsonp: "callback",
            async: false,
            data:{"OrgID":localStorage.getItem("OrgID")},
            url:  BasicUrl+'CriminalCnt/GetCurOrgCriminalCount' + "?callback=?",
            success: function (result) {
              vm.orgCriminalCount=result[0].Total
            },
            error: function (err) {
//            alert("请求异常")
            },
            complete: function (XHR, TS) {
              XHR = null;  //回收资源
            }
          });
        }

      },1000)

      /* Coding By Qianjf */

    }

  }
</script>

<style >

  .li2_parts {
    height: 740px;
    margin: 0px auto;
  }
  .li2_parts .tabHead{
    width: 100%;
    height: 40px;
  }
  .li2_parts .tab{
    width: 155px;
    height: 40px;
    background: #004bdc;
    font-size: 18px;
    text-align: center;
    float: left;
    color:white;
    line-height: 38px;
  }
  .li2_parts .tabing{
    background: white;
    font-size: 18px;
    color: #004bdc;
    text-align: center;
  }
  .li2_parts .partsBody{
    width: 100%;
    height: 421px;
    background: white;

  }
  .li2_parts .partsFoot{
    width: 100%;
    height: 84px;
    /*margin-top: -48px;*/
    background: #c5cfdb;

  }
  .li2_parts .bodyCon{
    height: 256px;
    padding: 20px;

  }
  .li2_parts .pages{
    width: 273px;
    height: 50px;
    margin: 0px auto;
  }
  .li2_parts .pageControl{

  }
  .li2_parts .pagesText {
    font-size: 24px;
    display: inline-block;
    width: 155px;
    text-align: center;
  }
  .li2_parts .sure{
    width: 126px;
    height: 40px;
    background: #004bdc;
    text-align: center;
    color: white;
    font-size: 20px;
    line-height: 36px;
    letter-spacing: 2px;
    float: right;
    margin: 0px 35px;
  }



  .li2_parts .criminal{
    width: 84%;
    height: 110px;
    float: left;
    background: #0048d9;
    margin: 6px 6px;
    text-align: center;
    padding: 2px;
  }
  .li2_parts .criminal img{
    border: 1px solid white;
  }
  .li2_parts .criminalName {
    font-size: 12px;
    color: white;
    display: inline-block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    width: 99%;
  }
  .li2_parts .bodyHead{
    width: 100%;
    height: 46px;
    background: #c5cfdb;
  }
  .li2_parts .title{
    font-size: 25px;
    font-weight: 800;
    color: #1d68e8;
    text-shadow: 2px 2px 2px #fff;
    line-height: 45px;
    text-indent: 15px;
    float: left;

  }
  .li2_parts .titleDescribe{
    line-height: 45px;
    font-size: 20px;
    width: 856px;
    text-align: left;
  }
  .tab2 .bodyCon{
    height: 600px;
  }
 .choosedcriminal{
    background: red !important ;
  }
  .li2_parts tr{
    height:27px;
  }

  .navheader{
  height: 122px;
  color: #fff;
  font-size: 20px;
}
</style>
