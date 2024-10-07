<template>
	<div class="pont-chaban" v-if="pont != null">

		
		<div class="preview">
			<div>Prochaine Fermeture</div>
			<div>le {{ nextClosing }}</div>
		</div>
		<div class="head" @click="showYear()" :class="{ 'hide' : !hide}">
			<i class="fa-solid fa-chevron-left" ></i>
			<div>{{ year }}</div>
		</div>
		<div class="month" :class="{ 'hide' : !hide}">
			<h3 :class="{ 'red': month === today.getMonth() && year === today.getFullYear()} ">{{ new Date(year, month, 1).toLocaleString('fr-FR', {month: "long"}) }}</h3>
			<div class="nav">
			<i class="fa-solid fa-chevron-left" @click="previousMonth()"></i>
			<span style="color: turquoise; cursor: pointer;" @click="goToday()">Aujourd'hui</span>
			<i class="fa-solid fa-chevron-right" @click="nextMonth()"></i>
			</div>
		</div>
		<div class="tryptique" :class="{ 'hide' : !hide}">
		<Calendrier @clik-on-day="setJour" v-touch:swipe.right="previousMonth" v-touch:swipe.left="nextMonth" :year="year" :month="month" :day="day" :pont="pont"/>
		</div>
		<div class="month" :class="{ 'hide' : hide}">
			<h3 :class="{ 'hide' : hide}">{{ year }}</h3>
			<div class="nav">
			<i class="fa-solid fa-chevron-left" @click="previousYear()"></i>
			<span style="color: turquoise; cursor: pointer;" @click="goToday()">Aujourd'hui</span>
			<i class="fa-solid fa-chevron-right" @click="nextYear()"></i>
			</div>
		</div>
		<div class="year-view" :class="{ 'hide' : hide}" v-touch:swipe.right="previousYear" v-touch:swipe.left="nextYear">
			<div v-for="i in 12" :key="i" class="min-month"  @click="showMonth(i)">
				<span :class="{ 'red': i - 1 === today.getMonth() && year === today.getFullYear()}">{{ new Date(year, i-1).toLocaleString('fr-FR', {month: "long"}) }}</span>
				<MinCalendrier @clik-on-day="setJour" :year="year" :month="i-1" :pont="pont"/>
			</div>
		</div>
		<div class="jour">
		<Jour :infos="returnInfo(dayInfo)" :class="{ 'hide' : !hide}" />
		</div>
	</div>
</template>

<script>
import Calendrier from "@/components/Calendrier.vue"
import MinCalendrier from "@/components/MinCalendrier.vue"
import Jour from "@/components/Jour.vue"
import Header from "@/components/Header.vue"

