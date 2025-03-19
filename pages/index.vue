<template>
  <v-app>
    <v-sheet class="mx-auto mt-15" width="400">
      <v-form @submit.prevent="updateAnosSelecionados">
        <v-text-field
          v-model="anoFinal"
          :rules="anoRules"
          label="Ano Final"
          type="number"
        />
        
        <v-select
          v-model="mesesSelecionados"
          :items="mesesDisponiveis"
          label="Selecione os meses"
          multiple
          chips
        />

        <v-btn class="mt-2" type="submit" block>Calcular</v-btn>
      </v-form>
    </v-sheet>

    <dias-mes v-if="mostrarResultado" :anos-selecionados="anosSelecionados" :meses-selecionados="mesesSelecionados"/>
  </v-app>
</template>

<script>
  import DiasMes from "../components/diasMes.vue";

  export default {
    components: {
      DiasMes,
    },
    data() {
      return {
        anoFinal: new Date().getFullYear(),
        anosSelecionados: this.generateAnosSelecionados(new Date().getFullYear()),
        mesesSelecionados: [],
        mesesDisponiveis: [
          "Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho",
          "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"
        ],
        anoRules: [
          (v) => !!v || "Required",
          (v) => v >= new Date().getFullYear() || "Ano deve ser maior ou igual ao ano atual"
        ],
        mostrarResultado: false,
      };
    },
    methods: {
      generateAnosSelecionados(anoFinal) {
        const currentYear = new Date().getFullYear();
        const anos = [];
        for (let ano = currentYear; ano <= anoFinal; ano++) {
          anos.push(ano);
        }
        return anos;
      },

      updateAnosSelecionados() {
        this.anosSelecionados = this.generateAnosSelecionados(this.anoFinal);
        this.mostrarResultado = true;
      },
    },
  };
</script>
