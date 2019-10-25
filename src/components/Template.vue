<template>
  <div class="wrapper" >
		<Header msg="Orçamento de marmoraria"></Header>    
		<Content></Content>
		<Footer></Footer>
  </div>
</template>

<script>

import Header from "./Header.vue";
import Content from "./Content.vue";

export default {
  name: 'Template',
  props: {
    msg: String
	},
	components:{
		Header,
		Content
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
