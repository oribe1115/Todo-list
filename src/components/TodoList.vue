<template>
    <div>
        <div>TodoList</div>

        <div class="unfinished">
            <div class="category-message">Let's begin!</div>
            <div v-for="item in items" :key="item.id">
                <div v-if="!(item.finish)">
                    <div class="id">{{item.id}}</div>
                
                    <input type="checkbox" id="checkbox" v-model="item.keepStatus">
                    <div class="name">タスク名：{{ item.name }}</div>
                </div>
            </div>
        </div>

        <div class="finished">
            <div class="category-message">Finished</div>
            <div v-for="item in items" :key="item.id">
                <div v-if="item.finish">
                    <div class="id">{{item.id}}</div>
                
                    <input type="checkbox" id="checkbox" v-model="item.keepStatus">
                    <div class="name">タスク名：{{ item.name }}</div>
                </div>
            </div>
        </div>
        

        <label for>
            タスク名
            <input type="text" v-model="newItemName">
        </label>
        <button @click="addItem">add</button>
        <button @click="update">update</button>
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
                    finish: false,
                    keepStatus: false
                },
                {
                    id: 2,
                    name: "公開する",
                    finish: false,
                    keepStatus: false
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
                    finish: false,
                    keepStatus: false
                });
                this.newItemName = "";
                this.count++;
            }
        },
        update() {
            for(const item of this.items){
                item.finish = item.keepStatus;
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