<template>
    <div>
        <div>TodoList</div>
        <div v-for="item in items" :key="item.id">
            <div class="id">{{item.id}}</div>
            <div class="name">タスク名：{{ item.name }}</div>
        </div>
    </div>
    
</template>

<script>
export default {
    name: "TodoList",
    data() {
        return {
            // 仮配列にあわせて適当な値から
            count: 3,
            items: [
                {
                    id: 1,
                    name: "TODOリストを作る!",
                    finish: false
                },
                {
                    id: 2,
                    name: "公開する",
                    finish: false
                }
            ]
        }
    }
};
</script>

<style>
</style>