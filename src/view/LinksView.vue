<script setup>
import { ref } from 'vue'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faFilePdf } from '@fortawesome/free-solid-svg-icons';


const props = defineProps(['congregation'])

const list = ref([])
fetch(`/api/ia/list/${props.congregation}`)
.then(response => response.json())
.then(d => list.value = d)
.catch(err => console.error("Load bus pass id error:", err))

function onDownloadPassID(sra_id) {
    let filename = ''
    fetch(`/api/ia/download/${sra_id}`)
    .then(response => {
        if(response.status !== 200) return
        const header = response.headers.get('Content-Disposition');
        const parts = header.split(';');
        filename = parts[1].split('=')[1].replaceAll("\"", "");
        console.log('{1} IA download pass card', response, filename)
        return response.blob()
    })
    .then((blob) => {
        let url = window.URL.createObjectURL(blob)
        console.log('{2} IA download pass card', blob, url)
        var a = document.createElement('a');
        a.href = url;
        a.download = filename;
        document.body.appendChild(a);
        a.click();
        a.remove();        
    })
    .catch(err => console.error("IA download pass id error:", err))
}

function format_bus_info(bt) {
    switch(bt) {
        case "minibus_9": return "Minibus do 9 osób"
        case "minibus_30": return "Minibus do 30 osób"
        case "autokar_50": return "Autokar do 50 osób"
        case "autokar_70": return "Autokar 60-70 osób"
        case "autobus_12m": return "Autobus miejski - 12m (pojedyńczy)"
        case "autobus_18m": return "Autobus miejski - 18m (przegubowy)"
    }
    return bt
}

function format_pilots(item) {
    let s = `${item.pilot1.fn} ${item.pilot1.ln}`
    if('pilot2' in item)
        s += `, ${item.pilot2.fn} ${item.pilot2.ln}`
    if('pilot3' in item)
        s += `, ${item.pilot3.fn} ${item.pilot3.ln}`
    return s
}
</script>

<template>
    <div class="container">
        <div class="card text-bg-dark">
            <div class="card-body">
                <template v-if="list.length == 1">
                    <h3>Identyfikator</h3>
                    <div v-for="(item, index) in list" :key="index" class="my-4">
                        <h4 class="text-muted">{{ format_bus_info(item.bus.type) }}</h4>
                        <h6 class="text-muted">Pilot: {{ format_pilots(item) }}</h6>
                        <!-- <div>{{ item }}</div> -->
                        <button class="mt-2 btn btn-lg btn-primary" @click="onDownloadPassID(item.id)">
                            <FontAwesomeIcon :icon="faFilePdf" />
                            Pobierz identyfikator
                        </button>
                    </div>
                </template>

                <template v-else-if="list.length > 1">
                    <h3>Identyfikatory</h3>
                    <ol>
                        <li v-for="(item, index) in list" :key="index" class="my-5">
                            <h4 class="text-muted">{{ format_bus_info(item.bus.type) }}</h4>
                            <h6 class="text-muted">Pilot: {{ format_pilots(item) }}</h6>
                            <!-- <div>{{ item }}</div> -->
                            <button class="mt-2 btn btn-lg btn-primary" @click="onDownloadPassID(item.id)">
                                <FontAwesomeIcon :icon="faFilePdf" />
                                Pobierz identyfikator
                            </button>
                        </li>
                    </ol>
                </template>
            </div>
        </div>

        <div class="mt-4 card text-bg-dark">
            <div class="card-body">
                <h3>Informacja dla pilotów i kierowców</h3>
                <p class="card-text">
                    Bardzo prosimy, by każdy pilot zapoznał się z treścią tego dokumentu oraz przekazał niezbędne informacje kierowcy i pasażerom.
                </p>
                
                <a 
                    href="Informacja dla pilotów i kierowców.pdf" 
                    download="Informacja dla pilotów i kierowców.pdf" 
                    class="btn btn-lg btn-primary"
                >
                    <FontAwesomeIcon :icon="faFilePdf" />
                    Pobierz plik
                </a>
            </div>
        </div>
    </div>
</template>