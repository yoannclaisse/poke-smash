<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import FooterView from './components/Footer.vue'
import CardView from './components/Card.vue'

</script>


<template>
		<div id="app">

			<!-- Header -->
				<div id="header-wrapper">
					<div id="stars-00"></div>
<div id="stars-01"></div>
<div id="stars-02"></div>
					<!-- Logo -->
					<div id="logo">
            			<img class="logoPoke centrer" src="../public/img/logoPokesmashBlanc.png">
					</div>
					<div class="centrer">
						<CardView />
					</div>
          		
          		<!-- <FooterView /> -->
				</div>
    </div>
</template>

<style scoped>
  #app {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
	font-family: 'Reem Kufi Fun', sans-serif;
	text-align: center;
    padding: 20px;
	background: rgb(143,0,0);
	background: linear-gradient(90deg, rgba(143,0,0,1) 0%, rgba(212,3,3,1) 25%, rgba(255,75,75,1) 50%, rgba(60,152,255,1) 50%, rgba(0,96,204,1) 76%, rgba(0,22,119,1) 100%);
  }

</style>