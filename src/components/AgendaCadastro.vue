<template>
    <div class="main-container">
        <!-- Barra de Perfil -->
        <BarraPerfil></BarraPerfil>

        <!-- Conteúdo da Agenda -->
        <div class="agenda-content">
            <div class="card flex justify-content-center" style="width: 100%; height: auto; display: flex; justify-content: center;align-items: center;">
                <div class="agenda-container">
                    <div class="header">
                        <h3>Cadastrar Agendamento</h3>
                    </div>
                    
                    <div class="form-container">
                        <div class="form-group">
                            <label for="data-agendamento" class="campo-label">Data do Agendamento</label>
                            <input type="date" v-model="agenda.dataHora" class="input-componente form-control" :class="{ 'input-vazio': !agenda.dataHora }" />
                        </div>

                        <div class="form-group">
                            <label for="tipo-cuidado" class="campo-label">Tipo de Cuidado</label>
                            <select v-model="agenda.tipoCuidado" class="input-componente form-control" :class="{ 'input-vazio': !agenda.tipoCuidado }">
                                <option value="" disabled>Selecione...</option>
                                <option value="Vacina">Vacina</option>
                                <option value="Consulta Veterinária">Consulta Veterinária</option>
                                <option value="Banho">Banho</option>
                                <option value="Tosa">Tosa</option>
                            </select>
                        </div>

                        <div class="form-group">
                            <label for="pet-id" class="campo-label">Nome do Pet</label>
                            <input type="text" v-model="agenda.pet" placeholder="Nome do Pet" class="input-componente form-control" :class="{ 'input-vazio': !agenda.pet }" />
                        </div>

                        <div class="form-group">
                            <label for="usuario-id" class="campo-label">Nome do Tutor</label>
                            <input type="text" v-model="agenda.tutor" placeholder="Nome do Tutor" class="input-componente form-control" :class="{ 'input-vazio': !agenda.tutor }" />
                        </div>

                        <div class="buttons-container">
                            <button @click="cancelar" class="cancelar">Cancelar</button>
                            <button @click="salvarAgenda" class="enviar">Salvar</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Rodapé -->
        <Rodape></Rodape>
    </div>
</template>

<script lang="ts">
import { cadastroAgenda } from '@/http';
import { defineComponent } from 'vue';
import BarraPerfil from './BarraPerfil.vue';
import Rodape from './Rodape.vue';

export default defineComponent({
    name: "AgendaCadastro",
    components:{
        BarraPerfil,
        Rodape
    },
    data() {
        return {
            agenda: {
                dataHora: '',
                tipoCuidado: '',
                pet: '',
                tutor: ''
            } 
        };
    },
    methods: {
        async salvarAgenda() {
            let mensagem = '';

            if (!this.camposPreenchidos()) {
                mensagem = 'Por favor, preencha todos os campos obrigatórios';
            } else {
                try {
                    // Envia os dados para o backend
                    await cadastroAgenda(this.agenda)

                    // Limpa os campos após salvar
                    this.limparCampos();
                    mensagem = 'Agendamento salvo com sucesso!';
                } catch (error) {
                    mensagem = 'Erro ao salvar o agendamento. Verifique os campos e tente novamente.';
                }
            }

            alert(mensagem);
        },
        cancelar() {
            this.limparCampos();
        },
        camposPreenchidos() {
            return this.agenda.dataHora && this.agenda.tipoCuidado && this.agenda.pet && this.agenda.tutor;
        },
        limparCampos() {
            this.agenda.dataHora = '';
            this.agenda.tipoCuidado = '';
            this.agenda.pet = '';
            this.agenda.tutor = '';
        }
    }
});
</script>

<style scoped>
.main-container {
    display: flex;
    flex-direction: column;
    height: 100vh;
}

.agenda-content {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 4%;
}

.agenda-container {
    width: 100%;
    max-width: 600px;
    background-color: white;
    padding: 2rem;
    box-shadow: 0px 6px 9px #5CB9EE;
    border-radius: 8px;
}

.header {
    text-align: center;
    margin-bottom: 20px;
}

.form-container {
    display: flex;
    flex-direction: column;
    gap: 15px;
}

.form-group {
    display: flex;
    flex-direction: column;
}

.campo-label {
    font-weight: bold;
    color: black;
    margin-bottom: 8px;
}

.input-componente {
    height: 2.5rem;
    padding: 0.5rem;
    border-radius: 4px;
    box-shadow: 0px 6px 9px #5CB9EE;
}

.input-vazio {
    border: 2px solid red;
}

.buttons-container {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
}

.enviar {
    background-color: #5CB9EE;
    color: white;
    border: none;
    padding: 0.8rem 1.5rem;
    border-radius: 8px;
    cursor: pointer;
}

.enviar:hover {
    background-color: #4fa9d6;
}

.cancelar {
    background-color: white;
    color: red;
    border: 2px solid red;
    padding: 0.8rem 1.5rem;
    border-radius: 8px;
    cursor: pointer;
}

.cancelar:hover {
    background-color: red;
    color: white;
}
</style>
