<template>
    <div class="wrapper" :class="{block: !block || flash}">
        <div
            class="field"
            :class="[{flash: color === field.color}, field.color]"
            @click="userColor(field.color)"
            v-for="field in fields"
            :key="field.id">
        </div>
    </div>
</template>

<script>
export default {
    props: {
        fields: Array,
        color: String,
        block: Boolean,
        flash: Boolean
    },
    methods: {
        userColor(color) {
            this.currentColor = color
            const audio = new Audio()
            audio.src = `assets/${color}.mp3`
            audio.play()
            this.$emit('create', color)
        },
    },
}
</script>

<style lang="scss">
    .wrapper {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
        width: 200px;
        position: relative;
    }
    .block{
        position: relative;
        &::before {
            content: '';
            width: 200px;
            height: 200px;
            position: absolute;
            z-index: 10;
        }
    }
    .field {
        width: 100px;
        height: 100px;
        cursor: pointer;
        opacity: .5;
        transition: .2s;
        &:active {
            opacity: 1;
        }
    }
    .blue {
        background-color: blue;
    }
    .red {
        background-color: red;
    }
    .yellow {
        background-color: yellow;
    }
    .green {
        background-color: green;
    }
    .flash {
        opacity: 1;
    }
</style>