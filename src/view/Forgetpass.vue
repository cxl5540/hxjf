<template>
 <div class="forget">
     <div class="header">
		 	<span @click="back()"><img src="../assets/b_fanhui.png"/>返回</span>
		 	<span>找回密码</span>
		 	<span style="opacity: 0;">title</span>
		 </div>
     <div class="msg">
     	<p>
     		<span><img src="../assets/b_shouji.png"/></span>
     		<input type="number"  placeholder="手机号" v-model="tel"/>
     	</p>
     	<p style="position: relative;">
     		<span><img src="../assets/b_yanzheng.png"/></span>
     		<input type="number"  placeholder="验证码" v-model="yzm"/>
     		<button  @click="getCode"  v-bind:class="{gray:wait_timer>0}">{{ getMobileCodeText() }}</button>
     	</p>
     	<p>
     		<span><img src="../assets/b_mima.png"/></span>
     		<input type="text" v-model="password"  placeholder="请输入新密码"/>
     	</p>
     </div>
     <div class="btn">
     	<button @click="submit">找回密码</button>
     </div>
 </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  components:{

  },
  data () {
    return {
    	tel:'',
    	check:false,
    	code:'',
    	yzm:'',
    	password:'',
    	wait_timer:false,
    }
  },
 
  created(){
  },
  mounted(){
		
  },
  methods:{
	back(){
		this.$router.go(-1)
	},
	 getCode(){
	  	if (this.wait_timer > 0) {
                return false;
            }
	  	if (!this.tel) {
               this.$toast('手机不能为空！');
                return false;
            }
            if(!/^1[3|4|5|7|8]\d{9}$/.test(this.tel)){
                 this.$toast('手机格式有误！');
                return false;
           }
		 this.wait_timer = 59;
            var that = this;
            var timer_interval = setInterval(function(){
                if(that.wait_timer > 0){
                    that.wait_timer -- ;
                }else{
                    clearInterval(timer_interval);
                }
            },1000);
			this.getForgetAuthCode()
              //在这里调取你获取验证码的ajax
		},
		getMobileCodeText(){
            if(this.wait_timer > 0){
                return this.wait_timer+'s后获取';
            }          
            if(this.wait_timer === 0){
                return '重新获取';
            }           
            if(this.wait_timer === false){
                return '获取验证码';
            }
           
        },
        getForgetAuthCode(){  //获取验证码
        	$('#loading').show()
			let _this=this;
	  		$.ajax({
			 	dataType:"json", 
			 	type:"post",
			 	url:this.testUrl+'mobile/getForgetAuthCode',
			 	data:{
			 		phone:this.tel,
			 	},
			 	
			 	success:function(res){
		       		if(res.code==200){
		       		
		       		}
		       
		         },          
		         error:function(res){
		          _this.$toast(res.msg);
		         },
		        complete:function(){
		        	$('#loading').hide()
		        }
			 });
       },
       submit(){   //设置密码
       	if(this.checkreg()){       	
	       	$('#loading').show()
				let _this=this;
		  		$.ajax({
				 	dataType:"json", 
				 	type:"post",
				 	url:this.testUrl+'mobile/retrievePassword',
				 	data:{
				 		phone:this.tel,
				 		password:this.password,
				 		authCode:this.yzm 
				 	},
				 	
				 	success:function(res){
						$('#loading').hide()
			       		if(res.code==200){							
			       		    _this.$toast(res.msg);	
						  _this.$router.push({path:'/login'})
			       		}else{
						_this.$toast(res.msg);	
						}
			       
			         },          
			         error:function(res){
			          _this.$toast(res.msg);
			         },
			        complete:function(){
			        	$('#loading').hide()
			        }
			 });
		 }
      },
     checkreg(){  //验证
      	var reg1 = /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,20}$/;  //姓名
       if(!reg1.test(this.password) || this.password==''){
      		 this.$toast('密码为6-20位数字+字母');
      		return false;
      	}else{
      		return true;
      	}
      	
      },
  }
 }	
</script>

<style scoped>
.msg,.btn{
	box-sizing: border-box;
	margin: 0 0.66rem;
}	
.msg>p{
	width: 100%;
	padding: 0.53rem 0;	
	text-align: left;
}
.msg>p>span{
	width: 25%;
	color: #333333;
	font-size: 0.42rem;
}
.msg>p>span>img{
	max-height: 0.42rem;
	margin-right: 0.26rem;
	position: relative;
	    top: 10px;
}
.msg input{
	width:90%;
	border: none;
	border-bottom: 1px solid #eee;
	border-radius: 0;
	font-size: 0.48rem;
	padding: 0.26rem 0;

}
.btn>button{
	width:8.66rem;
   height:1.33rem;
   background: #3168FA;
   color: #fff;
   font-size: 0.49rem;
   border: none;
   border-radius:6px;
   margin-top: 1.33rem;
}
p>button{
	width: 2.4rem;
	height: 0.8rem;
	background: #3168FA;
	color: #fff;
	font-size: 0.32rem;
	border-radius: 6px;
	position: absolute;
	right: 0;
	top: 0.74rem;
}
.gray{
	background: #C0C0C0;
}	
</style>