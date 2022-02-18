<template>
  <ul class="list" v-bind:class="listempty">
    <li class="list__item" v-for="(todoItem, index) in propsdata" v-bind:key="todoItem.item">
      <input type="checkbox" v-bind:id="todoItem.item" 
        v-bind:checked="todoItem.completed === true"
        v-on:change="toggleComplete(todoItem)"
      />
      <label v-bind:for="todoItem.item" class="list__label">
        <p class="list__text">{{todoItem.item}}</p>
      </label>
      <p class="list__date">{{todoItem. date}}</p>
      <button class="list__delete" v-on:click="removeTodo(todoItem, index)">
        <div class="blind">Delete</div>
      </button>
    </li>
  </ul>
</template>
<script>
export default {
  computed: {
    listempty() {
      return this.propEmpty ? "list--empty" : null;
    }
  },
  props:["propsdata", "propItems", "propEmpty"],
  methods: {
    removeTodo(todoItem, index) {
      this.$emit("removeItem", todoItem, index);
    },
    toggleComplete(todoItem){
      this.$emit("toggleItem", todoItem);
    }
  },
};

</script>