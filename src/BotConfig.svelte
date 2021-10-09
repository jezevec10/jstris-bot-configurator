<script>
import { library } from '@fortawesome/fontawesome-svg-core';
import { faPlus, faCheck, faTrashAlt, faCopy, faCog } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from 'fontawesome-svelte';
import WindowSlot from './WindowSlot.svelte';
import { flip } from 'svelte/animate';
library.add(faPlus, faCheck, faTrashAlt, faCopy, faCog);

let editing = null;
let selectedBotType;
let customBotName;
let speedType;
let botPPS;
export let bots = [];
export let botTypes = [];
export let onSubmitData = () => {};

function remove(bot) {
    bots = bots.filter(t => t !== bot);
}
function edit(bot) {
    editing=bot;
    selectedBotType = botTypes.filter(obj => {return obj.id == bot.type})[0];
    customBotName = bot.name;
    speedType = bot.speed;
    botPPS = ("PPS" == bot.speed) ? bot.pps : "1";
}
function copy(bot) {
    let newBot = Object.assign({}, bot);
    newBot.id = getNextId();
    bots = [...bots, newBot];
}
function openAddNewBot() {
    editing='new';
    // Defaults for a new bot
    selectedBotType = botTypes[0];
    customBotName = botTypes[0].name;
    speedType = "TB";
    botPPS = "1";
}
function getNextId() {
    let max = 0;
    for(const bot of bots){
        if(bot.id > max) max = bot.id;
    }
    return max+1;
}
function submitBotEdit() {
    let bot = ("new" == editing) ? {id: getNextId()} : editing;
    bot.type = selectedBotType.id;
    bot.speed = speedType;
    bot.name = customBotName;
    if("PPS" == speedType)
        bot.pps = botPPS;
    
    if("new" == editing)
        bots = [...bots, bot];

    editing=null;
}
function exportData(){
    onSubmitData({
        bots: bots
    });
}
</script>

{#if editing === null}
    <WindowSlot>
        <div slot="header" class="header">
            <h1>Bots</h1>
            <button on:click="{() => openAddNewBot()}" class="add-btn col-btn">
                <FontAwesomeIcon icon={['fas', 'plus']} size="md"/>Add bot
            </button>
        </div>

        <div slot="content" class="content">
            {#each bots as bot (bot.id)}
            <div class="botRow" animate:flip>
                <div class="botName">
                    {bot.name}
                    {#if bot.speed == "PPS"}
                        <span class="ppsLabel">{bot.pps} PPS</span>
                    {/if}
                </div>
                <div class="botOpts">
                    <span on:click="{() => remove(bot)}"><FontAwesomeIcon icon={['fas', 'trash-alt']} size="md"/></span>
                    <span on:click="{() => copy(bot)}" href="#"><FontAwesomeIcon icon={['fas', 'copy']} size="md"/></span>
                    <span on:click="{() => edit(bot)}" href="#"><FontAwesomeIcon icon={['fas', 'cog']} size="md"/></span>
                </div>
            </div>
            {/each}
        </div>

        <div slot="buttons" class="buttons">
            <button class="cancel-btn" on:click="{() => onSubmitData(false)}">Cancel</button>
            <button class="apply-btn col-btn" on:click="{() => exportData(bots)}"><FontAwesomeIcon icon={['fas', 'check']} size="md"/> Apply!</button>
        </div>
    </WindowSlot>
{:else}
    <WindowSlot>
        <div slot="header" class="header">
            {#if editing == "new"}
                <h1>Add bot</h1>
            {:else}
                <h1>Edit {editing.name}</h1>
            {/if}
        </div>

        <div slot="content" class="content">
            <table>
                <tr>
                    <td>Bot type:</td>
                    <td>
                        <select bind:value={selectedBotType} on:change="{() => {customBotName=selectedBotType.name}}">
                            {#each botTypes as botType}
                                <option value={botType}>
                                    {botType.name}
                                </option>
                            {/each}
                        </select>
                    </td>
                </tr>
                <tr>
                    <td>Name:</td>
                    <td>
                        <input type=text bind:value={customBotName}>
                    </td>
                </tr>
                <tr>
                    <td>Speed:</td>
                    <td>
                        <label>
                            <input type=radio bind:group={speedType} value={"TB"}>
                            Turn-based
                        </label>
                        
                        <label>
                            <input type=radio bind:group={speedType} value={"PPS"}>
                            Fixed PPS
                        </label>
                    </td>
                </tr>
                {#if speedType == "PPS"}
                    <tr>
                        <td>PPS:</td>
                        <td>
                            <label>
                                <input type=range bind:value={botPPS} min=0.1 max=10 step=0.1>
                                <br>
                                <input type=number bind:value={botPPS} min=0.1 max=10>
                            </label>
                        </td>
                    </tr>
                {/if}
            </table>
        </div>

        <div slot="buttons" class="buttons">
            <button class="cancel-btn" on:click="{() => {editing = null}}">Cancel</button>
            <button class="apply-btn col-btn" on:click="{() => submitBotEdit()}"><FontAwesomeIcon icon={['fas', 'check']} size="md"/>
                {#if editing == "new"}
                    Add bot
                {:else}
                    Save changes
                {/if}
            </button>
        </div>
    </WindowSlot>
{/if}


<style lang="scss">
:root{
    --saturation: 100%;
    --ligtness: 30%;
}
.header{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items:center;
    h1{
        font-size: 25px;
        padding: 16px 0;
        font-weight: bold;
    }
}
.content{
    color:white;

    table {
        width: 100%;
        td{padding-bottom: 1rem;}
        td:nth-child(1) { 
            font-weight: bold;
            width: 30%;
        }
    }

    input[type="text"], input[type="range"], input[type="number"], select {
        width: 250px;
    }

    input[type="radio"]{
        margin-right: 0.5em;
    }
}

.col-btn{
    background: hsl(var(--hue), var(--saturation), var(--ligtness));
    color:white;
}

.add-btn{
    --hue: 204;
}
.apply-btn{
    --hue: 80;
    --ligtness: 25%;
    font-weight: bold;
}

.cancel-btn{
    background: none;
    color: white;
}

button{
    cursor: pointer;
    padding: 0.5em 0.7em;
    border: none;
    margin-left: 1rem;
    :global(svg){
        margin-right: 0.5em;
    }
}

label{
    display: block;
    font-weight: normal;
}

.botRow{
    background-color: #0e0e0e;
    padding: 0.5rem 0.6rem;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-bottom: 0.5rem;

    .botName{
        color: white;
        font-weight: bold;

        .ppsLabel{
            color: grey;
            font-size: 0.85em;
            margin-left: 0.5em;
        }
    }

    .botOpts span{
        cursor: pointer;
        padding: 4px;
        color: rgb(42 148 255);
        margin-left: 0.1em;

        &:hover{
            color: white;
        }
    }

}

input, button, select {
    font-family: inherit;
    font-size: inherit;
    box-sizing: border-box;
    border-radius: 2px;
}

select, input{
    color: black;
    padding: 0.4em;
    margin: 0 0 0.5em 0;
    height: auto;
}
</style>
