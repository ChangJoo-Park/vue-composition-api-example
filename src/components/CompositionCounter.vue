<template>
  <div>
    <div>
      <strong>Counter</strong>
      <p>Please Open Console</p>
    </div>
    <div>
      <button @click="decrease">-</button>&nbsp;
      <span>{{ state.count }}</span>&nbsp;
      <button @click="increase">+</button>
    </div>
    <br>
    <div>
      <div>Doubled : {{ state.doubled }}</div>
      <div>Tripled : {{ tripleCount }}</div>
    </div>
    <br>
    <div>Another Counter : {{ anotherCounter }}</div>
    <br>
    <div>User Name from outside function => 
      <pre style="text-align: left;">{{ userState }}</pre>
    </div>
  </div>
</template>

<script>
import {
  createComponent,
  setup,
  reactive,
  computed,
  watch,
  onMounted,
  onBeforeMount,
  onUpdated,
  onBeforeUpdate,
  ref
} from "@vue/composition-api";

export default createComponent({
  name: "CompositionCounter",
  setup() {
    /**
     * Reactive Data, Computed
     */
    const state = reactive({
      count: 0,
      doubled: computed(() => state.count * 2)
    });

    const tripleCount = computed(() => state.count * 3);

    /**
     * ref. not ref on template
     */
    const anotherCounter = ref(0);

    /**
     * Watcher
     */
    watch(() => {
      if (state.count % 2 === 0) {
        console.log("[WATCH] Data Updated -> state.count is even.");
      }
      if (state.doubled) {
        console.log(
          "[WATCH] Another wather -> state.doubled -> ",
          state.doubled
        );
      }
    });

    watch(() => {
      if (tripleCount) {
        console.log("[WATCH] Computed Property Updated -> ", tripleCount.value);
      }
    });

    /**
     * Methods
     */
    function increase() {
      state.count++;
      anotherCounter.value++;
    }

    function decrease() {
      state.count--;
      anotherCounter.value--;
    }

    /**
     * Life Cycle Hooks
     */
    onMounted(() => {
      console.log("[LifeCycle] onMounted === mounted");
    });

    onBeforeMount(() => {
      console.log("[LifeCycle] onBeforeMount === beforeMount");
    });

    onUpdated(() => {
      console.log("[LifeCycle] onUpdated === updated");
    });

    onBeforeUpdate(() => {
      console.log("[LifeCycle] onBeforeUpdate === beforeUpdate");
    });

    /**
     * get ref from outside
     */
    const { userState } = useStore();

    return {
      // States
      state,
      tripleCount,
      anotherCounter,
      userState,
      // Methods
      increase,
      decrease
    };
  }
});

function useStore() {
  const userState = ref({ name: "ChangJoo Park" });
  return { userState };
}
</script>

<style>
</style>
