<script setup>
import { ref } from "vue";


const hoteles = ref([]);
const lugar = ref("");
const buscando = ref(false);

const buscarHotel = async () => {
    if (lugar.value.length == 0) return;

    buscando.value = true;

    let lugarFormateado = lugar.value.replace(" ", "_");

    const res = await fetch(`http://localhost:8000/hotel/${lugarFormateado}`, {
        method: 'GET',
        mode: 'cors',
    });

    const hotelesEncontrados = await res.json();

    hoteles.value = hotelesEncontrados;
    buscando.value = false;
}
</script>

<template>
    <div class="vista">
        <div class="formulario-busqueda">
            <input v-model="lugar" type="text" placeholder="New York">
            <button @click="buscarHotel" class="btn">🔍</button>
        </div>
        <div class="hoteles">
            <span v-if="buscando" class="loader"></span>
            <div v-for="(hotel, i) in hoteles" key="i" class="hotel">
                <div class="hotel-nombre">
                    <p>🏨</p>
                    <p>{{ hotel.name }}</p>
                </div>
                <div class="hotel-direccion">
                    <p>{{ `${hotel.hotelAddress.street}, ${hotel.hotelAddress.city}, ${hotel.hotelAddress.province}` }}
                    </p>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.formulario-busqueda {
    display: flex;
    width: 100%;
    justify-content: center;
    column-gap: 1rem;
}

.formulario-busqueda input {
    border-radius: 8px;
    border: 1px solid transparent;
    padding: 0.6em 1.2em;
    font-size: 1em;
    font-weight: 500;
    font-family: inherit;
    background-color: #f9f9f9;
    cursor: pointer;
    transition: border-color 0.25s;
}

.formulario-busqueda input:hover {
    border-color: #646cff;
}

.formulario-busqueda input:focus,
.formulario-busqueda input:focus-visible {
    outline: 4px auto -webkit-focus-ring-color;
}

.hoteles {
    width: 80%;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: 2rem;
}

.hoteles .hotel {
    display: flex;
    flex-direction: column;
    width: minmax(250px, 40%);
    padding: 1rem 1rem;
    border-bottom: 1px solid #646cff;
}

.hoteles .hotel p {
    margin: 0;
}

.hoteles .hotel .hotel-nombre {
    display: flex;
    column-gap: 0.5rem;
    font-size: 1.5rem;

}

.loader {
    width: 48px;
    height: 48px;
    border-radius: 50%;
    position: relative;
    animation: rotate 1s linear infinite
}

.loader::before {
    content: "";
    box-sizing: border-box;
    position: absolute;
    inset: 0px;
    border-radius: 50%;
    border: 5px solid #646cff;
    animation: prixClipFix 2s linear infinite;
}

@keyframes rotate {
    100% {
        transform: rotate(360deg)
    }
}

@keyframes prixClipFix {
    0% {
        clip-path: polygon(50% 50%, 0 0, 0 0, 0 0, 0 0, 0 0)
    }

    25% {
        clip-path: polygon(50% 50%, 0 0, 100% 0, 100% 0, 100% 0, 100% 0)
    }

    50% {
        clip-path: polygon(50% 50%, 0 0, 100% 0, 100% 100%, 100% 100%, 100% 100%)
    }

    75% {
        clip-path: polygon(50% 50%, 0 0, 100% 0, 100% 100%, 0 100%, 0 100%)
    }

    100% {
        clip-path: polygon(50% 50%, 0 0, 100% 0, 100% 100%, 0 100%, 0 0)
    }
}
</style>