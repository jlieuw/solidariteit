<template>
  <v-container fluid>
    <v-row>
      <v-spacer></v-spacer>
      <v-col cols="12" lg="6" xl="6">
        <v-row>
          <v-col cols="12">
            <v-text-field label="Bruto salaris" type="number" prefix="SRD" v-model="brutoSalaris"></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6">
            <v-text-field
              label="Netto normale salaris"
              type="number"
              prefix="SRD"
              :placeholder=" 'test' "
              v-model="nettoSalaris"
              readonly
              filled
            ></v-text-field>
          </v-col>
          <v-col cols="6">
            <v-text-field
              label="Netto Solidariteits salaris"
              type="number"
              prefix="SRD"
              v-model="nettoSalarissolidariteit"
              readonly
              filled
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6">
            <v-text-field
              label="Verschil"
              type="number"
              prefix="SRD"
              v-model="verschil"
              readonly
              filled
            ></v-text-field>
          </v-col>
        </v-row>
      </v-col>
      <v-spacer></v-spacer>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Salaris",

  data: () => ({
    vgb: 16.67,
    forfetairPercetange: 0.04,
    forfetairMax: 100,
    schijf1: 0.08,
    schijf2: 0.18,
    schijf3: 0.28,
    schijf4: 0.38,
    schijf3solidariteit: 0.38,
    schijf4solidariteit: 0.48,
    aovPercentage: 0.04,
    vrijeVoet: 220.5,
    schijf1Grens: 946.4,
    schijf2Grens: 1606.15,
    schijf3Grens: 2516,
    brutoSalaris: 0,
  }),
  computed: {
    forfetair() {
      let forfetair = this.brutoSalaris * this.forfetairPercetange;
      if (forfetair > 100) forfetair = 100;
      return forfetair;
    },
    belastbaar() {
      return this.brutoSalaris * 1 + this.vgb - this.forfetair - this.vrijeVoet;
    },
    zuiverloon() {
      return this.brutoSalaris * 1 + this.vgb - this.forfetair;
    },
    belastingSchijf1() {
      let schijf1Bedrag = this.belastbaar;
      if (this.belastbaar > this.schijf1Grens)
        schijf1Bedrag = this.schijf1Grens;
      return Math.max(this.schijf1 * schijf1Bedrag, 0);
    },
    belastingSchijf2() {
      let schijf2Bedrag = this.belastbaar - this.schijf1Grens;
      if (this.belastbaar > this.schijf2Grens)
        schijf2Bedrag = this.schijf2Grens - this.schijf1Grens;
      return Math.max(this.schijf2 * schijf2Bedrag, 0);
    },
    belastingSchijf3() {
      let schijf3Bedrag = this.belastbaar - this.schijf2Grens;
      if (this.belastbaar > this.schijf3Grens)
        schijf3Bedrag = this.schijf3Grens - this.schijf2Grens;
      return Math.max(this.schijf3 * schijf3Bedrag, 0);
    },
    belastingSchijf4() {
      return Math.max((this.belastbaar - this.schijf3Grens) * this.schijf4, 0);
    },
    belastingSchijf3Solidariteit() {
      let schijf3Bedrag = this.belastbaar - this.schijf2Grens;
      if (this.belastbaar > this.schijf3Grens)
        schijf3Bedrag = this.schijf3Grens - this.schijf2Grens;
      return Math.max(this.schijf3solidariteit * schijf3Bedrag, 0);
    },
    belastingSchijf4Soldariteit() {
      return Math.max(
        (this.belastbaar - this.schijf3Grens) * this.schijf4solidariteit,
        0
      );
    },
    aov() {
      return this.zuiverloon * this.aovPercentage;
    },
    loonBelasting() {
      return (
        this.belastingSchijf1 +
        this.belastingSchijf2 +
        this.belastingSchijf3 +
        this.belastingSchijf4
      );
    },
    nettoSalaris() {
      return Math.max(
        this.brutoSalaris * 1 - (this.aov + this.loonBelasting),
        0
      ).toFixed(2);
    },
    loonBelastingsoldariteit() {
      return (
        this.belastingSchijf1 +
        this.belastingSchijf2 +
        this.belastingSchijf3Solidariteit +
        this.belastingSchijf4Soldariteit
      );
    },
    nettoSalarissolidariteit() {
      return Math.max(
        this.brutoSalaris * 1 - (this.aov + this.loonBelastingsoldariteit),
        0
      ).toFixed(2);
    },
    verschil() {
      return (this.nettoSalaris - this.nettoSalarissolidariteit).toFixed(2);
    },
  },
};
</script>