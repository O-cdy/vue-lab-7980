<template>
  <!-- <div id="container">
    <strong>{{ name }}</strong>
    <p>Explore <a target="_blank" rel="noopener noreferrer" href="https://ionicframework.com/docs/components">UI
        Components</a></p>
  </div> -->
  <ion-card v-for="item in items" :key="item.id" @click="navigateWithId(item.id)">
    <img alt="Silhouette of mountains" :src="item.image" />
    <ion-card-header>
      <ion-card-title>{{item.name}}</ion-card-title>
      <!-- <ion-card-subtitle>{{item.image}}</ion-card-subtitle> -->
    </ion-card-header>

    <ion-card-content>
      {{item.desc}}
    </ion-card-content>
  </ion-card>
</template>

<script>
import { defineComponent, ref, onMounted, inject, computed } from 'vue';
import { useRouter } from 'vue-router';

export default defineComponent({
  name: 'ExploreContainer',
  props: {
    name: String
  },
  setup(props) {
    const items = ref([]);
    const router = useRouter();

    const navigateWithId = function (id) {
      router.push('/item/' + id)
    };

    onMounted(async () => {

      var response = await fetch("https://api.npoint.io/5529943ab6c290922ca9");

      if (response.ok) {
        let fashionItems = await response.json();

        items.value = fashionItems.filter(function (item) {
          return item.department == props.name;
        })
      }
    });

    return {
      items, navigateWithId
    };
  }
});
</script>

<style scoped>
#container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
