<template>
  <div class="warp_main">
    <div class="basic">
      <div class="basic_title mindex_top ac">补充信息</div>
      <!-- 会议补充信息 -->
      <div class="mindex_top table_top">
        <table class="mindex_table">
          <tr>
            <td class="table_name">预定台型</td>
            <td>
              <input class="mindex_input" type="text" name="" v-model="cname" readonly="readonly">
            </td>
          </tr>
          <!-- 对接 -->
          <tr>
            <td class="table_name">是否对接</td>
            <td>
              <template v-cloak v-if="Number(isDockingState)==1">
                是
              </template>
              <template v-cloak v-else>
                否
              </template>
            </td>
          </tr>
          <template v-cloak v-if="Number(isDockingState)==1">
            <tr>
              <td class="table_name">对接时间</td>
              <td>
                <input class="mindex_input form-control" id='datetime' type="type" name="" v-model="ocdockingtime"
                  readonly="readonly" style="background-color:#fff;border:none;ouline:none;box-shadow:none;padding-left:0;color:#000;">
              </td>
            </tr>
            <tr>
              <td class="table_name">使用联系人</td>
              <td><input class="mindex_input" type="text" name="" v-model="ocusename" readonly="readonly"></td>
            </tr>
            <tr>
              <td class="table_name">联系电话</td>
              <td><input class="mindex_input" type="number" name="" v-model="ocusephone" readonly="readonly"></td>
            </tr>
          </template>
          <!-- 对接 end -->
        </table>
      </div>
      <!-- 车位预留情况 -->
      <div class="mindex_top table_top basic">
        <table class="mindex_table">
          <caption class="table_title">车位预留情况</caption>
          <tr style="border-top: none;">
            <td class="table_name">大车位数量</td>
            <td><input class="mindex_input" type="number" name="" v-model="ocbigcarnum" readonly="readonly"></td>
          </tr>
          <tr>
            <td class="table_name">小车位数量</td>
            <td><input class="mindex_input" type="number" name="" v-model="ocsmallnum" readonly="readonly"></td>
          </tr>
          <!-- <tr>
            <td class="table_name">会议议程</td>
            <td><input class="mindex_input" type="text" name="" v-model="ocagenda" readonly="readonly"></td>
          </tr>
          <tr>
            <td class="table_name">打印份数</td>
            <td><input class="mindex_input" type="number" name="" v-model="ocprintnum" readonly="readonly"></td>
          </tr>
          <tr>
            <td class="table_name">主席台是否摆放</td>
            <td>
              <input class="table_radio" type="radio" name="rostrum" value="1" v-model="isOcplatform" disabled="disabled">是
              <input class="table_radio" type="radio" name="rostrum" value="2" v-model="isOcplatform" disabled="disabled">否
            </td>
          </tr> -->
        </table>
      </div>
      <!-- 会议布展情况 -->
      <div class="mindex_top table_top basic">
        <table class="mindex_table">
          <caption class="table_title">布展情况</caption>
          <tr style="border-top: none;">
            <td class="table_name">展台</td>
            <td><input class="mindex_input" type="number" name="" v-model="ocshowplatform" readonly="readonly"></td>
          </tr>
          <tr>
            <td class="table_name">展架</td>
            <td><input class="mindex_input" type="number" name="" v-model="ocshowshelf" readonly="readonly"></td>
          </tr>
        </table>
      </div>
      <!-- 桌椅及背签顺序 -->
      <div class="add_name mindex_top table_top basic">
        <table class="mindex_table" v-cloak v-if="peoqueueArr.length>0">
          <caption class="table_title">桌椅及背签顺序</caption>
          <tr style="border-top: none;">
            <!-- <td><input class="mindex_input ac" type="text" name=""></td>
            <td><input class="mindex_input ac" type="text" name=""></td>
            <td><input class="mindex_input ac" type="text" name=""></td>
            <td><input class="mindex_input ac" type="text" name=""></td> -->
            <div class="tableSignWrap">
              <span class="tableSignItem" v-for="(item,index) in peoqueueArr" :key="index">{{item}}</span>
            </div>
          </tr>
        </table>
      </div>
      <!-- 附件备注 -->
      <div class="mindex_top table_top basic">
        <table class="mindex_table">
          <tr>
            <td class="table_name" style="vertical-align:top;">附件</td>
            <td>
              <div v-cloak v-if="annexArr.length>0">
                <p style="overflow: hidden;text-overflow:ellipsis;white-space: nowrap;" v-for="(item,index) in annexArr"
                  :key="index">
                  <a style="font-size:15px;" :href="Global.domain+Global.host+item" :download="item">{{item}}</a>
                </p>
              </div>
              <div v-cloak v-else style="font-size:15px;">无附件</div>
            </td>
          </tr>
          <tr>
            <td colspan="2" class="table_name">备注:</td>
          </tr>
          <tr>
            <td class="remarks" colspan="2">
              <textarea placeholder="请描述一下详细内容～" v-model="ocremark" readonly="readonly"></textarea>
            </td>
          </tr>
        </table>
      </div>
    </div>

    <!-- 取消预定弹窗 -->
    <div class="modal fade" id="mySearch" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content" style="margin-top: 50px;">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal" aria-hidden="true">
              &times;
            </button>
            <h4 class="rej_title modal-title" id="myModalLabel">取消原因</h4>
          </div>
          <div class="modal-body">
            <div>
              <textarea class="rej_textarea" v-model="cancleContent"></textarea>
            </div>
          </div>
          <div class="modal-footer">
            <button type="button" class="rej_button btn cancle" @click="goCancle">确定</button>
          </div>
        </div>
      </div>
    </div>

    <!--提交按钮-->
    <div class="kong"></div>
    <div class="button_btn">
      <!-- <button @click="leftButton" v-bind:class="{'submit':yesc,'submitChange':noc}">{{leftButtonText}}</button>
      <button @click="rightButton" v-bind:class="{'mindex_tab cancle':yesB,'submitChange':noB}">{{rightButtonText}}</button> -->
      <button style="background-color:#fff;" @click="routerGoback">返回</button>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        Global: Global,

        isDockingState: 2, //是否对接
        isOcplatform: "",
        ocid: this.$route.query.ocid,
        dataArry: [], //data总数数据
        ocdockingtime: "",
        ocusename: "",
        ocusephone: "",
        ocbigcarnum: 0,
        ocsmallnum: 0,
        ocagenda: "",
        ocprintnum: 0,
        ocshowplatform: 0,
        ocshowshelf: 0,
        cname: "",
        ocremark: "",
        rid: 0,

        /*按钮状态*/
        yesB: true,
        noB: false,
        yesc: true,
        noc: false,
        leftButtonText: "", //左侧按钮显示文字
        rightButtonText: "", //右侧按钮显示文字
        right: 0, //权限
        opassstate: 3, //假设为3，该值是有上一个页面传来***********
        repulseContent: "", //拒绝原因
        cancleContent: "", //取消原因内容


        //桌签
        peoqueueArr: [],
        //附件
        annexArr: [],
      };
    },
    mounted() {
      this.initDo();
    },
    methods: {
      initDo() {
        let postData = {
          ocid: this.ocid
        };
        let self = this;
        Global.openLoading();
        this.axios
          .post(Global.host + "/order/selectOneInfo", this.qs.stringify(postData))
          .then(function (res) {
            Global.closeLoading();
            console.log(res);
            self.dataArry = res.data;
            console.log(self.dataArry)
            let beginTime = self.dataArry[0].ocdockingtime;
            self.ocdockingtime = beginTime != null ? Global.dateToFormat(new Date(beginTime)) : "";
            //self.ocdockingtime = Global.dateToFormat(new Date(beginTime))
            self.ocusename = self.dataArry[0].ocusename;
            self.ocusephone = self.dataArry[0].ocusephone;
            self.ocbigcarnum = self.dataArry[0].ocbigcarnum;
            self.ocsmallnum = self.dataArry[0].ocsmallnum;
            self.ocagenda = self.dataArry[0].ocagenda;
            self.ocprintnum = self.dataArry[0].ocprintnum;
            self.ocshowplatform = self.dataArry[0].ocshowplatform;
            self.ocshowshelf = self.dataArry[0].ocshowshelf;
            self.cname = self.dataArry[5];
            self.ocremark = self.dataArry[0].ocremark;

            self.rid = self.dataArry[0].rid;
            self.isDockingState = self.dataArry[0].ocdockingstate;
            self.isOcplatform = self.dataArry[0].ocplatform;
            //桌签
            if (self.dataArry[0].peoqueue && self.dataArry[0].peoqueue !== "null" && self.dataArry[0].peoqueue !== "") {
              self.peoqueueArr = self.dataArry[0].peoqueue.split(",")
            }
            //附件
            if (self.dataArry[0].ocaccessory && self.dataArry[0].ocaccessory !== "" && self.dataArry[0].ocaccessory !== "null") {
              let arr = self.dataArry[0].ocaccessory.split(",") //多一个“，”
              self.annexArr = arr.slice(0, arr.length - 1)
            }


            /*用户权限控制*/
            let session = localStorage.getItem("mUserInfo");
            let mySession = JSON.parse(session);
            let right = JSON.parse(localStorage.getItem("userRight"));
            right = 1; //测试临时数据
            self.right = right;
            /*更改按钮点击状态*/
            let state = self.dataArry[6];
            console.log(state);
            if (state == 0 && (right == 1 || right == 4)) {
              self.noB = true;
              self.yesB = false;
            }
            console.log(self.dataArry[3]);
            if (self.dataArry[3] != null && (right == 1 || right == 4)) {
              (self.yesc = false), (self.noc = true);
            }
            /*路由传值，获得状态opassstate*/
            if (self.opassstate != 3) {
              self.noB = true;
              self.yesB = false;
              (self.yesc = false), (self.noc = true);
            }
            //修改左侧和右侧按钮文字
            switch (self.right) {
              case 1:
                self.leftButtonText = "评价";
                self.rightButtonText = "取消";
                break;
              case 2:
                self.leftButtonText = "通过";
                self.rightButtonText = "拒绝";
                break;
              case 3:
                break;
              case 4:
                // alert("该用户没有权限")
                self.leftButtonText = "评价";
                self.rightButtonText = "取消";
                break;
              case 5:
                //管理员页面？
                break;
              default:
                break;
            }
          })
          .catch(function (res) {
            Global.closeLoading();
          });
        /*用户权限控制*/
        let session = localStorage.getItem("mUserInfo");
        let mySession = JSON.parse(session);
        let right = JSON.parse(localStorage.getItem("userRight"));
        self.right = right;
        //修改左侧按钮文字
        switch (right) {
          case 1:
            self.leftButtonText = "评价";
            self.rightButtonText = "取消";
            break;
          case 2:
            break;
          case 3:
            break;
          case 4:
            // alert("该用户没有权限")
            self.leftButtonText = "评价";
            self.rightButtonText = "取消";
            break;
          case 5:
            //管理员页面？
            break;
          default:
            break;
        }
      },
      goComment() {
        if (this.dataArry[3] == null) {
          this.$router.push({
            path: "/mlogin/memberlist/morderContent/base/mreview",
            query: {
              ocid: this.ocid,
              rid: this.rid
            }
          });
        } else {
          alert("你已经评论过啦");
        }
      },
      goCancleTan() {
        /*弹出弹窗*/
        $("#mySearch").modal("show");
      },
      goCancle() {
        /*弹出弹窗*/
        // $("#mySearch").modal("show")
        let self = this;
        let postData = {
          ocid: self.ocid,
          cancleContent: self.cancleContent
        };
        console.log(postData);

        $.ajax({
          type: "POST",
          url: "/order/memberUpdateOrder",
          data: postData,
          success: function (res) {
            if (res == 1) {
              alert("操作成功");
            } else {
              alert("操作失败");
            }
          }
        });
      },
      passCheck() {
        //取出oid
        let oid = this.oid;
        let pass = 2;
        let postData = {
          oid: oid,
          pass: pass
        };
        $.ajax({
          type: "POST",
          url: "/order/approveOrder",
          data: postData,
          success: function (res) {
            //console.log(res)
            if (res == 1) {
              alert("操作成功");
            } else {
              alert("操作失败");
            }
          }
        });
      },
      repulse() {
        /*弹出弹窗*/
        $("#repulse").modal("show");
      },
      goRepulse() {
        //取出oid
        let oid = this.oid;
        let pass = 1;
        let message = this.repulseContent;
        let postData = {
          oid: oid,
          pass: pass,
          message: message
        };
        $.ajax({
          type: "POST",
          url: "/order/approveOrder",
          data: postData,
          success: function (res) {
            //console.log(res)
            if (res == 1) {
              alert("操作成功");
            } else {
              alert("操作失败");
            }
          }
        });
      },
      leftButton() {
        let right = this.right;
        switch (right) {
          case 1:
            this.goComment();
            break;
          case 2:
            //通过按钮
            this.passCheck();
            break;
          case 3:
            break;
          case 4:
            // alert("该用户没有权限")
            this.goComment();
            break;
          case 5:
            //管理员页面？
            break;
          default:
            break;
        }
      },
      rightButton() {
        let right = this.right;
        switch (right) {
          case 1:
            this.goCancleTan(); //取消按钮
            break;
          case 2:
            //拒绝按钮
            this.repulse();
            break;
          case 3:
            break;
          case 4:
            // alert("该用户没有权限")
            this.goCancleTan(); //取消按钮
            break;
          case 5:
            //管理员页面？
            break;
          default:
            break;
        }
      },
      routerGoback() {
        //返回上一个路由
        this.$router.go(-1);
      }
    } //methods end
  };
</script>

<style scoped>
  @import "../assets/css/bootstrap.min.css";
  @import "../assets/css/reset.css";
  @import "../assets/css/mindex.css";
  @import "../assets/css/mdetail.css";

  .tableSignWrap {
    display: flex;
    flex-wrap: wrap;
  }

  .tableSignItem {
    border: 1px solid #e0e0e0;
    width: calc(25% - 6px);
    margin: 3px;
    text-align: center;
  }
</style>
