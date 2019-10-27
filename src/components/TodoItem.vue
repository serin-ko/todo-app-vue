<template>
    <div class="item">
        <input type="checkbox" class="item-input" v-model="todo.complete">
        <div class="item-title">{{ todo.text }}</div>
        <button type="button" class="item-delete" v-on:click="deleteTodo">삭제</button>
    </div>
</template>
<script>
export default {
    props : ['todo'],
    methods : {
        deleteTodo : function(){
            this.$emit('delete',this.todo);
        }
    }
}
</script>