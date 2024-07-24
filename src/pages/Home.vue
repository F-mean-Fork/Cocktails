<script setup>
    import {ref} from 'vue';
    import AppLayout from '../components/AppLayout.vue';
    import CocktailsThumb from '../components/CocktailsThumb.vue';
    import {useRootStore} from '@/stores/root';
    import {storeToRefs} from 'pinia'

    const rootStore = useRootStore();
    rootStore.getIngredients();

    const {ingredients, cocktails, ingredient} = storeToRefs(rootStore);

    function getCocktails(params) {
        rootStore.getCocktails(rootStore.ingredient);
    };

    function removeIngregient(){
        rootStore.setIngredient(null)
    }
</script>

<template>
    <AppLayout imgUrl="https://i.postimg.cc/4dBG267b/cocktail1.jpg" :backFunc="removeIngregient" :is-back-button-visible="!!ingredient">
        <div class="wrapper">
            <div v-if="!ingredient || !cocktails" class="info">
                <div class="title">Choose your drink</div>
                <div class="line"></div>
                <div class="select-wrapper">
                    <el-select 
                        v-model="ingredient" 
                        placeholder="Choose main ingredient" 
                        style="large" 
                        filterable
                        allow-create
                        class="select"
                        @change="getCocktails"
                    >
                        <el-option
                            v-for="item in ingredients"
                            :key="item.strIngredient1"
                            :label="item.strIngredient1"
                            :value="item.strIngredient1"
                        />
                    </el-select>
                </div>
                <div class="text">
                    Try our delicious cocktail recipes for every occasion. Find delicious cocktail recipes by ingredient through our cocktail generator.
                </div>
                <img src="../assets/img/exmpl.png" alt="cocktails" class="img">
            </div>
            <div v-else class="info">
                <div class="title">COCKTAILS WITH {{rootStore.ingredient}} </div>
                <div class="line"></div> 
                <div class="cocktails">
                    <CocktailsThumb 
                        v-for="cocktail in cocktails" 
                        :key="cocktail.idDrink" 
                        :cocktail="cocktail"
                    />
                </div>               
            </div>
        </div>
    </AppLayout>;
</template>

<style lang="scss" scoped>
    @import '../assets/styles/main';

    .select-wrapper{
        padding-top: 50px;
    };

    .select{
        width: 220px;
    };

    .text{
        padding-top: 50px;
        line-height: 36px;
        letter-spacing: 0.1em;
        color: $textMuted;
        max-width: 516px;
        margin: 0 auto;
    };

    .img{
        margin-top: 60px;
    };

    .cocktails{
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        max-height: 400px;
        overflow-y: auto;
        margin-top: 60px;
    }
</style>