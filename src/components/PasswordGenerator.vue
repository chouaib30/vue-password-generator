<template>
    <div class="container-fluid p-0 m-0">
        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
            <div class="alert alert-success my-4 alert-dismissible" v-show="isCopied" role="alert" :key="msg">
                <button type="button" class="close" @click.prevent="isCopied = !isCopied">
                    <span aria-hidden="true">&times;</span>
                </button>
                {{ msg }}
            </div>
        </transition-group>

        <div class="container">
            <!-- result container -->
            <div class="result__container mx-auto mt-2 mb-4">
                <div class="result" id="result">{{ finalePassword }}</div>
                <button class="btn btn-success clipboard__btn" @click.prevent="copyPassword" id="clipboard">
                    <i class="far fa-clipboard"></i>
                    <span class="ml-2">Copy</span>
                </button>
            </div>

            <form class="settings">
                <!-- Password length -->
                <vue-range-slider ref="slider" :min="min" :max="max" v-model="passwordLength" class="d-block w-100 my-3"></vue-range-slider>
                
                <!-- Lowercase letters -->
                <div class="pl-0 form-check setting my-3">
                    <input type="checkbox" id="lowerCase" v-model="hasLowerCase" class="form-check-input" name="lowerCase" checked>
                    <label for="lowerCase" class="checkbox__label form-check-label">Include Lowercase Letters</label>
                </div>

                <!-- Uppercase letters -->
                <div class="pl-0 form-check setting my-3">
                    <input type="checkbox" id="upperCase" v-model="hasUpperCase" class="form-check-input" name="upperCase" checked>
                    <label for="upperCase" class="checkbox__label form-check-label">Include Uppercase Letters</label>
                </div>

                <!-- Symbols -->
                <div class="pl-0 form-check setting my-3">
                    <input type="checkbox" id="symbols" v-model="hasSymbols" class="form-check-input" name="symbols" checked>
                    <label for="symbols" class="checkbox__label form-check-label">Include Symbols</label>
                </div>

                <!-- Numbers -->
                <div class="pl-0 form-check setting my-3">
                    <input type="checkbox" id="numbers" v-model="hasNumbers" class="form-check-input" name="numbers" checked>
                    <label for="numbers" class="checkbox__label form-check-label">Include Numbers</label>
                </div>
                <button class="btn btn-lg btn-success my-3" id="generate" @click.prevent="generatePassword">Generate Password</button>
                <button class="btn btn-lg btn-danger my-3" id="clear" @click.prevent="clearPassword">Clear Password</button>
            </form>
        </div>
    </div>
</template>

<script>
import "vue-range-component/dist/vue-range-slider.css"
import VueRangeSlider from "vue-range-component"


export default {
    data(){
        return {
            passwordLength: 4,
            hasLowerCase: false,
            hasUpperCase: false,
            hasNumbers: false,
            hasSymbols: false,
            finalePassword : "",
            typeCount : 0,
            isCopied: false,
            msg: "Well done!, The password is copied !!!"              
        }
    },


    created(){
        //vue-range-slider
        this.min = 4;
        this.max = 20;
    },

    methods: {
        // random lowercase letters
        randomLower(){
            return ( this.hasLowerCase ) ? String.fromCharCode(Math.floor(Math.random()*26)+97) : "";
        },

        // random uppercase letters
        randomUpper(){
            return ( this.hasUpperCase ) ? String.fromCharCode(Math.floor(Math.random()*26)+65) : "";
        }, 

        // random numbers
        randomNumber(){
            return ( this.hasNumbers ) ? String.fromCharCode(Math.floor(Math.random()*10)+48) : "";
        },

        // random symbols
        randomSymbols(){
            let symbol = ['!','$','_','-','/','^','+','*','<','>','|','{','}','[',']'];
            return ( this.hasSymbols ) ? symbol[Math.floor(Math.random()*symbol.length)] : "";
        },

        // generate password funcion
        generatePassword(){
            let password = "";
            let i = 0;  
            let length = parseInt(this.passwordLength);        
            let randomFunctions = [ this.randomLower , this.randomUpper , this.randomNumber , this.randomSymbols ];         
            this.typeCount = this.hasLowerCase + this.hasUpperCase + this.hasNumbers + this.hasSymbols;        
            if( this.typeCount === 0 )  return "";

            while( i < length ){
                randomFunctions.forEach( funcName => {
                    password += funcName();
                });
                i += this.typeCount;
            }
            this.finalePassword = password.slice(0 , length);
            return this.finalePassword
        },

        // copy password
        copyPassword(){
            let textArea = document.createElement("textarea");
            if(!this.finalePassword) return "";
            textArea.value = this.finalePassword;
            document.body.appendChild(textArea);
            textArea.select();
            document.execCommand("copy");
            document.body.removeChild(textArea);
            this.isCopied = true;
        },

        // clear password
        clearPassword(){
            this.finalePassword = "";
        }
    },

    components: {
        VueRangeSlider
    }
}
</script>

<style lang="scss" scoped>
    @import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css");

    .settings{
        display: flex;
        width: 100%;
        height: 100%;
        flex-direction: column;
    }

    input[type=checkbox]{
        opacity: 0;
        cursor: pointer;
        height: 0;
        width: 0;
        position: absolute;
    }

    label{
        font-size: 1.1rem;
    }

    .checkbox__label{
        display: flex;
        align-items:center;
        justify-content:space-between;
        cursor: pointer;
        height: 20px;
        width:100%;
        line-height: 20px;
        position: relative;
        &::before{
            content: "✓";
            display: inline-block;
            width: 24px;
            height: 24px;
            color: rgba(#fff,0);
            background: transparent;
            position: absolute;
            right:0;
            top:0;
            font-size:1.1rem;
            text-align:center;
            line-height: 20px;
            margin-left: 15px;
            border:2px solid #28a745;
            border-radius:4px;
            transition: color .1s linear,background .1s linear;
        }
    }

    input[type=checkbox]:checked + .checkbox__label::before{
        color: rgba(#fff,1);
        background: #28a745;
    }

    .container{
        background: #fff;
        border-radius:5px;
        box-shadow:
            0 2.8px 2.2px rgba(0, 0, 0, 0.02),
            0 6.7px 5.3px rgba(0, 0, 0, 0.028),
            0 12.5px 10px rgba(0, 0, 0, 0.035),
            0 22.3px 17.9px rgba(0, 0, 0, 0.042),
            0 41.8px 33.4px rgba(0, 0, 0, 0.05),
            0 100px 80px rgba(0, 0, 0, 0.07);
        padding:30px;
        .result__container{
            overflow: hidden;
            height:50px; 
            display: flex;
            align-items:center;
            position: relative;
            .result{
                text-align: left;
                position: absolute;
                top:0;
                left:0;
                z-index: 1;
                width:100%;
                height: 50px;
                line-height: 45px;
                background: #fff;
                padding:0 0 0 20px;
                border-radius:5px;
                border:2px solid #e9ecef;
            }
            .btn{
                position: absolute;
                right:8px;
                top:50%;
                transform: translateY(-50%);
                z-index: 2;
            }
        }
    }
</style>