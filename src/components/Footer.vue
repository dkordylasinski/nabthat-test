<template>
    <footer class="footer">
        <div class="footer__wrapper">
            <div class="footer__left">
                <div class="footer__left-content">
                    <p v-html="content.cssAwesome"></p>
                    <div class="footer__left-content-block"></div>
                </div>
            </div>
            <div class="footer__middle">
                <p>{{ content.nabthat }}</p>
            </div>
            <div class="footer__right">
                <div class="footer__right-box">
                    <div class="footer__right-button">
                        <p class="footer__right-button-text">{{ content.show }}</p>
                        <svg class="footer__right-button-svg" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                            <path d="M233.4 406.6c12.5 12.5 32.8 12.5 45.3 0l192-192c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L256 338.7 86.6 169.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3l192 192z"/>
                        </svg>
                        <div class="footer__right-content-box">
                            <div class="footer__right-content-box-content">
                                <a href="/" class="footer__right-content-box-option">
                                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
                                        <path d="M310.6 233.4c12.5 12.5 12.5 32.8 0 45.3l-192 192c-12.5 12.5-32.8 12.5-45.3 0s-12.5-32.8 0-45.3L242.7 256 73.4 86.6c-12.5-12.5-12.5-32.8 0-45.3s32.8-12.5 45.3 0l192 192z"/>
                                    </svg>
                                    <p>{{ content.reset }}</p>
                                </a>
                                <div @click="showAuthor" class="footer__right-content-box-option">
                                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512">
                                        <path d="M310.6 233.4c12.5 12.5 12.5 32.8 0 45.3l-192 192c-12.5 12.5-32.8 12.5-45.3 0s-12.5-32.8 0-45.3L242.7 256 73.4 86.6c-12.5-12.5-12.5-32.8 0-45.3s32.8-12.5 45.3 0l192 192z"/>
                                    </svg>
                                    <p>{{ content.author }}</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </footer>
</template>
<script setup>

    const content = {
        cssAwesome: 'css<br>is<br> awesome',
        nabthat: 'nabthat',
        show: 'pokaż',
        reset: 'zresetuj ustawieni',
        author: 'pokaż dane osobowe'
    }

    const showAuthor = () => {
        const headerAuthor = document.querySelector(".header__content-author")
        headerAuthor.classList.toggle("header--actived")
    }
    
</script>
<style lang="scss">

    .footer {
        border-top: 2px solid black;
        &__wrapper {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding-right: 3rem;
            align-items: stretch;
            @media only screen and (max-width: 600px) {
                flex-direction: column-reverse;
            }
        }
        &__left, &__middle, &__right {
            flex: 1;
        }
        &__left {
            padding-left: 3rem;
            overflow: hidden;
            display: flex;
            align-items: center;
            @media only screen and (max-width: 1100px) {
                padding: 30px 0;
                padding-left: 3rem;
            }
        }
        &__left-box {
            width: fit-content;
        }
        &__left-content {
            width: fit-content;
            position: relative;
            padding: 0 8px;
            p {
                font-size: 2rem;
                text-transform: uppercase;
                color: rgba($color: white, $alpha: .1);
            }
            &:hover {
                .footer__left-content-block {
                    rotate: 180deg;
                }
            }
        }
        &__left-content-block {
            position: absolute;
            z-index: -1;
            top: 0px;
            left: 0px;
            width: 74%;
            height: 100%;
            border: 1px solid #e88303;
            transition: .6s;
        }
        &__middle {
            display: flex;
            justify-content: center;
            align-items: center;
            p {
                font-size: 2rem;
                line-height: 2rem;
                color: rgba($color: white, $alpha: .1);
                position: relative;
                &::after, &::before {
                    content: '';
                    position: absolute;
                    top: 50%;
                    transform: translateY(-50%);
                    width: 100px;
                    height: 1px;
                    background-color: rgba($color: white, $alpha: .1)
                }
                &::after {
                    left: 110%;
                }
                &::before {
                    right: 110%;
                }
            }
            @media only screen and (max-width: 1100px) {
                display: none;
            }
        }
        &__right {
            display: flex;
            justify-content: flex-end;
            @media only screen and (max-width: 600px) {
                padding-left: 3rem;
                justify-content: flex-start;
            }
        }
        &__right-box {
            position: relative;
            padding: 4rem 0;
            cursor: pointer;
            &:hover {
                svg {
                    rotate: 0deg;
                }
                .footer__right-content-box {
                    max-height: 100px;
                }
            }
            @media only screen and (max-width: 600px) {
                padding: 20px 0;
            }
        }
        &__right-content-box {
            position: absolute;
            bottom: calc(100% + 3px);
            width: 100%;
            border-top-left-radius: 2px;
            border-top-right-radius: 2px;
            left: 0px;
            background-color: white;
            overflow: hidden;
            transition: .3s;
            max-height: 0px;
        }
        &__right-content-box-content {
            padding: 20px;
            display: flex;
            flex-direction: column;
            row-gap: 8px;
        }
        &__right-content-box-option {
            text-decoration: none;
            display: flex;
            align-items: center;
            column-gap: 10px;
            transition: .3s;
            svg {
                width: 10px;
                path {
                    transition: .3s;
                }
            }
            p {
                color: black;
                text-transform: uppercase;
                transition: .3s;
            }
            &:hover {
                transform: translateX(4px);
                svg {
                    path {
                        fill: #e88303;
                    }
                }
                p {
                    color: #e88303;
                }
            }
        }
        &__right-button-text {
            color: white;
            text-transform: uppercase;
            font-weight: 600;
            font-size: 1rem;
        }
        &__right-button-svg {
            width: 15px;
            rotate: 180deg;
            transition: .3s;
            path {
                fill: white;
            }
        }
        &__right-button {
            padding: 14px 100px;
            display: flex;
            align-items: center;
            justify-content: center;
            column-gap: 10px;
            cursor: pointer;
            background-color: transparent;
            outline: none;
            border: 1px solid white;
            border-radius: 2px;
        }
    }

</style>