<template>
    <div class="flex flex-wrap gap-5 justify-center">
        <div class="flex flex-wrap gap-5 justify-center">
            <carousel :items-to-show="4" :wrap-around="true">
                <slide v-for="image in images" :key="image.id">
                    <div class="max-w-sm border rounded-lg shadow shadow-black bg-gray-700 border-black">
                        <a :href="image.img_src" target="_blank">
                            <img class="rounded-t-lg" :src="image.img_src" alt="Image" />
                        </a>
                        <div class="p-5">
                            <p class="mb-3 text-white font-bold">{{ image.earth_date }}</p>
                            <p class="mb-3 font-bold text-white">{{ image.rover.name }}</p>
                            <p class="mb-3 font-bold text-white">{{ image.camera.full_name }}</p>
                        </div>
                    </div>
                </slide>

                <template #addons>
                    <navigation />
                    <pagination />
                </template>
            </carousel>
        </div>
    </div>
</template>
  
<script lang="ts">
import { defineComponent, type PropType } from 'vue';
import 'vue3-carousel/dist/carousel.css'
import { Carousel, Slide, Pagination, Navigation } from 'vue3-carousel'

interface Camera {
    id: number;
    name: string;
    rover_id: number;
    full_name: string;
}

interface Rover {
    id: number;
    name: string;
    landing_date: string;
    launch_date: string;
    status: string;
}

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
        Carousel,
        Slide,
        Pagination,
        Navigation,
    },
    props: {
        images: {
            type: Array as PropType<Image[]>,
            required: true,
            default: () => []
        },
    },
});
</script>