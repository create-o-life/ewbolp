<template>
    <v-sheet
        :color="(section.bgColor)? section.bgColor : color.section.bg"
        tile
        width="100%"
        class="d-flex justify-center"
    >
    <v-img
        :src="(bgImg)? bgImg : undefined"
        :height="(section.max)? '100vh' : undefined"
        :style="(bgImg)? 'background: white;' : undefined"
        :max-width="($vuetify.breakpoint.lgAndUp && !section.max)? '1264px' : '100vw'"
    >
        <v-sheet
            :color="(bgImg && section.bgColor)? section.bgColor : 'transparent'"
            tile
            height="100%"
        >
            <v-container :class="(!$route.params.id)? 'py-10' : 'pt-2 pb-10 px-sm-0'" style="height:100%;min-height:50%;">
                <v-row :id="section.id" style="height:100%;" class="justify-center align-center">
                    <v-col
                        v-if="section.contents"
                        v-html="section.contents"
                        cols="12"
                        sm="10"
                    ></v-col>
                    <template v-if="section.item">
                        <template v-if="section.slide">
                            <v-col cols="12" sm="8" md="6">
                                <v-carousel
                                    cycle
                                    height="auto"
                                    show-arrows-on-hover
                                    hide-delimiter-background
                                >
                                    <v-carousel-item
                                        v-for="(item,i) in section.item"
                                        :key="i"
                                    >
                                        <v-card
                                            color="transparent"
                                            class="ma-2"
                                            :flat="!item.card"
                                            :tile="!item.card"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    v-if="item.contents"
                                                    v-html="item.contents"
                                                    :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                    width="100%"
                                                    height="100%"
                                                    class="pa-10"
                                                ></v-sheet>
                                            </v-img>
                                        </v-card>
                                    </v-carousel-item>
                                </v-carousel>
                            </v-col>
                        </template>
                        <template v-else>
                            <template v-for="item in section.item">
                                <v-col
                                    v-if="!item.expand"
                                    cols="12"
                                    :sm="(section.col)? 6 : 6"
                                    :md="(section.col || section.max)? 4 : 6"
                                    :class="(item.card)? 'align-self-stretch px-4 px-sm-3' : 'align-self-stretch px-4 px-sm-0 py-0'"
                                >
                                    <template v-if="item.card">
                                        <v-card
                                            color="transparent"
                                            height="100%"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                    width="100%"
                                                    height="100%"
                                                    :class="(section.col)? 'pa-5 pa-sm-7 pa-md-5' : 'pa-5 pa-sm-7'"
                                                >
                                                    <div
                                                        v-if="item.contents"
                                                        v-html="item.contents"
                                                    ></div>
                                                    <v-row
                                                        v-if="item.btnLabel && item.btnLink"
                                                        class="py-2 justify-center"
                                                    >
                                                        <v-btn
                                                            @click="move(item.btnLink)"
                                                            :color="color.btn.bg"
                                                            class="title"
                                                            outlined
                                                            large
                                                        >
                                                            {{ item.btnLabel }}
                                                        </v-btn>
                                                    </v-row>
                                                </v-sheet>
                                            </v-img>
                                        </v-card>
                                    </template>
                                    <template v-else>
                                        <v-img
                                            :src="(item.bgImg)? item.bgImg.url : undefined"
                                            width="100%"
                                            height="100%"
                                        >
                                            <v-sheet
                                                :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                height="100%"
                                                width="100%"
                                                class="d-flex align-items-center"
                                                tile
                                            >
                                                <v-card
                                                    color="transparent"
                                                    :class="(section.col)? 'ma-auto pa-5 pa-sm-7 pa-md-5 d-flex align-center' : 'ma-auto pa-5 pa-sm-7 d-flex align-center'"
                                                    :height="($vuetify.breakpoint.smAndUp && !section.max)? '100%' : undefined"
                                                    width="100%"
                                                    flat
                                                    tile
                                                >
                                                    <div
                                                        v-if="item.contents"
                                                        v-html="item.contents"
                                                        class="flex-grow-1"
                                                    ></div>
                                                    <v-row
                                                        v-if="item.btnLabel && item.btnLink"
                                                        class="py-2 justify-center"
                                                    >
                                                        <v-btn
                                                            @click="move(item.btnLink)"
                                                            :color="color.btn.bg"
                                                            :style="`color:${color.btn.txt};`"
                                                            class="title"
                                                            large
                                                        >
                                                            {{ item.btnLabel }}
                                                        </v-btn>
                                                    </v-row>
                                                </v-card>
                                            </v-sheet>
                                        </v-img>
                                    </template>
                                </v-col>
                                <v-col
                                    v-else
                                    cols="12"
                                    sm="10"
                                    :class="(item.card)? 'align-self-stretch px-4 px-sm-3' : 'align-self-stretch px-4 px-sm-0 py-0'"
                                >
                                    <template v-if="item.card">
                                        <v-card
                                            color="transparent"
                                            height="100%"
                                        >
                                            <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                                <v-sheet
                                                    :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                    width="100%"
                                                    height="100%"
                                                    class="pa-10"
                                                >
                                                    <div
                                                        v-if="item.contents"
                                                        v-html="item.contents"
                                                    ></div>
                                                    <v-row
                                                        v-if="item.btnLabel && item.btnLink"
                                                        class="py-2 justify-center"
                                                    >
                                                        <v-btn
                                                            @click="move(item.btnLink)"
                                                            :color="color.btn.bg"
                                                            class="title"
                                                            outlined
                                                            large
                                                        >
                                                            {{ item.btnLabel }}
                                                        </v-btn>
                                                    </v-row>
                                                </v-sheet>
                                            </v-img>
                                        </v-card>
                                    </template>
                                    <template v-else>
                                        <v-img :src="(item.bgImg)? item.bgImg.url : undefined" width="100%" height="100%">
                                            <v-sheet
                                                :color="(item.bgColor)? item.bgColor : color.item.bg"
                                                height="100%"
                                                width="100%"
                                                class="d-flex"
                                                tile
                                            >
                                                <v-card
                                                    color="transparent"
                                                    class="ma-auto pa-3"
                                                    width="100%"
                                                    flat
                                                    tile
                                                >
                                                    <div
                                                        v-if="item.contents"
                                                        v-html="item.contents"
                                                    ></div>
                                                    <v-row
                                                        v-if="item.btnLabel && item.btnLink"
                                                        class="py-2 justify-center"
                                                    >
                                                        <v-btn
                                                            @click="move(item.btnLink)"
                                                            :color="color.btn.bg"
                                                            :style="`color:${color.btn.txt};`"
                                                            class="title"
                                                            large
                                                        >
                                                            {{ item.btnLabel }}
                                                        </v-btn>
                                                    </v-row>
                                                </v-card>
                                            </v-sheet>
                                        </v-img>
                                    </template>
                                </v-col>
                            </template>
                        </template>
                    </template>
                    <v-col v-if="section.movie" cols="12" sm="8" md="6">
                        <div style="width: 100%;padding-bottom: 56.25%;height: 0px;position: relative;">
                            <iframe
                                style="position: absolute;top: 50%;left: 50%;transform: translate(-50%, -50%);-webkit-transform: translate(-50%, -50%);-ms-transform: translate(-50%, -50%);"
                                width="100%"
                                height="100%"
                                title="Movie"
                                :src="`https://www.youtube.com/embed/${section.movie}`"
                                frameborder="0"
                                allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
                                allowfullscreen
                            ></iframe>
                        </div>
                    </v-col>
                    <v-col
                        v-if="section.btnLabel&&section.btnLink"
                        cols="12"
                        sm="10"
                        class="d-flex justify-center"
                    >
                        <v-btn
                            @click="move(section.btnLink)"
                            :color="color.btn.bg"
                            :style="`color:${color.btn.txt};`"
                            large
                        >
                            {{ section.btnLabel }}
                        </v-btn>
                    </v-col>
                    <v-col
                        v-if="section.contact"
                        cols="12"
                        sm="10"
                    >
                        <v-form
                            name="contact"
                            method="POST"
                            data-netlify="true"
                            netlify-honeypot="bot-field"
                            action="/"
                        >
                            <input type="hidden" name="form-name" value="contact">
                            <div v-if="selections != ''" class="d-flex">
                            <v-checkbox
                                v-for="selection in selections"
                                :key="selection"
                                v-model="checked"
                                :label="selection"
                                :value="selection"
                                name="checked[]"
                                class="pr-4"
                            ></v-checkbox>
                            </div>
                            <v-combobox
                                v-if="items != '' && comboLabel != ''"
                                v-model="selected"
                                :items="items"
                                :background-color="color.contact.input"
                                :label="comboLabel"
                                name="selected[]"
                                multiple
                                outlined
                            ></v-combobox>
                            <v-text-field
                                v-if="corporateLabel != ''"
                                v-model="corporateName"
                                :background-color="color.contact.input"
                                :label="corporateLabel"
                                name="corporate-name"
                                outlined
                            ></v-text-field>
                            <v-text-field
                                v-if="nameLabel != ''"
                                v-model="contactName"
                                :background-color="color.contact.input"
                                :label="nameLabel"
                                name="contact-name"
                                outlined
                            ></v-text-field>
                            <v-text-field
                                v-if="emailLabel != ''"
                                v-model="email"
                                :background-color="color.contact.input"
                                :label="emailLabel"
                                name="email"
                                outlined
                            ></v-text-field>
                            <v-textarea
                                v-if="contentLabel != ''"
                                v-model="content"
                                :background-color="color.contact.input"
                                :label="contentLabel"
                                name="content"
                                outlined
                            ></v-textarea>
                            <v-text-field
                                v-model="botField"
                                label="人間は入力しないでください"
                                name="bot-field"
                                v-show="false"
                            />
                            <v-btn
                                type="submit"
                                :color="color.btn.bg"
                                :style="`color:${color.btn.txt};`"
                                class="title"
                                large
                            >
                                送信
                            </v-btn>
                        </v-form>
                    </v-col>
                    <v-col
                      v-if="section.assessment"
                    >
                      <v-text-field
                        v-model="url"
                        label="サイトURL"
                      ></v-text-field>
                      <v-btn
                        :to="`/report?url=${url}`"
                        :color="color.btn.bg"
                        :style="`color:${color.btn.txt};`"
                        nuxt
                      >
                        確認する
                      </v-btn>
                    </v-col>
                </v-row>
            </v-container>
        </v-sheet>
    </v-img>
    </v-sheet>
