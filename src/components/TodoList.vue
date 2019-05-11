<template>
    <div class="body">
        <div class="title">TodoList</div>
        

        <div class="main">

            <div class="list">
                <div class="task unfinished">
                    <div class="category-message">Let's begin!</div>
                    <div v-for="item in items" :key="item.id">
                        <div v-if="!(item.finish)">
                            <div class="item">
                                <input type="checkbox" id="checkbox" v-model="item.keepStatus">
                                <div class="name">{{ item.name }}</div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="task finished">
                    <div class="category-message">Finished</div>
                    <div v-for="item in items" :key="item.id">
                        <div v-if="item.finish">
                            <div class="item">
                                <input type="checkbox" id="checkbox" v-model="item.keepStatus">
                                <div class="name">{{ item.name }}</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            

            <div class="for-change">
                <label for>
                    タスク名
                    <input type="text" v-model="newItemName">
                </label>
                <button @click="addItem">add</button>
                <button @click="update">update</button>
            </div>

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
            newItemName: "",
            items: [
                {
                    id: 1,
                    name: "TODOリストを作る!",
                    finish: false,
                    keepStatus: false,
                    importance: 0,
                    keepImportance: 0
                },
                {
                    id: 2,
                    name: "公開する",
                    finish: false,
                    keepStatus: false,
                    importance: 0,
                    keepImportance: 0
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
                    keepStatus: false,
                    importance: 0,
                    keepImportance: 0
                });
                this.newItemName = "";
                this.count++;
            }
        },
        update() {
            for(const item of this.items){
                item.finish = item.keepStatus;
                item.importance = item.keepImportance;
            }
        }
    }
};
</script>

<style src="./TodoList.css"></style>