<template>
    <button class="btn btn-lg btn-outline-primary">
        <FontAwesomeIcon :icon="faLockOpen" />
        <span class="ms-2">Zaloguj</span>
    </button>
</template>

<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faLockOpen } from '@fortawesome/free-solid-svg-icons';
</script>