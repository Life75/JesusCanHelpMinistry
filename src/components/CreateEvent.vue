<template>
    <div class="flex flex-col justify-center items-center">

        <div class="mr-auto mt-20">
            <span class="">
                <button @click="showDialog = !showDialog"
                    class="btn btn-xs sm:btn-sm md:btn-md lg:btn-lg bg-white  hover:shadow-xl font-normal shadow-md">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                        class="bi bi-plus-square-dotted mr-0.5" viewBox="0 0 16 16">
                        <path
                            d="M2.5 0q-.25 0-.487.048l.194.98A1.5 1.5 0 0 1 2.5 1h.458V0zm2.292 0h-.917v1h.917zm1.833 0h-.917v1h.917zm1.833 0h-.916v1h.916zm1.834 0h-.917v1h.917zm1.833 0h-.917v1h.917zM13.5 0h-.458v1h.458q.151 0 .293.029l.194-.981A2.5 2.5 0 0 0 13.5 0m2.079 1.11a2.5 2.5 0 0 0-.69-.689l-.556.831q.248.167.415.415l.83-.556zM1.11.421a2.5 2.5 0 0 0-.689.69l.831.556c.11-.164.251-.305.415-.415zM16 2.5q0-.25-.048-.487l-.98.194q.027.141.028.293v.458h1zM.048 2.013A2.5 2.5 0 0 0 0 2.5v.458h1V2.5q0-.151.029-.293zM0 3.875v.917h1v-.917zm16 .917v-.917h-1v.917zM0 5.708v.917h1v-.917zm16 .917v-.917h-1v.917zM0 7.542v.916h1v-.916zm15 .916h1v-.916h-1zM0 9.375v.917h1v-.917zm16 .917v-.917h-1v.917zm-16 .916v.917h1v-.917zm16 .917v-.917h-1v.917zm-16 .917v.458q0 .25.048.487l.98-.194A1.5 1.5 0 0 1 1 13.5v-.458zm16 .458v-.458h-1v.458q0 .151-.029.293l.981.194Q16 13.75 16 13.5M.421 14.89c.183.272.417.506.69.689l.556-.831a1.5 1.5 0 0 1-.415-.415zm14.469.689c.272-.183.506-.417.689-.69l-.831-.556c-.11.164-.251.305-.415.415l.556.83zm-12.877.373Q2.25 16 2.5 16h.458v-1H2.5q-.151 0-.293-.029zM13.5 16q.25 0 .487-.048l-.194-.98A1.5 1.5 0 0 1 13.5 15h-.458v1zm-9.625 0h.917v-1h-.917zm1.833 0h.917v-1h-.917zm1.834-1v1h.916v-1zm1.833 1h.917v-1h-.917zm1.833 0h.917v-1h-.917zM8.5 4.5a.5.5 0 0 0-1 0v3h-3a.5.5 0 0 0 0 1h3v3a.5.5 0 0 0 1 0v-3h3a.5.5 0 0 0 0-1h-3z" />
                    </svg>
                    Create Event</button>
            </span>
        </div>

       

        <el-dialog v-model="showDialog" title="Create Event" width="370"  center class="rounded-lg">

            <div class="flex flex-col gap-7">
                <span class="flex flex-col align-middle gap-2">
                    <p class=" text-xl  ml-0.5">Title</p>
                    <label class="input input-bordered flex items-center gap-2">

                        <input v-model="eventForm.title" type="text" class="grow" placeholder="" />

                    </label>
                </span>

                <span class="flex flex-col gap-2 align-middle">
                    <p class="text-xl   ml-0.5">Date for event</p>
                    <el-date-picker v-model="eventForm.date" type="datetime" placeholder="Select date and time"
                        :shortcuts="shortcuts" />
                </span>

                <span class="flex flex-col gap-2 align-middle">
                    <p class="text-xl  ml-0.5">Location of Event</p>
                    <label class="input input-bordered flex items-center gap-2">

                        <input v-model="eventForm.location" type="text" class="grow" placeholder="" />

                    </label>
                </span>


                <span class="flex flex-col gap-2 align-middle">
                    <p class="text-xl  ml-0.5">Event Thumbnail</p>
                    <input ref="inputFile" type="file" @change="handleFileUpload">

                </span>

                <span class="flex flex-col align-middle gap-2">
                    <p class=" text-xl   ml-0.5">Description</p>
                    <textarea v-model="eventForm.description"
                        placeholder="Add a description to describe what the event is for."
                        class="textarea  textarea-bordered textarea-md border w-full "></textarea>
                </span>

                <span class="flex flex-row w-full justify-end items-end gap-4 ">
                    <button
                        class="btn btn-xs sm:btn-sm md:btn-md lg:btn-lg  bg-red-400 text-white shadow-md hover:shadow-xl hover:bg-red-500"
                        @click="onCancel">Cancel</button>
                    <button
                        class="btn btn-xs sm:btn-sm md:btn-md lg:btn-lg  bg-blue-400 text-white shadow-md hover:shadow-xl hover:bg-blue-500"
                        @click="onCreate">Create</button>
                </span>

            </div>
        </el-dialog>
    </div>
</template>


<script setup lang="ts">
import { Ref, onBeforeMount, ref } from 'vue';
import Event from '../Interfaces/Event';
import EventService from "../services/EventService"
import EventRepository from "../repository/EventRepository"

let eventForm: Ref<Event> = ref(new Event())
let inputFile = ref(null)
let showDialog = ref(false)
let selectedFile = ref(null)
let emits = defineEmits<{
    (e: "event-created"): void
}>()



const shortcuts = [
    {
        text: 'Today',
        value: new Date(),
    },
    {
        text: 'Yesterday',
        value: () => {
            const date = new Date()
            date.setDate(date.getDate() - 1)
            return date
        },
    },
    {
        text: 'A week ago',
        value: () => {
            const date = new Date()
            date.setDate(date.getDate() - 7)
            return date
        },
    },
]

function handleFileUpload(event: any): void {
    const file = event.target.files[0]
    selectedFile.value = file
}


async function onCreate() {
    if (eventForm.value) await createEvent(eventForm.value)
    emits("event-created")

    showDialog.value = false 
}

async function createEvent(event: Event): Promise<void> {
    const service = new EventService(new EventRepository())
    if(selectedFile.value) {        
        await service.SaveImage(selectedFile.value, event.imageID.toString())
    } else {
        event.imageID = "undefined"
    }
    await service.Create(event)
}

function onCancel(): void {
    showDialog.value = !showDialog.value
    eventForm.value = new Event()
    selectedFile.value = null

    //clears input
    inputFile.value.type = "text"
    inputFile.value.type = "file"
}

</script>

<style></style>