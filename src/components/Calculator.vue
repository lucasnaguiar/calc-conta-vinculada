<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-12">
                <div class="card">
                    <div class="card-header">Calculadora - Conta Vinculada (Beta)</div>
                    <div class="card-body">
                        <form  v-on:submit.prevent="onSubmit" >
                            <div class="row g-3">
                                <div class="col-md-3">
                                    <label for="base-salary" class="form-label">Salário-base</label>
                                    <input type="text" class="form-control" id="base-salary" v-model="gain.base_salary">
                                </div>
                                <div class="col-md-3">
                                    <label for="overtime" class="form-label">Horas extras</label>
                                    <input type="text" class="form-control" id="overtime" v-model="gain.overtime">
                                </div>
                                <div class="col-md-3">
                                    <label for="nocturnal-added" class="form-label">Adicional noturno</label>
                                    <input type="text" class="form-control" id="nocturnal-added" v-model="gain.nocturnal_added">
                                </div>
                                <div class="col-md-3">
                                    <label for="insalubrity" class="form-label">Adicional de insalubridade</label>
                                    <input type="text" class="form-control" id="insalubrity" v-model="gain.insalubrity">
                                </div>
                            </div>
                            <div class="row g-3 mt-2">
                                <div class="col-md-3">
                                    <label for="tips" class="form-label">Gorjetas</label>
                                    <input type="text" class="form-control" id="tips" v-model="gain.tips">
                                </div>
                                <div class="col-md-3">
                                    <label for="dangerousness-added" class="form-label">Adicional de periculosidade</label>
                                    <input type="text" class="form-control" id="dangerousness-added" v-model="gain.dangerousness_added">
                                </div>
                                <div class="col-md-3">
                                    <label for="commission" class="form-label">Comissão</label>
                                    <input type="text" class="form-control" id="commission" v-model="gain.commission">
                                </div>
                                <div class="col-md-3">
                                    <label for="gratifications" class="form-label">Gratificações</label>
                                    <input type="text" class="form-control" id="gratifications" v-model="gain.gratifications">
                                </div>
                            </div>

                            <div class="col-12 mt-3">
                                <a class="btn btn-primary" v-on:click="doCalcs">Calcular</a>
                            </div>
                        </form>

                        <div class="mt-3" v-show="calculed">
                            <table class="table">
                                <thead>
                                <tr>
                                    <th scope="col">Valor de 13º salário</th>
                                    <th scope="col">Valor de férias + 1/3</th>
                                    <th scope="col">Multa sobre FGTS e aviso prévio indenizado ou trabalhado</th>
                                    <th scope="col">Encargos previdenciários e FGTS, INSS, SESI ou SESC, SENAI ou SENAC, INCRA, Salário Educação, FGTS, SEBRAE</th>
                                </tr>
                                </thead>
                                <tbody>
                                <tr>
                                    <td>{{'R$ ' + tot13.toFixed(2)}}</td>
                                    <td>{{'R$ ' + totVacation.toFixed(2)}}</td>
                                    <td>{{'R$ ' + totMultaFgts.toFixed(2)}}</td>
                                    <td>{{'R$ ' + totEncargos.toFixed(2)}} <br>
                                        <div class="m-1">
                                            <div class="form-check form-check-inline">
                                                <input class="form-check-input" type="radio" id="inlineRadio1"  value="0.01" v-model="encargosPercent">
                                                <label class="form-check-label" for="inlineRadio1">1%</label>
                                            </div>
                                            <div class="form-check form-check-inline">
                                                <input class="form-check-input" type="radio" id="inlineRadio2"  value="0.02" v-model="encargosPercent">
                                                <label class="form-check-label" for="inlineRadio2">2%</label>
                                            </div>
                                            <div class="form-check form-check-inline">
                                                <input class="form-check-input" type="radio" id="inlineRadio3"  value="0.03" v-model="encargosPercent">
                                                <label class="form-check-label" for="inlineRadio3">3%</label>
                                            </div>
                                        </div>
                                    </td>
                                </tr>
                                </tbody>
                            </table>

                            <h3>Total: {{ 'R$ ' + (tot13 + totVacation + totMultaFgts +   totEncargos).toFixed(2) }}</h3>
                            <button type="button" class="btn btn-link" v-on:click="reloadApp()">Novo Calculo</button>

                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    data: function () {
        return {
            gain: {
                base_salary: 0,
                overtime: 0,
                nocturnal_added: 0,
                insalubrity: 0,
                tips:0,
                dangerousness_added: 0,
                commission: 0,
                gratifications: 0,
            },
            calculed: false,
            totGains: 0,
            tot13: 0,
            totVacation: 0,
            totMultaFgts: 0,
            totEncargos: 0,
            encargosPercent: 0.01, 
            errors: []

        }
    },
    watch: {
        encargosPercent(newPercent, oldPercent) {
            let percent = parseFloat(this.encargosPercent)
            this.totEncargos = this.totGains*percent
        }
    }
    ,
    methods: {
        sumGains: function () {
            this.totGains = 0
            let tot = 0
            for (let key in this.gain) {
                tot += parseInt(this.gain[key]);
            }
            this.totGains = tot
            return tot
        },

        calc13: function () {
            this.tot13 = 0
            let value13 = this.totGains/12
            this.tot13 = value13

            return value13
        },

        calcVacation: function () {
            this.totVacation = 0
            let vacs = this.totGains/12
            vacs = vacs+vacs/3
            this.totVacation = vacs
            return vacs
        },

        calcMultaFgts: function () {
            this.totMultaFgts = 0
            let calcMulta = this.totGains * 0.04
            this.totMultaFgts = calcMulta
            return calcMulta
        },
        calcEncargos: function () {
            this.totEncargos = this.totGains*this.encargosPercent
        },
        
        checkForm: function() {
            
        },

        reloadApp: function() {
            Location.reload()
        },

        doCalcs: function () {
            this.sumGains()
            this.calc13()
            this.calcVacation()
            this.calcMultaFgts()
            this.calcEncargos()
            this.calculed = true
            this.gain = Object.values(0).flat()
        }
    }
}
</script>