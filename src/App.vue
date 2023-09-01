<template>
  <h3>
    {{ titleLengthMessage }}
    <input type="text" v-model="title" />
    <button @click="toggleIt">Göster/Gizle</button>
    <p v-if="show">
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Veritatis fugiat
      reiciendis voluptatibus illo modi. Modi doloribus deserunt magni officiis
      impedit sunt asperiores temporibus veritatis. Odit, magni delectus?
      Tenetur, quae debitis?
    </p>
    <hr />
    <button @click="counter++">Arttır</button>
    {{ showData }}
    {{ counter }}

    <hr />
    <input type="text" v-model="searchText" />
    <p v-if="isTyping">Şuan Yazıyor</p>
  </h3>
</template>
<script>
import { ref, computed, watch, watchEffect } from "vue";
export default {
  setup() {
    const title = ref("Bu Bir Başlık");
    const titleLengthMessage = computed(() => {
      return title.value.length + "adet  karakter bulunmaktadır";
    });

    //******************************************** */

    const counter = ref(0);
    const show = ref(true);
    const toggleIt = () => {
      show.value = !show.value;
    };
    const showData = computed(() => {
      return counter.value % 2 == 0 ? "Çift" : "tek";
    });
    watch([counter, showData, title], (newValue, oldValue) => {
      console.log(newValue, oldValue);
    });
    //******************************************** */
    const searchText = ref("");
    const isTyping = ref(false);
    // watch(searchText, () => {
    //   if (searchText.value.length > 0) {
    //     isTyping.value = true;
    //     setTimeout(() => {
    //       isTyping.value = false;
    //     }, 1500);
    //   } else {
    //     isTyping.value = false;
    //   }
    // });
    watchEffect((onInvalidate) => {
      if (searchText.value.length > 0) {
        isTyping.value = true;
        const typing = setTimeout(() => {
          isTyping.value = false;
        }, 1500);
        onInvalidate(() => clearTimeout(typing));
      }
    });

    return {
      title,
      show,
      counter,
      toggleIt,
      titleLengthMessage,
      showData,
      searchText,
      isTyping,
    };
  },
};
</script>
