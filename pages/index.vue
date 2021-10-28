<template>
  <main class="container">
    <section class="contents">
      <header>
        <h1>メール入力</h1>
      </header>
      <dl>
        <dt>
          <label for="subject">
            タイトル
            <span class="length">{{ options.subject.length }}</span></label
          >
        </dt>
        <dd>
          <input
            id="subject"
            v-model="options.subject"
            type="text"
            name="subject"
          />
        </dd>
        <dt>
          <label for="to">
            宛先
            <span class="length">{{ to.length }}</span></label
          >
        </dt>
        <dd>
          <input id="to" v-model="to" type="email" name="to" />
        </dd>
        <dt>
          <label for="cc">
            CC
            <span class="length">{{ options.cc.length }}</span></label
          >
        </dt>
        <dd>
          <input id="cc" v-model="options.cc" type="email" name="cc" />
        </dd>
        <dt>
          <label for="bcc">
            BCC
            <span class="length">{{ options.bcc.length }}</span></label
          >
        </dt>
        <dd>
          <input id="bcc" v-model="options.bcc" type="email" name="bcc" />
        </dd>
        <dt>
          <label for="body">
            本文 <span class="length">{{ options.body.length }}</span></label
          >
        </dt>
        <dd>
          <textarea
            id="body"
            v-model="options.body"
            name="body"
            rows="10"
          ></textarea>
        </dd>
      </dl>
    </section>

    <section class="contents">
      <header>
        <h1>MailToリンク</h1>
      </header>
      <dl>
        No Encoded<span class="length">{{ mailto.length }}</span>
      </dl>
      <dd>
        <a id="result" :href="mailto">{{ mailto }}</a>
      </dd>
      <dl>
        Encoded<span class="length">{{ mailtoEncoded.length }}</span>
      </dl>
      <dd>
        <a id="encodedResult" :href="mailtoEncoded">{{ mailtoEncoded }}</a>
      </dd>
    </section>
  </main>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  data() {
    return {
      to: 'example@example.com',
      options: {
        subject: '枕草子',
        cc: '',
        bcc: '',
        body: `　春はあけぼの。やうやう白くなりゆく山ぎは、すこしあかりて、紫だちたる 雲のほそくたなびきたる。
　夏は夜。月のころはさらなり。やみもなほ、蛍の多く飛びちがひたる。また、 ただ一つ二つなど、ほのかにうち光りて行くもをかし。雨など降るもをかし。
　秋は夕暮れ。夕日のさして山の端いと近うなりたるに、烏の寝どころへ行く とて、三つ四つ、二つ三つなど、飛びいそぐさへあはれなり。まいて雁などの つらねたるが、いと小さく見ゆるはいとをかし。日入りはてて、風の音、虫の 音など、はたいふべきにあらず。
　冬はつとめて。雪の降りたるはいふべきにもあらず、霜のいと白きも、また さらでもいと寒きに、火など急ぎおこして、炭もて渡るもいとつきづきし。 昼になりて、ぬるくゆるびもていけば、火桶の火も白き灰がちになりてわろし。`,
      },
    }
  },
  computed: {
    mailto(): string {
      const optionString = Object.entries(this.options)
        .filter(([_, value]) => !!value)
        .map(([key, value]) => `${key}=${value}`)
        .join('&')
      const query = optionString ? `?${optionString}` : ''
      return `mailto:${this.to}${query}`
    },
    mailtoEncoded(): string {
      const optionString = Object.entries(this.options)
        .filter(([_, value]) => !!value)
        .map(([key, value]) => `${key}=${encodeURI(value)}`)
        .join('&')
      const query = optionString ? `?${optionString}` : ''
      return `mailto:${this.to}${query}`
    },
  },
})
</script>

<style scoped>
.container {
  display: flex;
}

@media screen and (max-width: 600px) {
  .container {
    flex-direction: column;
  }
}

.contents {
  border: 1px solid #848484;
  border-radius: 2px;
  padding: 0.5em 2em;
  flex: 43vw 0 0;
}

dd {
  margin-left: 0;
  margin-bottom: 0.7em;
}

input,
textarea {
  width: 100%;
  padding: 0.5em;
}

.length::before {
  content: ':';
}
.length::after {
  content: '文字';
}
a {
  word-wrap: anywhere;
}
</style>
