<template>
  <div>
    <canvas ref="pdfCanvas" id="pdf-canvas"></canvas>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import pdfjs from 'pdfjs-dist/build/pdf';

export default {
  name: 'PdfViewer',
  props: {
    pdfPath: "/Users/ryan/Downloads/attackflow10.14-main/attackflow_frontend/public/HTML_Smuggling.pdf", // 传递 PDF 文件的路径
  },
  setup(props) {
    const pdfCanvas = ref(null);

    onMounted(() => {
      const canvas = pdfCanvas.value;
      const pdfOptions = {
        url: props.pdfPath,
        scale: 1,
      };

      const task = pdfjs.getDocument(pdfOptions);

      task.promise
          .then((pdf) => pdf.getPage(1))
          .then((page) => {
            const context = canvas.getContext('2d');
            const viewport = page.getViewport({ scale: 1 });
            canvas.height = viewport.height;
            canvas.width = viewport.width;
            const renderContext = {
              canvasContext: context,
              viewport: viewport,
            };
            page.render(renderContext);
          });
    });

    return { pdfCanvas };
  },
};
</script>
