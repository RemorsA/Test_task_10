<template>
    <div v-if="tableLoad" class="contaiener">

        <div class="header--container">
            <p class="header--title">Добавление товара</p>
            <div class="header--nav-container">
                <button class="header--nav-btn" @click="sortOpen">По умолчанию</button>
                <div v-show="sort_mod" class="header--nav-container-btn">
                    <p class="header--navigation-btn" @click="sort('min')">По цене min (от меньшего к большему).</p>
                    <p class="header--navigation-btn" @click="sort('max')">По цене max (от большего к меньшему).</p>
                    <p class="header--navigation-btn" @click="sort('name')">По наименованию.</p>
                </div>
            </div>
        </div>

        <div class="body--container">
            <div class="form--container">
                <div class="form--input">
                    <p>Наименование товара</p>
                    <input
                        v-model="title"
                        type="text"
                        placeholder="Введите наименование товара"
                        :style="valid_styles.title.t"
                    >
                    <p :style="valid_styles.title.p">Поле является обязательным</p>
                </div>
                <p>Описание товара</p>
                <textarea
                    v-model="about"
                    placeholder="Введите описание товара"
                ></textarea>
                <div class="form--input">
                    <p>Ссылка на изображение товара</p>
                    <input
                        v-model="img"
                        type="text"
                        placeholder="Введите ссылку"
                        :style="valid_styles.img.t"
                    >
                    <p :style="valid_styles.img.p">Поле является обязательным</p>
                </div>
                <div class="form--input">
                    <p>Цена товара</p>
                    <input
                        v-model="price"
                        type="number"
                        placeholder="Введите цену"
                        :style="valid_styles.price"
                    >
                    <p :style="valid_styles.price.p">Поле является обязательным</p>
                </div>
                <button
                    v-if="title === '' || img === '' || price === ''"
                    class="form--btn"
                    :disabled='true'
                >
                    Добавить товар
                </button>
                <button
                    v-else
                    type="submit"
                    class="form--btn btn-ch"
                    @click="addCard"
                >
                    Добавить товар
                </button>
            </div>

            <div class="cards--container">
                <div
                    v-for="(i, index) in data_cards"
                    :key="i.id"
                    class="card--container"
                    @mouseenter='showDeleteBtn(index)'
                    @mouseleave ='showDeleteBtn(index)'
                >
                    <img 
                        src="@/assets/delete-btn.svg"
                        class="card_delete"
                        @click="deletCard(index)"
                    >
                    <img :src="i.img" class="card--img">
                    <div class="card--data">
                        <p>{{i.title}}</p>
                        <p>{{i.about}}</p>
                        <p>{{i.price}} руб.</p> 
                    </div>
                </div>
            </div>
        </div>

        <div v-show="alert" class="succes_add">
            <p>Товар успешно добавлен</p>
        </div>

    </div>

    <div v-else class="spin"></div>
</template>

