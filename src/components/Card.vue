<!-- <template>	
            <div id="banner-wrapper" class="mt-3">
                <div id="banner" class="box container">
                    <div class="row">
                        <div class="col-7 col-12-medium">
                            <h2>Hi. This is Verti.</h2>
                            <p>It's a free responsive site template by HTML5 UP</p>
                        </div>
                        <div class="col-5 col-12-medium">
                            <ul>
                                <li><a href="#" class="button large icon solid fa-arrow-circle-right">Ok let's go</a></li>
                                <li><a href="#" class="button alt large icon solid fa-question-circle">More info</a></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>

</template> -->

<!-- idée : au clique du bouton : afficher SMAH ! Pass ! -->
<template>
    <div>
      <div v-if="pokemon">
        <div id="pokemon-details" class="cardGlow">
          <div class="imgBox">
            <!-- <p clas="pokedex">{{ pokemon.id }}/1010</p> -->
            <img :src="pokemon.sprite" alt="Pokemon Sprite" class="pokemon-image">
          </div>
          <p class="pokemon-info">{{ pokemon.name }}</p>
          <p clas="pokedex">{{ pokemon.id }}/1010</p>
          <div id="comment-section">
          <textarea v-model="comment" placeholder="Exprimez-vous sur ce Pokémon" class="comment-input"></textarea>
          <div class="button-group">
            <button @click="sendComment" class="send-button">Envoyer</button>
            <button @click="editComment" class="edit-button">Modifier</button>
          </div>
          </div>


        </div>
          <div id="buttons-container">
            <div class="button-group">
              <button @click="reloadPage('pass')" class="pass-button"> <img src="../assets/images/oak2.png" class="iconeVote"></button>
              <button @click="reloadPage('smash')" class="smash-button"><img src="../assets/images/smashpoke.png" class="iconeVote"></button>
            </div>
          </div>
        
      </div>
      <div v-else>
        <p>Chargement en cours...</p>
      </div>
    </div>
  </template>

