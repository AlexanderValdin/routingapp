<script setup>
import carsData from '@/data.json'
import { ref, watch, onMounted } from 'vue';
import { useRouter, useRoute } from 'vue-router';

const router = useRouter()
const route = useRoute()

const cars = ref(carsData)
const make = ref('')

onMounted(()=> {
    console.log(route.query)
    make.value = route.query.make || '' 
})

watch(make, () => {
    if(make.value) {
        if (make.value === 'All') cars.value = carsData
        else {
            cars.value = carsData.filter(car => car.make === make.value)
        }
    }
})

const handleChange = () => {
    router.push({query: {make: make.value}})
}
</script>

<template>
  <main class="container">
    <h1>Our Cars</h1>
    <select @change="handleChange" v-model="make">
        <option selected value="All">All</option>
        <option value="Chevrolet">Chevrolet</option>
        <option value="Porsche">Porsche</option>
        <option value="Audi">Audi</option>
    </select>
    <div class="cards">
      <div 
      v-for="car in cars" 
      class="card"
      @click="router.push(`/car/${car.id}`)"
      >
        <h2>{{car.make}}</h2>
        <p>$ {{car.price}}</p>
      </div>
    </div>
  </main>
</template>

<style scoped>
.cards {
  display: flex;
  width: 1000px;
  flex-wrap: wrap;
  margin-top: 50px;
  justify-content: center;
}
.card {
  box-shadow: 1px 1px 10px rgba(147, 147, 147, 0.207);
  padding: 15px;
  width: 150px;
  margin-right: 15px;
  cursor: pointer;
  margin-bottom: 20px;
}

</style>
