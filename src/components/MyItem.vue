<template>
  <li>
    <label>
      <input type="checkbox" :checked = "todo.done" @change="handleCheck(todo.id)"/>
      <!-- 如下代码也能实现功能，但是不太推荐，因为修改了Props的值 -->
      <!-- <input type="checkbox" v-model="todo.done"/> -->

      <span v-show="!todo.isEdit">{{todo.title}}</span>
      <input
       v-show="todo.isEdit" 
       type="text" 
       :value="todo.title"
       @blur="handleBlur(todo,$event)"
       ref="inputTitle"
       >
    </label>
    
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    <button v-show="!todo.isEdit" class="btn btn-edit" @click="handleEdit(todo)">编辑</button>
  </li>
</template>

<script>
export default {
  name: 'MyItem' ,
  props:['todo'],
  methods:{
    //勾选
    handleCheck(id){
      // 将todo对象的done值取反
      // this.checkTodo(id)
      this.$bus.$emit('checkTodo',id)
    },
    // 删除
    handleDelete(id){
      if(confirm("确认删除吗")){
        // this.deleteTodo(id)
          this.$bus.$emit('deleteTodo',id)
      }
    },
    handleEdit(todo){
      if(todo.hasOwnProperty('isEdit')){
        todo.isEdit = true
      }else{
        console.log('@')
        this.$set(todo,'isEdit',true)
      }
      this.$nextTick(function(){
        this.$refs.inputTitle.focus()
      })
      
    },
    //失去焦点回调
    handleBlur(todo,e){
      todo.isEdit = false
      if(!e.target.value.trim()) return alert('输入数据不能为空')
      this.$bus.$emit('updataTodo',todo.id,e.target.value)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">


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

  li:hover{
    button{
      display: block;
    }
    background-color: #ddd;
  }
</style>
