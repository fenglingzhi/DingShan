<template>
  <!--<div class="navheader">人员清点</div>-->
  <el-row class="menu_title_wrap home">
    <el-col :span="1" style="height:10px"></el-col>
    <el-col :span="22">
      <div class="li4_parts">
        <div class="tab1">
          <div class="partsBody">
            <div class="bodyHead">
              <div class="title">人员分布</div>
            </div>
            <div class="bodyCon">
              <!--键盘模块Star-->
              <div class="searchMode" v-show="searchModeShow">
                <input class="searchInput" v-model="seachNum" placeholder="请输入罪犯编号" type="text">
                <div class="searchBottons">
                  <div class="listY"  v-on:click="enterNum('1')">1</div>
                  <div class="listY" v-on:click="enterNum('2')">2</div>
                  <div class="listY" v-on:click="enterNum('3')">3</div>
                  <div class="listY" v-on:click="enterNum('4')">4</div>
                  <div class="listY" v-on:click="enterNum('5')">5</div>
                  <div class="listY" v-on:click="enterNum('6')">6</div>
                  <div class="listY" v-on:click="enterNum('7')">7</div>
                  <div class="listY" v-on:click="enterNum('8')">8</div>
                  <div class="listY" v-on:click="enterNum('9')">9</div>
                  <div class="listY" v-on:click="enterNum('0')">0</div>
                  <div class="listY" v-on:click="enterNum('Q')">Q</div>
                  <div class="listY" v-on:click="enterNum('W')">W</div>
                  <div class="listY" v-on:click="enterNum('E')">E</div>
                  <div class="listY" v-on:click="enterNum('R')">R</div>
                  <div class="listY" v-on:click="enterNum('T')">T</div>
                  <div class="listY" v-on:click="enterNum('Y')">Y</div>
                  <div class="listY" v-on:click="enterNum('U')">U</div>
                  <div class="listY" v-on:click="enterNum('I')">I</div>
                  <div class="listY" v-on:click="enterNum('O')">O</div>
                  <div class="listY" v-on:click="enterNum('P')">P</div>
                  <div  class="listY" style="width: 26px;border: none;box-shadow: none;background: none"></div>
                  <div class="listY" v-on:click="enterNum('A')">A</div>
                  <div class="listY" v-on:click="enterNum('S')">S</div>
                  <div class="listY" v-on:click="enterNum('D')">D</div>
                  <div class="listY" v-on:click="enterNum('F')">F</div>
                  <div class="listY" v-on:click="enterNum('G')">G</div>
                  <div class="listY" v-on:click="enterNum('H')">H</div>
                  <div class="listY" v-on:click="enterNum('J')">J</div>
                  <div class="listY" v-on:click="enterNum('K')">K</div>
                  <div class="listY" v-on:click="enterNum('L')">L</div>
                  <div class="listY" v-on:click="enterNum('-')"style="font-size: 23px;width:77px;">清除</div>

                  <div class="listY" v-on:click="enterNum('Z')">Z</div>
                  <div class="listY" v-on:click="enterNum('X')">X</div>
                  <div class="listY" v-on:click="enterNum('C')">C</div>
                  <div class="listY" v-on:click="enterNum('V')">V</div>
                  <div class="listY" v-on:click="enterNum('B')">B</div>
                  <div class="listY" v-on:click="enterNum('N')">N</div>
                  <div class="listY" v-on:click="enterNum('M')">M</div>
                    <div class="listY" style="font-size: 23px;width: 170px;" v-on:click="searchSub()">搜索</div>
                  <div class="listX">
                    <div class="listY" style="font-size: 23px;width: 926px;" v-on:click="searchModeShow=false">关闭</div>
                  </div>
                </div>
              </div>
              <!--键盘模块End-->
              <el-row >
                <el-col :span="5">
                  <el-row >
                    <div class="deailBody">
                      <div v-for="(item,index) in allMapLists" v-on:click="selectMap(index)" :class="['mapPic',{chosedMap:item.status}]" :key="12" >
                          <img :src="item.MapUrl" width="100%" alt="">
                          <span>{{item.AreaName}}</span>
                      </div>

                    </div>
                  </el-row>
                </el-col>
                <el-col :span="1" style="height: 1px"></el-col>
                <el-col :span="17">
                  <div style="height:0px;"></div>
                  <el-row >
                    <el-row style="overflow: hidden;height: 632px;margin: 18px 4px;width:1025px;">
                      <div class="map">
                        <img :src="positionMap" id="positionMap" width="" alt="" ref="abc">
                        <!--<img :src="mapPhoto" alt="" ref="abc">-->
                        <div  v-for="(item,index) in criminalLists" :class="['point', {pointed: true}]"  v-on:click="select(index)"  :style="{top:item.CriminalY+'px',left:item.CriminalX+'px'}" >
                          <img :src="item.pointImg" alt="">
                          <div class="pointTop" v-show="item.status" v-on:click="closeTip()">
                            <el-col :span="10">
                              <img :src="item.CriminalPhoto" style="height: 140px;width: 100px;" alt="">
                            </el-col>
                            <el-col :span="14" style="text-align: left;line-height: 30px;">
                              姓名：{{item.Name}}<br/>
                              番号：{{item.PSID}}<br/>
                              监区：{{item.OrgName}}<br/>
                              管理登记：{{item.ManageLevelName}}<br/>
                            </el-col>
                            <div style="width: 0px;height: 0px;border-left: 18px solid transparent;border-right: 16px solid transparent;border-top: 13px solid #022171;font-size: 0;line-height: 0;margin: 142px 42px;"></div>
                          </div>
                        </div>
                      </div>
                    </el-row>
                  </el-row>
                  <div>
                    <span style="font-size: 20px;color: black;font-weight: 700;">监区情况：</span>
                    <span style="color:#d98900;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: #d98900"></span>&nbsp;宽管：{{KGL}}人&nbsp;&nbsp;</span>　
                    <span style="color:#ff0000;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: #ff0000"></span>&nbsp;严管：{{YGL}}人&nbsp;&nbsp;</span>　
                    <span style="color:#00b322;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: #00b322"></span>&nbsp;普管：{{PGL}}人&nbsp;&nbsp;</span>　
                    <span style="color:#fa00d4;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: #fa00d4"></span>&nbsp;考察：{{KCL}}人&nbsp;&nbsp;</span>
                    <span style="color:#020508;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: rgba(73,158,250,0)"></span>&nbsp;总共人数：{{KCL+PGL+YGL+KGL}}人&nbsp;&nbsp;</span>

                  </div>
                </el-col>
                <el-col :span="1" style="height:10px;">
                  <!--中间空隙-->
                   <div>
                    <div class="slc" v-on:click="changeSize('+')"><img src="../../assets/bigger.png" alt=""></div>
                    <div class="slc" v-on:click="changeSize('-')"><img src="../../assets/smaller.png" alt=""></div>
                    <div class="slc" v-on:click="searchModeShow=true;seachNum=''"><img src="../../assets/search.png" alt=""></div>
                   </div>
                </el-col>
              </el-row>
            </div>
          </div>
        </div>
      </div>
    </el-col>
    <el-col :span="1"  style="height:10px"></el-col>
  </el-row>

