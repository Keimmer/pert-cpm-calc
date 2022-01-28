<template>
<div class="home">
    <div class="error-container">
        <div class="error">{{message}}</div>
    </div>
    <div class="small-container">
        <h1>Calculadora PERT/CPM</h1>
        <p>Elija el metodo:</p>
        <select v-model="method" id="">
            <option value="1">PERT</option>
            <option value="2">CPM</option>
        </select>
    </div>
    <div class="table-container" v-if="method == 2">
        <div class="row">
            <div class="col thead">Actividades</div>
            <div class="col thead">Predecesores</div>
            <div class="col thead">Duracion</div>
            <div class="col thead">Quitar Actividad</div>
        </div>
        <div class="row" v-for="(input, index) in  inputs" :key="index">
            <div class="col tfield"><input type="text" class="form-field" v-model="input.actName" :placeholder="'nombre de Act. ' + (index + 1)"></div>
            <div class="col tfield"><input type="text" class="form-field" v-model="input.actPredecesor" :placeholder="'Predecesor(es) de Act. ' + (index + 1)"></div>
            <div class="col tfield"><input type="text" class="form-field" v-model="input.actDuration" :placeholder="'Duracion de Act. ' + (index + 1)"></div>
            <div class="col tfield"><button class="btn" @click="deleteActivity(index)">-</button></div>
        </div>
        <button class="btn" @click="addInputs">Agregar Actividad</button>
        <button class="btn" @click="calcResult">Calcular Resultado</button>
    </div>

    <div class="table-container" v-if="method == 1">
        <div class="row">
            <div class="col thead">Actividades</div>
            <div class="col thead">Predecesores</div>
            <div class="col thead">T1</div>
            <div class="col thead">T2</div>
            <div class="col thead">T3</div>
            <div class="col thead">Quitar Actividad</div>
        </div>
        <div class="row" v-for="(input, index) in  inputs" :key="index">
            <div class="col tfield"><input type="text" class="form-field" v-model="input.actName" :placeholder="'nombre de Act. ' + (index + 1)"></div>
            <div class="col tfield"><input type="text" class="form-field" v-model="input.actPredecesor" :placeholder="'Predecesor(es) de Act. ' + (index + 1)"></div>
            <div class="col tfield"><input type="text" class="form-field" v-model="input.t1" :placeholder="'Valor de t1. de Actividad' + (index + 1)"></div>
            <div class="col tfield"><input type="text" class="form-field" v-model="input.t2" :placeholder="'Valor de t2. de Actividad' + (index + 1)"></div>
            <div class="col tfield"><input type="text" class="form-field" v-model="input.t3" :placeholder="'Valor de t3. de Actividad' + (index + 1)"></div>
            <div class="col tfield"><button class="btn" @click="deleteActivity(index)">-</button></div>
        </div>
        <button class="btn" @click="addInputs">Agregar Actividad</button>
        <button class="btn" @click="calcResult">Calcular Resultado</button>
    </div>
    <div class="table-container" v-if="method == 2">
        <div class="row">
            <div class="col thead">Actividades</div>
            <div class="col thead">Duracion</div>
            <div class="col thead">Predecesores</div>
            <div class="col thead">Tiempo Early</div>
            <div class="col thead">Tiempo Last</div>
            <div class="col thead">Holgura</div>
        </div>
        <div class="row" v-for="(input, index) in inputs" :key="index">
            <div class="col tfield">{{input.actName}}</div>
            <div class="col tfield">{{input.actDuration}}</div>
            <div class="col tfield">{{input.actPredecesor}}</div>
            <div class="col tfield">{{input.tEarly}}</div>
            <div class="col tfield">{{input.tLast}}</div>
            <div class="col tfield">{{input.holg}}</div>
        </div>
        <div class="row">
            <div class="col thead">
                Ruta Critica: {{criticalPath}}
            </div>
        </div>
    </div>
    <div class="table-container" v-if="method == 1">
        <div class="row">
            <div class="col thead">Actividades</div>
            <div class="col thead">Duracion</div>
            <div class="col thead">Predecesores</div>
            <div class="col thead">Desviacion Estandar</div>
            <div class="col thead">Varianza</div>
            <div class="col thead">Tiempo Early</div>
            <div class="col thead">Tiempo Last</div>
            <div class="col thead">Holgura</div>
        </div>
        <div class="row" v-for="(input, index) in inputs" :key="index">
            <div class="col tfield">{{input.actName}}</div>
            <div class="col tfield">{{input.actDuration}}</div>
            <div class="col tfield">{{input.actPredecesor}}</div>
            <div class="col tfield">{{input.desviacion}}</div>
            <div class="col tfield">{{input.varianza}}</div>
            <div class="col tfield">{{input.tEarly}}</div>
            <div class="col tfield">{{input.tLast}}</div>
            <div class="col tfield">{{input.holg}}</div>
        </div>
        <div class="row">
            <div class="col thead">
                Ruta Critica: {{criticalPath}}
            </div>
        </div>
    </div>

