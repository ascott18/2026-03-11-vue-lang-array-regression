<script setup lang="ts">
import { ref } from "vue";

class MapperArray<T, TInput> extends Array<T> {
  constructor(private mapper: (item: T | TInput) => T) {
    super();
  }

  override push(...items: (T | TInput)[]) {
    return super.push(...items.map(this.mapper));
  }
}

const array = ref(
  new MapperArray<string, string | number>((input) => input.toString()),
);
</script>

<template>
  <div v-for="item in array" :key="item">
    {{ item.includes("foo") ? "bar" : item }}
    <!-- ^^^^^^^^^^^ `item` type should be `string`, but is now `string | number` in 3.2.1+ -->
  </div>
</template>
