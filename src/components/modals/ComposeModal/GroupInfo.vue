<script setup lang="ts">
import TextInput from '../../utils/TextInput.vue';
import DropFileUpload from '../../utils/DropFileUpload.vue';
import PrimaryButton from '../../utils/PrimaryButton.vue';
</script>

<template>
    <div class="px-5">
        <!--inputs-->
        <div class="mb-5">
            <div class="mb-5">
                <TextInput type="text" placeholder="Group name" label="Name" />
            </div>

            <div>
                <DropFileUpload label="Avatar" />
            </div>
        </div>

        <!--next button-->
        <div class="flex pb-6">
            <div class="grow"></div>
            <PrimaryButton
                @click="$emit('active-page-change', { tabName: 'group-members', animationName: 'slide-left' })"
                class="px-5 bg-indigo-400 hover:bg-indigo-500 active:bg-indigo-500">
                Next
            </PrimaryButton>
        </div>
    </div>
</template>