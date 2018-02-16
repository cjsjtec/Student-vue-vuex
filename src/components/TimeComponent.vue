<template>
    <div class="container">
        <div class="row" v-if="visible == 'placar' ">
            <form class="form-inline">
                <div class="form-group">
                <input type="text" class="form-control" v-model="novoJogo.casa.gols" @keyup.enter="fimDeJogo">
                <label class="control-label">
                    {{novoJogo.casa.time.nome}}
                    <img :src="novoJogo.casa.time.escudo" style="height: 30px; width: 30px;">
                </label>
                </div>
                <span> X </span>
                <div class="form-group">
                <label class="control-label">
                    <img :src="novoJogo.fora.time.escudo" style="height: 30px; width: 30px;"> {{novoJogo.fora.time.nome}}
                </label>
                <input type="text" class="form-control" v-model="novoJogo.fora.gols">
                </div>
                <button type="button" class="btn btn-primary" @click="fimDeJogo">Fim de jogo</button>
            </form>
        </div>
        <div class="row" v-if="visible == 'tabela' ">
            <div class="col-md-12">
                <button type="button" class="btn btn-primary" @click="createNovoJogo">Novo placar</button>
                <input type="text" class="form-control" v-model="filter">
                <table class="table table-striped">
                <thead>
                    <tr>
                    
                        <th v-for="(coluna, index) in colunas" :key="index">
                            <a href="#" @click="sortBy(coluna)">
                                {{coluna | ucwords}}
                            </a>
                            
                        </th>
                    </tr>
                </thead>
                <tbody>
                    
                    <tr v-for="(time, index) in timesFiltered" :key="index">
                    <td>
                        <img :src="time.escudo" alt="" style="width:30px; height:30px"> 
                        <strong>{{time.nome}}</strong>
                    </td>
                    <td>{{time.pontos}}</td>
                    <td>{{time.gm}}</td>
                    <td>{{time.gs}}</td>
                    <td>{{time | saldo}}</td>
                    </tr>
                </tbody>
                </table>
            </div>
        </div>
    </div>
</template>
<script>

import { Time } from "../js/time"
import _ from 'lodash'

export default {
    name: "TimeComponent",
    data() {
        return {
            filter: "",
            order: {
            keys: ["pontos", "gm", "gs"],
            sort: ["desc", "desc", "asc"]
            },
            sort: ["desc", "desc", "asc"],
            colunas: ["nome", "pontos", "gm", "gs", "saldo"],
            times: [
            new Time("Palmeiras", require("../assets/palmeiras_60x60.png")),
            new Time("Flamengo", require("../assets/flamengo_60x60.png")),
            new Time("Atlético-MG", require("../assets/atletico_mg_60x60.png")),
            new Time("Santos", require("../assets/santos_60x60.png")),
            new Time("Botafogo", require("../assets/botafogo_60x60.png")),
            new Time("Atlético-PR", require("../assets/atletico-pr_60x60.png")),
            new Time("Corinthians", require("../assets/corinthians_60x60.png")),
            new Time("Grêmio", require("../assets/gremio_60x60.png")),
            new Time("Fluminense", require("../assets/fluminense_60x60.png")),
            new Time("Ponte Preta", require("../assets/ponte_preta_60x60.png")),
            new Time("Chapecoense", require("../assets/chapecoense_60x60.png")),
            new Time("São Paulo", require("../assets/sao_paulo_60x60.png")),
            new Time("Cruzeiro", require("../assets/cruzeiro_60x60.png")),
            new Time("Sport", require("../assets/sport_60x60.png")),
            new Time("Coritiba", require("../assets/coritiba_60x60.png")),
            new Time("Internacional", require("../assets/internacional_60x60.png")),
            new Time("Vitória", require("../assets/vitoria_60x60.png")),
            new Time("Figueirense", require("../assets/figueirense_60x60.png")),
            new Time("Santa Cruz", require("../assets/santa_cruz_60x60.png")),
            new Time("América-MG", require("../assets/america_mg_60x60.png"))
            ],
            novoJogo: {
            casa: {
                time: null,
                gols: 0
            },
            fora: {
                time: null,
                gols: 0
            }
            },
            visible: "tabela"
        }
    },
    methods: {
        fimDeJogo() {
        let timeAdversario = this.novoJogo.fora.time;
        let gols = +this.novoJogo.casa.gols;
        let golsAdversario = +this.novoJogo.fora.gols;

        this.novoJogo.casa.time.fimJogo(timeAdversario, gols, golsAdversario);
        this.visible = "tabela";
        },
        sortBy(coluna) {
        this.order.keys = coluna;
        this.order.sort = this.order.sort == "desc" ? "asc" : "desc";
        },
        createNovoJogo() {
        let indexCasa = Math.floor(Math.random() * 20);
        let indexFora = Math.floor(Math.random() * 20);

        this.novoJogo.casa.time = this.times[indexCasa];
        this.novoJogo.fora.time = this.times[indexFora];
        this.showView("placar");
        },
        showView(view) {
        this.visible = view;
        }
    },
    computed: {
        timesFiltered() {
        let collection = _.orderBy(this.times, this.order.keys, this.order.sort);

        return _.filter(collection, item => item.nome.indexOf(this.filter) >= 0);
        }
    },
    filters: {
        saldo(time) {
        return time.gm - time.gs;
        },
        ucwords(value) {
        return value.charAt(0).toUpperCase() + value.slice(1);
        }
}
};
</script>
