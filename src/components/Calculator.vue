<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-10">
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

                        <div class="mt-3">
                            <p><span>Valor de 13º salário:</span> </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style>

</style>
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
                gratifications: 0
            },
            totGains: 0,
            tot13: 0,
            totVacation: 0,
        }
    }, methods: {
        sumGains: function () {
            let tot = 0
            for (let key in this.gain) {
                tot += parseInt(this.gain[key]);
            }
            this.totGains = tot
            return tot
        },

        calc13: function () {
            let value13 = this.sumGains()/12
            this.tot13 = value13

            return value13
        },

        calcVacation: function () {
            let vacs = this.sumGains()/12
            vacs = vacs+vacs/3
            this.totVacation = vacs
            return vacs
        },

        doCalcs: function () {
            console.log(this.calc13())
            console.log(this.calcVacation())
            this.gain = Object.values(this.gain).flat()
        }
    }
}
</script>