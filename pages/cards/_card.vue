<template>
  <div>
    <confirmed-cases-details-card
      v-if="this.$route.params.card == 'details-of-confirmed-cases'"
    /><!-- ok -->
    <confirmed-cases-number-card
      v-else-if="this.$route.params.card == 'number-of-confirmed-cases'"
    /><!-- ok -->
    <confirmed-cases-attributes-card
      v-else-if="this.$route.params.card == 'attributes-of-confirmed-cases'"
    /><!-- ok -->
    <tested-number-card
      v-else-if="this.$route.params.card == 'number-of-tested'"
    /><!-- ok -->
    <consultation-desk-reports-number-card
      v-else-if="
        this.$route.params.card ==
          'number-of-reports-to-covid19-consultation-desk'
      "
    /><!-- ok -->
    <yamaguchi-map-card
      v-else-if="this.$route.params.card == 'map-in-yamaguchi'"
    />
  </div>
</template>

<script>
// import Data from '@/data/data.json'
// import MetroData from '@/data/metro.json'
// import agencyData from '@/data/agency.json'
// ↓ import patientData from '@/data/patient.json'
// import patientData from '@/data/patients.json'
// import PositiveByDiagnosedData from '@/data/positive_by_diagnosed.json'
// import PositiveRate from '@/data/positive_rate.json'
import ConfirmedCasesDetailsCard from '@/components/cards/ConfirmedCasesDetailsCard.vue'
import ConfirmedCasesNumberCard from '@/components/cards/ConfirmedCasesNumberCard.vue'
import ConfirmedCasesAttributesCard from '@/components/cards/ConfirmedCasesAttributesCard.vue'
// import ConfirmedCasesByMunicipalitiesCard from '@/components/cards/ConfirmedCasesByMunicipalitiesCard.vue'
import TestedNumberCard from '@/components/cards/TestedNumberCard.vue'
// import InspectionPersonsNumberCard from '@/components/cards/InspectionPersonsNumberCard.vue'
// import TelephoneAdvisoryReportsNumberCard from '@/components/cards/TelephoneAdvisoryReportsNumberCard.vue'
import ConsultationDeskReportsNumberCard from '@/components/cards/ConsultationDeskReportsNumberCard.vue'
// import MetroCard from '@/components/cards/MetroCard.vue'
// import AgencyCard from '@/components/cards/AgencyCard.vue'
// import PositiveNumberByDiagnosedDateCard from '@/components/cards/PositiveNumberByDiagnosedDateCard.vue'
// import PositiveRateCard from '@/components/cards/PositiveRateCard.vue'
import YamaguchiMapCard from '@/components/cards/YamaguchiMapCard.vue'

export default {
  components: {
    ConfirmedCasesDetailsCard,
    ConfirmedCasesNumberCard,
    ConfirmedCasesAttributesCard,
    TestedNumberCard,
    ConsultationDeskReportsNumberCard,
    YamaguchiMapCard
  },
  data() {
    let title, updatedAt
    switch (this.$route.params.card) {
      case 'details-of-confirmed-cases':
        title = this.$t('検査陽性者の状況')
        updatedAt = this.$store.state.data.hospitalizations.last_update
        break
      case 'number-of-confirmed-cases':
        title = this.$t('陽性患者数')
        updatedAt = this.$store.state.data.patientscnt.last_update
        break
      case 'attributes-of-confirmed-cases':
        title = this.$t('陽性患者の属性')
        updatedAt = this.$store.state.data.patients.last_update
        break
      case 'number-of-tested':
        title = this.$t('PCR検査実施件数（累計）')
        updatedAt = this.$store.state.data.inspections.last_update
        break
      case 'number-of-reports-to-covid19-consultation-desk':
        title = this.$t('新型コロナ受診相談窓口相談件数（累計）')
        updatedAt = this.$store.state.data.querents.last_update
        break
      case 'map-in-yamaguchi':
        title = this.$t('市区町村別陽性患者の累計')
        updatedAt = this.$store.state.data.mapupdate
        break
    }

    const data = {
      title,
      updatedAt
    }
    return data
  },
  head() {
    const url = 'https://yamaguchi.stopcovid19.jp'
    const timestamp = new Date().getTime()
    const ogpImage =
      this.$i18n.locale === 'ja'
        ? `${url}/ogp/${this.$route.params.card}.png?t=${timestamp}`
        : `${url}/ogp/${this.$i18n.locale}/${this.$route.params.card}.png?t=${timestamp}`
    const description = `${this.updatedAt} | ${this.$t(
      '当サイトは新型コロナウイルス感染症 (COVID-19) に関する最新情報を提供するために、山口県内外の有志が開設したものです。'
    )}`

    return {
      title: this.title,
      meta: [
        {
          hid: 'og:url',
          property: 'og:url',
          content: url + this.$route.path + '/'
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content:
            this.title +
            ' | ' +
            this.$t('山口県公認') +
            ' ' +
            this.$t('新型コロナウイルス感染症') +
            this.$t('対策サイト')
        },
        {
          hid: 'description',
          name: 'description',
          content: description
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: description
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: ogpImage
        },
        {
          hid: 'twitter:image',
          name: 'twitter:image',
          content: ogpImage
        }
      ]
    }
  }
}
</script>
