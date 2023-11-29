<script setup>
    const { data: tripImages } = await useFetch('http://localhost:8000/image')  
</script>


<template>
    <div>
        <h1>Trips Page!</h1>
        <div :id="'myImg-'+index" v-for="(imgReceived,index) in tripImages.results"
        :key="index">
            <NuxtLink :to="`/trips/${imgReceived.tripFk}`"> 
                <img :src="imgReceived.link">
            </NuxtLink>    
            <h2>{{ imgReceived.title }}</h2>
        </div>        
    </div>
</template>