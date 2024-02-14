<script>
import { store } from "../store.js"
import axios from "axios";
import EventCard from "../components/EventCard.vue";
export default {
    name: "TagDetail",
    components: {
        EventCard
    },
    props: ["id"],
    data() {
        return {
            store,
            event: null
        }
    },
    mounted() {
        // this.event = this.store.eventList.find(item => item.id == this.id);
        this.getTagDetail();
    },
    methods: {
        getTagDetail() {
            let url = this.store.apiUrl + this.store.apiTagEndPoint + this.id;

            axios.get(url).then(result => {
                if (result.status === 200) {
                    if (result.data.success) {
                        this.event = result.data.payload;
                    } else {
                        console.error("Ops... non siamo in grado di soddisfare la richiesta.");
                    }

                } else if (result.status === 301) {
                    console.error("Ops... ciò che cerchi non si trova più qui.");
                } else if (result.status === 400) {
                    console.error("Ops... non riusciamo a comprendere ciò che hai richiesto.");
                } else if (result.status === 404) {
                    console.error("Ops... non riusciamo a trovare ciò che hai richiesto.");
                } else if (result.status === 500) {
                    console.error("Ops... ci scusiamo per l'inconveniente, stiamo spegnendo l'incendio.");
                }
            }).catch(errore => {
                console.error(errore);
            });
        }
    }
}
</script>
<!-- user_id name date available_tickets user {id name email} -->
<template>
    <div class="container">
        <div class="row">
            <h1 class="mb-3">Nome tag: {{ event?.name }}</h1>
            <h3 class="mb-3">Identificativo tag: {{ event?.id }}</h3>
            <h3>Eventi con questo tag:</h3>
            <EventCard v-for="event in this.event?.events" :item="event" />

        </div>
    </div>
</template>