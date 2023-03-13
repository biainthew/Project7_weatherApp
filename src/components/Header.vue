<template>
    <div id="header">
        <span class="title-word title-word-1">Weather</span>
        <span class="title-word title-word-2">App</span>
    </div>
</template>

<script>
export default {
    name : 'Header'
}
</script>

<style scoped>
    #header{
        width: fit-content;
        margin: 0 auto;
        margin-top: 100px;
        margin-bottom: 20px;
    }
    .title-word{
        font-size: 100px;
        animation: color-animation 4s linear infinite;
        font-family: "Montserrat", sans-serif;
        font-weight: 800;
        text-transform: uppercase;
        text-shadow: 5px 5px 3px #00000096;
    }
    .title-word-1{
        --color-1: #DF8453;
        --color-2: #3D8DAE;
        --color-3: #E4A9A8;
    }
    .title-word-2 {
        --color-1: #DBAD4A;
        --color-2: #ACCFCB;
        --color-3: #17494D;
    }
    @keyframes color-animation {
        0%    {color: var(--color-1)}
        32%   {color: var(--color-1)}
        33%   {color: var(--color-2)}
        65%   {color: var(--color-2)}
        66%   {color: var(--color-3)}
        99%   {color: var(--color-3)}
        100%  {color: var(--color-1)}
    }
</style>