<template>
	<section>
		<div class="search">
			<input type="text" v-model="smeSearch" placeholder="Search ...">
			<button type="submit" class="search-button">
				<svg xmlns="http://www.w3.org/2000/svg" width="25" viewBox="0 0 512 512"><path fill="#fff" d="M495 466.2L377.2 348.4a206.6 206.6 0 1 0-28.9 28.9L466.1 495a20.4 20.4 0 0 0 28.9-28.8zm-277.5-83.3C126.2 382.9 52 308.7 52 217.5S126.2 52 217.5 52C308.7 52 383 126.3 383 217.5s-74.3 165.4-165.5 165.4z"/></svg>
			</button>
		</div>
		<div class="content">
			<ul>
				<li v-for="kamus in filteredList" :key="kamus.id">
					<a :href="'#/arti/' + kamus.id">
						<h2>{{ kamus.istilah }}</h2>
						<p>{{ kamus.arti | cropText }} ...</p>
					</a>
				</li>
			</ul>
		</div>
	</section>
</template>

<script>
import axios from 'axios'

export default {
	data () {
        return {
            dataIstilah: [],
			smeSearch: ''
        }
    },
	mounted() {
		axios.get("../../static/sheet-arti.json")
		.then(response => {
			this.dataIstilah = response.data
		})		
	},
	computed: {
        filteredList() {
            return this.dataIstilah.filter(dataIstilahs => {
                return dataIstilahs.istilah.toLowerCase().includes(this.smeSearch.toLowerCase());
            })
        }
    },
	filters: {
        cropText: function(txt) {
            return txt.substring(0, 75).replace(/(<([^>]+)>)/ig,"");
        }
    }
}
</script>

<style scoped>
	ul {
		list-style-type: none;
		padding: 0 25px;
	}

	li {
		margin-bottom: 12px;
		border-bottom: 1px #e0e0e0 solid;
	}

	li a {
		padding: 15px 0;
		display: block;
		background: #fff;
		text-transform: capitalize;
	}

	li p {
		color: #999;
		font-size: 15px
	}

	.search {
		width: 100%;
		position: fixed;
		top: 0;
		width: 100%;
		display: flex;
		z-index: 99;
		box-shadow: 0 5px 10px rgba(0, 0, 0, .1);
	}

	.search input {
		width: 100%;
		border: none;
		border-right: none;
		padding: 15px 25px;
		outline: none;
		background: #2886c8;
		color: #fff;
		font-size: 2rem;
	}

	.search-button {
		width: 75px;
		text-align: center;
		background: #2886c8;
		border: none;
	}

	.search-button svg {
		top: 2px;
		position: relative;
	}

	.content {
		position: relative;
		margin-top: 85px;
	}
</style>