<template>
    <div class="main--contaiener">
        <div class="header--title_nav">
            <p class="header--title">Добавление товара</p>
            <button class="header--btn">По умолчанию<img src="@/assets/arrow-d.svg" alt=""></button>
        </div>

        <div class="form_cards--container">
            <div class="form--container">
                <form action="">
                    <p>Наименование товара</p>
                    <input type="text" placeholder="Введите наименование товара">

                    <p>Описание товара</p>
                    <textarea name="" cols="30" rows="10" placeholder="Введите описание товара"></textarea>

                    <p>Ссылка на изображение товара</p>
                    <input type="text" name="" placeholder="Введите ссылку">

                    <p>Цена товара</p>
                    <input type="number" name="" placeholder="Введите цену">
                </form>
                <button>Добавить товар</button>
            </div>

            <div class="cards--container">
                <div class="card">
                    <img :src="val" alt="">
                    <div class="card--data">
                        <p>Наименование товара</p>
                        <p>Довольно-таки интересное описание товара в несколько строк. Довольно-таки интересное описание товара в несколько строк</p>
                        <p>10 000 руб.</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'IndexPage',
    data() {
        return {
        val: 'http://sun9-3.userapi.com/s/v1/if1/y72Ov_XZPLyuIeAQsCKjkYj31CV4h3ULfIBf755hFxDLRpqvvyxv0Rcs-RDqK0dOBufGqVX5.jpg?size=200x235&quality=96&crop=0,0,500,588&ava=1',
        ww: "dwawww",
        }
    },
    methods: {
        dd() {
        const a = '1'
        console.log(a)
        }
    },
    mounted() {
        console.clear()
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
        .header--btn img {
            padding-left: 5px;
        }
    }

    .form_cards--container {
        display: flex;
        margin-top: 16px;
        max-width: 440px;

        .form--container {
            background: $white;
            box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
            border-radius: 4px;
            padding: 24px;
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
        input::-webkit-input-placeholder {
            color: $gray;
        }
        input[type="number"]::-webkit-inner-spin-button {
            -webkit-appearance: none;
        }
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
        textarea::-webkit-input-placeholder {
            color: $gray;
        }

        .cards--container {
            max-width: 1028px;
            display: flex;
            flex-wrap: wrap;
            margin-left: 16px;
        }
        .card {
            background: #FFFEFB;
            box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04), 0px 6px 10px rgba(0, 0, 0, 0.02);
            border-radius: 4px;
            width: 332px;
            height: 423px;
            position: relative;
        }
        // add delete button
        // .card:hover::after {
        //     content: url('@/assets/delete-btn.svg');
        //     position: absolute;
        //     top: 0;
        //     right: -10px;
        // }
        .card img {
            max-width: 332px;
            max-height: 320px;
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
            // margin-top: 32px;
            font-style: normal;
            font-weight: 600;
            font-size: 24px;
            line-height: 30px;
            color: #3F3F3F;
        }
    }
}
</style>