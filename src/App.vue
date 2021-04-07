<template>
    <div id="app">
        <div class="active-block">
            <span class="prize-number">Prize number: {{ prizeNumber }}</span>
            <div class="active-btns">
                <button class="active-btn active-btn-add" type="button"
                        @click="!rolling && prizeNumber < 8 && (prizeNumber++)"
                        :disabled="rolling || prizeNumber === 8">
                    Add
                    <v-icon
                            small
                            color="red lighten-2"
                    >
                        mdi-plus
                    </v-icon>
                </button>
                <button class="active-btn active-btn-remove" type="button"
                        @click="!rolling && prizeNumber > 1 && (prizeNumber--)"
                        :disabled="rolling || prizeNumber === 1">
                    Remove
                    <v-icon
                            small
                            color="red lighten-2"
                    >
                        mdi-minus
                    </v-icon>
                </button>
            </div>
        </div>
        <div
                class="modal-view"
                v-show="showModal"
        >
            <div class="header">
                <span class="header__title">wheel fortune app</span>
                <button class="header__close" @click="showModal= !showModal">&times;</button>
            </div>
            <div class="content">
                <div class="content-cols">
                    <div class="col col-image">
                        <img :src="imageModalUrl" alt="">
                    </div>
                    <div class="col col-text">
                        <p>{{textModal}}</p>
                    </div>
                </div>
                <button class="content-btn" @click="clickModal">click</button>
            </div>
        </div>
        <div class="wheel-wrapper">
            <div
                    class="wheel-pointer"
                    @click="onClickRotate"
            >
                Start
            </div>
            <div
                    class="wheel-bg background-main"
                    :class="{freeze: freeze}"
                    :style="`transform: rotate(${wheelDeg}deg)`"
            >
                <div class="prize-list">
                    <div
                            class="prize-item-wrapper"
                            v-for="(item,index) in prizeList"
                            :key="index"
                    >
                        <div
                                class="prize-item"
                                :style="`transform: rotate(${(360/ prizeList.length) * index}deg)`"
                        >
                            <div class="prize-name">
                                {{ item.name }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    // import InfCircle from './components/InfCircle';
    // import ModalView from './components/ModalView';

    export default {
        name: 'App',

        components: {
            // InfCircle,
            // ModalView
        },

        data() {
            return {
                freeze: false,
                rolling: false,
                wheelDeg: 0,
                prizeNumber: 8,
                prizeListOrigin: [
                    {
                        icon: "https://thepeninsulairelandblog.files.wordpress.com/2012/09/10000.jpg",
                        name: "$10000"
                    },
                    {
                        icon: "https://cf.bstatic.com/images/hotel/max1024x768/223/223087771.jpg",
                        name: "Ticket to sea"
                    },
                    {
                        icon: "https://media.npr.org/assets/img/2020/01/10/gettyimages-450751241_wide-f6696af0a1919b1740e560a4161803acf53dde73.jpg?s=1400",
                        name: "$500"
                    },
                    {
                        icon: "https://thumbs.dreamstime.com/b/d-number-hundred-gold-white-background-35847463.jpg",
                        name: "$100"
                    },
                    {
                        icon: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRjtO0wGtDiJmaUFfpKFvF8HjkTADBAxRFdlA&usqp=CAU",
                        name: "Ticket to USA"
                    },
                    {
                        icon: "https://media.istockphoto.com/photos/years-golden-foil-balloon-anniversary-logotype-picture-id1140378120?k=6&m=1140378120&s=612x612&w=0&h=5kOtsR4AY-UxquKFxN8XAQDhlomjOCPiTD9ETCW2MYY=",
                        name: "$50"
                    },
                    {
                        icon: "https://betanews.com/wp-content/uploads/2015/07/10-600x550.jpg",
                        name: "$10"
                    },
                    {
                        icon: "https://www.reedpublicrelations.com/wp-content/uploads/2019/11/Reed-blog-post-image.jpg",
                        name: "Thank you!"
                    }
                ],
                showModal: false,
                imageModalUrl: "",
                textModal: ""
            };
        },
        computed: {
            prizeList() {
                return this.prizeListOrigin.slice(0, this.prizeNumber);
            }
        },
        methods: {
            clickModal() {
                this.showModal = !this.showModal
                setTimeout(() => {
                    this.onClickRotate()
                }, 100)
            },
            onClickRotate() {
                if (this.rolling) {
                    return;
                }
                const result = Math.floor(Math.random() * this.prizeList.length);
                this.roll(result);
            },
            roll(result) {
                this.rolling = true;
                const {wheelDeg, prizeList} = this;
                this.wheelDeg =
                    wheelDeg -
                    wheelDeg % 360 +
                    6 * 360 +
                    (360 - 360 / prizeList.length * result);
                setTimeout(() => {
                    this.rolling = false;
                    this.showModal = true;
                    this.textModal = prizeList[result].name;
                    this.imageModalUrl = prizeList[result].icon;
                    // alert("Result：" + prizeList[result].icon);
                }, 4500);
            }
        }
    };
</script>

<style lang="scss">
    html {
        background: cornflowerblue;
    }

    .active-block {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        position: relative;
        top: 40px;

        .prize-number {
            font-size: 20px;
            color: #fff;
            font-weight: bold;
            text-transform: uppercase;
        }

        .active-btns {
            display: flex;
            margin-top: 20px;

            .active-btn {
                width: 100px;
                height: 40px;
                border: 1px solid #fff;
                background: transparent;
                color: #fff;
                font-size: 18px;
                font-weight: bold;

                &-add {
                    background: green;
                }

                &-remove {
                    background: red;
                }
            }
        }
    }

    .wheel-wrapper {
        width: 300px;
        height: 300px;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        border: 4px solid;
        border-radius: 50%;
    }

    .wheel-pointer {
        font-size: 18px;
        text-transform: uppercase;
        font-weight: bold;
        width: 70px;
        height: 70px;
        border-radius: 50%;
        background: yellow;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
        line-height: 70px;
        z-index: 10;
        cursor: pointer;

        &::after {
            content: "";
            position: absolute;
            top: -32px;
            left: 50%;
            border-width: 0 8px 40px;
            border-style: solid;
            border-color: transparent transparent yellow;
            transform: translateX(-50%);
        }
    }

    .wheel-bg {
        width: 100%;
        height: 100%;
        border-radius: 1000px;
        overflow: hidden;
        transition: transform 4s ease-in-out;
        background: transparent;

        &.background-main {
            box-shadow: 1px 0 10px 10px rgba(15, 122, 255, 0.3);
            background: linear-gradient(rgba(180, 220, 255, 0.9), transparent),
            linear-gradient(-15deg, rgba(15, 122, 255, 0.3), transparent),
            linear-gradient(-45deg, rgba(15, 122, 255, 0.9), transparent);
            background-blend-mode: multiply;
        }
    }

    .prize-list {
        width: 100%;
        height: 100%;
        position: relative;
        text-align: center;
    }

    .prize-item-wrapper {
        position: absolute;
        top: 0;
        left: 50%;
        transform: translateX(-50%);
        width: 150px;
        height: 150px;
    }

    .prize-item {
        width: 100%;
        height: 100%;
        transform-origin: bottom;

        .prize-name {
            font-size: 15px;
            font-weight: bold;
            padding: 16px 0;
        }
    }

    .modal-view {
        position: absolute;
        width: 500px;
        top: 50%;
        left: 50%;
        z-index: 1;
        transform: translate(-50%, -50%);
        background: antiquewhite;
        transition: display ease-in-out .3s;
        @media screen and (max-width: 500px) {
            width: 100%;
        }

        .header {
            width: 100%;
            height: 30px;
            background: cadetblue;
            position: relative;

            display: flex;
            justify-content: center;
            align-items: center;

            &__title {
                font-size: 18px;
                text-transform: uppercase;
                font-weight: bold;
                color: #fff;
            }

            &__close {
                position: absolute;
                right: 8px;
                top: 50%;
                transform: translateY(-50%);
                font-size: 30px;
                color: #ffffff;
                font-weight: bold;
            }
        }

        .content {

            .content-cols {
                display: flex;
                justify-content: space-between;
                padding: 20px;
                @media screen and (max-width: 500px) {
                    display: flex;
                    justify-content: center;
                    padding: 20px;
                    flex-direction: column;
                    align-items: center;
                }

                .col {
                    height: 240px;
                    width: 220px;
                }

                .col-image {
                    img {
                        height: 100%;
                        width: 100%;
                    }
                }

                .col-text {
                    display: flex;
                    justify-content: center;
                    align-items: center;

                    p {
                        font-size: 35px;
                        font-weight: bold;
                        text-transform: uppercase;
                    }
                }
            }

            .content-btn {
                display: block;
                margin: 5px auto;
                border: 2px solid #000;
                padding: 10px 25px;
                font-size: 15px;
                font-weight: bold;
                text-transform: uppercase;
                background: cadetblue;
                color: #fff;
            }
        }
    }

</style>

