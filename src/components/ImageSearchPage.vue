<template>
    <div class="flex flex-col">
        <h1 class="text-secondary font-bold text-4xl px-4 pt-4 pb-2 text-center">NASA ROVER PICTURES API</h1>
        <p class="text-white text-xl text-center pb-6 italic">Fill the form with the date the picture was taken, the rover
            and the camera to start loading pictures.</p>
        <SearchForm @search-results="handleSearchResults" @search-error="handleSearchError"
            @form-submitted="setFormSubmitted" />
        <ImageCarousel class="py-8" :images="searchResults" v-if="searchResults.length > 0" />
        <div v-else-if="formSubmitted && searchError" class="flex flex-col items-center mt-12">
            <h2 class="text-3xl font-bold text-secondary">
                Error occurred: {{ searchError.message }}
            </h2>
            <p class="text-white text-sm italic">Please try again later.</p>
        </div>
        <div v-else-if="formSubmitted" class="flex flex-col items-center mt-12">
            <h2 class="text-3xl font-bold text-secondary">
                No pictures were found for that day! Try another date, rover, or camera.
            </h2>
            <p class="text-white text-sm italic">Want a tip? Try 11/02/2023 - Curiosity - RHAZ</p>
        </div>
        <div v-else class="flex flex-col items-center mt-12">
            <h2 class="text-3xl font-bold text-secondary">
                It's empty here :(
            </h2>
            <h2 class="text-3xl font-bold text-secondary">
                Try filling up the form to find cool pictures from the NASA rovers!
            </h2>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import SearchForm from './SearchForm.vue'
import ImageCarousel from './ImageCarousel.vue'

interface Image {
  id: number;
  img_src: string;
  earth_date: string;
  rover: {
    name: string;
  };
  camera: {
    full_name: string;
  };
}

export default defineComponent({
    components: {
        SearchForm,
        ImageCarousel
    },
    data() {
        return {
            searchResults: [] as Image[],
            formSubmitted: false,
            searchError: null as Error | null
        }
    },
    methods: {
        handleSearchResults(response: { photos: Image[] }): void {
            this.searchResults = response.photos
            this.formSubmitted = true
            this.searchError = null
        },
        handleSearchError(error: Error): void {
            this.searchError = error
        },
        setFormSubmitted() {
            this.formSubmitted = true
        }
    }
})
</script>