<script>
export default {
    name: 'IndexPage',
    data() {
        return {
            style_inp: {
                display: {
                    'display': 'block',
                    'color': '#FF8484',
                    'padding-top': '0px',
                    'position': 'absolute',
                    'bottom': '0',
                },
                display_none: {'display': 'none'},
                border: {'border': '1px solid #FF8484'},
                border_none: {'border': 'none'}
            },
            img: '',
            title: '',
            about: '',
            price: '',
            data_cards: [],
            valid_styles: {
                img: { t: {}, p: {'display': 'none'} },
                title: { t: {}, p: {'display': 'none'} },
                price: { t: {}, p: {'display': 'none'} },
            },
            btn: {
                style: {},
                disabled: false
            },
            alert: false,
            sort_mod: false,
            tableLoad: false,
            show_delete: null
        }
    },
    watch: {
        title(val) {
            this.valid_styles.title.t = val === '' ? this.style_inp.border : this.style_inp.border_none
            this.valid_styles.title.p = val === '' ? this.style_inp.display : this.style_inp.display_none
        },
        img(val) {
            this.valid_styles.img.t = val === '' ? this.style_inp.border : this.style_inp.border_none
            this.valid_styles.img.p = val === '' ? this.style_inp.display : this.style_inp.display_none

        },
        price(val) {
            this.valid_styles.price = val === '' ? this.style_inp.border : this.style_inp.border_none
            this.valid_styles.price.p = val === '' ? this.style_inp.display : this.style_inp.display_none 
        },
    },
    mounted() {
        console.clear()
        const parse = JSON.parse(localStorage.getItem('cards'))
        if (parse !== null) {
            this.data_cards = parse
        }
        setTimeout(() => {
            this.tableLoad = true
        }, 1000)
        
    },
    methods: {
        addCard() {
            const data = {
                id: Math.floor(Math.random() * 9999),
                img: this.img,
                title: this.title,
                about: this.about,
                price: this.price,
            }

            if (!data.img.match(/https?:\/\/(?:[-\w]+\.)?([-\w]+)\.\w+(?:\.\w+)?\/?.*/i)) {
                data.img = 'https://www.kerstinbruns.es/wp-content/themes/realestate-7/images/no-image.png'
            }
            data.price = data.price.replace(/\d{1,3}(?=(\d{3})+(?!\d))/g, '$& ')

            this.data_cards.push(data)
            localStorage.setItem('cards', JSON.stringify(this.data_cards))

            this.alert = true
            setTimeout(() => {
                this.alert = false
            }, 3000)
        },
        
        deletCard(index) {
            this.data_cards.splice(index, 1)
            localStorage.setItem('cards', JSON.stringify(this.data_cards))
        },
        sortOpen() {
            if (this.sort_mod === false) this.sort_mod = true
            else this.sort_mod = false
        },
        sort(mod) {
            this.tableLoad = false
            if (mod === 'min') {
                this.data_cards = this.data_cards.sort((a, b) => a.price > b.price ? 1 : -1)
            }
            if (mod === 'max') {
                this.data_cards = this.data_cards.sort((a, b) => a.price < b.price ? 1 : -1)
            }
            if (mod === 'name') {
                this.data_cards = this.data_cards.sort((a, b) => a.title.toLowerCase() > b.title.toLowerCase() ? 1 : -1)
            }
            setTimeout(() => {
                this.tableLoad = true
            }, 1000)
        },
        showDeleteBtn(val) {
            console.log(val)
            if (this.showDelete) this.showDelete = false
            else this.showDelete = val
            return val
            // console.warn(this.showDelete)
        }
    }
}
</script>

<style> * { box-sizing: border-box; margin: 0; padding: 0;} </style>
<style lang="sass" scoped>
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro&display=swap')

$graphite: #3F3F3F
$white: #FFFEFB
$gray: #B4B4B4
$pink: #ff8484 

$boxshadowheader: 0px 2px 5px rgba(0, 0, 0, 0.1)
$fontfamily: 'Source Sans Pro'

