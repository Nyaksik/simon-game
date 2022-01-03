<template>
    <div class="wrapper" :class="{block: !block}">
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
        block: Boolean
    },
    data() {
        return {
            currentColor: '',
        }
    },
    methods: {
        userColor(color, url) {
            this.currentColor = color
            const audio = new Audio(url)
            audio.src = `assets/${color}.mp3`
            audio.play()
            this.$emit('create', this.currentColor)
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
        margin: 50px auto;
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
            background-color: rgba($color: black, $alpha: .3);
            z-index: 10;
        }
    }
    .field {
        width: 100px;
        height: 100px;
        cursor: pointer;
        transition: .2s;
        &:active {
            opacity: 1;
        }
    }
    .blue {
        background-color: blue;
        opacity: .5;
    }
    .red {
        background-color: red;
        opacity: .5;
    }
    .yellow {
        background-color: yellow;
        opacity: .5;
    }
    .green {
        background-color: green;
        opacity: .5;
    }
    .flash {
        opacity: 1;
    }
</style>