<script>
    import { scale } from "svelte/transition";
    import { onMount } from "svelte";

    export let hoveredEvent;
    export let i;
    export let datos;
    export let infoElement;
    export let eventInfoElement;
    export let tooltipHeight = 0;
    export let eventTooltipHeight = 0;
    export let vertical = false;

    $: events = hoveredEvent
        ? [
              {
                  title: hoveredEvent.event,
                  desc: hoveredEvent.event_description,
                  image: hoveredEvent.image,
              },
              {
                  title: hoveredEvent.event1,
                  desc: hoveredEvent.event1_description,
                  image: hoveredEvent.image1,
              },
              {
                  title: hoveredEvent.event2,
                  desc: hoveredEvent.event2_description,
                  image: hoveredEvent.image2,
              },
          ].filter((e) => e.title)
        : [];

    const xSide = i / (datos.length - 1);

    $: imageSrc = hoveredEvent?.report
        ? `static/images/${hoveredEvent.report}.png`
        : null;

    onMount(() => {
        datos.forEach((d) => {
            if (d.report) new Image().src = `static/images/${d.report}.png`;
            ["image", "image1", "image2"].forEach((key) => {
                if (d[key]) new Image().src = `static/images/${d[key]}.png`;
            });
        });
    });

    const endThreshold = 0.8;
</script>

