<template>
    <div id="dashboard" class=" flex mb-auto   justify-center bg-[#f9eeea] pt-20">
        <span class="flex flex-col w-3/4">
            <div class="bg-purple-200 mb-7 text-center rounded-md text-3xl py-3 shadow-md">
                <p>Creator Dashboard </p>
            </div>
            <div class="flex text-2xl text-center bg-white shadow-lg rounded-md ">
                <span :class="`flex-grow border-r hover:bg-purple-400 py-2 px-2 rounded-sm ${selectedTab == Tab.Events ? 'bg-purple-400' : ''} `" >Events</span>
                <div></div>
                <span :class="`flex-grow hover:bg-purple-400 py-2 px-2 rounded-sm ${selectedTab == Tab.Sermons ? 'bg-purple-400' : ''} `">Sermons</span>
            </div>

            <span class="flex flex-col gap-2 ">
                <CreateEvent @event-created="onEventCreate"></CreateEvent>
                <EventsDisplay :reload="reloadEventDisplay"/>
            </span>
        </span>
    </div>
</template>

<script setup lang="ts">
import {ref} from "vue"
import CreateEvent from "./CreateEvent.vue"
//import EventCard from "./EventCard.vue"
import EventsDisplay from "./EventsDisplay.vue"

enum Tab {
    Events,
    Sermons 
}

let reloadEventDisplay = ref(false)

let selectedTab = ref(Tab.Events)


function onEventCreate(): void {
    reloadEventDisplay.value = true 
}


</script>


<style></style>