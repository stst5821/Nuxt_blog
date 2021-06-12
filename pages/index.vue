<template>
  <div>
    <!-- :workに取得したデータ"work"を入れて、Item.vueに送る -->
    <Item 
      v-for="work in works" 
      :key="work.sys.id"
      :work="work" 
    />
  </div>
</template>

<script>
  // itemコンポーネントをインポート
  import Item from '@/components/Item' 
  // contentfullプラグインをインポートして、clientに代入している。
  import { createClient } from '~/plugins/contentful.js'
  const client = createClient()

  // Nuxt.jsで用意されているasyncDataメソッド（参照：Nuxt.js - AsyncDataメソッド）を用いて、記事データを取得します。
  // asyncData内でcontentfulからデータを取得するには、clientのgetEntries関数を使います。
  export default { 
    components: {
      Item
    },
    asyncData() {
      return Promise.all([
        client.getEntries({
          'content_type': 'work', // workタイプの記事データを全取得
          order: '-sys.createdAt' // 作成日時順に並べる
        })
      ]).then(([works]) => {
        return {
          works: works.items // 取得したデータを配列worksに入れる
        }
      }).catch(console.error)
    }
  }
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
@apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