{#if hoveredEvent}
    <!-- 🟡 Tooltip normal -->
    {#if events.length}
        <g class="tooltip-group" transition:scale={{ duration: 400 }}>
            {#if !vertical}
                <line
                    y1="35"
                    y2={51 + eventTooltipHeight}
                    stroke="#ecba56"
                    stroke-width="1.5"
                />
            {/if}
            {#each events as e, index}
                {#if e.image && !vertical}
                    <foreignObject
                        x={xSide < 0.8 ? -93 : 5}
                        y={65 + index * 50}
                        width="90"
                        height="70"
                    >
                        <div
                            xmlns="http://www.w3.org/1999/xhtml"
                            style="display:flex; justify-content:center; align-items:center;"
                        >
                            <img
                                src={`static/images/${e.image}.png`}
                                alt={e.title}
                                style="width: 80px; height: auto;"
                                transition:scale={{
                                    duration: 400,
                                    delay: index * 150,
                                }}
                            />
                        </div>
                    </foreignObject>
                {/if}
            {/each}

            <foreignObject
                x={vertical ? 53 : xSide < 0.8 ? 8 : -228}
                y={vertical
                    ? xSide > endThreshold
                        ? -eventTooltipHeight + 21
                        : -6
                    : 65}
                width={vertical
                    ? innerWidth < 400
                        ? innerWidth / 2.9
                        : innerWidth / 2.7
                    : "250"}
                height={vertical ? "300" : "200"}
            >
                <div
                    xmlns="http://www.w3.org/1999/xhtml"
                    class="tooltip"
                    style="text-align: {vertical
                        ? 'left'
                        : xSide >= 0.8
                          ? 'right'
                          : 'left'};"
                    bind:this={eventInfoElement}
                >
                    {#each events as e, index}
                        <div
                            style="padding-bottom: 4px; pointer-events: auto;"
                            transition:scale={{
                                duration: index > 0 ? 400 : 0,
                                delay: index * 150,
                            }}
                        >
                            <!-- Título en negrita -->
                            <span style="font-weight: bold;">
                                {@html e.title.replace(
                                    /<a /g,
                                    '<a style="color:#ecba56; text-decoration:underline;" target="_blank" rel="noopener noreferrer" ',
                                )}
                            </span>

                            <!-- Descripción normal -->
                            {#if e.desc}
                                <span>
                                    {@html " " +
                                        e.desc.replace(
                                            /<a /g,
                                            '<a style="color:#ecba56; text-decoration:underline;" target="_blank" rel="noopener noreferrer" ',
                                        )}
                                </span>
                            {/if}
                        </div>

                        {#if index < events.length - 1 && !vertical}
                            <hr
                                class="tooltip-divider"
                                transition:scale={{
                                    duration: 400,
                                    delay: index * 200 + 50,
                                }}
                            />
                        {/if}
                    {/each}
                </div>
            </foreignObject>
        </g>
    {/if}

    <!-- 🔵 Tooltip info -->
    {#if hoveredEvent.info}
        <g class="tooltip-group" transition:scale={{ duration: 400 }}>
            {#if !vertical}
                <line
                    y1="-20"
                    y2={-tooltipHeight - 30}
                    stroke="#589ea4"
                    stroke-width="1.5"
                />
            {/if}

            <!-- Imagen sobre el tooltip info -->
            {#if imageSrc}
                <foreignObject
                    x={vertical ? -115 : xSide < 0.8 ? -95 : 5}
                    y={vertical
                        ? xSide > endThreshold
                            ? -tooltipHeight - 105
                            : -128
                        : -135}
                    width="90"
                    height="120"
                >
                    <div
                        xmlns="http://www.w3.org/1999/xhtml"
                        style="display:flex; justify-content:center; align-items:center;"
                        transition:scale={{ duration: 400 }}
                    >
                        <a
                            href={hoveredEvent.reportUrl}
                            target="_blank"
                            rel="noopener noreferrer"
                        >
                            <img
                                src={imageSrc}
                                alt={`${hoveredEvent.report}`}
                                style="width: 80px; height: auto;"
                            />
                        </a>
                    </div>
                </foreignObject>
            {/if}

            <!-- Número report -->
            {#if hoveredEvent.report}
                <rect
                    x={vertical ? -136 : xSide >= 0.8 ? "6" : "-29"}
                    y={vertical
                        ? xSide > endThreshold
                            ? -tooltipHeight - 6
                            : -28
                        : -158}
                    width="20"
                    height="16"
                    fill="white"
                    rx="4"
                    stroke="#589ea4"
                    user-select="none"
                />
                <text
                    x={vertical ? -126 : xSide >= 0.8 ? "16" : "-19"}
                    y={vertical
                        ? xSide > endThreshold
                            ? -tooltipHeight + 6
                            : -16
                        : -146}
                    text-anchor="middle"
                    font-size="0.7em"
                    fill="#589ea4"
                    font-weight="bold"
                >
                    {hoveredEvent.report}
                </text>
            {/if}

            <!-- Texto tooltip info -->
            <foreignObject
                x={vertical
                    ? innerWidth < 400
                        ? -170
                        : -210
                    : xSide < 0.8
                      ? 8
                      : -228}
                y={vertical
                    ? xSide > endThreshold
                        ? -tooltipHeight + 16
                        : -6
                    : -tooltipHeight - 32}
                width={vertical ? (innerWidth < 400 ? 140 : 180) : 250}
                height={tooltipHeight}
            >
                <div
                    xmlns="http://www.w3.org/1999/xhtml"
                    class="tooltip info-tooltip"
                    style="text-align: {vertical
                        ? 'right'
                        : xSide >= 0.8
                          ? 'right'
                          : 'left'};
           user-select: {vertical ? 'none' : 'text'};"
                    bind:this={infoElement}
                    on:mousedown|stopPropagation
                    on:mouseup|stopPropagation
                    on:click|stopPropagation
                >
                    {@html hoveredEvent.info
                        .replace(
                            /<b>/g,
                            '<b style="font-weight:bold; display:inline;">',
                        )
                        .replace(
                            /<a /g,
                            '<a style="color:#589ea4; text-decoration:underline;" target="_blank" rel="noopener noreferrer" ',
                        )}
                </div>
            </foreignObject>
        </g>
    {/if}
{/if}

<style>
    .tooltip {
        color: #ecba56;
        font-size: 13px;
        max-width: 220px;
        background: none;
        padding: 0;
    }

    .info-tooltip {
        color: #589ea4;
        font-size: 13px;
    }

    .tooltip-divider {
        border: none;
        border-top: 1px solid #ecba56;
        margin: 4px 0;
        opacity: 0.6;
    }

    img {
        border-radius: 4px;
        border: 1px solid rgba(0, 0, 0, 0.1);
        box-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
    }
</style>
