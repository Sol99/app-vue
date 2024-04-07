<template>
    <div class="card d-flex justify-content-center pb-1 mb-3 mt-3">
        <div class="card-body">
            <h5 class="card-title fw-bold">{{ name }}</h5>
            <div class="row">
                <div class="col-md-3">
                    <span class="number p-1 fw-semibold rounded">#{{ id }}</span>
                </div>
                <div class="col-md">
                    <span class="date fw-bolder">{{ formattedDate }}</span>
                </div>
            </div>
            <div class="row">
                <div class="col pt-1 pb-2">
                    <span class="address fw-bold">{{ address }}</span>
                </div>
                <hr>
            </div>
            <div v-if="state === 1" class="row">
                <a class="custom-link d-flex justify-content-end link-underline link-underline-opacity-0 fw-bolder pe-3" >VER FICHA</a>
            </div>
            <div v-if="state === 2" class="row">
                    <div class="col-md-6">
                        <div class="d-flex align-items-center">
                            <span class="style-title fw-bold">TIMBRADO:</span>
                            <span v-if="timbrado === true || timbradoPaid" style="margin-left: 10px;"><i class="bi bi-check-circle-fill fs-4 text-success "></i></span>
                            <span v-if="timbrado === false && !timbradoPaid" style="margin-left: 10px;"><button class="btn-pay btn" @click="handlePayment('timbrado')">PAGAR</button></span>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="d-flex align-items-center">
                            <span class="style-title fw-bold">CEP:</span>
                            <span v-if="cep === true || cepPaid"><i class="bi bi-check-circle-fill fs-4 text-success" style="margin-left: 10px;"></i></span>
                            <span v-if="cep === false&& !cepPaid"><button class="btn-pay btn" style="margin-left: 10px;" @click="handlePayment('cep')">PAGAR</button></span>
                        </div>
                    </div>
            </div>
            <div v-if="state === 3" class="row">
                <div class="d-flex justify-content-end" role="group">
                    <button type="button" class="btn btn-outline-secondary btn-longer" data-bs-toggle="dropdown" aria-expanded="false"><i class="bi bi-chevron-down"></i>
                    </button>
                    <ul class="dropdown-menu">
                        <li><a class="dropdown-item" href="#">Detalle ficha</a></li>
                        <li><a class="dropdown-item" href="#">Agregar Ampliación</a></li>
                        <li><a class="dropdown-item" href="#">Agregar certificado</a></li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { defineProps, defineEmits, ref } from 'vue';
const props = defineProps({
    name: String,
    id:Number,
    date: Date,
    address:String,
    state:Number,
    timbrado:Boolean,
    cep: Boolean
})
const emits = defineEmits(['payment']);
// Estado del pago de timbrado y cep
const timbradoPaid = ref(false);
const cepPaid = ref(false);

// Función para manejar el pago
const handlePayment = (type) => {
    if (type === 'timbrado') {
        console.log('TIMBRADO PAGADO');
        timbradoPaid.value = true; // Marcar el pago de timbrado como completado
        emitPayment(type); // Emitir evento de pago al componente padre
    } else if (type === 'cep') {
        console.log('CEP PAGADO');
        cepPaid.value = true; // Marcar el pago de cep como completado
        emitPayment(type); // Emitir evento de pago al componente padre
    }
};
// Función para emitir el evento de pago al componente padre
const emitPayment  = (type) => {
    emits('payment', type);
};

</script>

<style>
.number{
    background-color:#5eaeda;
    color:white
}
.date{
    font-size: 12px;
}
.custom-link{
    font-size: 10px;
}
.address{
    font-size: 15px;
    color: #495e5e;
}
.btn-pay{
    background-color:#1880a5;
    color: white;
}
.dropdown-item:hover {
    background-color: #C4E8FF; /* Cambia el color de fondo al pasar el cursor */
}
.style-title{
    font-size: 10px;
    color:#0A58CA
}
.btn-longer {
    width: auto; 
    padding-left: 20px; 
    padding-right: 20px; 
}
</style>

