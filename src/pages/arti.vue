<template>
    <section>
        <h1>{{ tampilkanArti.istilah }}</h1>

        <div class="content">
            <p id="semua" v-html="tampilkanArti.arti">{{ tampilkanArti.arti }}</p>
            <div v-show="sukses" class="alert-success">Copied !</div>      

            <div class="list">
                <button onclick="window.location.href='#/'" class="kiri">Back</button>
                <button @click="copyArti('semua')">Copy</button>
            </div>
        </div>
    </section>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            urlParams: window.location.href.split('/'),
            tampilkanArti: "",
            sukses: false
        }
    },
    mounted() {
        axios.get("../../static/sheet-arti.json")
		.then(response => {
            this.tampilkanArti = response.data.find(item => item.id == this.urlParams[this.urlParams.length-1])
        })
    },
    methods: {
        copyArti(containerid) {
           if (document.selection) { 
                var range = document.body.createTextRange();
                range.moveToElementText(document.getElementById(containerid));
                range.select().createTextRange();
                document.execCommand("copy"); 
            } else if (window.getSelection) {
                var range = document.createRange();
                range.selectNode(document.getElementById(containerid));
                window.getSelection().addRange(range);
                document.execCommand("copy");
                this.sukses = true;
                setTimeout(() => {this.sukses = false}, 2000);
            }
        },
    }
}
</script>

<style scoped>
    h1 {
		font-size: 1.5rem;
		font-weight: 600;
        text-transform: capitalize;
        background: #2886c8;
        color: #fff;
        padding: 15px 25px;
	}

    .content {
        margin: 25px;
        line-height: 1.4;
    }

    .list {
        display: inline-block;
        margin-top: 40px;
        width: 100%;
    }

    button {
        float: right;
        color: #fff;
        background-color: #1976d2;
        padding: 10px 30px;
        font-size: 1.1rem;
        border-radius: 3px;
        border: 0
    }

    .kiri {
        float: none
    }

    .alert-success {
        color: #155724;
        font-size: .8rem;
        background-color: #d4edda;
        position: relative;
        padding: .4rem 1.2rem;
        margin-top: 10px
    }
</style>
