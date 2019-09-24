<template>
    <b-card-group deck style="justify-content: center; margin-top: 20px;">
        <b-card
                    header="featured"
                    header-tag="header"
                    bg-variant="dark"
                    text-variant="white"
                    class="resultcard"
                    style="max-width: 50rem"
            >
            <div class="cardContent">

                <div>
                    {{result.key}}
                    {{result.scale}}
                    <label>Key</label>
                </div>

                <div>
                    {{result.bpm}}
                    <div>BPM</div>
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
</style>