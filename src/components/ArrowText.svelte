<script>
    import { fade } from "svelte/transition";

    export let innerWidth;
    export let height;
    export let datos;
    export let vertical = false;

    // Coordenadas base según orientación
    $: firstPos = vertical ? 10 : (0 / (datos.length - 1)) * innerWidth;

    $: center = vertical ? innerWidth / 2 : height / 2.5;

    export let show = true;

    const textHeight = 28;
    const arrowLength = 35;
</script>

<svg>
    {#if show}
        <!-- Texto superior / izquierdo -->
        <g
            in:fade={{ delay: 1000, duration: 500 }}
            out:fade={{ duration: 300 }}
        >
            {#if vertical}
                <text
                    y={firstPos}
                    fill="#589ea4"
                    font-size="13"
                    font-weight="600"
                    font-style="italic"
                    text-anchor="middle"
                >
                    <tspan x={center / 2.5} dy="0">Informe de la </tspan>
                    <tspan x={center / 2.5} dy="1.2em">
                        Cooperación Sur-Sur</tspan
                    >
                    <tspan x={center / 2.5} dy="1.2em"> y Triangular </tspan>
                    <tspan x={center / 2.5} dy="1.2em"> en Iberoamérica</tspan>
                </text>

                <!-- Flecha vertical hacia abajo -->
                <path
                    d="M {center / 2.5},{64} L {center / 2.5},{firstPos +
                        textHeight +
                        arrowLength}"
                    stroke="#589ea4"
                    stroke-width="1"
                    fill="none"
                />
                <path
                    d="M {center / 2.5 - 5},{firstPos +
                        textHeight +
                        arrowLength} L {center / 2.5 + 5},{firstPos +
                        textHeight +
                        arrowLength} L {center / 2.5},{firstPos +
                        textHeight +
                        arrowLength +
                        5} Z"
                    fill="#589ea4"
                />
            {:else}
                <!-- Horizontal -->
                <path
                    d="M {firstPos},{center - 75} L {firstPos + 80},{center -
                        75}"
                    stroke="#589ea4"
                    stroke-width="1"
                    fill="none"
                />
                <path
                    d="M {firstPos + 75},{center - 80} L {firstPos +
                        80},{center - 75} L {firstPos + 75},{center - 70} Z"
                    fill="#589ea4"
                />
                <text
                    x={firstPos - 50}
                    y={center - 105}
                    fill="#589ea4"
                    font-size="13"
                    font-weight="600"
                    font-style="italic"
                    text-anchor="start"
                >
                    <tspan x={firstPos} dy="0">Relatório da CSS e</tspan>
                    <tspan x={firstPos} dy="1.2em">TR na Ibero-América</tspan>
                </text>
            {/if}
        </g>

        <!-- Texto inferior / derecho -->
        <g
            in:fade={{ delay: 1000, duration: 500 }}
            out:fade={{ duration: 300 }}
        >
            {#if vertical}
                <text
                    y={firstPos}
                    fill="#ecba56"
                    font-size="13"
                    font-weight="600"
                    font-style="italic"
                    text-anchor="middle"
                >
                    <tspan x={center + center / 1.5} dy="0"
                        >Agenda Internacional</tspan
                    >
                    <tspan x={center + center / 1.5} dy="1.2em"
                        >para o Desenvolvimento</tspan
                    >
                    <tspan x={center + center / 1.5} dy="1.2em">
                        e Agenda Global
                    </tspan>
                    <tspan x={center + center / 1.5} dy="1.2em">
                        de CSS e TR
                    </tspan>
                </text>

                <!-- Flecha vertical hacia abajo -->
                <path
                    d="M {center + center / 1.5},{64} L {center +
                        center / 1.5},{firstPos + textHeight + arrowLength}"
                    stroke="#ecba56"
                    stroke-width="1"
                    fill="none"
                />
                <path
                    d="M {center + center / 1.5 - 5},{firstPos +
                        textHeight +
                        arrowLength} L {center + center / 1.5 + 5},{firstPos +
                        textHeight +
                        arrowLength} L {center + center / 1.5},{firstPos +
                        textHeight +
                        arrowLength +
                        5} Z"
                    fill="#ecba56"
                />
            {:else}
                <path
                    d="M {firstPos},{center + 75} L {firstPos + 80},{center +
                        75}"
                    stroke="#ecba56"
                    stroke-width="1"
                    fill="none"
                />
                <path
                    d="M {firstPos + 75},{center + 70} L {firstPos +
                        80},{center + 75} L {firstPos + 75},{center + 80} Z"
                    fill="#ecba56"
                />
                <text
                    x={firstPos + 85}
                    y={center + 96}
                    fill="#ecba56"
                    font-size="13"
                    font-weight="600"
                    font-style="italic"
                    text-anchor="start"
                >
                    <tspan x={firstPos} dy="0">Agenda Internacional para</tspan>
                    <tspan x={firstPos} dy="1.2em"
                        >o Desenvolvimento e Agenda</tspan
                    >
                    <tspan x={firstPos} dy="1.2em">Global de CSS e TR</tspan>
                </text>
            {/if}
        </g>
    {/if}
</svg>
