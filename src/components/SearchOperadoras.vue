<template>
    <div class="search-container">
      <div :class="['search-operadoras', { expanded: operadoras.length > 0 }]">
        <h1>Buscar Operadoras</h1>
  
        <input
          v-model="searchTerm"
          @input="searchOperadora"
          type="text"
          placeholder="Digite o nome da operadora"
          class="search-input"
        />
  
        <div v-if="loading" class="loading">Buscando...</div>
  
        <div v-if="errorMessage" class="error-message">{{ errorMessage }}</div>
  
        <div class="results-container">
          <ul>
            <li
              v-for="(operadora, index) in operadoras"
              :key="index"
              @click="selectOperadora(operadora)"
              class="result-item"
            >
              <strong>{{ operadora[2] }}</strong>
              <div v-if="selectedOperadora === operadora" class="operadora-details">
                <hr class="line-hr">
                <h3>Detalhes da Operadora</h3>
                <p><strong>Razão Social:</strong> {{ operadora[2] }}</p>
                <p><strong>Cidade/UF:</strong> {{ operadora[9] }} / {{ operadora[10] }}</p> 
                <p><strong>Nome do Representante:</strong> {{ operadora[16] }}</p>
                <p><strong>Cargo do Representante:</strong> {{ operadora[17] }}</p>
              </div>
            </li>
          </ul>
        </div>
        <div class="credits">
          Dados baseados no <a href="https://dadosabertos.ans.gov.br/FTP/PDA/operadoras_de_plano_de_saude_ativas/" target="_blank">Relatório CADOP</a>
        </div>
      </div>
      <i><p class="powered">Powered by Yveen Barbosa <br>
        Buscar Operadoras</p></i>
    </div>
  </template>
  
  <script>
  export default {
    // 1. Cria lista dos retornos.
    data() {
      return {
        searchTerm: "",
        operadoras: [],
        errorMessage: "",
        loading: false,
        selectedOperadora: null,
      };
    },
    methods: {
      // 2. Busca as operadoras partir do termo e as retorna.
      async searchOperadora() {
        if (this.searchTerm.trim() === "") {
          this.operadoras = [];
          this.errorMessage = "";
          return;
        }
  
        this.loading = true;
        this.errorMessage = "";
  
        try {
          const response = await fetch(`https://api-vue-flask-python.onrender.com/search?term=${this.searchTerm}`);
          const data = await response.json();
  
          if (response.ok) {
            this.operadoras = data.results;
          } else {
            this.operadoras = [];
            this.errorMessage = data.error || "Erro ao buscar operadoras.";
          }
        } catch (error) {
          this.operadoras = [];
          this.errorMessage = "Erro na comunicação com o servidor.";
        } finally {
          this.loading = false;
        }
      },
      // 3. Caso seja selecionado uma operadora, exibe seus detalhes.
      selectOperadora(operadora) {
        this.selectedOperadora = this.selectedOperadora === operadora ? null : operadora;
      },
    },
  };
  </script>
  
  <style scoped>
  html, body {
    margin: 0;
    padding: 0;
    height: 100%;
    width: 100%;
    overflow: hidden;
  }
  
  .search-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
    width: 100%;
    box-sizing: border-box;
    overflow: hidden;
  }
  
  .search-operadoras {
    max-width: 800px;
    width: 100%;
    padding: 20px;
    text-align: center;
    background-color: rgba(30, 30, 30, 0.9);
    border-radius: 30px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    position: relative;
    color: #fff;
    overflow: hidden;
    box-sizing: border-box;
  }
  
  .search-operadoras h1 {
    color: #fff;
  }
  
  .search-input {
    padding: 10px;
    padding-left: 20px;
    width: 80%;
    font-size: 18px;
    margin-bottom: 20px;
    border: 2px solid #444;
    border-radius: 20px;
    background-color: #333;
    color: #fff;
    outline: none;
  }
  
  .search-input:focus {
    border-color: #888;
    background-color: #555;
  }
  
  .loading {
    font-size: 18px;
    color: #bbb;
  }
  
  .error-message {
    color: #bbb;
    margin-top: 10px;
  }
  
  .results-container {
    margin-bottom: 30px;
    max-height: 300px;
    overflow-y: auto;
    padding-bottom: 20px;
    border-radius: 30px;
    box-sizing: border-box;
  }
  
  ul {
    list-style-type: none;
    padding: 0;
  }
  
  .result-item {
    padding: 10px 50px 10px 50px;
    margin: 5px 0;
    background-color: #444;
    border: 1px solid #666;
    border-radius: 50px;
    cursor: pointer;
    transition: background-color 0.3s;
    color: #fff;
  }
  
  .result-item:hover {
    background-color: #555;
  }
  
  .line-hr {
    border: none;
    height: 1px;
    background-color: #bbb;
    margin: 5px 0;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
    border-radius: 5px;
  }
  
  .operadora-details {
    margin-top: 10px;
    text-align: left;
    padding: 10px;
    border-radius: 45px;
    color: #fff;
  }
  
  .credits {
    position: absolute;
    bottom: 20px;
    right: 20px;
    font-size: 14px;
    color: #fff;
  }
  
  .credits a {
    color: #bbb;
    text-decoration: none;
  }
  
  .credits a:hover {
    text-decoration: underline;
  }
  
  .powered {
    display: flex;
    flex-direction: column;
    text-align: center;
    color:#c7c7c7
  }
  </style>
  