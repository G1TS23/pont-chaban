<template>
    <div v-if="infos.length !== 0" class="jour">
        <div v-for="info in infos">
            <h4>{{ info.bateau }}</h4>
            <span>Fermeture : {{ info.fermeture_a_la_circulation }}</span><br>
            <span>Ouverture : {{ info.re_ouverture_a_la_circulation }}</span>
        </div>
    </div>
    <div v-else class="jour">
        Pas de fermeture
    </div>
</template>

<script>

export default {
    props:{
        infos: null
    }
    
}

</script>

<style scoped>

.jour{
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 25vh;
    align-items: center;
    justify-content: center;
    border-radius: 10px;
    font-size: 1rem;
	background-color: white;
	color: rgb(32, 36, 41);
	margin: 30px;

}

</style>