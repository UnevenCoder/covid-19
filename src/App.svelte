<svelte:head>
    <script src="https://kit.fontawesome.com/66ac086c34.js" crossorigin="anonymous"></script>
  </svelte:head>
<script>
  let i = true
 	import { beforeUpdate } from 'svelte';
  let r1,r2,r3,r4,t1 ,t2 , t3 , t4 , yes=false,val='',location
//()=>loading()
beforeUpdate(() => {
  if(i){
fetch('https://covid-api.com/api/reports/total')
.then(res=>res.json())
.then(data=>{
  r1 =parseInt(data.data.confirmed).toLocaleString()
    r2 = parseInt(data.data.active).toLocaleString()
    r3 =parseInt(data.data.recovered).toLocaleString()
r4 = parseInt(data.data.deaths).toLocaleString()
i = false
})}
    })
  

	function search(){

if(val.split(' ').length>1){

let url = 'https://covid-api.com/api/reports?q='+val.split(' ').join('%20').toString()
if(val.toLowerCase() == 'united states of america'){url='https://covid-api.com/api/reports?iso=usa'}
console.log(url)
	fetch(url)
.then(r=>r.json())
.then(data=>{
  console.log(data.data['0'])
    location = data.data['0'].region.name ;
    t1 =parseInt(data.data['0'].confirmed).toLocaleString()
    t2 = parseInt(data.data['0'].active).toLocaleString()
    t3 =parseInt(data.data['0'].recovered).toLocaleString()
t4 = parseInt(data.data['0'].deaths).toLocaleString()

}
)
}
else{
fetch('https://covid2019-api.herokuapp.com/v2/country/'+val)
.then(res=>res.json())
.then(async(data)=>{
	let str = await val.charAt(0).toUpperCase() + val.slice(1)
 //confirmed: 90409, deaths: 4738, recovered:
 console.log(data)
 location = data['data'].location
 t1 = parseInt(data['data'].confirmed).toLocaleString()
 t2 = parseInt(data['data'].active).toLocaleString()
 t3 = parseInt(data['data'].recovered).toLocaleString()
 t4 = parseInt(data['data'].deaths).toLocaleString()
})
}
}

</script>

<main class='container'>
	<h1>Covid Tracker</h1>
	<p>Track the spread of the Coronavirus Outbreak</p>
	<section class="worldwide">
        <h2>Worldwide</h2>
        <div class="grid">
          <div class="g-box cases">
            <h6 class="g-heading">Total Cases</h6>
            <h3 id="total-cases">{r1}</h3>
          </div>
          <div class="g-box active">
            <h6 class="g-heading">Total Active</h6>
            <h3 id="total-active">{r2}</h3>
          </div>

          <div class="g-box recovered">
            <h6 class="g-heading">Total recovered</h6>
            <h3 id="total-recovered">{r3}</h3>
          </div>
          <div class="g-box deaths">
            <h6 class="g-heading">Total Deaths</h6>
            <h3 id="total-deaths">{r4}</h3>
          </div>
        </div>
      </section>
	  <section class="country-wise">
        <h2>Search for a particular country here.</h2>
        <div id="search">
          <label for='search-input'>Search</label>
          <input bind:value={val} id="search-input" placeholder="Search" class="search" type="text" >
          <button class="search-btn" on:click={()=>{
			search()  
			return yes=true}} id="search-btn">
            <i class="fa fa-search"></i>
           <p>Search</p>
          </button>
		</div>
    {#if yes} 
    {#if t1}
        <div id="result-data">
      <h2 class="country-name">{location}</h2>
      <div class="grid">
        <div class="g-box cases">
          <h6 class="g-heading">Total Cases</h6>
          <h3 id="total-cases">{t1}</h3>
        </div>
        <div class="g-box active">
          <h6 class="g-heading">Total Active</h6>
          <h3 id="total-active">{t2}</h3>
        </div>

        <div class="g-box recovered">
          <h6 class="g-heading">Total recovered</h6>
          <h3 id="total-recovered">{t3}</h3>
        </div>
        <div class="g-box deaths">
          <h6 class="g-heading">Total Deaths</h6>
          <h3 id="total-deaths">{t4}</h3>
        </div>
	  </div>
  </div>
  {:else}
<div style='margin-top:50px;color:black;'>
  <h5 style='margin-bottom:10px;'>There might be 3 things happening</h5>
  <h6 >1 : Data is loading</h6>
<h6>2 : Api might have some prob fetching data so <br>please enter their abbrevation like US for USA</h6>
<h6>3 :The country you entered doesn't exist</h6>
</div>
{/if}
	{/if}
	  </section>
	  
	  <footer>
		Developed by <a href="https://github.com/UnevenCoder">Ameen</a>
	  </footer>
  
</main>

<style>
  @import url('https://fonts.googleapis.com/css?family=Open+Sans:400,700&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

:global(body) {
  background-color: #fff6cfde;
  display:flex;
  width:100vw;
  justify-content: center;
  
  color: black;
  font-family: 'Open Sans', sans-serif;
}

  p{
    display:none;
    color:transparent;
    z-index:2
  }
  label{
    color:transparent
  }
  .country-wise{
    margin-top:30px;
  }
  #search{
    margin-top:15px;
  }
main {
  padding: 10px;
  width: 1220px;
  max-width: 100%;
  text-align:center;
  margin: 2rem auto 0 auto;
}
h1 {
  font-size: 2rem;
  line-height: 1.4;
  font-weight: 500;
}

p {
  opacity: 0.8;
}

main {
  margin-top: 2rem;
}

h2 {
  font-weight: 500;
}

h6 {
  letter-spacing: 1.5px;
  color: rgba(0, 0, 0, 0.6);
}

h3 {
  font-size: 35px;
}

.worldwide {
  margin: 3rem auto;
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  max-width: 650px;
  margin: 0 auto;
  margin-top: 3rem;
  grid-gap: 30px;
}

.g-box {
  background-color: #fff;
  box-shadow: 0 0 35px 0 rgba(73, 80, 87, 0.15);
  text-align: center;
  padding: 1.5rem;
  border-radius: 15px;
}

.active {
  background-color: rgba(228, 30, 30, 0.678);
}

.recovered {
  background-color: #0080009e;
}

.deaths {
  background-color: #000000b3;
}

.g-heading {
  text-transform: uppercase;
}

@media screen and (max-width: 768px) {
  .grid {
    max-width: 340px;
  }
  .g-box {
    grid-column-start: 1;
    grid-column-end: 3;
  }
}


.country-name {
  margin-top: 2rem;
}

.search,
.search-btn {
  border: none;
  padding: 10px;
  border-radius: 20px;
  background-color: black;
  color: white;
}

.search:focus,
.search-btn:focus {
  border: none;
}

.search-btn {
  cursor: pointer;
}

.search-btn:hover {
  transform: scale(1.1);
  translate: transform 0.5s ease;
}



@keyframes rotate {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}

@media screen and (max-width: 290px) {
  .search {
    width: 80%;
  }
}

footer {
  margin-top: 3rem;
}

a {
  text-decoration: none;
  color: rgb(255, 3, 3);
}

a:hover {
  color: blue;
}

</style>