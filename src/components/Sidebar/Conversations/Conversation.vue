<script setup lang="ts">
import { ArchiveBoxArrowDownIcon, InformationCircleIcon, MicrophoneIcon, TrashIcon } from "@heroicons/vue/24/outline";
import type { Ref } from "vue";
import { computed, defineProps, ref } from "vue";

import { Attachment, Conversation, Recording } from "../../../stores/chat";
import { getAvatar, getName, hasAttachments, shorten } from "../../../utils";

import Dropdown from "../../utils/Dropdown.vue";
import DropdownLink from "../../utils/DropdownLink.vue";
import Typography from "../../utils/Typography.vue";

const props = defineProps<{
    conversation: Conversation,
    activeId?: number,
    handleConversationChange?: (conversationId: number) => void,
}>();

const showContextMenu = ref(false);

const contextMenuCordinations: Ref<{ x: number, y: number } | undefined> = ref();

// open context menu.
const handleShowContextMenu = (event: any) => {
    showContextMenu.value = true;
    contextMenuCordinations.value = { x: event.pageX, y: window.innerHeight - 125 <= event.pageY ? window.innerHeight - 200 : event.pageY };
};

// (event) closes the context menu
const handleCloseContextMenu = () => {
    showContextMenu.value = false;
};

// (event) close context menu when opening a new one.
const contextConfig = {
    handler: handleCloseContextMenu,
    events: ['contextmenu'],
};

// (event) select this conversation.
const handleSelectConversation = () => {
    showContextMenu.value = false;
    if (props.handleConversationChange)
        props.handleConversationChange(props.conversation.id);
};

// last message in conversation to display
const lastMessage = computed(() => props.conversation.messages[props.conversation.messages.length - 1]);

// determine whether the conversation is active or not
const isActive: Ref<boolean> = computed(() => {
    return props.activeId === props.conversation.id
});
</script>

<template>
    <div>
        <a tabindex="0" v-click-outside="contextConfig" @contextmenu.prevent="handleShowContextMenu"
            @click="handleSelectConversation" href="#"
            class="w-full px-5 py-6 mb-3 flex rounded hover:bg-indigo-50 active:bg-indigo-100 focus:outline-none focus:bg-indigo-50 transition duration-200 ease-out"
            :class="{'bg-indigo-50': isActive}">
            <!--profile image-->
            <div class="mr-4">
                <div :style="{ backgroundImage: `url(${getAvatar(props.conversation)})`}"
                    class="w-7 h-7 rounded-full bg-cover bg-center">
                </div>
            </div>

            <div class="w-full flex flex-col">
                <div class="w-full">
                    <!--conversation name-->
                    <div class="flex items-start">
                        <div class="grow mb-4">
                            <Typography variant="heading-2">
                                {{getName(props.conversation)}}
                            </Typography>
                        </div>

                        <!--last message date-->
                        <Typography variant="body-1">
                            {{lastMessage?.date}}
                        </Typography>
                    </div>
                </div>

                <div>
                    <!--last message content -->
                    <Typography v-if="lastMessage.type !== 'recording' && lastMessage.content" variant="body-2">
                        {{shorten(lastMessage)}}
                    </Typography>

                    <Typography v-else-if="lastMessage.type === 'recording' && lastMessage.content" variant="body-2"
                        class="flex justify-start items-center">
                        <MicrophoneIcon class="w-4 h-4 mr-2 text-black opacity-60" /> Recording
                        {{(lastMessage.content as
                        Recording).duration}}
                    </Typography>

                    <!--attachments title-->
                    <Typography v-else-if="hasAttachments(lastMessage)" variant="body-2">
                        {{(lastMessage?.attachments as Attachment[])[0].name}}
                    </Typography>
                </div>
            </div>
        </a>

        <!--custom context menu-->
        <Dropdown :show="showContextMenu" :handle-close="handleCloseContextMenu"
            :handle-click-outside="handleCloseContextMenu"
            :cordinates="{left: contextMenuCordinations?.x + 'px', top: contextMenuCordinations?.y + 'px'}"
            :position="['top-0']">
            <DropdownLink :handle-click="handleCloseContextMenu">
                <InformationCircleIcon class="h-5 w-5 mr-3 text-black opacity-60 " />
                Conversation info
            </DropdownLink>

            <DropdownLink :handle-click="handleCloseContextMenu">
                <ArchiveBoxArrowDownIcon class="h-5 w-5 mr-3 text-black opacity-60 " />
                Archive conversation
            </DropdownLink>

            <DropdownLink :handle-click="handleCloseContextMenu"
                class="text-red-500 hover:bg-red-100 active:bg-red-100">
                <TrashIcon class="text-red-500  h-5 w-5 mr-3" />
                Delete conversation
            </DropdownLink>
        </Dropdown>
    </div>
</template>