</template>

<script>
  import { BasicUrl,IMG,ajaxUrl,MapUrl } from '../../config'
  import { ajax } from '../../assets/ajaxWebApiMethod'
  var scaleNum;

  export default {
    name: 'navheader',
    props:[
      'mapList',
      'criminalList',
    ],

    data () {
      return {
        searchModeShow:false,
        seachNum:"",
        personBand:[],
        mapPhoto:"",
        getMapAct:0,
        getSearch:0,
        criminalLists:[],
        criminalGroupIDs:localStorage.getItem("criminalGroupIDs"),
        allMapLists:[],
        criminalsituationLsit:'',
        positionMap:'',
        MapID:'',
        KGL:0,
        YGL:0,
        PGL:0,
        KCL:0


      }
    },
    methods: {
        /*关闭气泡详情*/
      closeTip:function () {
        let vm = this
        for(let i = 0; i<vm.criminalLists.length; i++){
          vm.criminalLists[i].status = false
        }

      },
      /*搜索罪犯*/
      searchSub:function () {
        let vm=this
        let sendMessage  = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:12
          },
          Body: JSON.stringify({
            PSID:vm.seachNum,
            Status:0,
            PSType:"2002"
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(sendMessage),
          success: function (result) {
//              console.log(vm.personBand,result)
            if(result.MapID !="00000000-0000-0000-0000-000000000000"){
            clearInterval(vm.getMapAct)
            vm.criminalLists=[]
            vm.positionMap=MapUrl+vm.mapList[0][result.MapID].MapUrl
              var ManageLevels=vm.personBand[0][result.PSID.toLowerCase()].ManageLevel
              var pointImg="";
              if(ManageLevels=="4201"){
                pointImg="../../static/KG.png"
              }else if(ManageLevels=="4202"){
                pointImg="../../static/PG.png"
              }else if(ManageLevels=="4203"){
                pointImg="../../static/YG.png"
              }else if(ManageLevels=="4204"){
                pointImg="../../static/KC.png"
              }
              vm.criminalLists.push({
                PSID:result.PSID.toLowerCase(),
                Name:vm.personBand[0][result.PSID.toLowerCase()].Name,
                PersonID:vm.personBand[0][result.PSID.toLowerCase()].PersonID,
                OrgName:vm.personBand[0][result.PSID.toLowerCase()].OrgName,
                ManageLevelName:vm.personBand[0][result.PSID.toLowerCase()].ManageLevelName,
                CriminalPhoto:vm.personBand[0][result.PSID.toLowerCase()].Photo,
                status:true,
                ManageLevel:vm.personBand[0][result.PSID.toLowerCase()].ManageLevel,
                CriminalX:result.X,
                CriminalY:result.Y,
                pointImg:pointImg
              })
              vm.getSearch=setInterval(function () {
                vm.searchPoint()
              },2000)
              vm.searchModeShow=false

            }else {
              vm.$message('查无此人信息，请确认罪犯编号是否有误')
            }


          },
          complete: function (XHR) {
            XHR = null;  //回收资源
          }
        });

      },
      /*搜索罪犯点位刷新*/
      searchPoint:function () {
        let vm=this
        let sendMessage  = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:12
          },
          Body: JSON.stringify({
            PSID:vm.seachNum,
            Status:0,
            PSType:"2002"
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(sendMessage),
          success: function (result) {
            if(result.MapID !="00000000-0000-0000-0000-000000000000"){
              vm.positionMap=MapUrl+vm.mapList[0][result.MapID].MapUrl
              var ManageLevels=vm.personBand[0][result.PSID.toLowerCase()].ManageLevel
              var pointImg="";
              if(ManageLevels=="4201"){
                pointImg="../../static/KG.png"
              }else if(ManageLevels=="4202"){
                pointImg="../../static/PG.png"
              }else if(ManageLevels=="4203"){
                pointImg="../../static/YG.png"
              }else if(ManageLevels=="4204"){
                pointImg="../../static/KC.png"
              }

              vm.criminalLists[0].Name=vm.personBand[0][result.PSID.toLowerCase()].Name
              vm.criminalLists[0].PersonID=vm.personBand[0][result.PSID.toLowerCase()].PersonID
              vm.criminalLists[0].OrgName=vm.personBand[0][result.PSID.toLowerCase()].OrgName
              vm.criminalLists[0].ManageLevelName=vm.personBand[0][result.PSID.toLowerCase()].ManageLevelName
              vm.criminalLists[0].CriminalPhoto=vm.personBand[0][result.PSID.toLowerCase()].Photo
              vm.criminalLists[0].ManageLevel=vm.personBand[0][result.PSID.toLowerCase()].ManageLevel
              vm.criminalLists[0].pointImg=pointImg
              vm.criminalLists[0].CriminalX=result.X
              vm.criminalLists[0].CriminalY=result.Y
            }

          },
          complete: function (XHR) {
            XHR = null;  //回收资源
          }
        });

      },
      /*搜索输入按键*/
      enterNum:function (Num) {
          if(Num=="-"){
            this.seachNum=""
          }else {
            this.seachNum= this.seachNum+Num
          }
      },
      /* 选择互监组成员 */
      select:function (index) {
        let vm = this
        for(let i = 0; i<vm.criminalLists.length; i++){
          vm.criminalLists[i].status = false
        }
        vm.criminalLists[index].status = true
        vm.criminalsituationLsit= vm.criminalLists[index].CriminalID
        if(vm.criminalLists[index].MapUrl==null){

        }else {
          vm.mapPhoto= vm.criminalLists[index].MapUrl
          vm.criminalLists[index].pointStatus=true
          for (var k=0 ;k<vm.criminalLists.length;k++){
            if(vm.criminalLists[k].MapUrl !=null){
              if(vm.mapPhoto !=vm.criminalLists[k].MapUrl){
                vm.criminalLists[k].pointStatus=false
              }
            }
          }
        }



      },
      /*获取新的绑卡人基础数据*/
      getNewData:function () {
        /* 人员分布人员基础信息 */
        let vm = this;
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          data: {OrgID: localStorage.getItem('OrgID')},
          url:  BasicUrl+'/CriminalCnt/GetCardBindPersonList',
          success: function (result) {
            //所有罪犯信息缓存(哈希，便于快速查找缓存中的罪犯详细信息)
            var person_hash = new Array();
            for(var i=0;i<result.length;i++){
              person_hash[result[i].PSID.toLowerCase()] = {
                FlnkID:result[i].FlnkID,
                PersonID:result[i].PersonID,
                Name:result[i].Name,
                Pinyin:result[i].Pinyin,
                PSType:result[i].PSType,
                ManageLevel:result[i].ManageLevel,
                OrderIndex:result[i].OrderIndex,
                ManageLevelName:result[i].ManageLevelName,
                ManageLevelColor:result[i].ManageLevelColor,
                OrgID:result[i].OrgID,
                OrgName:result[i].OrgName,
                Photo:MapUrl+result[i].Photo,
                PSID:result[i].PSID.toLowerCase(),
                IC:result[i].IC
              };
            }
//            console.log("personBand",person_hash)
            vm.personBand[0]=person_hash
          },
          complete: function (XHR, TS) {
            XHR = null;  //回收资源
          }
        });
      },

      selectMap:function (index) {
        let vm = this
        vm.pointS()
        for(let i = 0; i<vm.allMapLists.length; i++){
          vm.allMapLists[i].status = false
        }
        vm.allMapLists[index].status= true
        vm.positionMap=vm.allMapLists[index].MapUrl
        vm.MapID=vm.allMapLists[index].MapFlnkID

      },
      /*缩放地图*/
      changeSize:function (type) {
          if(type=="+"){
            scaleNum=scaleNum+0.1
          }else if(type=="-"){
              if(scaleNum>0.2){
                scaleNum=scaleNum-0.1
              }
          }else {
//            scaleNum=1
          }
        $(".map").css("transform", "scale("+scaleNum+")");
        $(".map").css("-ms-transform-origin", "center center");
        $(".map").css("transform-origin", "center center");
        $(".map").css("-webkit-transform-origin", "center center");
//        $(".map").css("transform-origin", "0 0");
//        $(".map").css("-webkit-transform-origin", "0 0");
        $(".map").css("-moz-transform-origin", "center center");
        $(".map").css("-o-transform-origin", "center center");
      },
      /* 地图定位 */
      pointS:function () {
        let vm = this
        clearInterval(vm.getSearch)
        vm.getNewData()
        vm.criminalLists=[]
        let sendMessage  = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:10
          },
          Body: JSON.stringify({
            MapID:vm.MapID,
            OrgID:localStorage.getItem("OrgID").toLowerCase(),
            ManageLevel:0,
            AreaID:"00000000-0000-0000-0000-000000000000",
            PSType:"2002"
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(sendMessage),
          success: function (result) {
//         console.log(result)
            for (let i=0;i<result.length;i++){
                var ManageLevels=vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevel
              var pointImg="";
              if(ManageLevels=="4201"){
                pointImg="../../static/KG.png"
              }else if(ManageLevels=="4202"){
                pointImg="../../static/PG.png"
              }else if(ManageLevels=="4203"){
                pointImg="../../static/YG.png"
              }else if(ManageLevels=="4204"){
                pointImg="../../static/KC.png"
              }

              vm.criminalLists.push({
                PSID:result[i].PSID.toLowerCase(),
                Name:vm.personBand[0][result[i].PSID.toLowerCase()].Name,
                PersonID:vm.personBand[0][result[i].PSID.toLowerCase()].PersonID,
                OrgName:vm.personBand[0][result[i].PSID.toLowerCase()].OrgName,
                ManageLevelName:vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevelName,
                CriminalPhoto:vm.personBand[0][result[i].PSID.toLowerCase()].Photo,
                status:false,
                ManageLevel:vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevel,
                CriminalX:result[i].X,
                CriminalY:result[i].Y,
                pointImg:pointImg
              })
            }
            let KG=[]
            let YG=[]
            let PG=[]
            let KC=[]
            for (let j=0;j<vm.criminalLists.length;j++){
              if(vm.criminalLists[j].ManageLevel=="4201"){
                KG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4202"){
                PG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4203"){
                YG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4204"){
                KC.push(vm.criminalLists[j])
              }
              vm.KGL=KG.length
              vm.YGL=YG.length
              vm.PGL=PG.length
              vm.KCL=KC.length
            }

          },
          complete: function (XHR) {
            vm.getMapAct=setInterval(function () {
              vm.pointShow()
            },2000)
            XHR = null;  //回收资源
          }
        });


      },
      /* 地图定位实时刷新 */
      pointShow:function () {
        let vm = this
        vm.getNewData()
//        vm.criminalLists=[]
        let sendMessage  = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:10
          },
          Body: JSON.stringify({
            MapID:vm.MapID,
            OrgID:localStorage.getItem("OrgID").toLowerCase(),
            ManageLevel:0,
            AreaID:"00000000-0000-0000-0000-000000000000",
            PSType:"2002"
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(sendMessage),
          success: function (result) {
//         console.log(result)
            for (let i=0;i<result.length;i++){
              for (let j=0;j<vm.criminalLists.length;j++){
                if(vm.criminalLists[j].PSID.toLowerCase()==result[i].PSID.toLowerCase()){
                  vm.criminalLists[j].Name=vm.personBand[0][result[i].PSID.toLowerCase()].Name
                  vm.criminalLists[j].PersonID=vm.personBand[0][result[i].PSID.toLowerCase()].PersonID
                  vm.criminalLists[j].OrgName=vm.personBand[0][result[i].PSID.toLowerCase()].OrgName
                  vm.criminalLists[j].ManageLevelName=vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevelName
                  vm.criminalLists[j].CriminalPhoto=vm.personBand[0][result[i].PSID.toLowerCase()].Photo
                  vm.criminalLists[j].ManageLevel=vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevel
                  vm.criminalLists[j].CriminalX=result[i].X
                  vm.criminalLists[j].CriminalY=result[i].Y
                }
              }
            }
            let KG=[]
            let YG=[]
            let PG=[]
            let KC=[]
            for (let j=0;j<vm.criminalLists.length;j++){
              if(vm.criminalLists[j].ManageLevel=="4201"){
                KG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4202"){
                PG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4203"){
                YG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4204"){
                KC.push(vm.criminalLists[j])
              }
              vm.KGL=KG.length
              vm.YGL=YG.length
              vm.PGL=PG.length
              vm.KCL=KC.length
            }


          },
          complete: function (XHR) {
            XHR = null;  //回收资源
          }
        });

      },
      /*返回*/
      cancle:function () {
        clearInterval(this.getMapAct)
        localStorage.setItem("criminalGroupIDs","")
        this.$router.push({ path: '/mutualsupervision' })
      }
    },

    mounted () {
      let vm = this

      scaleNum=1
      $.ajax({
        type: "get",
        contentType: "application/json; charset=utf-8",
        dataType: "jsonp",
        jsonp: "callback",
        async: false,
        url: BasicUrl + 'HomeIndex/GetBindJQ',
        success: function (result) {
          vm.prisonSelect=result
          /*保存所有监区信息，人员分布渲染左侧地图用*/
          var allMapList = result;
          for (let i=0;i<allMapList.length;i++){
            allMapList[i].MapUrl=MapUrl+vm.mapList[0][allMapList[i].MapFlnkID].MapUrl
            if(i==0){
              allMapList[i].status=true
              vm.positionMap=allMapList[i].MapUrl
              vm.MapID=allMapList[i].MapFlnkID
            }else {
              allMapList[i].status=false
            }
          }
          vm.allMapLists=allMapList
//          console.log("allMapList",allMapList)
        },
        error: function (err) {
          console.log(err)
        }
      })
      /* Coding By YanM */
      setTimeout(function () {
        vm.pointS()
      },1500)
//      setTimeout(function () {
//        scaleNum=1020/$("#positionMap").width()
//        vm.changeSize("0")
//      },1000)

      $(".map").draggable();

      /*获取监区信息，用于左侧地图选择*/

      /* Coding By YanM */
    },
    destroyed: function () {
      clearInterval(this.getMapAct)
      clearInterval(vm.getSearch)
      localStorage.setItem("criminalGroupIDs","")
    }
  }
