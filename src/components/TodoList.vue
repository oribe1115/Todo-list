<template>
    <div>
        <div>TodoList</div>
        <div v-for="item in items" :key="item.id">
            <div class="id">{{item.id}}</div>
            
            <input type="checkbox" id="checkbox" v-model="checked">
            
            <div class="finish">{{ item.finish }}</div>
            <div class="unchecked"> <font-awesome-icon icon="coffee" /></div>
            <div class="checked"> <font-awesome-icon icon="coffee" /></div>
            <div class="name">タスク名：{{ item.name }}</div>
        </div>

        <label for>
            タスク名
            <input type="text" v-model="newItemName">
        </label>
        <button @click="addItem">add</button>
    </div>
    
</template>

<script>
export default {
    name: "TodoList",
    data() {
        return {
            // 仮配列にあわせて適当な値から
            count: 3,
            newItemName: "",
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
    },
    methods: {
        addItem() {
            if(this.newItemName != ""){
                this.items.push({
                    id: this.count,
                    name: this.newItemName,
                    finish:false
                });
                this.newItemName = "";
                this.count++;
            }
        }
    }
};
</script>

<style>
.unchecked {
    color: rgb(194, 194, 194);
}
</style>