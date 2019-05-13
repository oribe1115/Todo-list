<template>
    <div class="body">
        <div class="title">TodoList</div>
        

        <div class="main">

            <div class="task unfinished">
                <div class="category-message">Let's begin!</div>
                <div v-for="item in items" :key="item.id">
                    <div v-if="!(item.finish)">
                        <div class="item">
                            <div class="check">
                                <div v-bind:class="{checked: item.keepStatus, unchecked: item.keepStatus == false}">
                                    <div @click="changeKeepStatus(item)">
                                        <font-awesome-icon icon="check" />
                                    </div>
                                </div>
                            </div>
                            <div class="name">{{ item.name }}</div>
                            <div class="star-box">
                                <div v-for="i in 5" :key="i">
                                    <div v-bind:class="{filled: i <= compareForFillStars(item), blank: i > compareForFillStars(item)}">
                                        <div @click="changeKeepImportance(i, item)" @mouseover="mouseOver(i, item)" @mouseleave="mouseLeave(item)">
                                            <font-awesome-icon icon="star" />
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>  

            <div class="for-change">
                <div class="category-message">New Task</div>
                <div class="buttons">
                    <div class="addition">
                        <label>
                            <input type="text" v-model="newItemName">
                        </label>
                        <button @click="addItem">add</button>
                    </div>
                    <div class="update">
                        <button @click="update">update</button>
                    </div>
                </div>
            </div>

            <div class="task finished">
                <div class="category-message">Finished</div>
                <div v-for="item in items" :key="item.id">
                    <div v-if="item.finish">
                        <div class="item">
                            <div class="check">
                                <div v-bind:class="{checked: item.keepStatus, unchecked: item.keepStatus == false}">
                                    <div @click="changeKeepStatus(item)">
                                        <font-awesome-icon icon="check" />
                                    </div>
                                </div>
                            </div>
                            <div class="name">{{ item.name }}</div>
                            <div class="star-box">
                                <div v-for="i in 5" :key="i">
                                    <div v-bind:class="{filled: i <= compareForFillStars(item), blank: i > compareForFillStars(item)}">
                                        <div @click="changeKeepImportance(i, item)" @mouseover="mouseOver(i, item)" @mouseleave="mouseLeave(item)">
                                            <font-awesome-icon icon="star" />
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            

            

        </div>
    </div>
    
</template>

<script>
import axios from "axios";

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
                    finish: true,
                    keepStatus: true,
                    importance: 3,
                    keepImportance: 3,
                    hoverStar: 0
                },
                {
                    id: 2,
                    name: "公開する",
                    finish: false,
                    keepStatus: false,
                    importance: 2,
                    keepImportance: 2,
                    hoverStar: 0
                }
            ],
            serverLink: "http://naro-todo-server.to-hutohu.trap.show/oribe/tasks"
        }
    },
    methods: {
        addItem() {
            if(this.newItemName != ""){
                const item = {
                    id: this.count,
                    name: this.newItemName,
                    finish: false,
                    keepStatus: false,
                    importance: 1,
                    keepImportance: 1,
                    hoverStar: 0
                };
                this.postToServer(item);
                this.getFromServer();    
                this.newItemName = "";
                this.count++;
            }
        },
        update() {
            for(const item of this.items){
                item.finish = item.keepStatus;
                item.importance = item.keepImportance;
            }
            this.sortWithImportance();
            this.updateForServer();
        },
        changeKeepImportance(i, item) {
            item.keepImportance = i;
        },
        mouseOver(i, item) {
            item.hoverStar = i;
        },
        mouseLeave(item) {
            item.hoverStar = 0;
        },
        compareForFillStars(item) {
            if(item.hoverStar != 0){
                return item.hoverStar;
            } else {
                return item.keepImportance;
            }
        },
        changeKeepStatus(item){
            if(item.keepStatus == true){
                item.keepStatus = false;
            } else {
                item.keepStatus = true;
            }
        },
        postToServer(item) {
            axios.post(this.serverLink, item);
        },
        getFromServer() {
            axios.get(this.serverLink).then(response => (this.items = response.data));
        },
        updateForServer(){
            axios.delete(this.serverLink);
            for(item in this.items) {
                axios.post(this.serverLink, item);
            }
        }
    },
    computed: {
        sortWithImportance(){
            this.items.sort((a, b) => b.importance - a.importance);
        }
    },
    mounted() {
        axios
            .get(this.serverLink)
            .then(response => (this.items = response.data))
    }
};
</script>

<style src="./TodoList.css"></style>