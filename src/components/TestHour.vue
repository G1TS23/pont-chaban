<template>
    <div class="hour">
        <i class="bi bi-clock"></i> {{ hours }}:{{ minutes }}:{{ seconds }}
    </div>
</template>

<script>

export default {
    data(){
        return{
            hours: new Date().getHours(),
            minutes: new Date().getMinutes(),
            seconds: new Date().getSeconds()
        }
    },
    created(){
        setInterval(() => {
            this.hours = new Date().getHours();
            this.minutes = String(new Date().getMinutes()).padStart(2, '0');
            this.seconds = String(new Date().getSeconds()).padStart(2, '0');
        }, 100);
    }
}

</script>

<style scoped>

.hour{
    font-family: 'Roboto', monospace;
}

@media screen and (max-width: 430px) {

    i{
        display: none;
    }
    
}
</style>