<template>
    <LayoutMain>
        <template #slotLayout>
            <Header :titulo="'Clientes'" :tituloBoton="'Crear Nuevo Cliente'" :abrir="abrirFormulario" />


            <Formulario :titulo="'Clientes'" v-model:is-open="mostrarFormulario" :is-edit="editandoFormulario"
                @save="guardarDatos" @update="actualizarDatos">
                <template #slotForm>
                    <el-row :gutter="20">
                        <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
                            <Form v-model:is-open="mostrarFormulario" :is-edit="editandoFormulario" ref="formRef"
                                :areas="clientes" :dataValue="dataClientesById" />
                        </el-col>
                    </el-row>
                </template>

            </Formulario>

            <el-table :data="clientes" stripe style="width: 100%">
                <el-table-column prop="nombre" label="nombre" />
                <el-table-column prop="apellido" label="apellido" />
                <el-table-column prop="documento" label="documento" />
                <el-table-column fixed="right" label="Acciones" min-width="120">
                    <template #default="registro">
                        <el-button link type="primary" size="large" :icon="Edit"
                            @click="editarFormulario(registro.row.id)">
                        </el-button>
                        <el-button link type="danger" :icon="Delete"
                            @click="eliminarCliente(registro.row.id)"></el-button>
                    </template>
                </el-table-column>
            </el-table>

        </template>


    </LayoutMain>
</template>


<script lang="ts" setup>
import { onMounted, reactive, ref, watch } from 'vue'
import LayoutMain from '../../components/LayoutMain.vue';
import Formulario from '../../components/Formulario.vue';
import Header from '../../components/Header.vue';
import { Delete, Edit } from "@element-plus/icons-vue"
import FormClientes from './form.Clientes.vue'
import { ElMessage, ElMessageBox } from 'element-plus'
import axios from 'axios';


const mostrarFormulario = ref(false)
const editandoFormulario = ref(false)
const formRef = ref()
const dataClientesById = ref()
const clientes = ref([])
const cargos = ref([])


const abrirFormulario = () => {
    mostrarFormulario.value = true
    editandoFormulario.value = false
}

const editarFormulario = async (id) => {
  
    mostrarFormulario.value = true
    editandoFormulario.value = true
}

const guardarDatos = async () => {
    const validacion = await formRef.value?.validarFormulario()
    if (validacion) {
        await crearCliente()
    }
}

const actualizarDatos = async () => {
    const validacion = await formRef.value?.validarFormulario()
    if (validacion) {
        await actualizarCliente()
    }
}

const crearCliente = async () => {

    const url =  'http://127.0.0.1:8000/api/cliente/datos'

    const dataFormulario = {
        nombre: formRef.value.formulario.nombre,
        apellido: formRef.value.formulario.apellido,
        documento: formRef.value.formulario.documento,
       
    }
    try {
        axios.post(url, dataFormulario)
            .then(function (response) {
                console.log(response);
                formRef.value?.limpiarFormulario()
                ElMessage({
                    message: 'El cliente se creo con exito    .',
                    type: 'success',
                })
                datosCliente()
                mostrarFormulario.value = false

            })
            .catch(function (error) {
                console.log(error);
            });

    } catch (error) {
        console.error('error crear cargo ', error)
    }
}

const actualizarCliente = async () => {
 
    const url = 'http://127.0.0.1:8000/api/cliente/update'

    const dataFormulario = {
        id:dataClientesById.value[0].id,
        nombre: formRef.value.formulario.nombre,
        apellido: formRef.value.formulario.apellido,
        documento: formRef.value.formulario.documento,
    }
    try {
        axios.put(url, dataFormulario)
            .then(function (response) {
                console.log(response);
                formRef.value?.limpiarFormulario()
                ElMessage({
                    message: 'El cliente se actualizo con exito    .',
                    type: 'success',
                })
                datosCliente()
                mostrarFormulario.value = false

            })
            .catch(function (error) {
                console.log(error);
            });

    } catch (error) {
        console.error('error crear cliente ', error)
    }

}


const eliminarCliente = async (id) => {

    const url = 'http://127.0.0.1:8000/api/cliente/delete'

    ElMessageBox.confirm(
        'Esta seguro de eliminar el cliente ',
        'Eliminar Registro',
        {
            confirmButtonText: 'SI',
            cancelButtonText: 'Cancelar',
            type: 'error',
        }
    )
        .then(() => {

            try {
                axios.delete(url, { data: { id } })
                    .then(function (response) {
                        datosCliente()

                    })
                    .catch(function (error) {
                        console.log(error);
                    });

            } catch (error) {
                console.error('error crear cliente ', error)
            }
            ElMessage({
                type: 'success',
                message: 'Se elimino correctamente el registro',
            })
        })
        .catch(() => {
            ElMessage({
                type: 'info',
                message: 'Eliminacion cancelada',
            })
        })

}
const datosCliente = async () => {

    const url = 'http://127.0.0.1:8000/api/cliente/update'

    try {
        axios.get(url)
            .then(function (response) {
                cargos.value = response.data.result
               // console.log(response);

            })
            .catch(function (error) {
                console.log(error);
            });

    } catch (error) {
        console.error('error crear cliente ', error)
    }


}
const getClientes = async () => {

    const url = 'http://127.0.0.1:8000/api/cliente/datos'

    try {
        axios.get(url)
            .then(function (response) {
                clientes.value = response.data.result
               // console.log(response);

            })
            .catch(function (error) {
                console.log(error);
            });

    } catch (error) {
        console.error('error crear cliente ', error)
    }




}

onMounted(() => {
    getClientes()
    datosCliente()
})

</script>