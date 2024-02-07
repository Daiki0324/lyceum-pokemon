<script setup>
const route = useRoute();
const config = useRuntimeConfig();
const { data: trainer } = await useFetch(
  () => `/api/trainer/${route.params.name}`,
  {
    default: () => [],
    baseUrl: config.public.backendOrigin,
  }
);
const {
  dialog: deleteDialog,
  onOpen: onOpenDelete,
  onClose: onCloseDelete,
} = useDialog();
</script>

<template>
  <div>
    <h1>トレーナー情報</h1>
    <div class="trainer-info">
      <img src="/avatar.png" />
      <span>{{ trainer.name }}</span>
    </div>
    <GamifyButton @click="onOpenDelete(true)"
      >マサラタウンにかえる</GamifyButton
    >
    <h2>てもちポケモン</h2>
    <CatchButton :to="`/trainer/${route.params.name}/catch`"
      >ポケモンをつかまえる</CatchButton
    >
    <GamifyList>
      <GamifyItem v-for="pokemon in trainer.pokemons" :key="pokemon.id">
        <img :src="pokemon.sprites.front_default" />
        <span class="pokemon-name">{{ pokemon.nickname || pokemon.name }}</span>
        <GamifyButton @click="onOpenNickname(pokemon)"
          >ニックネームをつける</GamifyButton
        >
        <GamifyButton @click="onOpenRelease(pokemon)"
          >はかせにおくる</GamifyButton
        >
      </GamifyItem>
    </GamifyList>
    <GamifyDialog
      v-if="deleteDialog"
      id="confirm-delete"
      title="かくにん"
      description="ほんとうに　マサラタウンに　かえるんだな！　この　そうさは　とりけせないぞ！"
      @close="onCloseDelete"
    >
      <GamifyList :border="false" direction="horizon">
        <GamifyItem>
          <GamifyButton @click="onCloseDelete">いいえ</GamifyButton>
        </GamifyItem>
        <GamifyItem>
          <GamifyButton @click="onDelete">はい</GamifyButton>
        </GamifyItem>
      </GamifyList>
    </GamifyDialog>
  </div>
</template>

<style scoped>
.item > label {
  display: block;
  margin-bottom: 0.25rem;
}

.gamify-item:hover img {
  animation: bounce;
  animation-duration: 0.8s;
  animation-iteration-count: infinite;
}

.trainer-info {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.trainer-info > img {
  width: 3rem;
  height: 3rem;
}
</style>
