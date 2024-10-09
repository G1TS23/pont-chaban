<template>
    <div class="calendar">
        <div class="view">
            <div class="week-day">
                <h3><span>l</span></h3>
                <h3><span>m</span></h3>
                <h3><span>m</span></h3>
                <h3><span>j</span></h3>
                <h3><span>v</span></h3>
                <h3><span class="week-end">s</span></h3>
                <h3><span class="week-end">d</span></h3>
            </div>
            <div class="day" v-for="i in (getDaysInMonth() + firstDay - 1)" :key="i">

                <div v-if="i < firstDay " class="hidden"></div>
                <div v-else class="show" :class="{  'today' : i - firstDay + 1 === today.getDate() && today.getFullYear() === year && today.getMonth() === month,
                                        'week-end' : ((i % 7 === 6) || (i % 7 === 0)),
                                        'highlight-today': i - firstDay + 1 === today.getDate() && today.getFullYear() === year && today.getMonth() === month && i === day + firstDay -1}">
                                        {{ i - firstDay + 1 }}
                </div>
                <div class="dot-hidden">.</div>
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
        year: Number,
        month: Number,
        day: Number
    },
    updated(){
        
        const day = new Date(this.year, this.month, 1).getDay()
        if(day === 0){
            this.firstDay = 7;
        } else {
            this.firstDay = day;
        }
        /* this.displayDay = this.firstDay;
        this.returnDayToDisplay(this.displayDay); */
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
    max-width: 500px;
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
    border-bottom: 1px solid grey;
}

.week-end{
    color: grey;
}

.week-day{
    display: flex;
    align-items: center;
    justify-content: space-around;
    width: 100%;
    border-bottom: 1px solid grey;
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
    /*height: 40px;*/
    aspect-ratio: 1;
    text-align: center;
    font-size: 1rem;
    border-radius: 100px;
    margin: 0;
    padding: 0;
    cursor: pointer;
}

.hidden{
    display: flex;
    align-items: center;
    justify-content: center;
    width: 70%;
    /*height: 40px;*/
    aspect-ratio: 1;
    text-align: center;
    font-size: 1rem;
    font-weight: 900;
    border-radius: 100px;
    margin: 0;
    padding: 0;
}

.today{
    color: turquoise;
}

.highlight{
    background-color: white;
    color: rgb(32, 36, 41);
}

.highlight-today{
    background-color: turquoise;
    color: rgb(32, 36, 41);
}

.dot-hidden{
    margin-top: -15px;
    padding: 0;
    font-size: 1.5rem;
    font-weight: 900;
    color: rgb(32, 36, 41);
}


</style>