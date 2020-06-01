<template>
  <main :class="[$style.main, themeStyles.main]" :style="mainStyles">
    <header v-if="header" :class="[$style.header, themeStyles.header]">
      <span style="margin: 0 20px">welcome to the demo</span>
    </header>
    <a v-if="logo" :class="[$style.logo, 'un-select', themeStyles.logo]">
      <h1>logo</h1>
    </a>
    <nav v-if="nav" :class="[$style.nav, themeStyles.nav]">
      <ul>
        <p><b>Techniques</b></p>
        <li>grid layout</li>
        <li>css modules</li>
        <li>multiple themes</li>
        <li>and more ...</li>
      </ul>
    </nav>
    <section :class="[$style.content, themeStyles.content]">
      <div :class="[$style.contentInner, themeStyles.contentInner]">
        <section class="section">
          <h1>colorful admin layout by grid</h1>
          <p>
            <label>
              <span class="un-select">Theme：</span>
              <select v-model="currentTheme">
                <option
                  v-for="theme in themes"
                  :key="theme.key"
                  :value="theme"
                  >{{ theme.name }}</option
                >
              </select>
            </label>
          </p>
        </section>
        <div class="hr"></div>
        <section class="section">
          <p>
            <label
              ><span class="un-select">Alone logo：</span
              ><input type="checkbox" v-model="logo"
            /></label>
          </p>
          <p>
            <label
              ><span class="un-select">Header：</span>
              <input type="checkbox" value="header" v-model="header"
            /></label>
            <span class="un-select"
              >（
              <label
                ><span class="un-select">Left：</span>
                <input
                  :disabled="!header"
                  type="checkbox"
                  v-model="areas[0][0]"
                  true-value="header"
                  false-value="nav"
              /></label>
              、
              <label
                ><span class="un-select">Right：</span>
                <input
                  :disabled="!header"
                  type="checkbox"
                  v-model="areas[0][2]"
                  true-value="header"
                  false-value="aside"
              /></label>
              ）</span
            >
          </p>
          <p class="un-select">
            <label
              ><span class="un-select">Nav：</span
              ><input type="checkbox" value="nav" v-model="nav"
            /></label>
            、
            <label
              ><span class="un-select">Aside：</span
              ><input type="checkbox" value="aside" v-model="aside"
            /></label>
          </p>
          <p>
            <label
              ><span class="un-select">Footer：</span>
              <input type="checkbox" value="footer" v-model="footer"
            /></label>
            <span class="un-select"
              >（
              <label
                ><span class="un-select">Left：</span>
                <input
                  :disabled="!footer"
                  type="checkbox"
                  v-model="areas[2][0]"
                  true-value="footer"
                  false-value="nav"
              /></label>
              、
              <label
                ><span class="un-select">Right：</span>
                <input
                  :disabled="!footer"
                  type="checkbox"
                  v-model="areas[2][2]"
                  true-value="footer"
                  false-value="aside"
              /></label>
              ）</span
            >
          </p>
          <p>
            <label>
              <span class="un-select">Gap：</span>
              <input v-model.number="gap" type="number" :min="0" :max="50" />
              <span class="un-select"> px</span>
            </label>
          </p>
        </section>
        <div class="hr"></div>
        <section class="section">
          <p>
            <label>
              <span class="un-select">Header Height：</span
              ><input
                v-model.number="headerHeight"
                type="number"
                :min="10"
                :max="1000"
              />
              <span class="un-select"> px</span>
            </label>
          </p>
          <p>
            <label>
              <span class="un-select">Footer Height：</span
              ><input
                v-model.number="footerHeight"
                type="number"
                :min="10"
                :max="1000"
              />
              <span class="un-select"> px</span>
            </label>
          </p>
          <p>
            <label>
              <span class="un-select">Nav Width：</span
              ><input
                v-model.number="navWidth"
                type="number"
                :min="10"
                :max="1000"
              />
              <span class="un-select"> px</span>
            </label>
          </p>
          <p>
            <label>
              <span class="un-select">aside Width：</span
              ><input
                v-model.number="asideWidth"
                type="number"
                :min="10"
                :max="1000"
              />
              <span class="un-select"> px</span>
            </label>
          </p>
        </section>
      </div>
    </section>
    <aside v-if="aside" :class="[$style.aside, themeStyles.aside]">
      <ul>
        <li><p>nothing here</p></li>
      </ul>
    </aside>
    <footer v-if="footer" :class="[$style.footer, themeStyles.footer]">
      <span style="margin: 0 20px">it's awesome</span>
    </footer>
  </main>
</template>

<script>
import themePack from "./theme";
const { themes, defaultTheme } = themePack;

export default {
  data() {
    return {
      currentTheme: defaultTheme,
      themes,

      logo: true,
      header: true,
      nav: true,
      aside: true,
      footer: true,
      headerHeight: 60,
      footerHeight: 60,
      navWidth: 200,
      asideWidth: 200,
      gap: 0,
      areas: [
        ["header", "header", "header"],
        ["nav", "content", "aside"],
        ["footer", "footer", "footer"]
      ]
    };
  },
  watch: {
    logo: {
      immediate: true,
      handler(val) {
        const firstRow = this.areas[0];
        firstRow[0] = val ? "logo" : "header";
        this.$set(this.areas, 0, firstRow);
      }
    },
    nav(val) {
      if (!val && this.logo) {
        this.logo = false;
      }
    },
    areas(val) {
      if (val[0][0] === "header" && this.logo) {
        this.logo = false;
      }
    }
  },
  computed: {
    mainStyles() {
      return {
        gridGap: this.gap + "px",
        gridTemplateAreas: this.areas
          .filter((_, index) => [this.header, true, this.footer][index])
          .map(
            row =>
              `"${row
                .filter((_, index) => [this.nav, true, this.aside][index])
                .join(" ")}"`
          )
          .join("\n"),
        gridTemplateRows: [
          this.header ? this.headerHeight + "px" : null,
          "1fr",
          this.footer ? this.footerHeight + "px" : null
        ]
          .filter(i => !!i)
          .join(" "),
        gridTemplateColumns: [
          this.nav ? this.navWidth + "px" : null,
          "1fr",
          this.aside ? this.asideWidth + "px" : null
        ]
          .filter(i => !!i)
          .join(" ")
      };
    },
    themeStyles() {
      return this.currentTheme.styles;
    }
  }
};
</script>

<style lang="less" module>
@import "./style/main.less";
</style>
