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
                <img alt="Silhouette of mountains" :src="item.image" />
                <ion-card-header>
                    <ion-card-title>{{ item.name }}</ion-card-title>
                    <ion-card-subtitle>{{ item.price }}</ion-card-subtitle>
                </ion-card-header>

                <ion-card-content>
                    {{item.desc}}
                </ion-card-content>

                <ion-button v-if="!selected" @click="addToCart()">Add To Cart</ion-button>
                <ion-button v-if="selected" @click="removeFromCart()">Remove From Cart</ion-button>
            </ion-card>
        </ion-content>
    </ion-page>
</template>
  
<script>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import { defineComponent, ref, onMounted, inject, computed } from 'vue';
import { useRoute } from 'vue-router';
// import ExploreContainer from '@/components/ExploreContainer.vue';


export default defineComponent({
    name: 'ItemPage',
    components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage },
    setup() {
        const item = ref({});
        const route = useRoute();
        const { id } = route.params;

        const cartItems = ref(inject('cart').value);

        const selected = computed(() => {

            return cartItems.value.includes(id);
        });

        const addToCart = function () {
            cartItems.value.push(id);
        };

        const removeFromCart = function () {

            const index = cartItems.value.indexOf(id);
            if (index > -1) { // only splice array when item is found
                cartItems.value.splice(index, 1); // 2nd parameter means remove one item only
            }
        }

        onMounted(async () => {

            var response = await fetch("https://api.npoint.io/5529943ab6c290922ca9");

            if (response.ok) {
                let fashionItems = await response.json();

                item.value = fashionItems.filter(function (item) {
                    return item.id == id;
                })[0];
            }
        });

        return {
            item, addToCart, removeFromCart, selected
        }
    }

});
</script>
  