<script lang="ts">
  import Banner from "./components/Banner.svelte";
  import Test from "./posts/test/Test.svx";
  import {
    Header,
    Tabs,
    Tab,
    TabContent,
    HeaderUtilities,
    HeaderAction,
    HeaderActionSearch,
    HeaderGlobalAction,
    HeaderPanelLinks,
    HeaderPanelDivider,
    HeaderPanelLink,
    Content,
    SideNavItems,
    SideNavMenuItem,
    SideNavMenu,
    SideNav,
    SideNavLink,
    SkipToContent,
  } from "carbon-components-svelte";
  import SettingsAdjust20 from "carbon-icons-svelte/lib/SettingsAdjust20";
  let isOpen = false;
  let isSideNavOpen = false;

  $: marginLeft = isSideNavOpen ? 16 : 0;
  $: document.documentElement.style.setProperty("--mleft", marginLeft + "rem");
</script>

<style lang="scss" global>
  /** Gray 10 theme **/
  @import "carbon-components-svelte/css/g10";
  #main-content {
    padding: 0;
  }

  #main-content.bx--content {
    margin-left: var(--mleft);
  }

  .observablehq canvas {
    width: 100%;
  }

  .bx--tabs__nav {
    height: 100%;
  }

  .header-banner {
    height: calc(var(--height) - 3rem);
    width: 100%;
    min-height: 250px;
    display: flex;
    justify-content: left;
    align-items: flex-end;
    padding: 2rem;
    background-color: #000;
    color: #fff;
  }

  .bx--side-nav__overlay {
    z-index: 100;
    position: fixed !important;
  }

  .tabs {
    background-color: #000;
  }

  .bx--tabs {
    position: sticky !important;
    top: 3rem;
    border-top: 1px solid #989696;
    background-color: #000 !important;
  }

  .bx--tabs__nav-item {
    background-color: #000 !important;
  }

  .bx--tabs__nav-link {
    color: #fff !important;
  }

  .bx--side-nav ~ .bx--content {
    margin-left: 0;
  }

  .content-1 {
    height: 1000px;
    background-color: #fff;
  }

  .haha {
    font-size: large;
  }

  .side-nav {
    background-color: #fff222;
  }
</style>

<Header company="MatrixMind" platformName="Carlos" bind:isSideNavOpen>
  <div slot="skip-to-content">
    <SkipToContent />
  </div>

  <HeaderUtilities>
    <HeaderActionSearch />
    <HeaderGlobalAction aria-label="Settings" icon={SettingsAdjust20} />
    <HeaderAction bind:isOpen>
      <HeaderPanelLinks>
        <HeaderPanelDivider>用户</HeaderPanelDivider>
        <HeaderPanelLink>登录</HeaderPanelLink>
        <HeaderPanelDivider>合作</HeaderPanelDivider>
        <HeaderPanelLink>关于</HeaderPanelLink>
        <HeaderPanelLink>支持我</HeaderPanelLink>
      </HeaderPanelLinks>
    </HeaderAction>
  </HeaderUtilities>
</Header>

<SideNav fixed bind:isOpen={isSideNavOpen}>
  <SideNavItems>
    <SideNavLink text="数学" />
    <SideNavLink text="Server" />
    <SideNavLink text="Web" />
    <SideNavMenu text="Algorithm">
      <SideNavMenuItem href="/" text="Link 1" />
      <SideNavMenuItem href="/" text="Link 2" />
      <SideNavMenuItem href="/" text="Link 3" />
    </SideNavMenu>
  </SideNavItems>
</SideNav>

<Content>
  <Banner title="Python 与 数学" />
  <Tabs type="container">
    <Tab label="基础" />
    <Tab label="进阶" />
    <Tab label="应用" />
    <div slot="content">
      <TabContent>
        <div class="content-1">
          <Test />
        </div>
      </TabContent>
      <TabContent>Content 2</TabContent>
      <TabContent>Content 3</TabContent>
    </div>
  </Tabs>
</Content>
