<template>
    <div>
    <v-app-bar
        fixed
        :color="(!scroll)? 'transparent' : layout.header.bgColor"
        :flat="!scroll"
    >
        <v-toolbar-items>
            <v-btn
                text
                class="px-0"
                aria-label="to-top"
                @click="$vuetify.goTo(0)"
            >
                <v-img
                    v-if="layout.header.logo"
                    :src="`${layout.header.logo.url}?fit=clip&h=56`"
                    alt=""
                ></v-img>
              
                <v-toolbar-title
                    v-if="layout.header.title"
                    :style="`color: ${layout.header.txtColor};text-transform:none;`"
                >
                    {{ layout.header.title }}
                </v-toolbar-title>
            </v-btn>
        </v-toolbar-items>
      
        <v-spacer></v-spacer>
        
        <v-menu
            v-if="$vuetify.breakpoint.xs"
            right
            nudge-right
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              :color="layout.header.txtColor"
              aria-label="menu"
              v-bind="attrs"
              v-on="on"
              icon
            >
                <v-icon large>mdi-menu</v-icon>
            </v-btn>
          </template>
          <v-list>
            <v-list-item
              v-for="menu in menus"
                :key="menu.section.id"
                @click="move(menu.section.id)"
            >
              <v-list-item-title>{{ menu.section.title }}</v-list-item-title>
            </v-list-item>
            <v-list-item
                @click="move('https://contact.create-o-life.com/')"
            >
              <v-list-item-title>お問い合わせ</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
        
        <v-toolbar-items v-else>
            <template v-for="menu in menus">
            <v-btn
                @click="move(menu.section.id)"
                color="transparent"
                :small="$vuetify.breakpoint.sm"
                :style="`color: ${layout.header.txtColor};`"
                depressed
            >
                <span>{{ menu.section.title }}</span>
            </v-btn>
            </template>
            <div class="d-flex">
                <v-btn
                    @click="move('https://contact.create-o-life.com/')"
                    :color="color.header.contact"
                    :small="$vuetify.breakpoint.sm"
                    class="align-self-center"
                    rounded
                    outlined
                    nuxt
                >
                    お問い合わせ
                </v-btn>
            </div>
        </v-toolbar-items>
    </v-app-bar>

    <v-main v-scroll="onScroll">
        <template v-for="(section, i) in sections">
            <Section :section="section.section"></Section>
        </template>
    </v-main>
    
    <v-footer padless :color="layout.footer.bgColor">
        <v-sheet tile width="100%" color="transparent">
            <v-row class="mx-0 justify-center">
                <v-col
                    cols="auto"
                    class="caption"
                    :style="`color: ${layout.footer.txtColor};`"
                >
                    {{ layout.footer.copyright }}
                </v-col>
            </v-row>
        </v-sheet>
    </v-footer>
    </div>
</template>

<script>
import Section from '~/components/section.vue';

export default {
    components: {
        Section
    },
    async asyncData ({ payload, app, $config }) {
        if(payload) {
            return {
                sections: payload.sections,
                menus: payload.menus,
                layout: payload.layout
            }
        }
        else if(process.env.NODE_ENV !== 'production') {
            var layout = await app.$axios.$get(`https://${process.env.serviceId}.microcms.io/api/v1/layout/layout`, {
                headers: { 'X-API-KEY': $config.apiKey }
            })
            var sections = layout.layout
            layout = {
                header: {
                    title: layout.title,
                    logo: layout.logo,
                    bgColor: (layout.bgColorH)? layout.bgColorH : 'blue',
                    txtColor: (layout.txtColorH)? layout.txtColorH : 'white'
                },
                footer: {
                    copyright: layout.copyright,
                    bgColor: (layout.bgColorF)? layout.bgColorF : 'blue',
                    txtColor: (layout.txtColorF)? layout.txtColorF : 'white'
                }
            }
            var menus = sections.filter(x => x.menu)
            return {
                    sections: sections,
                    menus: menus,
                    layout: layout
            }
        }
    },
    data() {
        return {
            scroll: false,
            color: {
                header: {
                    contact: process.env.colorContactBtn,
                }
            }
        }
    },
    methods: {
        onScroll (e) {
            var top = window.pageYOffset ||e.target.scrollTop || 0
            if(top >= window.innerHeight) {
                this.scroll = true
            }
            else {
                this.scroll = false
            }
        },
        move (id) {
            if (id.includes('http')) {
                window.location = id
            }
            else {
                return this.$vuetify.goTo(`#${id}`, this.option)
            }
        }
    },
    computed: {
        option () {
            if (this.$vuetify.breakpoint.mdAndUp) {
                return {offset: 64}
            }
            else {
                return {offset: 56}
            }
        }
    },
    head () {
        return {
            title: process.env.topTitle,
            titleTemplate: (process.env.topTemplate != '')? process.env.topTemplate : `%s - ${process.env.siteName}`,
        }
    }
}
</script>