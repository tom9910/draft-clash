
<script lang="ts">
   
type players = {
    name: string,
    position: number,
}[];

let teamList: players = [{'name': "Team 0", 'position': 1}] 

let hat: Array<string> = []

let pickedList: Array<string> = [] 

let numberOfPicks: number = 10

let pickCount: number = 0

function addTeam() {
    teamList = [...teamList, { name: `Team ${teamList.length}`, position: 0 }];
    console.log(teamList); 
  }

const addToHat = (teamList: players) => {
    hat = []
    teamList.forEach(team => {
        hat = [...hat, ...Array(team.position).fill(team.name)]
    })

}

const takePick = (hat: string[]) => {
    pickCount++;
    if(pickCount <= numberOfPicks){
    let ind = Math.floor(Math.random()*(hat.length))
    let picked = hat[ind]
    hat = hat.filter((name: string) => name !== picked)
    pickedList = [...pickedList, picked]
    }
   

    if(pickCount == numberOfPicks){
        console.log("here passed picks")
        //fill out picked list
        let remaining = teamList.filter((item: any)=> !pickedList.includes(item.name) ).sort((a,b) => a.position - b.position)
        remaining.forEach(name => {
            pickedList = [...pickedList, name.name]
        })
        hat = []
    }
    
    return hat
}

//background: linear-gradient(to right, #eee 75%, #333 0%);

$: addToHat(teamList)

</script>

<div class="mainCont">
<h1>Welcome to Clash Royale Draft Lottery</h1>
<p></p>
<div>
<label>Number of picks</label>
<input class="inputField" bind:value={numberOfPicks}/>
<br/>
<div class="box">

<button on:click={()=>{addTeam()}}>Add Team</button>
<br/>
{#each teamList as team, index}
    <label>{index+1}</label>
    <input class="inputField" bind:value={team.name}/>
    <input class="inputField" type="number" bind:value={team.position} />
    <br/>
{/each}

</div>
</div>

<div>
<div style="height: 25px; width: 98%; border: 1px solid grey; background-color: rgb(73, 72, 72);  border-radius: 12px; text-align:center">Draft Odds</div>
<div class="box">
<ul>
{#each teamList as team}
    {#if hat.includes(team.name)}
    <div style="display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid green; width: 270px;">
        <li>{team.name} --- {((team.position/hat.length)*100).toFixed(2)}%</li>
        <div style="width: 120px; border-radius: 10px; background: linear-gradient(to right, blue {((team.position/hat.length)*100).toFixed(2)}%, #eee 0%);; height: 10px; margin-top: 4px; margin-left: 20px"></div>
    </div>
        {:else}
        <div style="display: flex; border-bottom: 2px solid grey; width: 270px">
        <li>{team.name} --- 0%</li>
    </div>
    {/if}
    
{/each}
</ul>
</div>

</div>


<div style="width: 100%; margin-top: 15px; margin-bottom: 15px;">
<button style="height: 32px; width: 100%; background-color: blue; color: white; border: 0; border-radius: 10px" on:click={()=>{hat = takePick(hat)}}>Make Pick</button>
</div>
<!-- <p>{JSON.stringify(pickedList)}</p> -->

<div style="margin: auto;">
<div class="inputField" style="width: 230px;">DRAFT ORDER</div>
{#each pickedList as pick, index}
<div class="pickedBox">{index+1}: {pick}</div>
{/each}
</div>

<button on:click={()=>{addToHat(teamList); pickedList = []; pickCount=0}}>
    reset
</button>
<div class="box">

</div>





</div>

<svelte:body style="background-color: black;" />

<style>



.pickedBox {
    background-color: goldenrod;
    height: 25px;
    width: 230px;
    animation: pulse 2s infinite;
    
    text-align: center;
}

.mainCont {
    display: grid;
    grid-template-columns: 1fr 1fr;
}

.inputField {
    border-radius: 12px;
    background-color: rgb(73, 72, 72);
    height: 25px;
    color: rgb(184, 184, 194);
    border: 1px solid rgb(61, 61, 61);
    text-align: center;

}

.box{
    background-color: rgb(61, 60, 60);
    padding: 10px;
    border-radius: 20px;
    width: 95%;
    text-align: center;
    align-items: center;
}

:global(.teal) {
		background-color: teal;
	}

</style>
