<template>
  <div class="col s12 m6 l8">
    <div class="card orange darken-3 bill-card">
      <div class="card-content white-text">
        <div class="card-header">
          <span class="card-title">{{ 'Home_Rates' | localize }}</span>
        </div>
        <table>
          <thead>
            <tr>
              <th>{{ 'Home_RatesCurrency' | localize }}</th>
              <th>{{ 'Home_RatesRate' | localize }}</th>
              <th>{{ 'Home_RatesDate' | localize }}</th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="cur of currencies" :key="cur">
              <td>{{ cur }}</td>
              <td>{{ rates[cur].toFixed(2) }}</td>
              <td>{{ date | dateFilter('date') }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['rates', 'date'],
  data: () => ({
    currencies: ['PLN', 'USD', 'EUR']
  })
}
</script>
