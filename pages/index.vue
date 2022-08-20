<template>
    <div class="main--contaiener">
        <div class="header--title_nav">
            <p class="header--title">Добавление товара</p>
            <button class="header--btn">По умолчанию<img src="@/assets/arrow-d.svg" alt=""></button>
        </div>

        <div class="form_cards--container">
            <div class="form--container">
                <form action="">
                    <div class="form--inp">
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

                    <div class="form--inp">
                        <p>Ссылка на изображение товара</p>
                        <input
                            v-model="img"
                            type="text"
                            placeholder="Введите ссылку"
                            :style="valid_styles.img.t"
                        >
                        <p :style="valid_styles.img.p">Поле является обязательным</p>
                    </div>

                    <div class="form--inp">
                        <p>Цена товара</p>
                        <input
                            v-model="price"
                            type="number"
                            placeholder="Введите цену"
                            :style="valid_styles.price"
                        >
                        <p :style="valid_styles.price.p">Поле является обязательным</p>
                    </div>
                </form>
                <button
                    v-if="title === '' || img === '' || price === ''"
                    :disabled='true'
                    style="transition: .5s;"
                >
                    Добавить товар
                </button>
                <button
                    v-else
                    type="submit"
                    style="
                        color: #FFFFFF;
                        transition: .5s;
                        background: #7BAE73;
                    "
                    @click="addCard"
                >
                    Добавить товар
                </button>
            </div>

            <div class="cards--container-main">
                <div 
                    v-for="(i, index) in data_cards"
                    :key="i.id"
                    class="cards--container"
                >
                    <img 
                        src="@/assets/delete-btn.svg" 
                        class="card_delete"
                        @click="deletCard(index)"
                    >
                    <div class="card">
                        <div class="card--img">
                            <img :src="i.img">
                        </div>
                        <div class="card--data">
                            <p>{{i.title}}</p>
                            <p>{{i.about}}</p>
                            <p>{{i.price}} руб.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div v-if="alert === true" class="succes_add">
            <p>Товар успешно добавлен</p>
        </div>
    </div>
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
            alert: false
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
            // https://grandgames.net/img/upload/fee2a439e342333522a1ccd30f74be63.jpg
        },
        deletCard(index) {
            this.data_cards.splice(index, 1)
            localStorage.setItem('cards', JSON.stringify(this.data_cards))
        }
    }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro&display=swap');

$graphite: #3F3F3F;
$white: #FFFEFB;
$gray: #B4B4B4;

$font-f-ssp: 'Source Sans Pro';

* {
    box-sizing: border-box;
    padding: 0%;
    margin: 0%;
}
body {
    font-family:$font-f-ssp;
    background: #E5E5E5;
}
.main--contaiener {
    max-width: 1400px;
    margin: auto;
    margin-top: 32px;

    .header--title_nav {
        display: flex;
        justify-content: space-between;

        .header--title {
            font-weight: 600;
            font-size: 28px;
            line-height: 35px;
            color: $graphite;
        }

        .header--btn {
            background: $white;
            box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
            border-radius: 4px;
            width: 121.49px;
            height: 36px;
            border: none;
            cursor: pointer;
            font-family: $font-f-ssp;
            font-weight: 400;
            font-size: 12px;
            line-height: 15px;
            color: $gray;
        }
        .header--btn img { padding-left: 5px; }
    }

    .form_cards--container {
        display: flex;
        margin-top: 16px;

        .form--container {
            background: $white;
            box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
            border-radius: 4px;
            padding: 24px;
            max-height: 440px;
        }
        .form--container button {
            width: 100%;
            background: #EEEEEE;
            border-radius: 10px;
            border: none;
            padding: 10px 0px 11px 0px;
            color: #B4B4B4;
            cursor: pointer;
            font-weight: 600;
            font-size: 12px;
            line-height: 15px;
        }

        form p {
            text-align: start;
            color: #49485E;
            margin-block: 4px;
            font-family: $font-f-ssp;
            font-weight: 400;
            font-size: 10px;
            line-height: 13px;
            letter-spacing: -0.02em;
            font-style: normal;
            position: relative;
        }
        form p:nth-child(1)::after,
        form p:nth-child(5)::after,
        form p:nth-child(7)::after {
            content: url('@/assets/required.svg');
            position: absolute;
            bottom: 4px;
            margin-left: 1px;
        }

        .form--inp {
            position: relative;
        }

        input {
            border: none;
            background: #FFFEFB;
            box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
            border-radius: 4px;
            width: 284px;
            height: 36px;
            padding-left: 16px;
            color: #3F3F3F;
            outline: none;
            font-family: $font-f-ssp;
            font-weight: 400;
            font-size: 10px;
            line-height: 13px;
            margin-bottom: 16px;
        }
        input::-webkit-input-placeholder { color: $gray; }
        input[type="number"]::-webkit-inner-spin-button { -webkit-appearance: none; }

        textarea {
            border: none;
            background: #FFFEFB;
            box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
            border-radius: 4px;
            width: 284px;
            height: 108px;
            padding: 10px 0px 0px 16px;
            color: #3F3F3F;
            outline: none;
            font-family: $font-f-ssp;
            font-weight: 400;
            font-size: 10px;
            line-height: 13px;
            margin-bottom: 16px;
            resize: none;
        }
        textarea::-webkit-input-placeholder { color: $gray; }

        .cards--container-main {
            display: flex;
            flex-wrap: wrap;
        }
        .cards--container {
            position: relative;
        }
        .card {
            background: #FFFEFB;
            box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
            border-radius: 4px;
            width: 332px;
            height: 423px;
            position: relative;
            margin: 0px 0px 16px 16px;
        }
        .card_delete {
            position: absolute;
            top: -10px;
            right: -20px;
            z-index: 1;
            cursor: pointer;
        }
        .card--img {
            width: 332px;
            height: 200px;
        }
        .card img {
            width: 100%;
            height: 100%;
            border-radius: 4px 4px 0px 0px;
        }
        .card--data {
            padding: 16px;
        }
        .card--data p:nth-child(1) {
            font-style: normal;
            font-weight: 600;
            font-size: 20px;
            line-height: 25px;
            color: #3F3F3F;
        }
        .card--data p:nth-child(2) {
            margin-top: 16px;
            font-style: normal;
            font-weight: 400;
            font-size: 16px;
            line-height: 20px;
            color: #3F3F3F;
        }
        .card--data p:nth-child(3) {
            position: absolute;
            bottom: 16px;
            font-style: normal;
            font-weight: 600;
            font-size: 24px;
            line-height: 30px;
            color: #3F3F3F;
        }
    }

    .succes_add {
        width: 300px;
        height: 40px;
        background: #7BAE73;
        border-radius: 20px;
        position: absolute;
        top: 20px;
        right: 50%;
        left: 50%;
        transition: .5s;
    }
    .succes_add p {
        padding: 7px 0px 5px 0px;
        text-align: center;
        color: #FFFFFF;
    }
}
</style>