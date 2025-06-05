<script setup lang="ts">
import * as pdflib from "pdfjs-dist"
import { useTemplateRef, onMounted } from "vue"
pdflib.GlobalWorkerOptions.workerSrc = new URL(
  'pdfjs-dist/build/pdf.worker.min.mjs',
  import.meta.url
).toString()
const canvas = useTemplateRef('pdf-canvas')

const props = defineProps<{
    src: string
}>()

onMounted(() => {
    console.log(props.src)
    let loadingTask = pdflib.getDocument(props.src);
    loadingTask.promise.then(function(pdf) {
        console.log(props.src)
        pdf.getPage(1).then(function getPageHelloWorld(page) {
            console.log(props.src)
            let viewport = page.getViewport({scale: 2});
   
            //
            // Prepare canvas using PDF page dimensions
            //
            if (!canvas.value) return;
            let context = canvas.value.getContext('2d') as CanvasRenderingContext2D;
            canvas.value.height = viewport.height;
            canvas.value.width = viewport.width;
            console.log(viewport.height, viewport.width)

            //
            // Render PDF page into canvas context
            //
            page.render({canvasContext: context, viewport: viewport});
        });
        // you can now use *pdf* here
    });
})

</script>

<template>
    <canvas ref="pdf-canvas"></canvas>
</template>
