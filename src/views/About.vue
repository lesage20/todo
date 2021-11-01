<template>
  <v-row align="center"
      justify="center">
      <v-col md='6' class="mx-auto text-center">
            <v-card color="grey lighten-4" flat>
                <p class="text-center">
                    A PROPOS 
                </p>
                <v-card-content>
                  <p>
                    <strong>Sage Todo</strong> est un gestionnaire de tâche developpé par <span class="primary--text">{{auteur}}</span>  pour expliquer l'importance des framework a son partenaire  Franck.
                  </p>
                  <p>
                    Auteur: {{auteur}} <br>
                    version: {{version}}
                  </p>

                </v-card-content>
            </v-card>
        </v-col>
  </v-row>

</template>

<script>
export default {
  data (){
    return {
      auteur: "Le Sage Code",
      version: "1.0.0-bêta"
    }
  }
}
</script>