<template>
  <v-container v-if="data.dataLoaded">
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>
            <h2 class="headline font-weight-bold mb-5 text-center">
              Ramadan Calendar - 2023 (রমজান ক্যালেন্ডার - ২০২৩)
            </h2>

            <div class="text-center text-wrap">
              <h3 class="headline font-weight-bold">
                Sehri Dua (সাহরির দু'আ)
              </h3>
              <p>
                {{ data.sehri_dua.arabic }}
              </p>
              <p>
                <strong>উচ্চারণঃ</strong>
                {{ data.sehri_dua.bn_translation }}
              </p>
              <p>
                <strong>অর্থঃ</strong>
                {{ data.sehri_dua.bn_meaning }}
              </p>
              <p>
                <strong>বাংলা দোয়াঃ</strong>
                {{ data.sehri_dua.bn_niyot }}
              </p>
              <p>
                <strong>English Dua:</strong>
                {{ data.sehri_dua.en_niyot }}
              </p>
            </div>
            <hr>
            <div class="text-center text-wrap mt-5">
              <h3 class="headline font-weight-bold">
                Iftar Dua (ইফতারের দু'আ)
              </h3>
              <p>
                {{ data.iftar_dua.arabic }}
              </p>
              <p>
                <strong>উচ্চারণঃ</strong>
                {{ data.iftar_dua.bn_translation }}
              </p>
              <p>
                <strong>অর্থঃ</strong>
                {{ data.iftar_dua.bn_meaning }}
              </p>
              <p>
                <strong>বাংলা দোয়াঃ</strong>
                {{ data.iftar_dua.bn_niyot }}
              </p>
              <p>
                <strong>English Dua:</strong>
                {{ data.iftar_dua.en_niyot }}
              </p>
            </div>
          </v-card-title>
          <hr>
          <v-card-text>
            <v-table fixed-header class="text-center text-wrap">
              <thead>
              <tr>
                <th class="text-center font-weight-black">
                  Ramadan (রমজান)
                </th>
                <th class="text-center">
                  Date (তারিখ)
                </th>
                <th class="text-center">
                  Day (দিন)
                </th>
                <th class="text-center">
                  Sehri Last Time (সাহরির শেষ সময়)
                </th>
                <th class="text-center">
                  Iftar Time (ইফতারের সময়)
                </th>
              </tr>
              </thead>
              <tbody>
              <tr v-for="(calendar, index) in data.calendars" :key="index">
                <td>
                  {{ index + 1 }}
                </td>
                <td>
                  {{ date_format(calendar.date) }}
                </td>
                <td>
                  {{ day_format(calendar.date) }}
                </td>
                <td>
                  {{ time_format(calendar.date + ' ' + calendar.sehri_time) }}
                </td>
                <td>
                  {{ time_format(calendar.date + ' ' + calendar.iftar_time) }}
                </td>
              </tr>
              </tbody>
            </v-table>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup>
import {onMounted, reactive} from "vue";
import dayjs from "dayjs";

const date_format = (date) => {
  return dayjs(date, 'YYYY-MM-DD', 'bn').format('DD/MM/YYYY')
}

const day_format = (date) => {
  return dayjs(date, 'YYYY-MM-DD').format('dddd')
}

const time_format = (time) => {
  return dayjs(time, 'HH:mm:ss').format('hh:mm A')
}

const data = reactive({
  dataLoaded: false,
  calendars : [],
  sehri_dua : null,
  iftar_dua : null,
})

const fetchCalendars = async () => {
  const response = await fetch('https://gist.githubusercontent.com/mishajib/a45181a514c33f7040e027b2effb7c17/raw/fdacb774c016555bb4365151a6ae4d1d0aba3082/ramandan_calendar_2023.json')
  const calendar = await response.json();
  data.calendars = calendar.calendars;
  data.sehri_dua = calendar.sehri_dua;
  data.iftar_dua = calendar.iftar_dua;
}

onMounted(async () => {
  await fetchCalendars();
  data.dataLoaded = true;
});
</script>