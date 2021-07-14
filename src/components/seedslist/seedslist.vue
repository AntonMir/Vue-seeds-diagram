<template>
    <div class="seeds-list-section">
        <h3 class="seeds-list-section-title">Удаление элементов
            <button @click="hiddenSeeds">{{hiddenSeedsValue ? 'ᐯ' : 'ᐱ' }}</button>
        </h3>
        <ul :class="hiddenSeedsValue ? 'seeds-list' : 'hidden'">
            <li 
                class="seeds-list-el"
                v-for="seed in seedCurrentList" 
                :key="seed.name"> 
                {{seed.name}}
                <button @click="$emit('remove-seed', seed.name)">🗙</button>
            </li>
        </ul>
    </div>
</template>

<script>

export default {
    props: [
        'currentSeeds'
    ],

    data() {
        return {
            seedCurrentList: this.currentSeeds,
            hiddenSeedsValue: true
        }
    },
    methods: {
        hiddenSeeds() {
            // скрывает\показываем панель удаления семян
            this.hiddenSeedsValue = !this.hiddenSeedsValue;
        }
    },
    watch: {
        currentSeeds: {
            handler() {
                this.seedCurrentList = this.currentSeeds;
            }
        }
    }
    
};
</script>

<style lang="scss" src="./seedslist.sass"></style>
