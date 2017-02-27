<template>
	<div id="app">
		<form>
			<div>
				<label>用户名</label>
				<p><input placeholder="请输入用户名..." v-model="name"/></p>
			</div>
			<div>
				<label>密码</label> 
				<p><input v-model="pwd" type="password" placeholder="请输入密码..."/></p> 
			</div>
			<br/>
			<button @click="isLogin">登陆</button> 
		</form>
		</div>
	</template>
	<script>
		export default {
			data() {
				return {
					name:'',
					pwd:'',
					error:''
				}
			},
			computed: {
				user() {
					return this.$store.state.user
				}
			},
			methods:{
				isLogin:function() {
					this.$http.get('http://localhost:3000/users?username='+this.name+'&password='+this.pwd).then((response) => { 
						//这里在isLogin方法中先判断一下后台返回的是否为空值，如果不是然后提交后台返回的值。
						//注意这里是个难点，Vuex与Vue-Resource结合使用。 
						if(response.body != null & response.body.length > 0){ 
							this.$store.commit('isLogin',response.body[0]);
							this.name=''
							this.pwd= ''
							this.$router.push({ path: 'main' }) 
						}else{
							alert('请输入正确的用户名和密码！！！');
							this.name=''
							this.pwd= ''
						}
						
					}).then((error)=> this.error = error)
				}
			}
		}
	</script>
	<style>
		
	</style>