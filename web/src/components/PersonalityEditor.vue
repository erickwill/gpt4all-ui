<template>
  <div v-if="show"  class="fixed top-0 left-0 right-0 bottom-0 flex items-center justify-center bg-black bg-opacity-50 z-20">
    <div class="relative w-full max-h-full bg-bg-light dark:bg-bg-dark">
      <div class="w-full h-full relative items-center gap-2 rounded-lg border  bg-bg-light dark:bg-bg-dark p-1.5 shadow-sm hover:shadow-none dark:border-gray-800 dark:bg-gray-900" >
        <button type="button" @click="hide()"
          class="absolute top-1 right-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-800 dark:hover:text-white">
          <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"
            xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd"
              d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
              clip-rule="evenodd"></path>
          </svg>
          <span class="sr-only">Close modal</span>
        </button>        
        <div class="justify-center text-center items-center w-full  bg-bg-light dark:bg-bg-dark">
          <div class="w-full flex flex-row mt-4 text-center justify-center">
          <button type="submit" @click.prevent="submitForm" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
            Commit AI to Server
          </button>
          <button @click.prevent="hide()" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
            Close
          </button>
        </div>        
        <div class="w-full max-h-full container  bg-bg-light dark:bg-bg-dark">
          <form class="mb-4 w-full">
            <table class="w-full  bg-bg-light dark:bg-bg-dark">
            <tr>
              <td><label for="personalityConditioning">Personality Conditioning:</label></td><td><textarea class="dark:bg-black dark:text-primary w-full" id="personalityConditioning" v-model="config.personality_conditioning"></textarea></td>
            </tr>
            <tr>
              <td><label for="userMessagePrefix">User Message Prefix:</label></td><td><input class="dark:bg-black dark:text-primary w-full" type="text" id="userMessagePrefix" v-model="config.user_message_prefix"></td>
            </tr>
            <tr>
              <td><label for="aiMessagePrefix">AI Message Prefix:</label></td><td><input class="dark:bg-black dark:text-primary w-full" type="text" id="aiMessagePrefix" v-model="config.ai_message_prefix"></td>
            </tr>

            <tr>
              <td><label for="linkText">Link Text:</label></td><td><input class="dark:bg-black dark:text-primary w-full" type="text" id="linkText" v-model="config.link_text"></td>
            </tr>
            <tr>
              <td><label for="welcomeMessage">Welcome Message:</label></td><td><textarea class="dark:bg-black dark:text-primary w-full" id="welcomeMessage" v-model="config.welcome_message"></textarea></td>
            </tr>
            <tr>
              <td><label for="modelTemperature">Model Temperature:</label></td><td><input type="number" id="modelTemperature" v-model="config.model_temperature"></td>
            </tr>
            <tr>
              <td><label for="modelTopK">Model Top K:</label></td>
              <td><input class="dark:bg-black dark:text-primary w-full" type="number" id="modelTopK" v-model="config.model_top_k"></td>
            </tr>
            <tr>
              <td><label for="modelTopP">Model Top P:</label></td>
              <td><input class="dark:bg-black dark:text-primary w-full" type="number" id="modelTopP" v-model="config.model_top_p"></td>
            </tr>
            <tr>
              <td><label for="modelRepeatPenalty">Model Repeat Penalty:</label></td>
              <td><input class="dark:bg-black dark:text-primary w-full" type="number" id="modelRepeatPenalty" v-model="config.model_repeat_penalty"></td>
            </tr>
            <tr>
              <td><label for="modelRepeatLastN">Model Repeat Last N:</label></td>
              <td><input class="dark:bg-black dark:text-primary w-full" type="number" id="modelRepeatLastN" v-model="config.model_repeat_last_n"></td>
            </tr>
            <tr>
              <td><label for="recommendedBinding">Recommended Binding:</label></td>
              <td><input class="dark:bg-black dark:text-primary w-full" type="text" id="recommendedBinding" v-model="config.recommended_binding"></td>
            </tr>
            <tr>
              <td><label for="recommendedModel">Recommended Model:</label></td>
              <td><input class="dark:bg-black dark:text-primary w-full" type="text" id="recommendedModel" v-model="config.recommended_model"></td>
            </tr>
            <tr>
              <td><label class="dark:bg-black dark:text-primary w-full" for="dependencies">Dependencies:</label></td>
              <td><textarea class="dark:bg-black dark:text-primary w-full" id="dependencies" v-model="config.dependencies"></textarea></td>
            </tr>
            <tr>
              <td><label for="antiPrompts">Anti Prompts:</label></td>
              <td><textarea class="dark:bg-black dark:text-primary w-full" id="antiPrompts" v-model="config.anti_prompts"></textarea></td>
            </tr>                 
          </table>
        </form>
        </div>
      </div>        
      </div>
    </div>
  </div>


</template>

<script>
import axios from 'axios';

export default {
  props: {
    personality: {
      type: [Object, null],
      default: null
    },
    config: {
      type: Object,
      required: true
    },
  },
  data() {
    return {
      show:false,
      title: 'Add AI Agent',
      iconUrl: '',
      file: null,
      tempConfig: {} // Create a temporary copy of the 'config' prop
    };
  },
  methods: {
    showForm() {
      this.showDialog = true;
    },
    hideForm() {
      this.showDialog = false;
    },
    selectIcon(event) {
      if (event.target.files) {
        this.file = event.target.files[0];
        this.iconUrl = URL.createObjectURL(this.file);
      }
    },
    showPanel(){
      this.show = true;
    },
    hide(){
      this.show = false;
    },
    submitForm() {
      axios.post('/set_personality_config', {
                client_id: this.$store.state.client_id,
                category: this.personality.category,
                name: this.personality.folder,
                config: this.config
            })
            .then(response => {
            const data = response.data;
            console.log("Done")
            if (data.status) {
                // Update the currentPersonConfig with the received data
                this.currentPersonConfig = data.config;
                this.showPersonalityEditor = true;
            } else {
                // Handle the error
                console.error(data.error);
            }
            })
            .catch(error => {
            // Handle the error
            console.error(error);
            });

    },
  },
};
</script>
