<template>
	<div class="header">
		   <span  :style="{'opacity': (path=='/index' ? '1':path=='/' ?'1':'0')}" @click="changelg">{{$store.state.lg=='C'?'English':'简体中文'}}</span>
      	<span>银河金服</span>
      	<span @click="gomine">{{name}}</span>
	</div>
</template>

<script>
export default {
	  name: 'tabar',
	  components:{
	  },
	  data () {
	    return {
		  name:'未登录',
      path:this.$route.path
	    }
	  },

	  created(){
	  	this.getinfo();
      this.path=this.$route.path

	  },
	  mounted(){

	  },
	  methods:{
		gomine(){
			if(localStorage.getItem('uid')&&this.name!=='未登录'){
				this.$router.push({path:'/mine'})
			}else{
				this.$router.push({path:'/login'})
			}
		 },
		 getinfo(){
		   	let _this=this;
		   	$.ajax({
			 	dataType:"json", 
			 	type:"get",
			 	url:this.testUrl+'mobile/getMyCenter',
			 	data:{
			 		uid:localStorage.getItem('uid')
			 	},
			 	success:function(res){
		       		if(res.code==200){
		       			if(res.data.realName){
		       				 _this.name=res.data.realName
		       			}else{
		       				_this.name='未登录';
		       			}

		       		}
		         },
		         error:function(res){
		           _this.$toast('网络错误');
		         },
		        complete:function(){
		        	$('#loading').hide()
		        }
			 });
		   },
       changelg(){
         console.log(this.$store.state.lg)
          if(this.$store.state.lg=='C'){
            this.$store.state.lg='N'
          }else{
           this.$store.state.lg='C'
          }
          
         }
	  	}
	  }
</script>

<style scoped="scoped">
	.header{
		height: 1.17rem;
		line-height: 1.17rem;
		font-size: 0.48rem;
		color: #333333;
		background: #fff;
		display: flex;
		justify-content: space-between;
		box-sizing: border-box;
		padding: 0 0.26rem;
	}
	.header>span{
		display: inline-block;
		width: 33%;
	}
	.header>span:nth-child(1){
		text-align: left;
	}
	.header>span:nth-child(3){
		text-align: right;
	}
</style>
