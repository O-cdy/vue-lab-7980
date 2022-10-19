<template>
    <div class="home">
        <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
        <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
        <div class="container">
            <h1><b style="text-align:center">Detail</b></h1>
            <ul class="list-group">
                <li class="list-group-item">{{ booking._id }}</li>
                <li class="list-group-item">{{ booking.email }}</li>
                <li class="list-group-item">{{ booking.numTickets }}</li>
                <li class="list-group-item">{{ booking.team }}</li>
                <li class="list-group-item">{{ booking.superhero }}</li>
                <li class="list-group-item">{{ booking.payment }}</li>
            </ul>
        </div>
        <PaginatedBookings msg="Welcome to Your Vue.js App" />
    </div>
</template>
  
<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import PaginatedBookings from '@/components/PaginatedBookings.vue'
import { useRoute } from "vue-router";
import { ref, onMounted } from "vue";


export default {
    name: 'BookingView',
    setup() {
        const booking = ref({});
        const route = useRoute();

        onMounted(async () => {

            var response = await fetch("/api/bookings/" + route.params.id);

            if (response.ok) {
                booking.value = await response.json();
            } else {
                alert(response.statusText);
            }
        })

        return {
            booking
        }
    },
    components: {
        // HelloWorld
        PaginatedBookings
    }
}
</script>
  