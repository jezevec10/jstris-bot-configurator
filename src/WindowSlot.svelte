<div class="botConfig" id="botConfig">
    <div class="bcHeader">
        <slot name="header"></slot>
    </div>
    <div class="bcContent">
        <div class="fixedH">
            <slot name="content"></slot>
        </div>
        <div class="bcBottomBtns">
            <slot name="buttons"></slot>
        </div>
    </div>
    
</div>
<style lang="scss">
.botConfig{
    text-align: left;
    max-width: 600px;
    height: 500px;
    background-color: #1c1c1c;
    position: absolute;
    inset: 0;
    margin: 100px auto;

    .bcHeader{
        background-color: #0e0e0e;
        color: white;
        padding: 0 15px;
    }

    .bcContent{
        padding: 20px 15px;
    }

    .bcBottomBtns{
        margin-top: 3px;
        text-align: right;
        border-top: 1px solid #414141;
        padding-top: 12px;
    }
}

.fixedH{
    display: flex;
    flex-direction: column;
    align-items: stretch;
    height: 330px;
    overflow-y: auto;
}

</style>
