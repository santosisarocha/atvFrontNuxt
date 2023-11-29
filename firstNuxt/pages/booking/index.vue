<script setup>
    console.log("fdhosjdoifjoij")
    
    
    

    const bookingDelete = async (idToDelete) =>{
       // console.log("id to delete: ", idToDelete)
       const {} = await useFetch(`http://localhost:8000/booking/${idToDelete}`, {
        method: 'delete',
        onResponse(){
            alert("booking successfully deleted")
            refreshNuxtData('loadBooking')
            
        },
        onResponseError(){
            alert("was not possible to delete this booking!")
        }
       });

    }


</script>


<template>
    <div>
      <h1>My booking List:</h1>
        <div v-for="booking in bookings.results" :key="booking.id">
            <span><strong>Booking id::</strong> {{ booking.id }} - </span>
            <span><strong>Trip:</strong> {{ booking.tripFK }} - </span>
            <span><strong>Start Date:</strong> {{ booking.startDate }} - </span>
            <span><strong>End Date:</strong> {{ booking.endDate }} - </span>
            <span><strong>Costumer:</strong> {{ booking.customUserFK }}. </span>
            <button @click="bookingDelete(booking.id)">Delete</button>
            <br/><br/>
        </div>
    </div>
  </template>