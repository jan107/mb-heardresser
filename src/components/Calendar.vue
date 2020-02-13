
<template>
  <div>
    <v-sheet tile height="54" color="grey lighten-3" class="d-flex">
      <v-btn icon class="ma-2" @click="$refs.calendar.prev()">
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>

      <v-spacer></v-spacer>

      <v-btn icon class="ma-2" @click="$refs.calendar.next()">
        <v-icon>mdi-chevron-right</v-icon>
      </v-btn>
    </v-sheet>
    
    <v-sheet height="600">
      <v-calendar
        ref="calendar"
        v-model="value"
        :weekdays="weekday"
        :type="type"
        :events="events"
        :event-overlap-mode="mode"
        :event-overlap-threshold="30"
        :event-color="getEventColor"
        @change="getEvents">
      </v-calendar>
    </v-sheet>



    <v-row justify="center">
      <v-dialog v-model="dialog" persistent max-width="600px">
        <template v-slot:activator="{ on }">
          <v-btn color="primary" dark v-on="on">Nueva Reserva</v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="headline">Nueva Reserva</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12" sm="6">
                  <v-text-field label="Nombre*" required></v-text-field>
                </v-col>
                <v-col cols="12" sm="6">
                  <v-text-field label="Apellidos*" required></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field label="Telefono*" hint="móvil o fijo (preferentemente móvil)" persistent-hint required></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field label="Email" hint="opcional" persistent-hint ></v-text-field>
                </v-col>
                
                <v-col cols="12" sm="6">
                  <v-select
                    :items="['Hombre', 'Mujer']"
                    label="Sexo*"
                    required
                  ></v-select>
                </v-col>
                <v-col cols="12" sm="6">
                  <v-autocomplete
                    :items="['Cortar', 'Afeitar Navaja', 'Arreglar Barba', 'Peinar']"
                    label="Servicio/s"
                    multiple
                  ></v-autocomplete>
                </v-col>
              </v-row>
            </v-container>
            <small>*Campos obligatorios</small>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text @click="dialog = false">Cancelar</v-btn>
            <v-btn color="blue darken-1" text @click="dialog = false">Reservar</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>




  </div>

  

</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      type: 'month',
      mode: 'stack',
      modes: ['stack', 'column'],
      weekday: [1, 2, 3, 4, 5, 6, 0],
      value: '',
      events: [],
      colors: ['blue', 'indigo', 'green'],
      names: ['Alejandro', 'Christian', 'Megan', 'Ferdia', 'David'],
    }),
    methods: {
      getEvents ({ start, end }) {
        const events = []
        const min = new Date(`${start.date}T00:00:00`)
        const max = new Date(`${end.date}T23:59:59`)
        const eventCount = 3;

        const firstTimestamp = this.rnd(min.getTime(), max.getTime())

        for (let i = 0; i < eventCount; i++) {
          const first = new Date(firstTimestamp - (firstTimestamp % 900000) + 900000 * i);
          const second = new Date(first.getTime() + 900000);

          events.push({
            name: this.names[this.rnd(0, this.names.length - 1)],
            start: this.formatDate(first),
            end: this.formatDate(second),
            color: this.colors[this.rnd(0, this.colors.length - 1)],
          })
        }
        this.events = events;
      },
      getEventColor (event) {
        return event.color
      },
      rnd (a, b) {
        return Math.floor((b - a + 1) * Math.random()) + a
      },
      formatDate (a) {
        return `${a.getFullYear()}-${a.getMonth() + 1}-${a.getDate()} ${a.getHours()}:${a.getMinutes()}`;
      },
    },
  }
</script>