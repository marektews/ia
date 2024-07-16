<script setup>
import { ref } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faRegistered } from '@fortawesome/free-solid-svg-icons'

import IntroView from './view/IntroView.vue'
import CongregationView from './view/CongregationView.vue'
import LinksView from './view/LinksView.vue'



const mode = ref(0)
let congregation = ref("")

</script>

<template>
    <div class="container">
        <header>
            <FontAwesomeIcon :icon="faRegistered" size="2xl" />
            <div>Identyfikatory autobusów</div>
        </header>

        <main class="mt-4">
            <IntroView v-if="mode === 0" @next="mode = 1" />

            <CongregationView v-else-if="mode === 1"
                v-model="congregation"
                @next="mode = 2" 
            />

            <LinksView v-else-if="mode === 2"
                :congregation="congregation"
            />
        </main>
    </div>
</template>

<style scoped>
header {
    display: flex;
    align-items: center;
    gap: 16pt;
    margin-bottom: 24pt;
}
header > div {
    font-size: 2rem;
}

main {
    min-width: 60vw;
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    gap: 9pt;
}

main > div {
    flex-grow: 1;
}
</style>
