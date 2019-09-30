<template>
    <b-card-group deck style="justify-content: center; margin-top: 20px;">
        <b-card
                    :header= "result.name"
                    header-tag="header"
                    bg-variant="secondary"
                    text-variant="white"
                    class="resultcard"
                    style="max-width: 50rem"
            >
            <div class="cardContent">

                <div>
                    {{result.key}}
                    {{result.scale}}
                    <div class="subtitle">Key</div>
                </div>

                <div>
                    {{result.bpm}}
                    <div class="subtitle">BPM</div>
                </div>
            </div>

        </b-card>
    </b-card-group>
</template>
<script>
export default {
  name: 'result',
  props: ['result'],
  components: {
  },
}
</script>

<style scoped>
.cardContent {
    display: flex;
    justify-content: space-around;
}
.subtitle {
    font-weight: bold;
}
</style>