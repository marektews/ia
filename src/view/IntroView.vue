<script setup>
defineEmits('next')
</script>

<template>
    <div class="container">
        <div>
            Zaloguj się za pomocą otrzymanych danych, pobierz i wydrukuj identyfikator.
        </div>

        <button class="mt-5 btn btn-lg btn-outline-primary" @click="$emit('next')">
            ZACZYNAMY
        </button>

    </div>
</template>