.contaiener
    max-width: 1440px
    margin: auto
    font-family: $fontfamily
    color: $graphite
    transition: .5s
    margin-top: 32px

    .header--container
        display: flex
        justify-content: space-between
        align-items: center
        margin-bottom: 16px 
        .header--title
            font-weight: 600
            font-size: 28px
            line-height: 35px
            font-style: normal
            font-weight: 600
            font-size: 28px
            line-height: 35px
        .header--nav-container
            position: relative
            .header--nav-btn
                background: $white
                box-shadow: $boxshadowheader
                border-radius: 4px
                border: none
                width: 121.49px
                height: 36px
                padding: 10px 0px 11px 0px
                color: $gray
                cursor: pointer
                font-family: $fontfamily
            .header--nav-btn:hover
                border-bottom: 1px solid $pink
            .header--nav-btn::after
                content: url('@/assets/arrow-d.svg')
                padding-left: 5px
            .header--nav-container-btn
                position: absolute
                width: 320px
                background: $white
                box-shadow: $boxshadowheader
                border-radius: 4px
                left: -200px
                top: 50px
                padding: 10px
                z-index: 2
                .header--navigation-btn
                    cursor: pointer
                .header--navigation-btn:hover::before
                    content: ' - '
                    color: $pink
                    cursor: pointer

    .body--container
        display: flex
        .form--container
            background: $white
            box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02)
            border-radius: 4px
            padding: 24px
            // max-height: 440px
            max-height: 460px
            min-width: 332px
            .form--input
                position: relative
        .cards--container
            display: flex
            flex-wrap: wrap
            .card--container
                background: #FFFEFB
                box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02)
                border-radius: 4px
                min-width: 332px
                height: 423px
                position: relative
                margin: 0px 0px 16px 16px
                .card_delete
                    position: absolute
                    cursor: pointer
                    top: -10px
                    right: -20px
                    z-index: 1
                .card--img
                    width: 332px
                    height: 200px
                    border-radius: 4px 4px 0px 0px
                .card--data
                    padding: 16px
                .card--data p:nth-child(1)
                    font-style: normal
                    font-weight: 600
                    font-size: 20px
                    line-height: 25px
                    color: #3F3F3F
                .card--data p:nth-child(2)
                    margin-top: 16px
                    font-style: normal
                    font-weight: 400
                    font-size: 16px
                    line-height: 20px
                    color: #3F3F3F
                .card--data p:nth-child(3)
                    position: absolute
                    bottom: 16px
                    font-style: normal
                    font-weight: 600
                    font-size: 24px
                    line-height: 30px
                    color: #3F3F3F
        .form--btn
            transition: .5s
            width: 284px
            height: 36px
            border-radius: 10px
            background: #EEEEEE
            border: none
            cursor: pointer
        .btn-ch
            color: #FFFFFF
            transition: .5s
            background: #7BAE73

input
    border: none
    background: #FFFEFB
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1)
    border-radius: 4px
    width: 100%
    height: 36px
    padding-left: 16px
    color: #3F3F3F
    outline: none
    font-family: $fontfamily
    font-weight: 400
    font-size: 10px
    line-height: 13px
    margin: 4px 0px 16px 0px
input::-webkit-input-placeholder
    color: $gray
input[type="number"]::-webkit-inner-spin-button 
    -webkit-appearance: none

textarea
    border: none
    background: #FFFEFB
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1)
    border-radius: 4px
    width: 100%
    height: 108px
    padding: 10px 0px 0px 16px
    color: #3F3F3F
    outline: none
    font-family: $fontfamily
    font-weight: 400
    font-size: 10px
    line-height: 13px
    margin: 4px 0px 16px 0px
    resize: none
textarea::-webkit-input-placeholder
    color: $gray

.succes_add
    width: 300px
    height: 40px
    background: #7BAE73
    border-radius: 20px
    position: absolute
    top: 20px
    right: 50%
    left: 50%
    transition: .5s
.succes_add p
    padding: 7px 0px 5px 0px
    text-align: center
    color: #FFFFFF

.spin
    position: absolute
    left: 50%
    right: 50%
    top: 40%
    bottom: 50%
    width: 100px
    height: 100px
    border-radius: 50%
    border: 5px solid #f3f3f3
    border-top: 5px solid #ff8484
    animation-name: spin
    animation-duration: 1000ms
    animation-iteration-count: infinite
    animation-timing-function: linear

=keyframes($spin)
    @-webkit-keyframes #{$spin}
        @content
    @-moz-keyframes #{$spin}
        @content
    @-ms-keyframes #{$spin}
        @content
    @keyframes #{$spin}
        @content
+keyframes(spin)
    0%
        transform: rotate(0deg)
    100%
        transform: rotate(360deg)

@media screen and (max-width: 425px)
    .body--container
        display: block !important
        width: 100%
        .form--container
            width: 100%
        .cards--container
            margin-top: 40px
            .card--container
                width: 100%
                .card--img
                    min-width: 100%
</style>