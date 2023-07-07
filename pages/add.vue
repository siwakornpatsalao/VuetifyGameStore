<template>
      <div style="margin-top: 100px ">
      <v-row style="margin-left:300px">     
      <v-img src="https://images.pexels.com/photos/1346154/pexels-photo-1346154.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500" height="700" style="max-width: 440px;"></v-img>
        <v-stepper  style="max-width: 1000px" width="50%" v-model="e4">
          <v-stepper-header>
            <v-stepper-step :complete="e4 > 1" step="1">
              Basic Information
              <small>Summarize if needed</small>
            </v-stepper-step>

            <v-divider></v-divider>

              <v-stepper-step :complete="e4 > 2" step="2"
                >Specification</v-stepper-step
              >

              <v-divider></v-divider>

              <v-stepper-step :complete="e4 > 3" step="3"
                >Add Game Description</v-stepper-step
              >
          </v-stepper-header>

          <!-- :rules="[() => !!title || 'Game Title Required']"
          :rules="[() => !!thumbnail || 'Game Image Required']" -->
    <v-stepper-content step="1">
      <v-card color="#121212" class="mb-5" height="520px">
        <v-card-text>
          <v-form ref="form" v-model="isFormValid">
            <v-text-field v-model="title"
                          outlined
                          label="Game Title"
                          >
            </v-text-field>
            <v-select v-model="genre"
                outlined
                :items="genres"
                label="Select Genre"
                :rules="[() => !!genre || 'Genre Required']">
            </v-select> 
            <v-icon v-if=!thumbnail width="250.25" height="270.66"></v-icon>
            <v-img v-else cover :src="url" width="250.25" height="270.66"></v-img>
            <v-file-input @change="pre_thumbnail"
                          style="max-width: 200px;"
                          v-model="thumbnail" 
                          label="Input thumbnail"
                          accept="image/*"
                          ></v-file-input>
            </v-form>
            
        </v-card-text>
      </v-card>
      <v-btn color="rgb(28, 123, 139)" :disabled="!isFormValid" @click="e4=2">Continue</v-btn> <!-- :disabled="!check1" -->

      <v-btn @click="e4=1">Cancel</v-btn>
    </v-stepper-content>

    <v-stepper-content step="2">
        <v-card color="#121212" class="mb-5" height="500px">
          <v-card-text>
            <v-form ref="form2" v-model="isFormValid2">
            <v-select v-model="platform"
                :items="platforms"
                label="Select Platform"
                outlined
                :rules="[() => !!platform || 'Game Platform Required']">
            </v-select> 
            <v-text-field outlined v-model="publisher" label="Publisher"
            :rules="[() => !!publisher || 'Publisher Required']">
            </v-text-field>
            <v-text-field outlined v-model="developer" label="Developer"
            :rules="[() => !!developer || 'Developer Required']"
            >
            </v-text-field>
            <v-text-field outlined v-model="game_url" label="Game URL (Optional)"
            :rules="[() => !!game_url || 'Game Website Required']"
            >
            </v-text-field>
            <v-col>
                <v-menu
                  v-model="menu"
                  :close-on-content-click="false"
                  :nudge-right="40"
                  transition="scale-transition"
                  offset-y
                  min-width="auto">
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="date"
                      label="Pick Game Released Date"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on">
                    </v-text-field>
                  </template>
                  <v-date-picker  v-model="date" 
                      :max="new Date().toISOString().substr(0, 10)" 
                      @input="menu = false"
                      locale="th"
                  ></v-date-picker>
                </v-menu>
              </v-col>
            </v-form>

          </v-card-text>
        </v-card>
      <v-btn color="primary" :disabled="!isFormValid2" @click="e4 = 3">Continue</v-btn>
      <v-btn @click="e4 = 1">Cancel</v-btn>
    </v-stepper-content>

    <v-stepper-content step="3">
      <v-card color="#121212" class="mb-5" height="200px">
        <v-card-text>
          <v-form ref="form3" v-model="isFormValid3">
          <v-text-field outlined v-model="short_description" label="Game Description"
          :rules="[() => !!short_description || 'Description Required']"></v-text-field>
        </v-form>
        </v-card-text>
      </v-card>
        <v-btn color="primary"  :disabled="!isFormValid3" @click="goIndex()" >Continue</v-btn>

      <v-btn @click="e4 = 2">Cancel</v-btn>
    </v-stepper-content>
  </v-stepper>
