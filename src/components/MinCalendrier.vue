<template>
    <div class="calendar">
        <div class="view">
            <div class="day" v-for="i in (getDaysInMonth() + firstDay - 1)" :key="i">

                <div v-if="i < firstDay " class="hidden"></div>
                <div v-else class="show" :class="{  'today' : i - firstDay + 1 === today.getDate() && today.getFullYear() === year && today.getMonth() === month,
                                        'week-end' : ((i % 7 === 6) || (i % 7 === 0))}">
                                        {{ i - firstDay + 1 }}
                </div>
            </div>
        </div>
    </div>


</template>

<script>

import Jour from "@/components/Jour.vue"

export default {
    components: {
        Jour
    },
    props:{
        pont: null,
        year: Number,
        month: Number
    },
    updated(){
        
        const day = new Date(this.year, this.month, 1).getDay()
        if(day === 0){
            this.firstDay = 7;
        } else {
            this.firstDay = day;
        }
    },
    data(){
        return{
            today: new Date(),
            firstDay: new Date(this.year, this.month, 1).getDay(),
            displayYear: new Date().getFullYear(),
            displayMonth: new Date().getMonth(),
            displayDay: new Date().getDay(),
            highlight: new Date().getDay()
        }
    },
    methods: {
        getDaysInMonth(){
            const daysInMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
            
            if(this.month === 1 && this.year % 4 === 0 ){
                return 29;
            }

            return daysInMonth[this.month];
        },
        returnDayToDisplay(i){
            this.displayDay = i;
            const dayToDisplay = new Date(this.year, this.month, i - this.firstDay + 1);

            this.$emit('clikOnDay', dayToDisplay);
        },
        returnFormatedDateToDisplay(date){
			const year = date.getFullYear();
			const month = String(date.getMonth() + 1).padStart(2, '0');
			const day = String(date.getDate()).padStart(2, '0');
			return `${year}-${month}-${day}`;
		}
    }
}

</script>

<style scoped>

.calendar{
    display: flex;
    align-items: center;
    justify-content: center; 
    width: 100%;
    max-width: 600px;
}

.view{
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    gap: 0;
}


.day{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 14.2%;
    height: 10%;
    gap: 0;
    margin-top: 3px;
}

.week-end{
    color: grey;
}


h3{
    margin: 0;
    
}

span{
    display: flex;
    align-items: center;
    justify-content: center;
    width: 30%;
    height: 10%;
    text-align: center;
    border-radius: 100px;
    margin: 0;
    padding: 0;
    
}

.show{
    display: flex;
    align-items: center;
    justify-content: center;
    width: 70%;
    aspect-ratio: 1;
    text-align: center;
    font-size: 0.7rem;
    border-radius: 100px;
    margin: 0;
    padding: 0;
}

.hidden{
    display: flex;
    align-items: center;
    justify-content: center;
    width: 70%;
    aspect-ratio: 1;
    text-align: center;
    font-size: 0.7rem;
    font-weight: 900;
    border-radius: 100px;
    margin: 0;
    padding: 0;
}

.today{
    background-color: turquoise;
    color: rgb(32, 36, 41);
}


</style>