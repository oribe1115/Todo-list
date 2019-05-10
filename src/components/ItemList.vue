<template>
  <div>
    <div>ItemList</div>
    <div v-for="item in items" :key="item.name">
      <div class="item">
        <div class="name">名前: {{ item.name }}</div>
        <div class="price">{{ item.price }} 円</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ItemList",
  data() {
    return {
      items: [
        {name: 'たまご', price: 100},
        {name: 'りんご', price: 160}
      ]
    }
  }
};
</script>