<script lang="ts">
  export default {
    data() {
      return {
        pokemon: null,
        comment: '',
        isEditMode: false
      };
    },
    mounted() {
      this.loadRandomPokemon();
    },
    methods: {
      loadRandomPokemon() {
        const randomId = Math.floor(Math.random() * 1010) + 1;

        fetch(`http://localhost:9999/api/pokemon/${randomId}`)
          .then(response => {
            if (!response.ok) {
              throw new Error(`Erreur HTTP! Statut: ${response.status}`);
            }
            return response.json();
          })
          .then(data => {
            this.pokemon = data;
          })
          .catch(error => {
            console.error('Erreur lors de la requête à l\'API', error);
          });
      },

      reloadPage(action) {
        console.log(action);
        fetch(`http://localhost:9999/api/pokemon/${this.pokemon.id}/${action}`, {method: 'PATCH'})
          .then(response => {
            if (!response.ok) {
              throw new Error(`Erreur HTTP! Statut: ${response.status}`);
            }
            return response.json();
          })
          .then(data => {
            this.pokemon = data;
          })
          .catch(error => {
            console.error('Erreur lors de la requête à l\'API', error);
          });
        window.location.reload();
      },

      sendComment() {
        if (this.comment.trim() === '') {
          console.warn('Le commentaire ne peut pas être vide.');
          return;
        }

        const reqBody = { pokemon_comment_content: this.comment, pokemon_id: this.pokemon?.id, pokemon_comment_author: 'author' }
        console.log('Commentaire à envoyer à l\'API :', reqBody);

        fetch(`http://localhost:9999/api/pokemon/comment`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(reqBody)
        })
          .then(response => {
            if (!response.ok) {
              throw new Error(`Erreur HTTP! Statut: ${response.status}`);
            }
            console.log('Commentaire envoyé avec succès.');
            this.comment = '';
          })
          .catch(error => {
            console.error('Erreur lors de l\'envoi du commentaire à l\'API', error);
          });
      },
      editComment() {
        window.alert("L'API n'est pas encore développée");
        // fetch(`http://localhost:9999/comment`)
      }
    }
  };
  </script>

  <style scoped>

  .iconeVote {
	max-width: 50px;
  }
  .cardGlow {
	box-shadow: 0px 1px 43px 5px #c5b785 !important;
	box-shadow: 0px 1px 43px 5px #fdecb1 !important;
    -webkit-box-shadow: 0px 1px 43px 5px #fff1c3c5 !important;
	}



  #pokemon-details {
	width: 330px !important;
    height: 450px !important;
	background: radial-gradient(circle at 100% 100%, #ffffff00 0, #ffffff00 2px, transparent 2px) 0% 0%/17px 17px no-repeat,
            radial-gradient(circle at 0 100%, #ffffff00 0, #ffffff00 2px, transparent 2px) 100% 0%/17px 17px no-repeat,
            radial-gradient(circle at 100% 0, #ffffff00 0, #ffffff00 2px, transparent 2px) 0% 100%/17px 17px no-repeat,
            radial-gradient(circle at 0 0, #ffffff00 0, #ffffff00 2px, transparent 2px) 100% 100%/17px 17px no-repeat,
            linear-gradient(#ffffff00, #ffffff00) 50% 50%/calc(100% - 30px) calc(100% - 34px) no-repeat,
            linear-gradient(#ffffff00, #ffffff00) 50% 50%/calc(100% - 34px) calc(100% - 30px) no-repeat,
            linear-gradient(316deg, #ffc91c 0%, rgba(255,255,155,1) 26%, #ffc200 49%, rgba(255,255,155,1) 72%, #ffc200 100%);
	border-radius: 17px;
	padding: 31px;
	box-sizing: border-box;

    background-color: #fff;
    border-radius: 10px;
    padding: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
  }

  .imgBox {
	/* background-image: url("../assets/images/galaxy.jpg"); */
  background: rgb(58,171,96);
  background: linear-gradient(0deg, rgba(58,171,96,1) 0%, rgba(99,207,136,1) 22%, rgba(77,214,162,1) 41%, rgba(56,217,193,1) 54%, rgba(145,228,254,1) 83%, rgba(208,254,255,1) 100%);
  border-radius: 5px;
	width: 290px;
	height: 200px;
	box-shadow: 4px 4px 8px -1px rgba(0,0,0,0.24) inset;
  -webkit-box-shadow: 4px 4px 8px -1px rgba(0,0,0,0.24) inset;
  -moz-box-shadow: 4px 4px 8px -1px rgba(0,0,0,0.24) inset;
	background-color: #fff9f463;
  }
  .pokemon-image {
	max-width: 65% !important;
  }

  .pokemon-info {
    margin-top: 10px !important;
		max-width: 100% !important;
    text-shadow: rgb(255, 255, 255) 4px 0px 0px, rgb(255, 255, 255) 3.87565px 0.989616px 0px, rgb(255, 255, 255) 3.51033px 1.9177px 0px, rgb(255, 255, 255) 2.92676px 2.72656px 0px, rgb(255, 255, 255) 2.16121px 3.36588px 0px, rgb(255, 255, 255) 1.26129px 3.79594px 0px, rgb(255, 255, 255) 0.282949px 3.98998px 0px, rgb(255, 255, 255) -0.712984px 3.93594px 0px, rgb(255, 255, 255) -1.66459px 3.63719px 0px, rgb(255, 255, 255) -2.51269px 3.11229px 0px, rgb(255, 255, 255) -3.20457px 2.39389px 0px, rgb(255, 255, 255) -3.69721px 1.52664px 0px, rgb(255, 255, 255) -3.95997px 0.56448px 0px, rgb(255, 255, 255) -3.97652px -0.432781px 0px, rgb(255, 255, 255) -3.74583px -1.40313px 0px, rgb(255, 255, 255) -3.28224px -2.28625px 0px, rgb(255, 255, 255) -2.61457px -3.02721px 0px, rgb(255, 255, 255) -1.78435px -3.57996px 0px, rgb(255, 255, 255) -0.843183px -3.91012px 0px, rgb(255, 255, 255) 0.150409px -3.99717px 0px, rgb(255, 255, 255) 1.13465px -3.8357px 0px, rgb(255, 255, 255) 2.04834px -3.43574px 0px, rgb(255, 255, 255) 2.83468px -2.82216px 0px, rgb(255, 255, 255) 3.44477px -2.03312px 0px, rgb(255, 255, 255) 3.84068px -1.11766px 0px, rgb(255, 255, 255) 3.9978px -0.132717px 0px;		font-size: 25px !important;
    font-size: 18px;
    margin: 2px 0;
  }

  .pokedex {
    margin-bottom: 0px !important;
  }
  #buttons-container {
    margin-top: 10px;
  }

  .button-group {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .pass-button,
  .smash-button,
  .send-button,
  .edit-button {
    background-color: #ffffffc5;
    color: #000;
    border: none;
    /* padding: 10px 20px; */
    font-size: 16px;
    cursor: pointer;
    border-radius: 100px;
    transition: background-color 0.3s;
    margin: 0 30px;
  }

  .pass-button:hover,
  .smash-button:hover,
  .send-button:hover,
  .edit-button:hover {
    background-color: #ffffff;
  }

  .comment-input {
    width: 100%;
    padding: 10px;
    border: 1px solid #ffe000;
    background: #f7f7f72e;
    font-family: 'Poppins', sans-serif;    border-radius: 5px;
  }

  .comment-input:focus { outline: white ; }

  .send-button,
  .edit-button {
    margin: 5px;
  }

  </style>