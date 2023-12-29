<template>
  <li>
    <label>
      <!--      这里勾选和取消勾选可以使用change和click作为事件处理-->
      <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)"/>
      <!--v-model数据的双向绑定，checkbox使用v-model来双向绑定其是否被勾选,也可以实现效果但不推荐(因为其实修改了props中的数据)-->
      <!--这里修改了从List修改过来的props,这里的不允许改是浅层次，就是如果props是一个对象则这个修改这个对象的某一个属性vue是放行的-->
      <!-- <input type="checkbox" v-model="todo.done"/>-->
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input v-show="todo.isEdit" :value="todo.title" @blur="blurEdit(todo,$event)"
            ref="titleEdit"
      />
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    <button class="btn btn-Edit" v-show="!todo.isEdit" @click="handleEdit(todo)">编辑</button>
  </li>
</template>

<script>
export default {
  name: "TodoItem",
  //声明接收todo
  props: ['todo'],
  methods: {
    handleCheck(id) {
      // this.checkTodo(id);
      this.$bus.$emit('checkTodo', id);
    },
    handleDelete(id) {
      if (confirm(`确定删除编号为${id}的todo吗`)) {
        // console.log(id);
        // this.deleteTodo(id);
        this.$bus.$emit('deleteTodo', id);
      }
    }
    ,// 处理编辑
    handleEdit(todo) {
      //
      if (todo.hasOwnProperty('isEdit')) {
        todo.isEdit = true;
      } else {
        this.$set(todo, 'isEdit', true);
      }

      this.$nextTick(function () {
        this.$refs.titleEdit.focus();
      });


    }
    , blurEdit(todo, e) {
      // console.log('blur')
      todo.isEdit = false;
      if (e.target.value.trim().length === 0) {
        alert('内容不能为空');
       // todo.isEdit = false;
        return;
      }
      // 失去焦点
      // console.log(e.target.value);
      // this.editTodo(e.target.value, this.todo.id);
      if ( e.target.value.trim() !== this.todo.title.trim()) {
        this.$bus.$emit('editTodo', this.todo.id, e.target.value);
      }

    }
  }
}
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background: #ddd;
}

li:hover button {
  display: block;
}
</style>
