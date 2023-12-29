<template>
  <div>
    <h1 @click="showName">姓名:{{name}}</h1>
    <h2>年龄:{{myAge}}</h2>
    <h2>性别:{{sex}}</h2>
    <button @click="changeAge">修改年龄</button>
  </div>
</template>

<script>
import {my_mix_one} from "../myMixin";
export default {
  name: "Student"
  ,mixins:[my_mix_one]
 // ,props:['name','age','sex']
  ,props:{

    name:{
      type:String,
      required:true
    },
    age:{
      type:Number,
      required:true
    },
    sex:{
      type:String,
      required:true
    }
  }
  ,created(){
    // console.log(this.name)
    // console.log(this.age)
    // console.log(this.sex)
  }
,methods:{
    changeAge() {
      this.myAge = this.myAge + 10;
    }
  }
  ,data(){
    return {
      // name:"厦门测试学校"
      // ,age
      myAge :this.age
    }
  }
}
</script>

<style scoped>

</style>