export default {
	components: {
		Calendrier,
		MinCalendrier,
		Jour,
		Header
  },
	data() {
		return {
			pont: null,
			today: new Date(),
			dayInfo: String,
			year: new Date().getFullYear(),
			month: new Date().getMonth(),
			hide: true,
			day: new Date().getDate(),
			nextClosing: String
		}
	},
	async created() {
		const response = await fetch(`https://opendata.bordeaux-metropole.fr/api/explore/v2.1/catalog/datasets/previsions_pont_chaban/records?limit=100`);
		const result = await response.json();
		this.pont = result.results;
		this.getNextClosing();
		/* this.pont = this.pont.filter((item) => {
			return this.returnFormatedDate(new Date(item.date_passage)) >= this.returnFormatedDate(this.today);
		}); */

		this.pont.sort((a, b) => {
			if(a.date_passage < b.date_passage){
				return -1;
			}
			if(a.date_passage > b.date_passage){
				return 1;
			}
			return 0;
		});

		this.dayInfo = this.returnFormatedDateToDisplay(this.today);
	},
	methods: {
		returnFormatedDate(date){
			const year = date.getFullYear();
			const month = String(date.getMonth() + 1).padStart(2, '0');
			const day = String(date.getDate()).padStart(2, '0');
			return `${year}/${month}/${day}`;
		},
		returnInfo(formatedDate){
			return this.pont.filter((item) => item.date_passage === formatedDate);
		},
		setJour(jour, i){
			this.dayInfo = this.returnFormatedDateToDisplay(jour);
			this.day = i;
		},
		returnFormatedDateToDisplay(date){
			const year = date.getFullYear();
			const month = String(date.getMonth() + 1).padStart(2, '0');
			const day = String(date.getDate()).padStart(2, '0');
			return `${year}-${month}-${day}`;
		},
		previousMonth(){
			if(this.month - 1 === this.today.getMonth() && this.year === this.today.getFullYear()){
				this.day = this.today.getDate();
			} else this.day = 1;
			if(this.month === 0){
				this.month = 11;
				this.year -= 1;
			} else this.month -=1;
			this.dayInfo = this.returnFormatedDateToDisplay(new Date(this.year, this.month, this.day));
		},
		nextMonth(){
			if(this.month + 1 === this.today.getMonth() && this.year === this.today.getFullYear()){
				this.day = this.today.getDate();
			} else this.day = 1;
			if(this.month === 11){
				this.month = 0;
				this.year += 1;
			} else this.month +=1;
			this.dayInfo = this.returnFormatedDateToDisplay(new Date(this.year, this.month, this.day));
		},
		previousYear(){
			this.year--;
		},
		nextYear(){
			this.year++;
		},
		showYear(){
			this.hide = !this.hide;
		},
		showMonth(month){
			this.hide = !this.hide;
			this.month = month - 1;
			if(this.month === this.today.getMonth() && this.year === this.today.getFullYear()){

				this.day = this.today.getDate();
			} else this.day = 1;
		},
		goToday(){
			this.month = this.today.getMonth();
			this.year = this.today.getFullYear();
			this.day = this.today.getDate();
		},
		getNextClosing(){
			const infos = this.pont.find((item) => {
							if(item.date_passage === this.returnFormatedDateToDisplay(this.today)
								 && (this.today) - (item.fermeture_a_la_circulation.slice(0,2) + item.fermeture_a_la_circulation.slice(3)) < 0){
								return true;
							}
							if(item.date_passage > this.returnFormatedDateToDisplay(this.today)){
								return true;
							}
						 	
								});
			const date = new Date(infos.date_passage);
			const heure = infos.fermeture_a_la_circulation;
			this.nextClosing = date.toLocaleDateString('fr-FR', { weekday: "long", day: "numeric", month: "long"});
			this.nextClosing += " à " + heure;
		},
		returnFormatedHour(date){
			const hours = date.getHours();
			const minutes = date.getMinutes();
			return hours + ":" + minutes;
		}
	}

}
</script>

<style scoped>

.pont-chaban{
	display: flex;
	flex-direction: column;
	align-items: flex-start;
	justify-content: flex-start;
	text-align: center;
	max-width: 500px;
	
}

.preview{
	display: flex;
	width: 90%;
	height: 35%;
	flex-direction: column;
	align-items: center;
	justify-content: space-around;
	border-radius: 10px;
	background-color: white;
	color: rgb(32, 36, 41);
	padding: 15px;
	margin: 20px 5px;
	font-weight: 600;
	gap: 1rem;
}

.month{
	display: flex;
	justify-content: space-between;
	align-items: center;
	font-size: 1.2rem;
	width: 100%;
	text-align: left;
	padding: 5px;
}

.nav{
	display: flex;
	align-items: center;
	justify-content: center;
	gap: 1.5rem;
	font-size: 1rem;
}

.head{
	display: flex;
	align-items: baseline;
	cursor: pointer;
	gap: 10px;
	color: turquoise;
}

.year-view{
	display: flex;
	align-items: flex-start;
	justify-content: space-between;
	flex-wrap: wrap;
	width: 100%;
	row-gap: 1rem
}

.jour{
	display: flex;
	justify-content: center;
	width: 100%;
}
.min-month{
	display: flex;
	flex-direction: column;
	align-items: flex-start;
	width: 33%;
	cursor: pointer;
}

.tryptique{
	display: flex;
	align-items: center;
	justify-content: center;
}

i{
	cursor: pointer;
	color: turquoise;
}

.hide{
	display: none;
}

.red{
    color: turquoise;
}

</style>

