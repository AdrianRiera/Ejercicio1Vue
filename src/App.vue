<script setup>
    import { ref, reactive, onMounted, watch } from 'vue'
    import {db} from './data/guitarras'
    import Guitarra from './components/Guitarra.vue'
    import Header from './components/Header.vue'
    import Footer from './components/Footer.vue'
    
    const guitarras = ref([])
    const carrito = ref([])
    const guitarra = ref ({})
    
    watch(carrito, () =>{
        guardarLocalStorage()
    }, {
        deep: true
    })

    onMounted(() =>{
        guitarras.value = db;
        guitarra.value = db[2]

        const carritoLocalStorage = localStorage.getItem('carrito')
        if(carritoLocalStorage){
            carrito.value = JSON.parse(carritoLocalStorage)
        }
    })

    const guardarLocalStorage = () =>{
        localStorage.setItem('carrito', JSON.stringify(carrito.value))
    }

    const agregarCarrito = (guitarra) =>{
        const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)
        if (existeCarrito >=0){
            carrito.value[existeCarrito].cantidad++
        } else{
            guitarra.cantidad = 1
            carrito.value.push(guitarra)
        }

        
        
    }

    const decrementarCantidad = (id) =>{
        const index=carrito.value.findIndex(producto => producto.id === id)
        if(carrito.value[index].cantidad>1){
            carrito.value[index].cantidad--
        }
        
    }
    const aumentarCantidad = (id) =>{
        const index=carrito.value.findIndex(producto => producto.id === id)
        carrito.value[index].cantidad++
        
    }

    const eliminarProducto = (id) =>{
       carrito.value = carrito.value.filter(producto => producto.id !== id)
        
    }

    const vaciarCarrito = () =>{
        carrito.value = []
        
    }
</script>

<template>
    <Header 
        :carrito="carrito"
        :guitarra="guitarra"
        @decrementar-cantidad="decrementarCantidad"
        @aumentar-cantidad="aumentarCantidad"
        @agregar-carrito="agregarCarrito"
        @eliminar-producto="eliminarProducto"
        @vaciar-carrito="vaciarCarrito"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
            <Guitarra
                v-for="guitarra in guitarras" 
                :guitarra="guitarra"
                @agregar-carrito="agregarCarrito"      
            />
        </div>
    </main>

    <Footer />
</template>

<style scoped>

</style>
 