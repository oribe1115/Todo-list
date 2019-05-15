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
                                <div v-bind:class="{checked: item.finish, unchecked: item.finish == false}">
                                    <div @click="changeStatus(item)">
                                        <font-awesome-icon icon="check" />
                                    </div>
                                </div>
                            </div>
                            <div class="name">{{ item.name }}</div>
                            <div class="star-box">
                                <div v-for="i in 5" :key="i">
                                    <div v-bind:class="{filled: i <= compareForFillStars(item), blank: i > compareForFillStars(item)}">
                                        <div @click="changeImportance(i, item)" @mouseover="mouseOver(i, item)" @mouseleave="mouseLeave(item)">
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
                                <div v-bind:class="{checked: item.finish, unchecked: item.finish == false}">
                                    <div @click="changeStatus(item)">
                                        <font-awesome-icon icon="check" />
                                    </div>
                                </div>
                            </div>
                            <div class="name">{{ item.name }}</div>
                            <div class="star-box">
                                <div v-for="i in 5" :key="i">
                                    <div v-bind:class="{filled: i <= compareForFillStars(item), blank: i > compareForFillStars(item)}">
                                        <div @click="changeImportance(i, item)" @mouseover="mouseOver(i, item)" @mouseleave="mouseLeave(item)">
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
            count: 1,
            newItemName: "",
            items: [],
            serverLink: "https://to-hutohu.trap.show/naro-todo-server/oribe/tasks"
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
                this.newItemName = "";
                this.count++;
                this.postToServer(item);
            }
        },
        update() {
            // for(const item of this.items){
            //     item.finish = item.keepStatus;
            //     item.importance = item.keepImportance;
            // }
            this.sortWithImportance;
        },
        changeImportance(i, item) {
            item.importance = i;
            axios.put(this.serverLink + "/" + item.id, item);
        },
        mouseOver(i, item) {
            item.hoverStar = i;
            axios.put(this.serverLink + "/" + item.id, item);
        },
        mouseLeave(item) {
            item.hoverStar = 0;
            axios.put(this.serverLink + "/" + item.id, item);
        },
        compareForFillStars(item) {
            if(item.hoverStar != 0){
                return item.hoverStar;
            } else {
                return item.importance;
            }
        },
        changeStatus(item){
            if (item.finish == true){
                item.finish = false;
                axios.put(this.serverLink + "/" + item.id, item);
            } else {
                item.finish = true;
                axios.put(this.serverLink + "/" + item.id, item);
            }
        },
        postToServer(item) {
            axios.post(this.serverLink, item).then(response => (this.items = response.data));
        },
        getFromServer() {
            axios.get(this.serverLink).then(response => (this.items = response.data));
        }
        /*
        ,
        async updateForServer(){
            for(const item of this.items){
                await axios.put(this.serverLink + "/" + item.id, item);
            }
            this.getFromServer();
        }
        */
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