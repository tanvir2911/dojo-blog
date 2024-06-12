<template>
  <div class="home">
    <p ref="p">My name is {{ name }} and my age is {{ age }}</p>
    <button @click="handleClick">click me</button>
    <p>My name is {{ name1 }} and my age is {{ age1 }}</p>
    <button @click="handleClick1">click me</button>
    <button @click="age1++">add 1 to age</button>
    <input type="text" v-model="name1" />

    <hr />
    <h2>refs vs reactive</h2>
    <h3>Refs</h3>
    <p>{{ ninjaOne.name }} - {{ ninjaOne.age }}</p>
    <button @click="updateNinjaOne">Update ninja one</button>
    <h3>Reactive</h3>
    <p>{{ ninjaTwo.name }} - {{ ninjaTwo.age }}</p>
    <button @click="updateNinjaTwo">Update ninja two</button>

    <hr />
    <h2>Computed values</h2>
    <input type="text" @keyup="searchName" v-model="search" />
    <div v-for="name in matchingNames" :key="name">
      {{ name }}
    </div>

    <hr />
    <h2>watch and watchEffect</h2>
    <button @click="stopWatching">stop watching</button>
  </div>

  <!-- props -->
  <h2>using props in setup</h2>
  <PostList :posts="posts" />
</template>

<script>
import { computed, reactive, ref, watch, watchEffect } from "vue";
import PostList from "../components/PostList.vue";
// @ is an alias to /src

export default {
  name: "HomeView",
  components: { PostList },
  setup() {
    // non reactive variables
    let name = "mario";
    let age = 30;

    // ref for template referencing
    const p = ref(null);
    console.log(p, p.value);

    // ref for reactive values
    const name1 = ref("Tanvir");
    const age1 = ref(35);

    const handleClick = () => {
      console.log(p, p.value);
      p.value.classList.add("test");
      p.value.textContent = "hello, ninjas";
    };
    const handleClick1 = () => {
      name1.value = "Tonmoy";
      age1.value = 55;
    };

    // refs vs reactive
    // refs
    const ninjaOne = ref({ name: "kala", age: 55 });

    const updateNinjaOne = () => {
      ninjaOne.value.age = 40;
    };

    // reactive
    const ninjaTwo = reactive({ name: "Nibir", age: 44 });
    // we can't use primitive values with reactive to be reactive
    const name4 = reactive("Tayef");
    const updateNinjaTwo = () => {
      ninjaTwo.age = 66;
    };

    // using computed values
    const names = ref([
      "mario",
      "yoshi",
      "luigi",
      "toad",
      "bowser",
      "koopa",
      "peach",
    ]);
    const search = ref("");
    const matchingNames = computed(() => {
      return names.value.filter((name) => name.includes(search.value));
    });

    // watch & watchEffect
    const stopWatch = watch(search, () => {
      console.log("watch function ran");
    });

    watchEffect(() => {
      console.log(
        "watchEffect function ran once initially while there was no dependency inside the function"
      );
    });
    const stopWatchEffect = watchEffect(() => {
      console.log(
        "watchEffect function ran for any dependency inside the function changes",
        search.value
      );
    });
    // to stop watching for watch and watch effect we need to store the return value of those functions and invoke them
    const stopWatching = () => {
      stopWatch();
      stopWatchEffect();
    };

    // props
    const posts = ref([
      { title: "welcome to the blog", body: "lorem ipsum", id: 1 },
      { title: "top 5 css tips", body: "lorem ipsum", id: 2 },
    ]);

    return {
      name,
      age,
      handleClick,
      p,
      name1,
      age1,
      handleClick1,
      ninjaOne,
      updateNinjaOne,
      ninjaTwo,
      updateNinjaTwo,
      names,
      search,
      matchingNames,
      stopWatching,
    };
  },
};
</script>
