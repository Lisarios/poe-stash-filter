<template>
    <v-app>
        <div class="text-xs-center">
    <v-dialog
      v-model="dialog"
      width="500"
    >
      <v-btn
        slot="activator"
        color="success"
        dark
      >
        Ajouter un compte
      </v-btn>

      <v-card>
        <v-card-title
          class="headline grey lighten-2"
          primary-title
        >
          Ajout d'un compte
        </v-card-title>

        <v-card-text>
          La récupération de votre stash d'item sur Path of Exile nécessite une authentification sur le site https://www.pathofexile.com. Lors de votre saisie, aucune information n'est stockée ni détournée.
          <v-form ref="form" v-model="valid" lazy-validation>
              <v-container fluid>
                <v-text-field
                    v-model="email"
                    :rules="emailRules"
                    label="Email"
                    required
                    ></v-text-field>

                <v-text-field
                    v-model="password"
                    :append-icon="showPassword ? 'visibility_off' : 'visibility'"
                    :type="showPassword ? 'text' : 'password'"
                    label="Not visible"
                    hint="At least 8 characters"
                    value="wqfasds"
                    class="input-group--focused"
                    @click:append="showPassword = !showPassword"
                ></v-text-field>

                <v-btn
                    :disabled="!valid"
                    @click="submit"
                    >
                    submit
                    </v-btn>
                <v-btn @click="clear">clear</v-btn>

              </v-container>

            
            </v-form>
        </v-card-text>

      </v-card>
    </v-dialog>
  </div>
        
    </v-app>

</template>

<script>
import axios from 'axios'
import cheerio from 'cheerio'

export default {
    data: () => ({
      dialog: false,
      showPassword : false,
      valid: true,
      email: '',
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid'
      ],
      password: '',
      passwordRules: [
        v => !!v || 'Password is required',
      ],
      remember_me: 0,
      login : 'Login',
      captcha_id : '',
      captcha_input: '',
    }),

    methods: {
      submit () {
        if (this.$refs.form.validate()) {
            // Native form submission is not yet supported
            axios.get('https://www.pathofexile.com/login', {
                method: 'GET',
                mode: 'cors',
                headers: {
                'Access-Control-Allow-Origin': '*',
                'Content-Type': 'application/json',
                },
                //withCredentials: true,
                //credentials: 'same-origin',
            })
            .then(response => {
                var $ = cheerio.load(response.data);
                var hash = $('input[name="hash"]').val();
                
                console.log(hash);
                console.log(this.email);
                console.log(this.password);

                axios.post('https://www.pathofexile.com/character-window/get-items', {
                  character: 'bobby',
                  accountName: 'bobby'
                })
                .then(response => {
                  console.log(response.data);
                })
            })
            .then(response => {
                
            });

            
            
        }
      },
      clear () {
        this.$refs.form.reset()
      }
    }
}
</script>

<style>

</style>


