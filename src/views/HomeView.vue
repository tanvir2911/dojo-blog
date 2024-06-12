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
  <hr />
  <h2>using props in setup</h2>
  <PostList v-if="showPosts" :posts="posts" />

  <!-- lifecycle hooks -->
  <hr />
  <button @click="showPosts = !showPosts">hide or show posts</button>
  <button @click="posts.pop()">delete a posts</button>
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
      {
        title: "welcome to the blog",
        body: "Lorem ipsum dolor sit amet consectetur adipisicing elit. Ipsum alias facilis accusamus iste, quidem fuga, assumenda perspiciatis porro quos, harum reiciendis atque a ratione. Dolor mollitia expedita non praesentium voluptates laborum esse similique commodi quisquam eligendi? Iusto, impedit quae aliquam quia accusantium magnam commodi nisi perspiciatis voluptates. Nobis laboriosam accusamus ratione recusandae consequuntur praesentium neque libero expedita, rerum sequi dolorum architecto eveniet voluptas numquam, officia exercitationem! Consequatur ipsa harum, molestiae dolorem aliquam deleniti reprehenderit unde voluptas ea suscipit sint neque quas ex praesentium assumenda, distinctio est incidunt nesciunt alias eius sit, delectus sequi? Maxime quis dolor placeat laboriosam atque totam obcaecati dolore architecto, accusamus labore quasi a ex tempora tempore ut est voluptatum consectetur earum in officia et praesentium? Delectus maiores tenetur numquam doloribus iure odit explicabo, reprehenderit distinctio nemo hic fuga at nulla error iusto commodi nobis vero. Unde odit fugit culpa ut, exercitationem aspernatur dolorum id architecto totam ratione explicabo qui quia amet necessitatibus esse, neque mollitia numquam voluptatum itaque dolore repellat ex ad deserunt? Quia ut laudantium facere et perferendis praesentium aut maiores explicabo, accusamus at animi recusandae harum repudiandae quis sint delectus aliquid illo nulla expedita similique totam, libero minima! Aspernatur dolor soluta mollitia impedit maxime. Praesentium facilis excepturi eos exercitationem? Libero assumenda tempore eius suscipit consequatur culpa debitis labore iure magni enim beatae molestias quas vel non, ea expedita ad! Eius fugiat explicabo maxime minus, reprehenderit ex soluta velit porro quaerat dolore dolorem voluptatem autem recusandae modi culpa enim molestias molestiae ratione eum totam maiores fuga possimus doloribus quos. Dolorem iusto sunt impedit architecto nam praesentium odit? Ullam, rem soluta maiores odit accusamus numquam error maxime unde suscipit fuga a consequuntur at obcaecati quod ducimus, quas non repellendus corrupti incidunt sit iste, deserunt quidem animi. Ipsum voluptatem molestias in magnam explicabo illo, eligendi adipisci commodi.",
        id: 1,
      },
      { title: "top 5 css tips", body: "lorem ipsum", id: 2 },
    ]);

    // lifecycle hooks
    const showPosts = ref(true);

    // fetching data in setup
    const postsDb = ref([]);
    const error = ref(null);

    const load = async () => {
      try {
        let data = await fetch("http://localhost:3000/posts");
        console.log(data);
        if (!data.ok) {
          throw Error("no data available");
        }
        postsDb.value = data.json();
      } catch (err) {
        error.value = err.message;
        console.log(error.value);
      }
    };
    load();

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
      posts,
      showPosts,
      postsDb,
    };
  },
};
</script>
