<template>
  <v-container id="rol-view" fluid tag="section">
    <v-row>
      <v-col cols="8">
        <v-card elevation="2">
          <v-card-title> Administracion de roles del sistema </v-card-title>
          <v-card-subtitle> Administracion y Permisos </v-card-subtitle>
          <v-spacer></v-spacer>
          <v-card-actions>
            <v-row>
              <v-col cols="12" md="6">
                <validation-observer v-slot="{ handleSubmit }">
                  <form @submit.prevent="handleSubmit(validarFormRoles)">
                    <v-row align="center" dense>
                      <v-col cols="12">
                        <validation-provider
                          v-slot="{ errors }"
                          name="nombre de rol"
                          rules="required|min:3|max:200"
                        >
                          <v-text-field
                            v-model="rol.nombre"
                            :error-messages="errors"
                            label="nombre del rol"
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
            <div class="pa-3 text-center">
              <v-btn class="ma-2" outlined color="indigo" type="submit">
                <v-icon dark right>mdi-checkbox-marked-circle</v-icon>
                _Agregar
              </v-btn>
            </div>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import { required, max, min } from "vee-validate/dist/rules";
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
export default {
  name: "RolesView",
  $_veeValidate: {
    validatior: "new",
  },
  data: () => ({
    nombre: "",
    rol: {
      nombre: "",
    },
  }),
  methods: {
    validarFormRoles() {
      alert("se validaron los campos");
    },
  },
};
</script>
