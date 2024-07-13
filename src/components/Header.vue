<template>
    <header class="header">
        <div class="container">
            <div class="header__wrapper">
                <a class="header__logo" href="/">
                    <svg fill="none" height="2500" width="2183" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 124 141.53199999999998">
                        <path class="background" d="M10.383 126.894L0 0l124 .255-10.979 126.639-50.553 14.638z" fill="#e34f26"/>
                        <path class="background" d="M62.468 129.277V12.085l51.064.17-9.106 104.851z" fill="#ef652a"/>
                        <path class="number" d="M99.49 41.362l1.446-15.49H22.383l4.34 47.49h54.213L78.81 93.617l-17.362 4.68-17.617-5.106-.936-12.085H27.319l2.128 24.681 32 8.936 32.255-8.936 4.34-48.17H41.107L39.49 41.362z" fill="#fff"/>
                    </svg>
                </a>
                <div class="header__content">
                    <p v-html="content.task"></p>
                    <span class="header__content-author">{{ content.author }}</span>
                </div>
            </div>
        </div>
    </header>
</template>
<script setup>
    import { ref } from 'vue'

    const content = {
        task: 'Zadanie <strong>rekrutacyjne</strong>',
        author: 'Damian Kordylasiński'
    }

</script>
<style lang="scss">
    .header {
        padding: 1.2rem 0;
        background-color: #0b0d0f;
        position: fixed;
        z-index: 999;
        top: 0px;
        left: 0px;
        width: 100%;
        &__wrapper {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        &__logo {
            width: clamp(100px, 100%, 100px);
            height: auto;
            aspect-ratio: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #303030;
            border-radius: 50%;
            transition: .3s;
            svg {
                width: 50px;
                height: auto;
                path {
                    transition: .3s;
                }
                .background {
                    fill: white;
                }
                .number {
                    fill: #303030;
                }
                @media only screen and (max-width: 960px) {
                    width: 40px;
                }
            }
            &:hover {
                background-color: #e88303;
                .number {
                    fill: #e88303;
                }
            }
            @media only screen and (max-width: 960px) {
                width: clamp(70px, 100%, 70px);
            }
        }
        &__content {
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            row-gap: 10px;
            font-size: 2.2rem;
            span {
                font-size: 1.6rem;
                text-align: right;
                @media only screen and (max-width: 960px) {
                    font-size: 1.3rem;
                }
                @media only screen and (max-width: 550px) {
                    font-size: 1rem;
                }
            }
            @media only screen and (max-width: 960px) {
                font-size: 1.6rem;
            }
            @media only screen and (max-width: 550px) {
                font-size: 1.3rem;
            }
        }
        &__content-author {
            transition: .3s;
            height: 100%;
            max-height: 0;
            overflow: hidden;
        }
        &--actived {
            max-height: 35px;
            @media only screen and (max-width: 960px) {
                max-height: 28px;
            }
            @media only screen and (max-width: 550px) {
                max-height: 22px;
            }
        }
    }
</style>