<script setup lang="ts">
import Typography from '../utils/Typography.vue';
</script>

<template>
    <div class="w-full min-h-[80px] max-h-fit px-5 py-6 flex justify-between items-center">
        <Typography variant="heading-1">
            <slot name="title"></slot>
        </Typography>

        <div>
            <slot name="actions"></slot>
        </div>
    </div>
</template>