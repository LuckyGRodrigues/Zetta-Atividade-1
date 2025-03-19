<template>
  <v-app>
    <v-container>
      <h2 class="align-center mt-15">Total de Dias: {{ totalDias }}</h2>

      <v-row>
        <v-col 
          v-for="ano in anosSelecionados" 
          :key="ano" 
          cols="12"
          md="2"
          class="ml-11 mt-10"
        >
          <h3>{{ ano }}</h3>
          <v-table height="400">
            <thead>
              <tr>
                <th>Mês</th>
                <th>Dias</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="mes in mesesExibidos(ano)" :key="mes.nome">
                <td>{{ mes.nome }}</td>
                <td>{{ mes.dias }}</td>
              </tr>
            </tbody>
          </v-table>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
  export default {
    props: {
      anosSelecionados: {
        type: Array,
        required: true,
      },
      mesesSelecionados: {
        type: Array,
        required: true,
      }
    },

    computed: {
      totalDias() {
        return this.anosSelecionados.reduce((total, ano) => {
          return total + this.mesesExibidos(ano).reduce((soma, mes) => soma + mes.dias, 0);
        }, 0);
      },
    },

    methods: {
      diasDoMes(ano) {
        return [
          { nome: "Janeiro", dias: 31 },
          { nome: "Fevereiro", dias: this.anoBissexto(ano) ? 29 : 28 },
          { nome: "Março", dias: 31 },
          { nome: "Abril", dias: 30 },
          { nome: "Maio", dias: 31 },
          { nome: "Junho", dias: 30 },
          { nome: "Julho", dias: 31 },
          { nome: "Agosto", dias: 31 },
          { nome: "Setembro", dias: 30 },
          { nome: "Outubro", dias: 31 },
          { nome: "Novembro", dias: 30 },
          { nome: "Dezembro", dias: 31 },
        ];
      },

      anoBissexto(ano) {
        return (ano % 4 === 0 && ano % 100 !== 0) || ano % 400 === 0;
      },

      mesesExibidos(ano) {
        const todosOsMeses = this.diasDoMes(ano);
        if (this.mesesSelecionados.length === 0) {
          return todosOsMeses;
        }
        return todosOsMeses.filter(mes => this.mesesSelecionados.includes(mes.nome));
      }
    },
  };
</script>

<style>
  .align-center {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>
  