<script setup lang="ts">
import { defineProps } from "vue";
import { LinkIcon } from "@heroicons/vue/24/outline";

import { PreviewData } from "../../../stores/chat";

const props = defineProps<{
    self?: boolean,
    previewData: PreviewData,
}>();
</script>

<template>
    <div class="border-l pl-3 cursor-pointer border-opacity-50 duration-200"
        :class="props.self ? ['border-white'] : ['border-black']">
        <!--title-->
        <p class="mb-3 font-semibold text-xs leading-4 tracking-[0.16px]"
            :class="props.self ? ['text-white']:['text-black', 'opacity-50']">
            {{props.previewData.title}}
        </p>

        <!--description-->
        <p class="font-normal text-xs leading-4 tracking-[0.16px]"
            :class="props.self ? ['text-white']:['text-black', 'opacity-50'] ">
            {{props.previewData.description}}
        </p>

        <!--image-->
        <img :src="props.previewData.image" class="my-5" />

        <!--link domain-->
        <a :href="props.previewData.link" class="font-normal text-xs leading-4 tracking-[0.16px] flex items-center"
            :class="props.self ? ['text-black', 'opacity-50'] : ['text-black', 'opacity-50']">
            <LinkIcon class="w-4 h-4 mr-2 stroke-1" /> {{props.previewData.domain}}
        </a>
    </div>
</template>