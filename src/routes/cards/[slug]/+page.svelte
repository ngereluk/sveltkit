
<script lang="ts">
  	import { onMount } from 'svelte';
    import { page } from '$app/stores';
    import SiteHeader from '../../components/SiteHeader.svelte'
    import LoadingAnimation from '../../components/LoadingAnimation.svelte'
    import CardDetailsPage from '../../components/CardDetailPage.svelte'
    import type { CardType } from "../../../types/card.type";

   export let data:CardType
   let loading = true //will hide/show loading animation

   onMount(async()=>{
    //artificially skip the reuqest to show pre pop card for demo purposes
    loading = true
    if($page.params.slug !== '5f8287b1-5bb6-5f4c-ad17-316a40d5bb0c'){
      const res =  await fetch("/api/GetCardDetails", {
          method: "POST",
          headers: {
            "Content-type": "application/json",//specifies media type of ressource
          },
          body: JSON.stringify({
              cardId: $page.params.slug//again, why stringify?
          }),
        });
        const response = await res.json()
        data = response as CardType
        loading = false   
    }
    else{
      loading = true
      data= {
        "name": "Ancestor's Chosen",
        "manaCost": "{5}{W}{W}",
        "cmc": 7,
        "colors": ["W"],
        "colorIdentity": ["W"],
        "type": "Creature — Human Cleric",
        "types": ["Creature"],
        "subtypes": ["Human", "Cleric"],
        "rarity": "Uncommon",
        "set": "10E",
        "setName": "Tenth Edition",
        "text": "First strike (This creature deals combat damage before creatures without first strike.)\nWhen Ancestor's Chosen enters the battlefield, you gain 1 life for each card in your graveyard.",
        "artist": "Pete Venters",
        "number": "1",
        "flavor": "The will of all, by my hand done.",
        "layout": "normal",
        "imageUrl": "http://gatherer.wizards.com/Handlers/Image.ashx?multiverseid=130550&type=card",
        "id": "5f8287b1-5bb6-5f4c-ad17-316a40d5bb0c"
      }
      loading = false   

    }
  })
  

  </script>
 
<SiteHeader/>

<!-- show loading animation while loading -->
{#if loading === true} 
  <div class='padding'>
    <div class="loadingAnimationContainer">
      <LoadingAnimation />
      <div class="loadingText">Loading...</div>
    </div>
  </div>

  <!-- show card details component once data is loaded -->
{:else if  loading === false}
  <CardDetailsPage data = {data}/>
{/if} 

<style>
  .padding{
    padding-top:9%
  }
/* note - in all other instances, the loadingText and loadingAnimationContainer classes
are imported from styles.js (for organization). However, importing them here cause a lag - the styles took a sec
to load and the animation at first would appear without its styles. Defining them here explicitly solves the issue */
.loadingText{
  color: #ababab;
  padding-top: 2%;
  font-weight: 700;
  font-family: "Lucida Grande", monospace !important;
}

.loadingAnimationContainer{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100%;
  padding-top: 20px;
}

</style>
  