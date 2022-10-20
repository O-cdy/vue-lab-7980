<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Tab 3</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 3</ion-title>
        </ion-toolbar>
      </ion-header>

      <!-- <ExploreContainer name="Tab 3 page" /> -->
      <ion-card>
        <ion-card-header>
          <ion-card-title>Total Price</ion-card-title>
          <ion-card-subtitle>{{totalPrice}}</ion-card-subtitle>
        </ion-card-header>
        <ion-card-content>
          <ion-list>
            <ion-item v-for="item in selectedItems" :key="item.id">
              <ion-thumbnail slot="start">
                <img alt="Silhouette of mountains" :src="item.image" />
              </ion-thumbnail>
              <ion-label>{{item.name}}</ion-label>
              <ion-label>{{item.price}}</ion-label>
            </ion-item>
          </ion-list>
        </ion-card-content>
      </ion-card>
    </ion-content>
  </ion-page>
</template>

<script>
import { defineComponent, ref, onMounted, inject, computed } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
// import ExploreContainer from '@/components/ExploreContainer.vue';

export default defineComponent({
  name: 'Tab3Page',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage },
  setup() {
    const items = ref([]);

    const cartItems = ref(inject('cart').value);

    const selectedItems = computed(() => {

      return items.value.filter((item) => {
        return cartItems.value.includes(item.id)
      })

    });

    const totalPrice = computed(() => {

      var price = 0;

      items.value.forEach((item) => {
        if (cartItems.value.includes(item.id)) {
          price += item.price
        }
      })

      return price;

    });

    onMounted(async () => {

      var response = await fetch("https://api.npoint.io/5529943ab6c290922ca9");

      if (response.ok) {
        items.value = await response.json();
      }
    });

    return {
      selectedItems, totalPrice
    }
  }

});
</script>
