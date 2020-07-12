<template>
  <div
    class="row border p-1 m-1 aufgabenbg align-items-center justify-content-center"
    v-bind:class="{'bg-warning' : isWarn(), 'bg-success' : isSuccess()}"
  >
    <!-- <div class="border col-sm-3 p-1 m-1 aufgabenbg"  -->
    <p class="col">{{ info }}</p>
    <div class="input-group mb-2 p-1">
      <label class="lead p-2">{{aufgabe}}</label>
      <input
        type="number"
        class="form-control"
        id="eingabe"
        ref="eingabe"
        v-on:keyup.enter="checkResult()"
        tabindex="1"
        placeholder="Ergebnis?"
      />
      <div class="input-group-append">
        <button
          tabindex="5"
          class="btn btn-primary"
          type="button"
          @click="checkResult()"
        >Pr&uuml;fen</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Ref } from "vue-property-decorator";

@Component
export default class Aufgabe extends Vue {
  @Prop() private aufgabe!: string;
  @Prop() private ergebnis!: number;

  @Ref("eingabe") eingabe!: HTMLInputElement;

  richtig = false;
  berechnet = false;
  info = "";
  letzteEingabe = "";

  checkResult(): void {
    console.log("result is " + this.eingabe.value);
    this.berechnet = true;
    const eingabeStr = this.eingabe.value;
    console.log("Eingabe = " + eingabeStr + ", trimmed: " + eingabeStr.trim());
    const eingabeNr = Number(this.eingabe.value.trim());
    this.richtig = eingabeNr == this.ergebnis;
    this.letzteEingabe = eingabeStr;
    if (!this.richtig) {
      this.info = this.letzteEingabe + " ist leider nicht richtig - versuche es nochmal!";
      this.eingabe.value="";
    } else {
      this.info = "richtig!";
    }
    console.log(
      "Richtig = " +
        this.richtig +
        ", weil " +
        eingabeNr +
        " ?= " +
        this.ergebnis
    );
  }

  isWarn(): boolean {
    return !this.richtig && this.berechnet;
  }

  isSuccess(): boolean {
    return this.richtig && this.berechnet;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.aufgabenbg {
  background-color: antiquewhite;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
