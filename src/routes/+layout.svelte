<script>
  import "./styles.css"
  import "../tailwind.css"
  import logo from "$lib/images/logo.png"
</script>

<div class="app h-full">
  <div class="navbar bg-base-300">
    <div class="flex-1">
      <a
        class="btn btn-ghost normal-case text-xl"
        href="https://cardanocrocsclub.com"
        ><img src={logo} alt="" width="200px" /></a
      >
    </div>
    <div class="flex-none">
      <ul class="menu menu-horizontal px-1">
        <li><a href="/">CCC</a></li>
        <li><a href="/coming-soon">RA</a></li>
        <li><a href="/coming-soon">Babies</a></li>
      </ul>
    </div>
  </div>
  <main>
    <slot />
  </main>
  <footer
    class="pt-2 footer items-center p-4 bg-neutral text-neutral-content b-0"
  >
    <div class="grid grid-cols-1 place-items-center text-center">
      <div class="text-center">
        <p>Copyright © 2022 - $kashmoney</p>
      </div>
    </div>
  </footer>
</div>

<style lang="postcss">
  main {
    @apply pb-4;
  }
  .footer {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
  }
</style>
