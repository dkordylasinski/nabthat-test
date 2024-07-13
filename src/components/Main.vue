<template>
    <main class="main">
        <div class="container">
            <h1 class="main__title">{{ content.title }}</h1>
            <div class="main__wrapper">
                <div class="main__box">
                    <div class="main__left-block">
                        <h2 class="main__left-block-title">{{ content.leftBlock.title }}</h2>
                        <div class="main__left-block-radio">
                            <div class="main__left-block-radio-option radio-input" v-for="option in radioContent">
                                <input class="input" type="radio" name="content-option" :id="option.id" :value="option.value" v-model="radioValue">
                                <label :for="option.id">{{ option.content }}</label>
                            </div>
                        </div>
                    </div>
                    <div class="main__middle-block">
                        <h2 class="main__middle-block-title">{{ content.middleBlock.title }}</h2>
                        <div class="main__middle-block-buttons">
                            <button @click="replaceText"><p>{{ content.middleBlock.leftButton }}</p></button>
                            <button @click="pasteText"><p>{{ content.middleBlock.rightButton }}</p></button>
                        </div>
                    </div>
                </div>
                <div class="main__right-block">
                    <h2 class="main__right-block-title" :title="content.rightBlock.title">{{ cutTitle(content.rightBlock.title) }}</h2>
                    <div class="main__right-block-content" ref="visibleSentences"></div>
                </div>
            </div>
        </div>
    </main>
</template>
<script setup>
    import { ref, onMounted } from 'vue'

    // Content
    const fileDoesntExist = ref(null)
    const sentences = ref([])
    const fileResponse = ref(null)
    const visibleSentences = ref()
    const pasteTextUsedIndex = ref([])
    const radioValue = ref(null)
    const content = {
        title: 'Nagłówek H1',
        leftBlock: {
            title: 'Blok pierwszy'
        },
        middleBlock: {
            title: 'Blok drugi',
            leftButton: 'Zastąp',
            rightButton: 'Doklej'
        },
        rightBlock: {
            title: 'Blok z długą nazwą która sama się przycina'
        } 
    }
    const cutTitleLength = 20
    const radioContent = [
        {id: 'firstOption', value: 1, content: 'Opcja pierwsza'},
        {id: 'secondOption', value: 2, content: 'Opcja druga'},
        {id: 'thirdOption', value: 3, content: 'Opcja losowa'},
    ]
    const errorsContent = {
        firstBlockEmpty: 'Zaznacz opcję w pierwszym bloku.',
        firstContentExist: 'Pierwsza treść już została doklejona.',
        secondContentExist: 'Druga treść już została doklejona.',
        textsContentExist: 'Wykorzystano wszystkie dostępne treści.',
    }

    // Functions
    const loadSentences = async () => {
        try {
            fileResponse.value = await fetch('/sentences.json');
            if (!fileResponse.value.ok) {
                throw new Error(`HTTP error! status: ${fileResponse.value.status}`);
            }
            const data = await fileResponse.value.json();
            sentences.value = data
            localStorage.setItem('sentences', JSON.stringify(data))
            fileDoesntExist.value = false
        } catch (error) {
            console.error('Problem with file.', error);
            fileDoesntExist.value = true
        }
    }

    const replaceText = () => {
        if (fileDoesntExist.value) { alert("Wystąpił problem z plikiem, sprawdź jego nazwę oraz poprawną sciezkżę"); return; }
        pasteTextUsedIndex.value = []
        if(!radioValue.value) {  alert(errorsContent.firstBlockEmpty);  return; }
        switch (radioValue.value) {
            case 1: replaceTextOption(1); break;
            case 2: replaceTextOption(2); break;
            case 3: replaceTextOption(3); break;
        }
    }
    const replaceTextOption = (num) => {
        const p = document.createElement('p');
        switch (num) {
            case 1: p.textContent = sentences.value.sentences[0]; pasteTextUsedIndex.value.push(1); break;
            case 2: p.textContent = sentences.value.sentences[1]; pasteTextUsedIndex.value.push(2); break;
            case 3: const rnd = Math.floor(Math.random() * sentences.value.sentences.length); p.textContent = sentences.value.sentences[rnd]; break;
        }
        visibleSentences.value.textContent = '';
        visibleSentences.value.appendChild(p);
    }

    const pasteText = () => {
        if (fileDoesntExist.value) { alert("Wystąpił problem z plikiem, sprawdź jego nazwę oraz poprawną sciezkżę"); return; }
        if(!radioValue.value) {  alert(errorsContent.firstBlockEmpty);  return; }
        switch (radioValue.value) {
            case 1: pasteTextOption(0); break;
            case 2: pasteTextOption(1); break;
            case 3: pasteTextRandomOption(); break;
        }
    }
    const pasteTextOption = (num) => {
        const p = document.createElement('p');
        let firstElementFound = false;
        pasteTextUsedIndex.value.forEach(el => {
            if (el == num+1) {
                firstElementFound = true;
                switch (num) {
                    case 0: alert(errorsContent.firstContentExist); break;
                    case 1: alert(errorsContent.secondContentExist); break;
                }
                return;
            }
        });
        if (!firstElementFound) {
            p.textContent = sentences.value.sentences[num];
            pasteTextUsedIndex.value.push(num+1);
        }
        visibleSentences.value.appendChild(p);
    }
    const pasteTextRandomOption = () => {
        const rnd = Math.floor(Math.random() * sentences.value.sentences.length);
        const p = document.createElement('p');
        let firstElementFound = false;
        pasteTextUsedIndex.value.forEach(el => {
            if (el == rnd + 1) {
                firstElementFound = true;
                return;
            }
        });
        if (!firstElementFound) {
            p.textContent = sentences.value.sentences[rnd];
            pasteTextUsedIndex.value.push(rnd + 1);
        } else {
            try {
                pasteTextRandomOption()
            } catch (error) {
                alert(errorsContent.textsContentExist)
            }
        }
        visibleSentences.value.appendChild(p);
    }

    const cutTitle = (title) => {
        if (title.length >= cutTitleLength) {
            return `${title.slice(0, cutTitleLength)}...`
        } else {
            return title
        }
    }

    onMounted(() => {
        loadSentences()
    })

