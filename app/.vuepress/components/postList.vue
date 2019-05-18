<template>
    <div>
        <div class="post" v-for="post in posts">
            <p class="post-date">{{ date(post.frontmatter.date) }}</p>
            <h2 class="post-title" ><a v-bind:href="post.path">{{post.title}}</a></h2>
            <p>{{post.frontmatter.description}}</p>
            <a v-bind:href="post.path">続きを読む</a>
        </div>
    </div>
</template>
<script>
export default {
    props: [
      'filter'
    ],
    computed: {
        posts() {
            return this.$site.pages
                // blogディレクトリ以下を投稿記事一覧表示の対象とする
                .filter(post => post.path.startsWith(`/${this.filter}/`))
                // dateに設定した日付の降順にソートする
                .sort((a, b) => new Date(b.frontmatter.date) - new Date(a.frontmatter.date));
        }
    },
    methods: {
    date(postDate) {
      const date = new Date(postDate);
      const yyyy = date.getFullYear();
      const MM = ("0" + (date.getMonth() + 1)).slice(-2);
      const dd = ("0" + date.getDate()).slice(-2);
      return [yyyy, MM, dd].join("/");
    }
  }
}
</script>

<style lang="scss" scoped>
  .post {
    padding-bottom: 35px;

    .post-date {
      color: #b7b3b2;
      font-size: 14px;
      margin: 0;
    }

    .post-title {
      margin: 0 0 10px;
    }
  }
</style>