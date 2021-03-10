<template>
<!--  <div class="demo-input-size">-->
<!--    请输入ip  <el-input size="meduim" id="Ping" type="text" v-model="loginForm.ping" placeholder="请输入用户名" clearable/>-->
<!--    <br><br>-->
<!--    密码： <el-input type="password" v-model="loginForm.password" placeholder="请输入密码" clearable/>-->
<!--    <br><br>-->
<!--    <button v-on:click="login">登录</button>-->
<!--  </div>-->
  <el-container>
    <el-header>PING & TELNET</el-header>
    <el-container>
    <el-aside width="350px">
      <el-row>
        PING
        </el-row>
      <el-row>
        <el-col><el-input v-model="pingForm.ping" placeholder="请输入ip" clearable></el-input> <el-button icon="el-icon-s-promotion" type="primary" v-on:click="ping" >连接</el-button></el-col>
      </el-row>
      <br>
      <el-row>
        TELNET
        </el-row>
      <el-row>
        <el-col> <el-input v-model="tellnetForm.ip" placeholder="请输入ip" clearable></el-input>  </el-col>
        &emsp;
        <el-col> <el-input  v-model="tellnetForm.port" placeholder="请输入端口号" clearable></el-input></el-col>
          <el-col>
            <el-button icon="el-icon-s-promotion" type="primary"  v-on:click="tellNet" >连接</el-button>
          </el-col>
      </el-row>
    </el-aside>
    <el-main>
      <el-row>
      <el-input
      type="textarea"
      placehodler="结果显示"
      v-model="hd"
      id="Result"
      prefix-icon="el-icon-ship"
      clearable
      disabled
      autofocus
      >
      </el-input>
      </el-row>
      <el-row>
        &nbsp;
      </el-row>
      <el-row>
        <el-button id="delete" type="danger" icon="el-icon-delete" circle @click="deleteAll"></el-button>
      </el-row>
      </el-main>
      </el-container>
  </el-container>
</template>

<script>

  export default {
    name: 'Login',
    data () {
      return {
        hd:'',
        pingForm: {
          ping: '',
        },
        tellnetForm:{
          ip:'',
          port:'',
        },
        responseResult: []
      }
    },
    methods: {
      ping () {
        var time =(new Date()).getHours()+":"+(new Date()).getMinutes()+":"+(new Date()).getSeconds()+"    "
        this.$axios
          .post('/ping', {
            ping: this.pingForm.ping,
          })
          .then(successResponse => {
            if (successResponse.data.code === 100) {
             // this.$router.replace({path: '/index'})
              this.hd=this.hd+"\n"+time+"ping成功"
            }
            if(successResponse.data.code === 200){
               this.hd=this.hd+"\n"+time+"ping失败,连接超时"
            }
            if(successResponse.data.code === 404){
              this.hd=this.hd+"\n"+time+"ping失败,以下是错误日志"
              this.hd=this.hd+"\n"+"---------------------------------------------------------"
              this.hd=this.hd+"\n"+successResponse.data.error
              this.hd=this.hd+"\n"+"---------------------------------------------------------"
            }
            if(successResponse.data.code === 401){
              this.hd=this.hd+"\n"+time+"ping------输入IP不能为空！！请重新输入正确IP"
            }
            if(successResponse.data.code === 300){
              this.hd=this.hd+"\n"+time+"ping------请输入正确的ip地址"
            }
          })
          .catch(failResponse => {
            alert("响应失败,请检查程序是否正常运行");
          })
      },
      tellNet(){
        var time = (new Date()).getFullYear()+"/"+((new Date()).getMonth()+1)+"/"+(new Date()).getDay()+" "+(new Date()).getHours()+":"+(new Date()).getMinutes()+":"+(new Date()).getSeconds()+"    "
        this.$axios
            .post('/tellNet', {
              ip:this.tellnetForm.ip,
              port:this.tellnetForm.port
            })
            .then(successResponse => {
              if(successResponse.data.code==200){
                this.hd=this.hd+"\n"+time+"telnet成功"
              }
              if(successResponse.data.code === 404){
                this.hd=this.hd+"\n"+time+"telnet失败,以下是错误日志"
                this.hd=this.hd+"\n"+"---------------------------------------------------------"
                this.hd=this.hd+"\n"+successResponse.data.error
                this.hd=this.hd+"\n"+"---------------------------------------------------------"
              }
              if(successResponse.data.code === 100){
                this.hd=this.hd+"\n"+time+"telnet------ip或端口号为空，请重新输入"
              }
              if(successResponse.data.code === 300){
                this.hd=this.hd+"\n"+time+"telnet------请输入正确的ip地址"
              }
              if(successResponse.data.code === 301){
                this.hd=this.hd+"\n"+time+"telnet------请输入正确的端口号"
              }
            })
            .catch(failResponse => {
              alert("响应失败,请检查程序是否正常运行");
            })
        },
      deleteAll(){
        this.hd='';
      }
      }
  }
</script>
<style>
.el-input{
  width:300px;
}
  #Result{
    color: black;
    height: 400px;
  }
  #delete{
  }

</style>
