<template>
  <form class="bg-gray-700 shadow-lg rounded-md flex flex-col md:flex-row items-center justify-center py-8 px-4"
    @submit.prevent="searchImages">
    <input type="date" v-model="searchDate" required placeholder="select a date"
      class="border text-sm rounded-lg block w-full max-w-sm pl-10 p-2.5 bg-black border-gray-600 placeholder-gray-400 text-white focus:ring-blue-500 focus:border-blue-500 md:mr-4" />
    <select v-model="selectedRover" required
      class="border text-md w-full max-w-sm rounded-lg block p-2.5 bg-black border-gray-600 placeholder-gray-400 text-white focus:ring-blue-500 focus:border-blue-500 md:mr-4 mt-4 md:mt-0">
      <option value="curiosity">Curiosity</option>
      <option value="opportunity">Opportunity</option>
      <option value="spirit">Spirit</option>
    </select>
    <select id="cameras" v-model="selectedCamera" required
      class="border text-md w-full max-w-sm rounded-lg block p-2.5 bg-black border-gray-600 placeholder-gray-400 text-white focus:ring-blue-500 focus:border-blue-500 md:mr-4 mt-4 md:mt-0">
      <option v-for="camera in roverCameras[selectedRover]" :value="camera" :key="camera">{{ camera }}</option>
    </select>

    <button type="submit"
      class="text-white bg-gradient-to-r from-secondary to-primary hover:bg-gradient-to-br focus:ring-4 focus:outline-none font-medium rounded-lg text-sm px-10 py-2.5 text-center mt-4 md:mt-0">
      Search
    </button>
  </form>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import axios from 'axios'

interface Image {
  id: number
  img_src: string
  earth_date: string
  camera: {
    name: string
  }
}

interface RoverCameras {
  [rover: string]: string[]
}

export default defineComponent({
  data() {
    return {
      searchDate: '',
      selectedRover: 'curiosity',
      selectedCamera: 'FHAZ',
      roverCameras: {
        curiosity: ['FHAZ', 'RHAZ', 'NAVCAM', 'MAST', 'CHEMCAM', 'MAHLI', 'MARDI'],
        opportunity: ['FHAZ', 'RHAZ', 'NAVCAM', 'PANCAM', 'MINITES'],
        spirit: ['FHAZ', 'RHAZ', 'NAVCAM', 'PANCAM', 'MINITES'],
      } as RoverCameras,
    }
  },

  methods: {
    async searchImages(): Promise<void> {
      try {
        const apiKey = import.meta.env.VITE_API_KEY
        const url = `https://api.nasa.gov/mars-photos/api/v1/rovers/${this.selectedRover}/photos`

        const response = await axios.get<Image[]>(url, {
          params: {
            earth_date: this.searchDate,
            camera: this.selectedCamera,
            api_key: apiKey
          }
        })
        const images = response.data

        this.$emit('search-results', images)
      } catch (error) {
        console.error('Error occurred while looking for images:', error)
      }
    }
  },
  computed: {
    roverCameras(): string[] {
      return this.roverCameras[this.selectedRover] || []
    },
  },
})
</script>
