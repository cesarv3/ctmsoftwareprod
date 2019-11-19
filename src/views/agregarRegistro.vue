<template>
        <v-dialog v-model="dialog" persistent max-width="600px">
        <v-card>
            <v-card-title>
            <span class="headline">Nuevo Usuario</span>
            </v-card-title>
            <v-card-text>
            <v-container>
                    <v-text-field
                    v-model="nombreUsuario"
                    ref="nombreUsuario"
                    label="Nombre de Usuario *"
                    hint="ejemplo: IngenieroVilla"
                    persistent-hint
                    required
                    ></v-text-field>
                    <v-text-field
                    v-model="password"
                    ref="password"
                    type="password"
                    label="Contraseña*"
                    required
                    ></v-text-field>
                    <v-text-field
                    v-model="licencia"
                    ref="licencia"
                    type="password"
                    label="Licencia*"
                    required
                    ></v-text-field>
            </v-container>
            <small>* Campos Obligatorios</small>
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="orange" text @click="cancel()">Cerrar</v-btn>
                <v-btn color="orange" text @click="ok()">Guardar</v-btn>
            </v-card-actions>
        </v-card>
        </v-dialog>
</template>

<script>
export default {
    name: "AgregarRegistro",
    props:{
        mostrar: Boolean,
    },
    data() {
        return {
            nombreUsuario: '',
            password: '',
            licencia: ''
        };
    },
    computed: {
        dialog: function(){
            return this.mostrar;
        }
    },
    methods: {
        cancel() {
            this.nombreUsuario='',
            this.password='',
            this.licencia='',
            this.$emit("cancel")
        },
        ok () {
            this.crearRegistro(),
            this.nombreUsuario='',
            this.password='',
            this.licencia='',
            this.$emit("cancel")
        },
        crearRegistro () {
            if(this.licencia === "elvilloida"){
                fetch("https://ctmbackendprod.herokuapp.com/api/users", {
                    method: "POST",
                    body: JSON.stringify({
                        "nombreUsuario": this.nombreUsuario,
                        "password": this.password
                    }),
                    headers:{
                        "Content-Type": "application/json"
                    }
                })
                .then(response => {
                    return response.json();
                })
                .catch(error => {
                    console.log(error);
                });
                alert("Registro creado con éxito");
        }else{
            alert("Licencia incorrecta")
        }
        }
    }
};
</script>
<style scoped></style
