<template>
  <v-container id="rol-view" fluid tag="section">
    <v-row>
      <v-col cols="6">
        <v-card elevation="2">
          <v-card-title><h1>Formulario</h1> </v-card-title>
          <v-card-subtitle> Ingresa tus datos </v-card-subtitle>
          <v-spacer></v-spacer>
          <v-card-actions>
            <v-row>
              <v-col cols="6" lg="12">
                <validation-observer v-slot="{ handleSubmit }">
                  <form @submit.prevent="handleSubmit(validarFormRoles)">
                    <v-row align="center" dense>
                      <v-col class="col-lg">
                        <validation-provider
                          v-slot="{ errors }"
                          name="Nombre"
                          rules="required|min:1|max:50"
                        >
                          <v-text-field
                            v-model="Nombre"
                            :error-messages="errors"
                            label="Nombre"
                            color="secondary"
                          >
                          </v-text-field>
                        </validation-provider>
                      </v-col>
                    </v-row>

                    <v-row>
                      <v-col>
                        <validation-provider
                          v-slot="{ errors }"
                          name="Apellidos"
                          rules="required|min:1|max:100"
                        >
                          <v-text-field
                            v-model="Apellidos"
                            :error-messages="errors"
                            label="Apellidos"
                            color="secondary"
                          >
                          </v-text-field>
                        </validation-provider>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col>
                        <validation-provider
                          v-slot="{ errors }"
                          name="Edad"
                          rules="required|min:1|max:100|numeric"
                        >
                          <v-text-field
                            v-model="Edad"
                            :error-messages="errors"
                            label="Edad"
                            color="secondary"
                          >
                          </v-text-field>
                        </validation-provider>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col>
                        <validation-provider
                          v-slot="{ errors }"
                          name="Estado"
                          rules="required|min:3|max:100"
                        >
                          <v-text-field
                            v-model="Estado"
                            :error-messages="errors"
                            label="Estado"
                            color="secondary"
                          >
                          </v-text-field>
                        </validation-provider>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col>
                        <validation-provider
                          v-slot="{ errors }"
                          name="Email"
                          rules="required|email"
                        >
                          <v-text-field
                            v-model="Email"
                            :error-messages="errors"
                            label="E-mail"
                            color="secondary"
                          >
                          </v-text-field>
                        </validation-provider>
                      </v-col>
                    </v-row>
                  </form>
                </validation-observer>
              </v-col>
            </v-row>
          </v-card-actions>
        </v-card>
        <div class="pa-3 text-center">
          <v-btn
            large
            class="ma-2"
            outlined
            color="indigo"
            type="submit"
            @click="Guardar()"
          >
            <v-icon dark right>mdi-checkbox-marked-circle</v-icon>
            _Agregar
          </v-btn>
        </div>
      </v-col>
    </v-row>

    <v-card>
      <table style="width: 100%">
        <thead id="thead">
          <tr>
            <th>Nombre</th>
            <th>Apellidos</th>
            <th>Edad</th>
            <th>Estado</th>
            <th>E-mail</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <th>{{ Nombre }}</th>
            <th>{{ Apellidos }}</th>
            <th>{{ Edad }}</th>
            <th>{{ Estado }}</th>
            <th>{{ Email }}</th>
          </tr>
          <tr v-for="persona in arrayPersonas">
            <td v-text="persona.Nombre"></td>
            <td v-text="persona.Apellidos"></td>
            <td v-text="persona.Edad"></td>
            <td v-text="persona.Estado"></td>
            <td v-text="persona.Email"></td>
          </tr>
        </tbody>
      </table>
    </v-card>
  </v-container>
</template>
<script>
import axios from "axios";
import { required, max, min, numeric, email } from "vee-validate/dist/rules";
import { extend, setInteractionMode } from "vee-validate";

setInteractionMode("eager");
extend("max", {
  ...max,
  message: "El campo {_field_} no debe tener mas de {length} caracteres",
});
extend("min", {
  ...min,
  message: "El campo {_field_} debe tener al menos {length} caracteres",
});
extend("required", {
  ...required,
  message: "El campo {_field_} no debe estar vacio",
});
extend("numeric", {
  ...numeric,
  message: "Introduce solo números",
});
extend("email", {
  ...email,
  message: "Introduce un Correo electronico valido",
});
export default {
  name: "RolesView",
  $_veeValidate: {
    validatior: "new",
  },
  data() {
    return {
      arrayPersonas: [],
      Nombre: "",
      Apellidos: "",
      Edad: "",
      Estado: "",
      Email: "",
    };
  },
  methods: {
    validarFormRoles() {
      alert("se validaron los campos");
    },
    Guardar() {
      axios
        .post(
          "https://datos-80f27-default-rtdb.firebaseio.com/registros.json",
          {
            Nombre: this.Nombre,
            Apellidos: this.Apellidos,
            Edad: this.Edad,
            Estado: this.Estado,
            Email: this.Email,
          }
        )
        .then((response) => {
          console.log(response);
        })
        .catch((err) => console.error(err));

      let persona = {
        Nombre: this.Nombre,
        Apellidos: this.Apellidos,
        Edad: this.Edad,
        Estado: this.Estado,
        Email: this.Email,
      };
      this.arrayPersonas.push(persona);

      this.Nombre = "";
      this.Apellidos = "";
      this.Edad = "";
      this.Estado = "";
      this.Email = "";
    },
  },
};
</script>

<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#thead {
  background-color: rgb(183, 205, 220);
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>
