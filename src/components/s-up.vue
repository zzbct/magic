<!--
Copyright:MR-team
Author: bct
Date:2016-09
Description: 注册界面
-->
<style scoped>
    input{
        position: absolute;
        left:80px;
        top:0;
        width:220px;
        height:30px;
        border:1px solid grey;
        border-radius: 6px;
        padding: 5px;
    }
    p{
        position: relative;
        height: 42px;
        line-height: 42px;
        margin-bottom: 10px;
    }
    button{
        width:180px;
        height:30px;
        font-size: 20px;
        background-color: dodgerblue;
        border: none;
        border-radius: 6px;
    }
    .s-up{
        padding: 20px;
        border:1px solid dodgerblue;
    }
</style>
<template>
    <div class="s-up" clearfix>
            <p class="gap">
                <span>用户名:&nbsp</span>
                <input @blur="checkUser" type="text" placeholder="请输入用户名" v-model="data.name"/>
                <span class="error-msg">{{msg0}}</span>
            </p>
            <p class="gap">
                <span>手机号:&nbsp</span>
                <input @blur="checkPhone" type="text" placeholder="请输入有效的手机号" v-model="data.phone"/>
                <span class="error-msg">{{msg1}}</span>
            </p>
            <p class="gap">
                <span>邮箱:&nbsp</span>
                <input @blur="checkEmail" type="text" placeholder="请输入有效的邮箱地址" v-model="data.email"/>
                <span class="error-msg">{{msg2}}</span>
            </p>
            <p class="gap">
                <span>密码:&nbsp&nbsp</span>
                <input @blur="checkPsd" type="password" placeholder="以字母开头，长度在6~31之间" v-model="data.psd"/>
                <span class="error-msg">{{msg3}}</span>
            </p>
            <p class="gap">
                <span>确认密码:</span>
                <input @blur="matchPsd" type="password" placeholder="再次输入密码" v-model="confirm"/>
                <span class="error-msg">{{msg4}}</span>
            </p>
           <!--  <p class="gap">
               <span>授权口令:</span>
               <input type="text" placeholder="请输入授权口令" v-model="data.key"/>
               <span class="error-msg">{{msg4}}</span>
           </p> -->
            <button class="gap" @click="prin">注册</button>
    </div>
</template>
<script>
    module.exports = {
        data: function() {
            return {
                data:{
                    name:"",
                    phone:"",
                    email:"",
                    psd:"",
                    key:""
                },
                msg0:'',
                msg1:'',
                msg2:'',
                msg3:'',
                msg4:'',
                confirm:''
            }
        },
        ready: function() {
        },
        beforeDestroy: function() {

        },
        methods: {
//            用户名
            checkUser:function () {
                let vm = this;
                if(vm.data.name!=''){
                    vm.$http.post('nameExi',{UserName:vm.data.name})
                        .then((response) => {
                            if(response.data.Msg=='Success'){
                                vm.msg0="";
                            }
                            else{
                                vm.msg0=response.data.Msg;
                            }
                        })
                }
                else{
                  vm.msg0="用户名不能为空";
                }
            },
//            手机号正则
            checkPhone:function () {
                let vm = this;
                if(!(/^1[3|4|5|7|8]\d{9}$/.test(vm.data.phone))){
                    vm.msg1="手机号格式不正确";
                }
                else{
                   vm.msg1="";
                }
            },
//            邮箱正则
            checkEmail:function () {
                let vm =this;
                let reg=/^([a-zA-Z0-9_\.\-])+\@(([a-zA-Z0-9\-])+\.)+([a-zA-Z0-9]{2,4})+$/;
                if(!reg.test(this.data.email)){
                   this.msg2="邮箱格式不正确";
                }
                else{
                    this.msg2="";
                }
            },
//            密码正则
            checkPsd:function () {
                let vm=this;
                let reg = /^[a-zA-Z]\w{5,31}$/;
                if(!reg.test(this.data.psd)){
                    this.msg3="密码格式不正确";
                }
                else{
                    this.msg3="";
                }
            },
//            两次密码是否正确
            matchPsd:function(){
                let vm=this;
                if(vm.data.psd!=vm.confirm){
                    this.msg4="两次密码不一致";
                }
                else{
                    this.msg4="";
                }
            },
//            授权口令正则
            checkAcount:function () {

            },
            prin:function () {
                let vm=this;
                vm.checkUser();
                vm.checkPhone();
                vm.checkEmail();
                vm.checkPsd();
                vm.matchPsd();
                if(vm.msg1!=''||vm.msg2!=''||vm.msg3!=''||vm.msg4!=''){
                    confirm("请先纠正错误的信息！")
                }
                else{
                    let info={
                        UserName:vm.data.name,
                        Tel:vm.data.phone,
                        Email:vm.data.email,
                        Password:vm.data.psd,
                        AllowP:vm.data.key
                    };
                    vm.$http.post('reg',info)
                        .then((response) => {
                            if(response.data.Msg=='注册成功'){
                                 confirm("注册成功");
                                 vm.$dispatch('change_acount')
                            }
                            else{
                                confirm(response.data.Msg);
                            }
                        })
                }
            }
        }
    }
</script>
