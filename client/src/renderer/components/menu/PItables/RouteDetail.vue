<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="routes"
      hide-actions
      class="elevation-2"
    >
      <template slot="items" slot-scope="props">
        <tr :style="backgroundColor" dark :active="props.item.targeted">
        <td>{{ props.item.receiver }}</td>
        <td class="text-xs">{{ props.item.route }}</td>
        <td class="text-xs">{{ props.item.depth }}</td>
        <td class="text-xs">{{ props.item.maxsep }}</td>
        <td class="text-xs">{{ props.item.avgsep }}</td>
        </tr>

      </template>
    </v-data-table>
  </div>
</template>

<script>
import {mapGetters} from 'vuex'
export default {
  computed: {
    ...mapGetters({
      trackingData: 'trackingData',
      currentPage: 'currentPage',
      routeData: 'routeData'
    })
  },
  data () {
    return {
      headers: [
        {text: 'Receiver', align: 'center', sortable: false, value: 'rec'},
        {text: 'Route', value: 'rte', sortable: false, align: 'center'},
        {text: 'Depth', value: 'depth', align: 'center', sortable: false},
        {text: 'Max Separation', value: 'maxsep', sortable: false, align: 'center'},
        {text: 'Avg Separation', value: 'avgsep', sortable: false, align: 'center'}
      ],
      backgroundColor: 'rgb(0, 17, 119)',
      routes: [
        {
          receiver: 'test',
          route: null,
          depth: null,
          maxsep: null,
          avgsep: null,
          targeted: true
        },
        {
          receiver: null,
          route: null,
          depth: null,
          maxsep: null,
          avgsep: null,
          targeted: false
        },
        {
          receiver: null,
          route: null,
          depth: null,
          maxsep: null,
          avgsep: null,
          targeted: false
        },
        {
          receiver: null,
          route: null,
          depth: null,
          maxsep: null,
          avgsep: null,
          targeted: false
        }
      ]
    }
  },
  methods: {
    fill () {
      let playerInfo = []
      let offense = this.trackingData.mPlayerRoles.offense
      // set cnt var for index reference
      let count = 0
      for (let i of offense) {
        // set init name
        if (i.route.name !== '') {
          // set player names
          // for (let j of this.trackingData.mTeamRoster.offense) {
          //   if (i.playerId === j.playerId) {
          //     name = j.firstName + ' ' + j.lastName
          //   }
          // }
          let targetReceiver = false
          // if player id is equal to that of intented receiver set targetReceiver to true
          if (i.playerId === this.routeData['Intended Receiver']) {
            targetReceiver = true
          }
          // set average seperation
          let arr1 = this.routeData.Separation[count]
          let avgSep = arr1.reduce((accumulator, currentValue) => {
            return accumulator + currentValue
          }, arr1[0]) / arr1.length

          playerInfo.push({
            value: false,
            receiver: this.trackingData.pffpos[i.playPos].pff,
            route: i.route.name,
            depth: i.route.depth + ' yds',
            maxsep: (Math.max(...this.routeData.Separation[count])).toFixed(1) + ' yds',
            avgsep: avgSep.toFixed(1) + ' yds',
            targeted: targetReceiver
          })
          count++
        }
      }
      this.routes = playerInfo
    }
  },
  mounted () {
    this.fill()
  }
}
</script>

<style scoped>
.application .theme--light.table tbody tr[active], .theme--light .table tbody tr[active] {
  background-color: #2c3e50;
  color: white;
}
</style>
