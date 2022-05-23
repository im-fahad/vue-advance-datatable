<template>
    <div id="app" style="padding: 30px; background-color: #ebecf2; min-height: 100vh; box-sizing: border-box">
        <data-table id="dataTable" :headers="headers" :items="data" :pagination="blueprint"
                    @pagination-change-page="changePagination"
                    @per-page="changePagination"
                    :rows-per-page="limit" :loading="false" @search="setSearchData">

            <!--                  <template slot="header">-->
            <!--                      vgfbgfbfhfhfhfh-->
            <!--                  </template>-->

            <template slot="placeholder">
                gfdgdgdgd
            </template>
            <tr v-for="(item, i) in data" v-bind:key="i">
                <td class="text-nowrap" v-text="item.id"></td>
                <td class="text-nowrap" v-text="item.name"></td>
                <td class="text-nowrap" v-text="item.year"></td>
                <td class="text-nowrap" v-text="item.color"></td>
                <td class="text-nowrap" v-text="item.pantone_value"></td>
            </tr>
        </data-table>

    </div>
</template>

<script>
import DataTable from './components/DataTable'

export default {
    name: 'App',
    components: {
        DataTable
    },
    data() {
        return {
            headers: [
                {text: '#', sortable: true, sortKey: 'id'},
                {text: 'Name', sortable: true, sortKey: 'name'},
                {text: 'Year', sortable: true, sortKey: 'year'},
                {text: 'Color', sortable: true, sortKey: 'color'},
                {text: 'Pantone Value', sortable: true, sortKey: 'pantone_value'}
            ],
            blueprint: {},
            limit: 5,
            data: [
                {
                    id: 1,
                    name: "Quatar Airways",
                    country: "Quatar",
                    logo: "https://upload.wikimedia.org/wikipedia/en/thumb/9/9b/Qatar_Airways_Logo.svg/300px-Qatar_Airways_Logo.svg.png",
                    slogan: "Going Places Together",
                    head_quaters: "Qatar Airways Towers, Doha, Qatar",
                    website: "www.qatarairways.com",
                    established: "1994"
                },
                {
                    id: 2,
                    name: "Singapore Airlines",
                    country: "Singapore",
                    logo: "https://upload.wikimedia.org/wikipedia/en/thumb/6/6b/Singapore_Airlines_Logo_2.svg/250px-Singapore_Airlines_Logo_2.svg.png",
                    slogan: "A Great Way to Fly",
                    head_quaters: "Airline House, 25 Airline Road, Singapore 819829",
                    website: "www.singaporeair.com",
                    established: "1947"
                },
                {
                    id: 3,
                    name: "Cathay Pacific",
                    country: "Hong Kong",
                    logo: "https://upload.wikimedia.org/wikipedia/en/thumb/1/17/Cathay_Pacific_logo.svg/300px-Cathay_Pacific_logo.svg.png",
                    slogan: "Move Beyond",
                    head_quaters: "Cathay City, Hong Kong International Airport, Chek Lap Kok, Hong Kong",
                    website: "www.cathaypacific.com",
                    established: "1946"
                },
                {
                    id: 4,
                    name: "Emirates",
                    country: "Dubai",
                    logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/Emirates_logo.svg/150px-Emirates_logo.svg.png",
                    slogan: "From Dubai to destinations around the world.",
                    head_quaters: "Garhoud, Dubai, United Arab Emirates",
                    website: "www.emirates.com/",
                    established: "1985"
                },
                {
                    id: 5,
                    name: "Eva Air",
                    country: "Taiwan",
                    logo: "https://upload.wikimedia.org/wikipedia/en/thumb/e/ed/EVA_Air_logo.svg/250px-EVA_Air_logo.svg.png",
                    slogan: "Sharing the World, Flying Together",
                    head_quaters: "376, Hsin-Nan Rd., Sec. 1, Luzhu, Taoyuan City, Taiwan",
                    website: "www.evaair.com",
                    established: "1989"
                },
                {
                    id: 6,
                    name: "Qantas",
                    country: "Austrailia",
                    logo: "https://upload.wikimedia.org/wikipedia/en/thumb/0/02/Qantas_Airways_logo_2016.svg/300px-Qantas_Airways_logo_2016.svg.png",
                    slogan: "Spirit of Australia",
                    head_quaters: "Mascot, Sydney, Australia",
                    website: "qantas.com",
                    established: "1920"
                },
                {
                    id: 7,
                    name: "Deutsche Lufthansa AG",
                    country: "Germany",
                    logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/b/b8/Lufthansa_Logo_2018.svg/300px-Lufthansa_Logo_2018.svg.png",
                    slogan: "Say yes to the world",
                    head_quaters: "Cologne, Germany",
                    website: "lufthansa.com",
                    established: "1953"
                },
                {
                    id: 8,
                    name: "Thai Airways",
                    country: "Thailand",
                    logo: "https://upload.wikimedia.org/wikipedia/en/thumb/5/58/Thai_Airways_Logo.svg/200px-Thai_Airways_Logo.svg.png",
                    slogan: "Smooth as Silk / I Fly THAI",
                    head_quaters: "Jom Phol Subdistrict, Chatuchak, Bangkok, Thailand",
                    website: "www.thaiairways.com",
                    established: "1960"
                },
                {
                    id: 9,
                    name: "Japan Airlines",
                    country: "Japan",
                    logo: "https://upload.wikimedia.org/wikipedia/en/thumb/d/dd/Japan_Airlines_Logo_%282011%29.svg/300px-Japan_Airlines_Logo_%282011%29.svg.png",
                    slogan: "Fly into tomorrow",
                    head_quaters: "Shinagawa, Tokyo, Japan",
                    website: "www.jal.com",
                    established: "1951"
                },
                {
                    id: 10,
                    name: "Swiss International Air Lines",
                    country: "Switzerland",
                    logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Swiss_International_Air_Lines_Logo_2011.svg/200px-Swiss_International_Air_Lines_Logo_2011.svg.png",
                    slogan: "Made of Switzerland. The Airline of Switzerland",
                    head_quaters: "EuroAirport Basel Mulhouse Freiburg near Basel, Switzerland",
                    website: "www.swiss.com",
                    established: "2002"
                },
                {
                    id: 11,
                    name: "Swiss International Air Lines",
                    country: "Switzerland",
                    logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Swiss_International_Air_Lines_Logo_2011.svg/200px-Swiss_International_Air_Lines_Logo_2011.svg.png",
                    slogan: "Made of Switzerland. The Airline of Switzerland",
                    head_quaters: "EuroAirport Basel Mulhouse Freiburg near Basel, Switzerland",
                    website: "www.swiss.com",
                    established: "2002",
                    createdDate: "2020-06-21T14:43:49.150Z"
                },
                {
                    id: 12,
                    name: "Sri Lankan Airways",
                    country: "Sri Lanka",
                    logo: "https://upload.wikimedia.org/wikipedia/en/thumb/9/9b/Qatar_Airways_Logo.svg/sri_lanka.png",
                    slogan: "From Sri Lanka",
                    head_quaters: "Katunayake, Sri Lanka",
                    website: "www.srilankaairways.com",
                    established: "1990"
                },
                {
                    id: 13,
                    name: "Swiss International Air Lines",
                    country: "Switzerland",
                    logo: "https://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Swiss_International_Air_Lines_Logo_2011.svg/200px-Swiss_International_Air_Lines_Logo_2011.svg.png",
                    slogan: "Made of Switzerland. The Airline of Switzerland",
                    head_quaters: "EuroAirport Basel Mulhouse Freiburg near Basel, Switzerland",
                    website: "www.swiss.com",
                    established: "2002"
                },
                {
                    id: 63,
                    name: "Yo Airways",
                    country: "Earth",
                    logo: "https://yochansons.fr/wp-content/uploads/2018/12/Yo-simple-contour-blc-larg.png",
                    slogan: "Smooth as Silk / I Fly THAI",
                    head_quaters: "Jom Phol Subdistrict, Chatuchak, Bangkok, Thailand",
                    website: "www.yoairways.com",
                    established: "1983"
                },
                {
                    id: 14,
                    name: "Thai Airways",
                    country: "Thailand",
                    logo: "https://upload.wikimedia.org/wikipedia/en/thumb/5/58/Thai_Airways_Logo.svg/200px-Thai_Airways_Logo.svg.png",
                    slogan: "Smooth as Silk / I Fly THAI",
                    head_quaters: "Jom Phol Subdistrict, Chatuchak, Bangkok, Thailand",
                    website: "www.thaiairways.com",
                    established: "1960"
                },
            ],
        }
    },
    created() {
        this.fetchData()
    },
    methods: {
        changePagination() {
            alert(123)
        },
        setSearchData(data) {
            this.data = data
        },
        fetchData(page = 1) {
            fetch(`https://reqres.in/api/product?page=${page}&per_page=${this.limit}`, {
                method: 'get',
            })
                .then(response => response.json())
                .then(response => {
                    let {data, ...blueprint} = response
                    this.data = data
                    this.blueprint = blueprint
                    console.log({response})
                })
        }
    }
}
</script>

<style>
body {
    width: 100%;
    padding: 0;
    margin: 0;
}
</style>
