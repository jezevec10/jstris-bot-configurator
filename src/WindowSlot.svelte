<div class="botConfig">
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
    width: 600px;
    height: 500px;
    background-color: #1c1c1c;
    position: relative;

    .bcHeader{
        background-color: #0e0e0e;
        color: white;
        padding: 0 1rem;
    }

    .bcContent{
        padding: 1.5rem 1rem;
    }

    .bcBottomBtns{
        margin-top: 0.4rem;
        text-align: right;
        border-top: 1px solid rgb(65, 65, 65);
        padding-top: 0.8rem;
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