</v-row>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

interface GameData {
  id: number;
  thumbnail: string | null;
  title: string;
  release_date: string;
  genre: string;
  platform: string;
  url : string | ArrayBuffer | null;
  publisher: string;
  developer: string;
  short_description: string;
  game_url: string;
}

export default Vue.extend({
    name: 'AddPage',
    data(){
        return{
            datas:[] as GameData[],
            e4: 1,
            isFormValid: false,
            isFormValid2: false,
            isFormValid3: false,
            title: "",
            genre: "",
            thumbnail: null,
            url: null as string | ArrayBuffer | null,
            platform: "",
            publisher: "",
            developer: "",
            game_url: "",
            id: 0,
            date: new Date().toISOString().substr(0, 10),
            menu: false,
            short_description: "",
            genres: [],
            platforms: [],
        }
    },
    async created() {
      try {
          const { data } = await this.$axios.get("http://localhost:9000/games");
          //localStorage.clear();
          data.Games.sort((a:GameData, b:GameData) => Date.parse(b.release_date) - Date.parse(a.release_date));
          this.datas = data.Games;
          this.genres = data.genres;
          this.platforms = data.platforms;
      }catch (error) {
        console.error(error);
      }
    },
    computed:{
        form():GameData{
            return{
              title: this.title,
              genre: this.genre,
              thumbnail: this.url as string | null,
              platform: this.platform,
              game_url: this.game_url,
              publisher: this.publisher,
              developer: this.developer,
              short_description: this.short_description,
              id: this.id,
              release_date: this.date,
              url: this.url as string | ArrayBuffer | null, 
            }
        },
    },
    methods:{
        pre_thumbnail(){
          const file = this.thumbnail;

          if(file){
            const reader = new FileReader();
          reader.onload = () => {
            this.url = reader.result;
          };
          reader.readAsDataURL(file);
          }else{
            this.url = null;
          }
        },
        goIndex(){
          const Swal = require('sweetalert2')
          Swal.fire({
            title: '<strong>You Are Adding New Game</strong>',
            text: 'Are You Sure?',
            showDenyButton: true,
            confirmButtonText: 'Yes',
            confirmButtonColor: '#3085d6',
            denyButtonText: `Cancel`,
          }).then(async (result: { isConfirmed: any; }) => {
            if (result.isConfirmed) {
              this.id = this.getMaxId() + 1;
              try{
                await this.$axios.post("http://localhost:9000/games", this.form);
              Swal.fire({
                title: 'Your Game is Added', 
                icon:'success',
                showCancelButton: true,
                confirmButtonText: 'Go To Index Page',
                cancelButtonText: 'Add New Game Data'
              }).then((result: { isConfirmed: any; isDismissed: any; })=>{
                  if(result.isConfirmed){
                    this.$router.push({name:"index"});
                  }else if(result.isDismissed){
                    Swal.fire('We will take you to Add Game page', '', 'info')
                    this.reset();
                  }
                })
              }catch (error:any){
                const errors = error.response.data.errors;
                let errorMessage = '';
                errors.forEach((err: { msg: string }) => {
                  errorMessage += err.msg + '\n';
                });
                Swal.fire({
                  title: errorMessage,
                  icon: "error",
                  confirmButtonText: "OK",
                  confirmButtonColor: "#F1D00A",
                });
                this.e4=1;
                console.log(errorMessage)
              }
            } 
          })
        },
        getMaxId() {
          const maxId = this.datas.reduce(
            (max, item) => (item.id > max ? item.id : max),this.datas[0].id);
          return maxId;
        },
        reset(){
          this.e4 = 1;
          (this.$refs.form as HTMLFormElement).reset();
          (this.$refs.form2 as HTMLFormElement).reset();
          (this.$refs.form3 as HTMLFormElement).reset();
          this.date =  new Date().toISOString().substr(0, 10);
        },
    }
})
</script>
