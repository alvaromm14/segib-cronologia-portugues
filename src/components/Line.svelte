<script>
    export let height;
    export let innerWidth;
    export let datos;
    export let vertical = false;
</script>

<!-- Línea principal -->
<line
    x1={vertical ? innerWidth / 2 : 0}
    y1={vertical ? 0 : height / 2.5}
    x2={vertical ? innerWidth / 2 : innerWidth}
    y2={vertical ? height : height / 2.5}
    stroke="#589ea4"
    stroke-width="1.5"
/>

<!-- Segmento 1 -->
<line
    x1={vertical ? innerWidth / 2 : (7 / (datos.length - 1)) * innerWidth}
    y1={vertical ? (7 / (datos.length - 1)) * height : height / 2.5}
    x2={vertical ? innerWidth / 2 : (12 / (datos.length - 1)) * innerWidth}
    y2={vertical ? (12 / (datos.length - 1)) * height : height / 2.5}
    stroke="#589ea4"
    stroke-width="1.5"
/>

<!-- Segmento 2 -->
<line
    x1={vertical ? innerWidth / 2 : (14 / (datos.length - 1)) * innerWidth}
    y1={vertical ? (14 / (datos.length - 1)) * height : height / 2.5}
    x2={vertical ? innerWidth / 2 : (20 / (datos.length - 1)) * innerWidth}
    y2={vertical ? (20 / (datos.length - 1)) * height : height / 2.5}
    stroke="#589ea4"
    stroke-width="1.5"
/>