</div>
</template>

<script>
export default {
    data() {
        return {
            inputs: [],
            /* inputs: [{
                    actName: 'a',
                    actDuration: 16.1667,
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'b',
                    actDuration: 52.1667,
                    actPredecesor: 'a',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'c',
                    actDuration: 17.6667,
                    actPredecesor: '',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'd',
                    actDuration: 39.8333,
                    actPredecesor: 'b',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'e',
                    actDuration: 45.5,
                    actPredecesor: 'b',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'f',
                    actDuration: 27,
                    actPredecesor: 'b',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'g',
                    actDuration: 72.5,
                    actPredecesor: 'c',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'h',
                    actDuration: 34.8333,
                    actPredecesor: 'c',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'i',
                    actDuration: 44.5,
                    actPredecesor: 'c',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'j',
                    actDuration: 59.8333,
                    actPredecesor: 'd',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'k',
                    actDuration: 59.8333,
                    actPredecesor: 'e',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'l',
                    actDuration: 22.6667,
                    actPredecesor: 'f',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'm',
                    actDuration: 78,
                    actPredecesor: 'g',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'n',
                    actDuration: 19.1667,
                    actPredecesor: 'h',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'o',
                    actDuration: 17.1667,
                    actPredecesor: 'i',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'p',
                    actDuration: 21.1667,
                    actPredecesor: 'l,m',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'q',
                    actDuration: 11,
                    actPredecesor: 'k,p,n',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 'r',
                    actDuration: 38.8333,
                    actPredecesor: 'j,q',
                    tEarly: 0,
                    tLast: 0
                },
                {
                    actName: 's',
                    actDuration: 63.8333,
                    actPredecesor: 'j,q',
                    tEarly: 0,
                    tLast: 0
                },
            ], */
            count: '',
            message: '',
            usedNames: [],
            method: '',
            criticalPath: '',
        }
    },
    methods: {
        // metoo para agregar mas campos
        addInputs() {
            this.count++;
            this.inputs.push({
                key: this.count,
                actPredecesor: '',
                tEarly: 0,
                tLast: 0,
            });
        },
        addInputsPERT() {
            this.count++;
            this.inputs.push({
                key: this.count,
                actPredecesor: '',
                tEarly: 0,
                tLast: 0,
            });
        },
        // con este metodo se procede a calcular los resultados
        calcResult() {
            this.checkErrors() //antes que todo se buscan errores de entrada para evitar errores matematicos
            if (this.method == 1) {
                this.calcActDuration() // si el metodo de entrada es PERT calculamos la duracion de las actividades con los 3 tiempos
            }
            this.makeFinalAct() // creando la actividad Final
            this.calculateTEarly() //se calculan y se asignan todos los tearly
            this.calculateTLast() //ya con todos los tearly se procede a calcular los tlast
            this.calculateHolg() // calculamos la holgura de cada actividad y la ruta critica
        },
        calcActDuration() {
            this.inputs.forEach(input => {
                //ordenamos los tiempos de menor a mayor
                let times = []
                times.push(input.t1, input.t2, input.t3);
                let temp = 0;
                for (let i = 0; i < times.length; i++) {
                    for (let j = 0; j < times.length; j++) {
                        if (times[i] < times[j]) {
                            temp = times[j];
                            times[j] = times[i];
                            times[i] = temp;
                        }
                    }
                }
                input.actDuration = parseFloat((parseFloat(times[0]) + (4*parseFloat(times[1])) + parseFloat(times[2]))/6).toFixed(4);
                input.desviacion = parseFloat((parseFloat(times[2]) - parseFloat(times[0]))/6).toFixed(4);
                input.varianza = parseFloat(input.desviacion * input.desviacion).toFixed(4);
            });
        },
        calculateHolg() {
            this.inputs.forEach(input => {
                if (input.tEarly == 0) {
                    input.holg = parseFloat(input.tLast - input.actDuration).toFixed(4);
                } else {
                    input.holg = parseFloat(input.tLast - input.tEarly).toFixed(4);
                }
                if (input.holg == 0) {
                    this.criticalPath += input.actName + ', ';
                }
            });
        },
        calculateTEarly() {
            this.inputs.forEach(input => {
                if (input.actPredecesor) {
                    if (input.actPredecesor.length > 1) {
                        let predecesors = input.actPredecesor.split(",");
                        let precededAct = [];
                        let precededActTearly = [];
                        let lastActTearly = [];
                        if (input.lastAct) {
                            predecesors.forEach(i => {
                                for (let j = 0; j < this.inputs.length; j++) {
                                    if (this.inputs[j].actName == i) {
                                        lastActTearly.push(this.inputs[j].tEarly); //agregamos las duraciones de los tEarly de las actividades predecesoras a un arreglo para luego encontrar la que tenga mayor duracion
                                    }
                                }
                            });
                        }
                        predecesors.forEach(i => {
                            for (let j = 0; j < this.inputs.length; j++) {
                                if (this.inputs[j].actName == i && this.inputs[j].tEarly > 0) {
                                    precededActTearly.push(this.inputs[j].tEarly); //agregamos las duraciones de los tEarly de las actividades predecesoras a un arreglo para luego encontrar la que tenga mayor duracion
                                } else {
                                    precededAct.push(this.inputs[j].actDuration); //agregamos las duraciones de las actividades predecesoras a un arreglo para luego encontrar la que tenga mayor duracion
                                }
                            }
                        });
                        if (!input.lastAct) {
                            if (precededActTearly.length > 0) {
                                input.tEarly = parseFloat(input.actDuration) + parseFloat(this.findMaxValue(precededActTearly)) //en caso de que haya una actividad con tearly mayor que 0 se utilizara para la suma
                                input.tEarly = parseFloat(input.tEarly.toFixed(4));
                            } else {
                                input.tEarly = parseFloat(input.actDuration) + parseFloat(this.findMaxValue(precededAct)); //con esta linea obtenemos el tEarly de la actividad si tiene predecesores
                                input.tEarly = parseFloat(input.tEarly.toFixed(4));
                            }
                        } else {
                            if (lastActTearly.length > 0) {
                                console.log(lastActTearly);
                                input.tEarly = parseFloat(this.findMaxValue(lastActTearly)) //en caso de que haya una actividad con tearly mayor que 0 se utilizara para la suma
                                input.tEarly = parseFloat(input.tEarly.toFixed(4));
                            }
                        }
                    } else {
                        let precededAct = this.inputs.filter(res => res.actName == input.actPredecesor).map(res => res.actDuration); //al haber una sola actividad predecesora solo se suma con el tearly
                        let precededActTearly = this.inputs.filter(res => res.actName == input.actPredecesor).map(res => res.tEarly); //tambien seleccionamos el tearly de la actividad predecesora para sumarlo
                        if (parseFloat(precededActTearly) > 0) {
                            input.tEarly = parseFloat(input.actDuration) + parseFloat(precededActTearly);
                            input.tEarly = parseFloat(input.tEarly.toFixed(4));
                        } else {
                            input.tEarly = parseFloat(precededAct) + parseFloat(input.actDuration);
                            input.tEarly = parseFloat(input.tEarly.toFixed(4));
                        }
                    }
                }

            });
        },
        makeFinalAct() {
            //creando la actividad final antes de colocar los tlast
            if (!this.inputs[this.inputs.length - 1].lastAct) {
                this.inputs.push({
                    key: this.inputs.length + 1,
                    actName: 'Final',
                    actPredecesor: '',
                    lastAct: true,
                    tEarly: 0,
                    tLast: 0,
                })

                let allPredecesors = '';
                this.inputs.forEach(input => {
                    if (!input.lastAct) {
                        if (input.actPredecesor) {
                            allPredecesors += input.actPredecesor;
                        }
                    }
                });
                for (let i = 0; i < this.inputs.length; i++) {
                    if (!this.inputs[i].lastAct) {
                        if (!allPredecesors.includes(this.inputs[i].actName)) {
                            this.inputs[this.inputs.length - 1].actPredecesor += this.inputs[i].actName + ',';
                        }
                    }
                }
            }
        },
        calculateTLast() {
            //colocando todas las actividades predecesoras en un string
            let allPredecesors = '';
            this.inputs[this.inputs.length - 1].tLast = this.inputs[this.inputs.length - 1].tEarly; // asignamos el tlast de la ultima actividad
            this.inputs.forEach(input => {
                if (input.actPredecesor) {
                    allPredecesors += input.actPredecesor;
                }
            });
            //calculando los tlast de las ultimas actividades            
            for (let i = 0; i < this.inputs.length; i++) {
                if (!allPredecesors.includes(this.inputs[i].actName)) {
                    console.log(this.inputs[i].actName);
                    this.inputs[i].tLast = this.inputs[i].tEarly;
                }
            }
            //calculando los tlast
            for (let i = this.inputs.length - 1; i >= 0; i--) {
                let nextTlast = 0;
                if (this.inputs[i].lastAct) {
                    nextTlast = this.inputs[i].tEarly;
                } else {
                    nextTlast = this.inputs[i].tLast - this.inputs[i].actDuration;
                }
                if (nextTlast >= 0) {
                    //chequeamos si esta actividad tiene predecesores y si tiene mas de un predecesor
                    if (this.inputs[i].actPredecesor && this.inputs[i].actPredecesor.length > 1) {
                        let predecesors = this.inputs[i].actPredecesor.split(",");
                        predecesors.forEach(x => {
                            for (let y = 0; y < this.inputs.length; y++) {
                                if (this.inputs[y].actName == x) {
                                    if ((this.inputs[y].tLast) > nextTlast || (this.inputs[y].tLast == 0)) {
                                        this.inputs[y].tLast = parseFloat(nextTlast.toFixed(4));
                                    }
                                }
                            }
                        });
                    } else if (this.inputs[i].actPredecesor != null) {
                        for (let y = 0; y < this.inputs.length; y++) {
                            if (this.inputs[y].actName == this.inputs[i].actPredecesor) {
                                if ((this.inputs[y].tLast) > nextTlast || (this.inputs[y].tLast == 0)) {
                                    this.inputs[y].tLast = parseFloat(nextTlast.toFixed(4));
                                }
                            }
                        }
                    }
                }

            }

        },
        findMaxValue(arr) {
            let max = 0;
            for (let i = 0; i < arr.length; i++) {
                for (let j = 0; j < arr.length; j++) {
                    if (arr[j] > max) {
                        max = arr[j];
                    }
                }
            }
            return max;
        },
        //metodo que busca errores en las entradas de datos
        checkErrors() {
            this.message = '';
            this.usedNames = [];
            this.inputs.forEach(input => {
                this.usedNames.push(
                    input.key,
                    input.actName
                );
                if (!input.actName) {
                    this.message += '\n falta el nombre de la actividad ' + (input.key);
                }
                if (!input.actDuration) {
                    this.message += '\n falta la duracion de la actividad ' + (input.key);
                }
                let repCount = 0;
                this.usedNames.forEach(names => {
                    if (input.actName == names) {
                        repCount++;
                        if (repCount > 1) {
                            return this.message += '\n Nombre de actividad duplicado en la actividad ' + (input.key);
                        }
                    } else {

                    }
                });
            });
        },
        deleteActivity(index) {
            this.inputs.splice(index, 1);
        }
    },
}
</script>