</script>

<style lang="scss" scoped>
  .searchInput::-webkit-input-placeholder{
    color: #fff;
  }
  .searchMode{
    position: absolute;
    z-index: 9999999;
    width: 1020px;
    background: #a2bcdb;
    height: 435px;
    border-radius: 7px;
    margin: 150px 338px;
  }
  .searchInput{
    width: 924px;
    height: 47px;
    margin: 18px 0px;
    font-size: 30px;
    text-align: center;
    background: #697a8f;
    color: white;
  }

  .searchBottons{
    width: 948px;
    height: 10px;
    margin: 0px auto;

  }
  .listX{
    width: 100%;

  }
  .listY{
    width: 76px;
    height: 49px;
    float: left;
    border: 1px solid blue;
    margin: 8px 8px;
    font-size: 30px;
    line-height: 51px;
    background: #3f73fb;
    color: white;
    border-radius: 8px;
    box-shadow: 1px 1px 4px -2px;

  }
  .listY:active{
    width: 76px;
    height: 49px;
    float: left;
    border: 1px solid blue;
    margin: 8px 8px;
    font-size: 30px;
    line-height: 51px;
    border-radius: 8px;
    box-shadow: 1px 1px 4px 4px;
  }

  .mapPic{
    margin: 10px auto;
    width: 218px;
    border: 2px solid rgba(0, 60, 242, 0);

  }
  .chosedMap{
    border: 2px solid #003cf2;
  }
  .li4_parts {
    height: 740px;
    margin: 0px auto;
  }
  .li4_parts .deailBody{
    background: #d9e3fe;
    height: 694px;
    overflow: auto;
    padding: 20px;
  }

  .li4_parts .partsBody {
    width: 100%;
    height:780px;
    background: white;
  }
  .li4_parts .partsFoot{
    width: 100%;
    height: 84px;
    background: #c5cfdb;
  }
  .li4_parts .bodyCon{
    height: 592px;
    /*padding: 20px;*/
  }
  .li4_parts .sure{
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
  .li4_parts .criminal{
    height: 150px;
    background: #0048d9;
    margin:10px 5px;
    padding: 5px;
  }
  .li4_parts .criminalName {
    font-size: 12px;
    color: white;
    display: inline-block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    width: 99%;
  }
  .li4_parts .bodyHead{
    width: 100%;
    height: 46px;
    background: #c5cfdb;
  }
  .li4_parts .title{
    font-size: 25px;
    font-weight: 800;
    color: #1d68e8;
    text-shadow: 2px 2px 2px #fff;
    line-height: 45px;
    text-indent: 15px;
    float: left;
  }
  .criminal_active{
    background: #ce8900 !important;
  }
  .map{
    position: relative;
    display: inline-block;
    img{
      display: block;
    }
  }
  .point{
    /*width: 10px;*/
    /*height: 10px;*/
    /*background: red;*/
    position: absolute;
    /*border-radius: 18px;*/
    /*box-shadow: 1px 0px 10px 3px;*/
    /*animation:mymove 1s infinite;*/
    /*-webkit-animation:mymove 1s infinite; !*Safari and Chrome*!*/
  }
  @keyframes mymove
  {
    0% { box-shadow:1px 0px 10px 5px;}
    50% { box-shadow:1px 0px 10px 0px;}
    100%{ box-shadow:1px 0px 10px 5px;}
  }

  @-webkit-keyframes mymove /*Safari and Chrome*/
  {
    0% { box-shadow:1px 0px 10px 5px;}
    50% { box-shadow:1px 0px 10px 0px;}
    100%{ box-shadow:1px 0px 10px 5px;}
  }
  .pointed{
    /*background: blue;*/
  }
  .pointTop{
    width: 250px;
    color: white;
    background: #022171;
    text-align: center;
    height: 140px;
    padding: 3px;
    border-radius: 5px;
    margin: -189px -54px;
    position: absolute;
    z-index: 9999;
  }
  .slc{
    /*width: 32px;*/
    /*height: 33px;*/
    /*line-height: 32px;*/
    /*border-radius: 99px;*/
    /*font-size: 35px;*/
    /*border: 2px solid blue;*/
    margin: 14px 9px;
    float: left;
  }

</style>
