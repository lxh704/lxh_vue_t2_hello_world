<template>
  <div>
    <h1 @click="showName">{{name}}</h1>
    <h2>{{address}}</h2>
  </div>
</template>

<script>
import {my_mix_one} from "@/myMixin";
export default {
  name: "School"
  ,mixins:[my_mix_one]
  ,data(){
    return {
      // master 中 11111111
      // 这个是在hot fix中写的注释
      name:"厦门测试学校 real third commit"
      ,address:"厦门软件园"
    }
  }
}
</script>

<style scoped>

</style>