</template>

<script>
export default {
    data() {
        return {
            color: {
                section: {bg: process.env.colorSectionBg},
                item: {bg: process.env.colorItemBg},
                btn: {bg: process.env.colorBtnBg, txt: process.env.colorBtnTxt},
                contact: {input: process.env.colorContactInput}
            },
            comboLabel: process.env.contactComboLabel,
            corporateLabel: process.env.contactCorporateLabel,
            nameLabel: process.env.contactNameLabel,
            emailLabel: process.env.contactEmailLabel,
            contentLabel: process.env.contactContentLabel,
            selected: [],
            checked: [],
            corporateName: '',
            contactName: '',
            email: '',
            content: '' ,
            botField: '',
            url: ''
        }
    },
    methods: {
        move (link) {
            if (link.includes('http')) {
                return window.open(link, '_blank')
            }
            else if (link.includes('#')) {
                return this.$vuetify.goTo(link, this.option)
            }
            else {
                return this.$router.push(link)
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
        },
        bgImg () {
            if (this.section.bgImg) {
                var bgImg = this.section.bgImg.url
                if (this.$vuetify.breakpoint.xs) {
                    if (this.section.bgImgS) {
                        bgImg = this.section.bgImgS.url
                    }
                    return `${bgImg}?auto=compress&fit=clip&w=600`
                }
                else if (this.$vuetify.breakpoint.sm) {
                    return `${bgImg}?auto=compress&fit=clip&w=960`
                }
                else {
                    return `${bgImg}?auto=compress&fit=clip&w=1264`
                }
            }
        },
        selections () {
            return process.env.contactSelections.split(',')
        },
        items () {
            return process.env.contactComboItems.split(',')
        }
    },
    props: ['section']
}
</script>