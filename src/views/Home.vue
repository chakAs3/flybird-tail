<script setup lang="ts">
import { onMounted } from "vue";

import useChatStore from "../stores/chat";
import { fetchData } from "../stores/fakeData";

import Chat from "../components/Chat/Chat.vue";
import Navigation from "../components/Navigation/Navigation.vue";
import Sidebar from "../components/Sidebar/Sidebar.vue";

const chat = useChatStore();

chat.$subscribe((mutation, state) => {
    localStorage.setItem('chat', JSON.stringify(state));
});

// update home page state;
onMounted(async () => {
    chat.status = 'loading';

    setTimeout(() => {
        chat.delayLoading = false;
    });

    const request = await fetchData();

    chat.$patch({
        status: 'success',
        contacts: request.data.contacts,
        conversations: request.data.conversations,
        notifications: request.data.notifications,
        archivedConversations: request.data.archivedConversations,
    });
});
</script>

<template>
    <KeepAlive>
        <div class="h-screen w-screen flex overflow-hidden">
            <Navigation />
            <Sidebar />
            <Chat />
        </div>
    </KeepAlive>
</template>