<template>
<v-main>
  <v-container>
    <v-row>
      <v-col cols="12">
        <div class="text-center text-h6">総合評価</div>
      </v-col>
    </v-row>
    <v-row no-gutters class="justify-center pb-8">
      <v-col cols="10">
        <div class="text-center text-h1 font-weight-black" :style="`color:${grade.color};`">{{ grade.grade }}</div>
        <div class="text-center text-h5 font-weight-black">{{ grade.note }}</div>
      </v-col>
    </v-row>
    <v-row class="justify-center pb-16">
      <v-col cols="auto">
        <v-btn
          :href="`https://ewbolp.create-o-life.com/?checked=%E3%81%94%E8%B3%AA%E5%95%8F&selected=WEB%E3%82%B5%E3%82%A4%E3%83%88%E8%A8%BA%E6%96%AD&content=${$route.query.url}%E8%A9%B3%E7%B4%B0%E8%A9%95%E4%BE%A1%E7%A2%BA%E8%AA%8D#contact`"
          :color="color.btn.bg"
          :style="`color:${color.btn.txt};`"
          block
          large
        >
          詳しく知りたい
        </v-btn>
      </v-col>
      <v-col cols="auto">
        <v-btn
          to="/#assessment"
          block
          large
          nuxt
        >
          戻る
        </v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <div class="text-center text-h6">項目別評価</div>
      </v-col>
    </v-row>
    <template v-for="(report, i) in reports">
    <v-row class="justify-center pb-12">
      <v-col cols="12">
        <div v-if="i == 0" class="text-center text-subtitle-2">スマホ</div>
        <div v-if="i == 1" class="text-center text-subtitle-2">パソコン</div>
      </v-col>
      <template v-for="item in report">
      <v-col cols="auto">
        <div :style="($vuetify.breakpoint.xs)? 'width:72px' : 'width:120px'">
          <div style="position:relative;" class="d-flex justify-center">
            <Doughnut
              :data="bg(bgColor(item), 0)"
              :class="$style.doughnut"
            />
            <Doughnut
              :data="main(item, mainColor(item), 85)"
              :class="$style.doughnut" 
              style="position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);"
            />
            <h2
              :style="`position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);color:${mainColor(item)};`"
            >
              {{ Math.floor(item.score * 100) }}
            </h2>
          </div>
          <div class="text-center text-caption mt-2" :style="($vuetify.breakpoint.xs)? 'height:40px' : undefined">{{ item.label }}</div>
        </div>
      </v-col>
      </template>
    </v-row>
    </template>
    <v-row class="justify-center pb-16">
      <v-col cols="auto">
        <v-btn
          :href="`https://ewbolp.create-o-life.com/?checked=%E3%81%94%E8%B3%AA%E5%95%8F&selected=WEB%E3%82%B5%E3%82%A4%E3%83%88%E8%A8%BA%E6%96%AD&content=${$route.query.url}%E8%A9%B3%E7%B4%B0%E8%A9%95%E4%BE%A1%E7%A2%BA%E8%AA%8D#contact`"
          :color="color.btn.bg"
          :style="`color:${color.btn.txt};`"
          block
          large
        >
          詳しく知りたい
        </v-btn>
      </v-col>
      <v-col cols="auto">
        <v-btn
          to="/#assessment"
          block
          large
          nuxt
        >
          戻る
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</v-main>
</template>

<script>
import Doughnut from '~/components/doughnut.vue'

export default {
  components: {
    Doughnut
  },
  async asyncData ({app, route}) {
    const urls = [
      `https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=${route.query.url}&key=AIzaSyC0Hq7VAqbWrv1f3eSeikZv430He1N2qbc&category=accessibility&category=best-practices&category=performance&category=seo&category=pwa&strategy=mobile`,
      `https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=${route.query.url}&key=AIzaSyC0Hq7VAqbWrv1f3eSeikZv430He1N2qbc&category=accessibility&category=best-practices&category=performance&category=seo&category=pwa&strategy=desktop`
    ]
    const res = await Promise.all(urls.map(app.$axios.$get))
    var report = ''
    const reports = res.map(x => {
      report = x.lighthouseResult.categories
      report.performance.label = 'パフォーマンス'
      report.accessibility.label = 'アクセシビリティ'
      report['best-practices'].label = 'ベストプラクティス'
      report.seo.label = 'SEO'
      report.pwa.label = 'PWA'
      return report
    })
    return  {reports}
  },
  data() {
    return {
      color: {
          btn: {bg: process.env.colorBtnBg, txt: process.env.colorBtnTxt}
      }
    }
  },
  computed: {
    main () {
      return function (item, color, percentage) {
        return {
          chartData: {
            datasets: [
              {
                data: [item.score, 1 - item.score],
                backgroundColor: [color, 'transparent'],
                borderWidth: 0
              }
            ]
          },
          options: {
            maintainAspectRatio: false,
            legend: {
              display: false
            },
            tooltips: {
              enabled: false
            },
            cutoutPercentage: percentage
          }
        }
      }
    },
    bg () {
      return function (color, percentage) {
        return {
          chartData: {
            datasets: [
              {
                data: [1],
                backgroundColor: [color],
                borderWidth: 0
              }
            ]
          },
          options: {
            maintainAspectRatio: false,
            legend: {
              display: false
            },
            tooltips: {
              enabled: false
            },
            cutoutPercentage: percentage
          }
        }
      }
    },
    mainColor () {
      return function(item) {
        const score = item.score
        const colorList = [
          {score: .49, color: 'rgba(255, 78, 66, 1)'},
          {score: .89, color: 'rgba(255, 164, 0, 1)'},
          {score: 1, color: 'rgba(12, 206, 107, 1)'}
        ]
        const color = score => {
          return colorList.find(x => score <= x.score).color
        }
        return color(score)
      }
    },
    bgColor () {
      return function(item) {
        const score = item.score
        const colorList = [
          {score: .49, color: 'rgba(255, 78, 66, .1)'},
          {score: .89, color: 'rgba(255, 164, 0, .1)'},
          {score: 1, color: 'rgba(12, 206, 107, .1)'}
        ]
        const color = score => {
          return colorList.find(x => score <= x.score).color
        }
        return color(score)
      }
    },
    grade () {
      const sum = report => {
        return report.performance.score
          + report.accessibility.score
          + report['best-practices'].score
          + report.seo.score
          + report.pwa.score
      }
      const score = sum(this.reports[0]) + sum(this.reports[1])
      const colorList = [
        {score: 7.99, color: 'rgba(255, 78, 66, 1)', grade: 'C', note: '集客に大きな悪影響の恐れ'},
        {score: 8.99, color: 'rgba(255, 164, 0, 1)', grade: 'B', note: '集客に悪影響の恐れ'},
        {score: 10, color: 'rgba(12, 206, 107, 1)', grade: 'A', note: '訪問者にとって快適'}
      ]
      const color = score => {
        return {...colorList.find(x => score <= x.score), sum:Math.floor(score * 100)}
      }
      return color(score)
    }
  }
}
</script>
<style module>
  .doughnut {
    height:72px;
    width:72px;
  }
</style>
