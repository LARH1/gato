<template>
<div class="container">
    <!-- Ventana Inicial -->
    <template v-if="current_stage==1">
        <gato-start @ChangeStageParent="ChangeStage" />
    </template>
    <template v-if="current_stage==2">
        <gato-game @showWinner="showWinner" />
    </template>
    <template v-if="current_stage==3">
        <gato-winner @exit="exit" @restart="restart" />
    </template>
</div>
</template>

<script>
const Game = r => require.ensure([], () => r(require("./Game/GameComponent.vue")), "game");
const Start = r => require.ensure([], () => r(require("./Game/StartComponent.vue")), "game");
const Winner = r => require.ensure([], () => r(require("./Game/WinnerComponent.vue")), "game");
export default
{
    components:
    {
        "gato-game": Game,
        "gato-start": Start,
        "gato-winner": Winner,
    },
    data()
    {
        return {
            current_stage: 0,
        }
    },
    methods:
    {

        /**
         * Cambiar el progreso actual del juego al siguiente
         */
        ChangeStage()
        {
            this.current_stage = this.$root.ChangeStage(this.current_stage);
        },

        /**
         * Obtener el progreso actual
         */
        GetCurrentStage()
        {
            this.current_stage = this.$root.current_progress;
        },

        /**
         * Mostrar el ganador del juego
         */
        showWinner()
        {
            this.current_stage = 3;
        },

        /**
         * Terminar el juego y volver a pantalla inicial
         */
        exit()
        {
            this.current_stage = 1;
            this.$root.EndGame();
        },

        /**
         * Volver a jugar con los mismos jugadore
         */
        restart()
        {
            this.current_stage=2;
        },
    },
    mounted()
    {
        this.GetCurrentStage();
    }
}
</script>
