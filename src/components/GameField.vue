<template>
    <div class="wrapper">
        <div
            class="field"
            :class="[{flash: color === field.color}, field.color]"
            @click="userColor(field.color)"
            v-for="field in fields"
            :key="field.id">
        </div>
        {{currentColor}}
    </div>
</template>

<script>
export default {
    props: {
        fields: Array,
        color: String,
        modelValue: Array,
        game: Boolean
    },
    data() {
        return {
            currentColor: ''
        }
    },
    methods: {
        userColor(color) {
            this.currentColor = color
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