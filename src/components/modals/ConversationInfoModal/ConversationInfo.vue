<script setup lang="ts">
import { ArrowLeftOnRectangleIcon, AtSymbolIcon, BellIcon, ChevronRightIcon, NoSymbolIcon, ShareIcon, TrashIcon, UserIcon } from "@heroicons/vue/24/outline";
import { ArrowUturnLeftIcon, XMarkIcon } from "@heroicons/vue/24/solid";
import { defineProps } from "vue";

import { Contact, Conversation } from "../../../stores/chat";
import { getAvatar, getFullName, getName, getOddContact } from "../../../utils";

import Typography from "../../utils/Typography.vue";
import InfoItem from "./InfoItem.vue";

const props = defineProps<{
    conversation?: Conversation,
    contact?: Contact,
    closeModal: () => void
}>();
</script>

<template>
    <div>
        <div class="mb-6 px-5 flex justify-between items-center">
            <!--title-->
            <Typography variant="heading-1">
                <span v-if="(props.conversation as Conversation).type === 'couple' || props.contact">Contact</span>
                <span v-else-if="(props.conversation as Conversation).type === 'group'">Group</span>
                <span v-else-if="(props.conversation as Conversation).type === 'boradcast'">Broadcast</span>
                Info
            </Typography>

            <!--close button-->
            <button v-if="!props.contact" @click="props.closeModal"
                class="group p-2 outline-none border rounded-full border-gray-200 text-sm text-black opacity-50 focus:outline-none focus:bg-red-100 hover:bg-red-100 hover:border-red-100 transition-all duration-200">
                <XMarkIcon class="w-5 h-5 group-hover:text-red-500" />
            </button>

            <!--return button-->
            <button v-else @click="$emit('active-page-change', {tabName: 'members', animationName: 'slide-right'})"
                class="group p-2 outline-none border rounded-full border-gray-200 text-sm text-black opacity-50 focus:outline-none focus:bg-red-100 hover:bg-red-100 hover:border-red-100 transition-all duration-200">
                <ArrowUturnLeftIcon class="w-5 h-5 group-hover:text-indigo-500" />
            </button>
        </div>

        <!--top-->
        <div class="w-full p-5 pb-6">
            <div class="flex">
                <!--avatar-->
                <div class="mr-5">
                    <div v-if="props.contact" :style="{ backgroundImage: `url(${props.contact.avatar})`}"
                        class="w-[38px] h-[38px] rounded-full bg-cover bg-center">
                    </div>

                    <div v-else :style="{ backgroundImage: `url(${getAvatar(props.conversation)})`}"
                        class="w-[38px] h-[38px] rounded-full bg-cover bg-center">
                    </div>
                </div>

                <!--name-->
                <div class="flex flex-col items-start">
                    <Typography variant="heading-2" class="mb-3">
                        <span v-if="props.contact">
                            {{getFullName(props.contact)}}
                        </span>
                        <span v-else>
                            {{getName(props.conversation)}}
                        </span>
                    </Typography>

                    <Typography variant="body-2" class="font-extralight">
                        <!--last seen-->
                        <span v-if="(props.conversation as Conversation).type === 'couple' || contact">
                            Last seen Dec 16, 2019
                        </span>

                        <!--or number of group memebers-->
                        <span v-else-if="['group', 'boradcast'].includes((props.conversation as Conversation).type)">
                            {{(props.conversation as Conversation).contacts.length}} Contacts
                        </span>
                    </Typography>
                </div>
            </div>
        </div>

        <!--middle-->
        <div class="w-full py-5 border-t border-gray-100">
            <!--(contact) email-->
            <div v-if="(conversation as Conversation).type === 'couple' || props.contact"
                class="flex px-5 pb-5 items-center">
                <InfoItem :icon="AtSymbolIcon" :title="getOddContact(props.conversation)?.email" />
            </div>

            <!--(group) members-->
            <div v-if="['group', 'boradcast'].includes((props.conversation as Conversation).type) && !props.contact"
                class="px-5 flex items-center pb-5">
                <InfoItem :icon="UserIcon" title="memebers" link chevron
                    @click="$emit('active-page-change', {tabName: 'members', animationName: 'slide-left'})" />
            </div>

            <!--(both) notifications-->
            <div class="px-5 flex items-center">
                <InfoItem :icon="BellIcon" title="notifications" switch />
            </div>

            <!--media-->
            <div class="px-5 pt-5 flex items-center">
                <InfoItem :icon="ShareIcon" title="shared media" link chevron
                    @click="$emit('active-page-change', {tabName: 'sharedMedia', animationName: 'slide-left'})" />
            </div>
        </div>

        <!--bottom-->
        <div class="w-full border-t border-gray-100">
            <!--block contact-->
            <div v-if="(props.conversation as Conversation).type === 'couple' || props.contact" class="px-5 pt-5 group">
                <InfoItem :icon="NoSymbolIcon" title="block contact" link danger />
            </div>

            <!--delete contact-->
            <div v-if="(props.conversation as Conversation).type === 'couple' || props.contact" class="px-5 pt-5 group">
                <InfoItem :icon="TrashIcon" title="delete contact" link danger />
            </div>

            <!--exit group-->
            <div v-if="['group', 'boradcast'].includes((props.conversation as Conversation).type) && !props.contact"
                class="px-5 pt-5 flex items-center group">
                <InfoItem :icon="ArrowLeftOnRectangleIcon" title="exit group" link danger />
            </div>
        </div>
    </div>
</template>