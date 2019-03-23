<template>
    <div class="alert alert-primary">
        <h2>Success: {{ stats.success }}</h2>
        <h2>Error: {{ stats.error }}</h2>
        <div class="buttons">
            <button class="btn btn-primary" @click="$emit('repeat')">Repeat level</button>
            <button v-if="level < 2" class="btn btn-success" @click="$emit('nextLevel')">Next Level</button>
            <button v-if="level == 2" class="btn btn-primary" @click="$emit('toStart')">To Start</button>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['stats', 'level']
    }
</script>

<style lang="scss">
    
</style>