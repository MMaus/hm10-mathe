<template>
  <div id="app">
    <div class="jumbotron">
      <h3>Kopfrechnen - Malaufgaben</h3>
    </div>
    <Aufgabe v-for="agf in aufgaben_mal" :key="agf.idx" :aufgabe="agf.txt" :ergebnis="agf.ergebnis" />

    <div class="jumbotron">
      <h3>Kopfrechnen - Plusaufgaben</h3>
    </div>
    <Aufgabe v-for="agf in aufgaben_plus" :key="agf.idx" :aufgabe="agf.txt" :ergebnis="agf.ergebnis" />

    <div class="jumbotron">
      <h3>Kopfrechnen - Minusaufgaben</h3>
    </div>
    <Aufgabe v-for="agf in aufgaben_minus" :key="agf.idx" :aufgabe="agf.txt" :ergebnis="agf.ergebnis" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import Aufgabe from "./components/Aufgabe.vue";

const oneToTen = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const baseMul = Math.floor(Math.random() * 9) + 2;
let idx = 1;

function shuffle<T>(array: Array<T>): Array<T> {
  let currentIndex = array.length,
    temporaryValue,
    randomIndex;

  // While there remain elements to shuffle...
  while (0 !== currentIndex) {
    // Pick a remaining element...
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex -= 1;

    // And swap it with the current element.
    temporaryValue = array[currentIndex];
    array[currentIndex] = array[randomIndex];
    array[randomIndex] = temporaryValue;
  }

  return array;
}

class Rechenaufgabe {
  txt: string;
  ergebnis: number;
  idx: number;
  constructor(txt: string, ergebnis: number) {
    this.txt = txt;
    this.ergebnis = ergebnis;
    this.idx = idx;
    idx = idx + 1;
  }
}

function malAufgabe(nr: number): Rechenaufgabe {
  const ergebnis = nr * baseMul;
  const aufgabenText: string =
    Math.random() > 0.5
      ? " " + nr + " * " + baseMul
      : " " + baseMul + " * " + nr;
  return new Rechenaufgabe(aufgabenText, ergebnis);
}

const malAufgaben = shuffle(oneToTen).map(nr => malAufgabe(nr));

function plusAufgabe(limit: number): Rechenaufgabe {
  const ergebnis = Math.round(Math.random() * limit);
  const summand1 = Math.round(Math.random() * ergebnis);
  const summand2 = ergebnis - summand1;
  const aufgabenText: string = summand1 + " + " + summand2;
  return new Rechenaufgabe(aufgabenText, ergebnis);
}

function minusAufgabe(limit: number): Rechenaufgabe {
  const minuend = Math.round(Math.random() * limit);
  const subtrahend = Math.round(Math.random() * minuend);
  const ergebnis = minuend - subtrahend;
  const aufgabenText: string = minuend + " - " + subtrahend;
  return new Rechenaufgabe(aufgabenText, ergebnis);
}

const plusAufgabenLimit = [100, 100, 100];
const plusAufgaben = plusAufgabenLimit.map(nr => plusAufgabe(nr));

const minusAufgabenLimit = [100, 100, 100];
const minusAufgaben = minusAufgabenLimit.map(nr => minusAufgabe(nr));

@Component({
  components: {
    Aufgabe
  }
})
export default class App extends Vue {
  aufgaben_mal = malAufgaben;
  aufgaben_plus = plusAufgaben;
  aufgaben_minus = minusAufgaben;
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
