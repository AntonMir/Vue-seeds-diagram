<template>
    <div id="app">
        <span class="app-wrapper">
            <Diagram
                :currentSeeds="currentSeeds"
            />

            <SeedsInput
                @getNewSeedData="pushNewSeed"
                :currentSeeds="currentSeeds"
                :removedSeed="removedSeed"
            />
        </span>
        <SeedsList 
            :currentSeeds="currentSeeds"
            @remove-seed="removeSeed"
        />
    </div>
</template>

<script>
// components
import SeedsInput from '../seedsinput/seedsinput.vue';
import Diagram from '../diagram/diagram.vue';
import SeedsList from '../seedslist/seedslist.vue'
// data
import baseCurrentSeeds from '../../data/currentseeds';

export default {
    name: "App",
    components: {
        SeedsInput,
        Diagram,
        SeedsList
    },
    data() {
        return {
            currentSeeds: baseCurrentSeeds,
            removedSeed: ''
        }
    },
    methods: {
        pushNewSeed(data) {
            // получаем из формы новый объект с данными семян
            // и добавляем его в наш массив данных
            this.currentSeeds.push(data)
        },
        removeSeed(name) {
            // удаляем смена по соответствию имени
            this.currentSeeds = this.currentSeeds.filter(seedEl => {
               return seedEl.name !== name
            });
            this.removedSeed = name;
        }
    },
};

</script>

<style lang="scss" src="./app.sass"></style>