</script>
<style lang="scss">

    .main {
        overflow: hidden;
        background-color: #2a2d36;
        min-height: calc(100vh - 182px);
        padding-top: 13rem;
        padding-bottom: 100px;
        @media only screen and (max-width: 960px) {
            padding-top: 10rem;
        }
        &__title {
            width: fit-content;
            margin: 0 auto;
            font-weight: 300;
            font-size: 3.2rem;
            position: relative;
            margin-bottom: 5rem;
            &::after {
                content: '';
                position: absolute;
                bottom: -1.5rem;
                left: 50%;
                transform: translateX(-50%);
                width: 78%;
                height: 1px;
                background-color: white;
            }
            @media only screen and (max-width: 1400px) {
                font-size: 2.5rem;
            }
        }
        &__wrapper, &__box {
            display: flex;
            justify-content: space-between;
            column-gap: 40px;
        }
        &__wrapper {
            @media only screen and (max-width: 960px) {
                flex-direction: column;
                row-gap: 40px;
            }
        }
        &__box {
            flex: 2;
            @media only screen and (max-width: 550px) {
                flex-direction: column;
                row-gap: 20px;
            }
        }
        &__left-block, &__middle-block, &__right-block {
            flex: 1;
        }
        &__left-block-title, &__middle-block-title, &__right-block-title {
            text-align: center;
            text-transform: uppercase;
            margin-bottom: 50px;
            @media only screen and (max-width: 1400px) {
                font-size: 1.2rem;
            }
            @media only screen and (max-width: 960px) {
                margin-bottom: 20px;
            }
        }
        &__left-block-title {
            @media only screen and (max-width: 960px) {
                text-align: left;
            }
        }
        &__middle-block-title {
            @media only screen and (max-width: 960px) {
                text-align: right;
            }
            @media only screen and (max-width: 550px) {
                text-align: left;
            }
        }
        &__right-block-title {
            @media only screen and (max-width: 550px) {
                text-align: left;
            }
        }
        &__left-block-radio-option {
            display: flex;
            align-items: center;
            column-gap: 15px;
            label {
                font-size: 1.4rem;
                @media only screen and (max-width: 1400px) {
                    font-size: 1rem;
                }
            }
        }
        &__middle-block-buttons {
            margin: 0 auto;
            width: fit-content;
            display: flex;
            align-items: center;
            column-gap: 30px;
            button {
                padding: 12px 45px;
                background-color: transparent;
                outline: none;
                border-radius: 2px;
                border: 1px solid white;
                cursor: pointer;
                text-transform: uppercase;
                transition: .3s;
                position: relative;
                overflow: hidden;
                p {
                    position: relative;
                    z-index: 2;
                    font-size: 1.2rem;
                    font-weight: 600;
                    color: rgba($color: white, $alpha: .5);
                    transition: .3s;
                }
                &::after {
                    content: '';
                    position: absolute;
                    z-index: 1;
                    top: 0px;
                    left: -105%;
                    width: 100%;
                    height: 100%;
                    background-image: linear-gradient(to right, #cd7303, #d97e0c, #e58814, #f2931b, #fe9e22);
                    transition: .3s;
                }
                &:hover {
                    p {
                        color: white;
                    }
                    &::after {
                        left: 0%;
                    }
                }
            }
            @media only screen and (max-width: 1400px) {
                flex-direction: column;
                row-gap: 20px;
            }
            @media only screen and (max-width: 960px) {
                margin-right: 0;
            }
            @media only screen and (max-width: 550px) {
                margin-left: 0;
            }
        }
        &__right-block-content {
            p {
                margin-bottom: 10px;
                font-size: 1rem;
                text-align: center;
                color: rgba($color: white, $alpha: .5);
            }
        }




        // #region custom input
        .input {
            -webkit-appearance: none;
            /* remove default */
            display: block;
            margin: 10px;
            width: 34px;
            height: 34px;
            border-radius: 12px;
            cursor: pointer;
            vertical-align: middle;
            box-shadow: hsla(0,0%,100%,.15) 0 1px 1px, inset hsla(0,0%,0%,.5) 0 0 0 1px;
            background-color: hsla(0,0%,0%,.2);
            background-image: -webkit-radial-gradient( hsla(200,100%,90%,1) 0%, rgb(255, 255, 255) 15%, hsla(0, 0%, 100%, 0.3) 28%, hsla(0, 0%, 100%, 0) 70% );
            background-repeat: no-repeat;
            -webkit-transition: background-position .15s cubic-bezier(.8, 0, 1, 1),
                -webkit-transform .25s cubic-bezier(.8, 0, 1, 1);
            outline: none;
            }

            .input:checked {
            -webkit-transition: background-position .2s .15s cubic-bezier(0, 0, .2, 1),
                -webkit-transform .25s cubic-bezier(0, 0, .2, 1);
            }

            .input:active {
            -webkit-transform: scale(1.5);
            -webkit-transition: -webkit-transform .1s cubic-bezier(0, 0, .2, 1);
            }



            /* The up/down direction logic */

            .input,
            .input:active {
            background-position: 0 34px;
            }

            .input:checked {
            background-position: 0 0;
            }

            .input:checked ~ .input,
            .input:checked ~ .input:active {
            background-position: 0 -34px;
            }
        // #endregion
    }
</style>