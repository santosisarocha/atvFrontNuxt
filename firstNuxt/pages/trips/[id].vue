<script setup>
    const route = useRoute();
    console.log("route", route);

    const { data: trip } = await useFetch(`http://localhost:8000/trip/${route.params.id}`,{
        key:'tripRequest'
    }) 
    const { data: availabilities } = await useFetch(`http://localhost:8000/availability?tripFK=${route.params.id}&onlyAvailable=true`,{
        key:'availRequest'
    })

    let showForm = false;
    const setShowForm = () => {
        showForm = true;
        console.log("showForm", showForm)
        refreshNuxtData() // call nuxt to update dom
    }   

    let startDate;
    let endDate;
    let peopleAmount;
    let hasPet;

    const sendForm = async () => {
        console.log("startDate",startDate)
        console.log("endDate",endDate)
        console.log("peopleAmount",peopleAmount)
        console.log("hasPet",hasPet)

        await useFetch('http://localhost:8000/booking/',{
            method: 'POST',
            body: {
                customUserFK: 1,
                tripFK: route.params.id,
                startDate: startDate,
                endDate: endDate,
                people: peopleAmount,
                hasPet: hasPet
            },
            key: 'bookingRequest'            
        })

         
    }
</script>



<template>
    <div>
        <h1> #{{ trip.id }}: {{ trip.title }} </h1>
        <p> {{ trip.description  }} </p>
        <span>Endereço: </span> <span> {{ trip.address  }} </span> <br>
        <h3> Investimento: R$ {{ trip.daily }} </h3>

        <button @click="setShowForm">Reserve já!</button> 
        
        <br/><br/>
            
        

        <!-- <p v-for="availability in availabilities.results"
        :key="availability.id">{{ availability.date }}</p> -->
        
        
        
        <section v-if="showForm === true">
            <select>
                <option  v-for="availability in availabilities.results"
                :key="availability.id" :value="availability.date">{{ availability.date }}</option>
            </select>
            <div>
                <label for="">Data Inicial:</label> <input type="date" v-model="startDate"> <br>
            </div>
            <div>
                <label for="">Data Final:</label> <input type="date" v-model="endDate"> <br>
            </div>
            <div>
                <label for="">Qtd Pessoas:</label> <input type="number" v-model="peopleAmount"> <br>
            </div>
            <div>
                <label for="">Tem Pet?</label> <input type="checkbox" v-model="hasPet"> <br>
            </div>
            <button @click="sendForm">Enviar</button>
        </section>

        <!-- <p>Nome: {{ peopleFound.name }}</p>
        <p>Height: {{ peopleFound.height }}</p>
        
        <h3>Do you want another character?</h3>
        <label for="">Character Id: </label><input v-model="character" type="number">
        <button @click="getCharacter">Find</button> -->
    </div>
</template>