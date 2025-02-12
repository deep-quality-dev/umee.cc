<template>
  <div
    class="container mx-auto h-[95px] flex justify-between align-middle py-3 items-center md:mb-6"
  >
    <NuxtLink to="/">
      <SVGUmeeLogo class="w-[130px] md:w-[162px]"></SVGUmeeLogo>
    </NuxtLink>

    <div class="text-lg flex items-center">
      <a
        class="hidden md:inline-block"
        target="_blank"
        href="https://app.umee.cc/"
        >Markets</a
      >
      <a
        class="hidden md:inline-block ml-6"
        target="_blank"
        href="https://wallet.keplr.app/#/umee/governance"
        >Governance</a
      >
      <a
        class="hidden md:inline-block ml-6"
        href="https://docs.umee.cc/umee/"
        target="_blank"
        >Docs</a
      >

      <div
        class="hidden md:inline-block ml-6 relative cursor-pointer"
        @mouseover="subMenu = true"
        @mouseleave="subMenu = false"
      >
        More

        <svg
          class="inline-block relative ml-1 top-[-2px]"
          width="15"
          height="15"
          viewBox="0 0 15 15"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M7.5 0V15"
            class="stroke-navy dark:stroke-white"
            stroke-width="1.3"
            stroke-miterlimit="10"
          />
          <path
            d="M0 7.5H15"
            class="stroke-navy dark:stroke-white"
            stroke-width="1.3"
            stroke-miterlimit="10"
          />
        </svg>
        <transition name="fade" appear mode="out-in">
          <div v-if="subMenu" class="absolute top-0 pt-[45px] right-0 z-10">
            <ul
              class="p-6 bg-white rounded-3xl shadow text-[17px] text-midGreyOnWhite min-w-[190px]"
            >
              <li>
                <a
                  class="py-1 block hover:text-navy"
                  target="_blank"
                  href="/umee-whitepaper.pdf"
                  >Whitepaper</a
                >
              </li>
              <li>
                <NuxtLink class="py-1 block hover:text-navy" to="/faqs"
                  >FAQs</NuxtLink
                >
              </li>
              <li>
                <a
                  class="py-1 block hover:text-navy"
                  target="_blank"
                  href="https://medium.com/umeeblog"
                  >Blog</a
                >
              </li>
              <li>
                <a
                  class="py-1 block hover:text-navy"
                  target="_blank"
                  href="https://twitter.com/Umee_CrossChain "
                  >Twitter</a
                >
              </li>
              <li>
                <a
                  class="py-1 block hover:text-navy"
                  target="_blank"
                  href="https://discord.gg/umee"
                  >Discord</a
                >
              </li>
              <li>
                <a
                  class="py-1 block hover:text-navy"
                  target="_blank"
                  href="https://github.com/umee-network"
                  >Github</a
                >
              </li>
              <li>
                <a
                  class="py-1 block hover:text-navy"
                  target="_blank"
                  href="https://t.me/umeecrosschain"
                  >Telegram</a
                >
              </li>
              <li>
                <a
                  class="py-1 block hover:text-navy"
                  target="_blank"
                  href="https://www.reddit.com/r/UmeeCrossChain/"
                  >Reddit</a
                >
              </li>
              <li>
                <a
                  class="py-1 block hover:text-navy"
                  target="_blank"
                  href="https://drive.google.com/drive/folders/1A9G2HM5RAka4FLGyVvRC4NeazpAYBh7Z?usp=sharing"
                  >Media Kit</a
                >
              </li>
            </ul>
          </div>
        </transition>
      </div>

      <!-- <UIButtonGradient
        href="https://app.umee.cc/"
        class="ml-6 hidden md:inline-block"
        >Launch App</UIButtonGradient
      > -->

      <div
        v-if="account !== undefined"
        class="ml-6 px-8 py-3 rounded-full text-center"
        style="background: #eee"
      >
        {{ account }}
      </div>

      <UILightMode />
      <button
        class="text-xl md:hidden cursor-pointer ml-2"
        @click="showMobileMenu"
      >
        Menu
        <svg
          class="inline-block ml-2 relative -top-px"
          width="15"
          height="15"
          viewBox="0 0 15 15"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            class="stroke-navy dark:stroke-white"
            d="M7.5 0V15"
            stroke="#16183C"
            stroke-width="1.3"
            stroke-miterlimit="10"
          />
          <path
            class="stroke-navy dark:stroke-white"
            d="M0 7.5H15"
            stroke="#16183C"
            stroke-width="1.3"
            stroke-miterlimit="10"
          />
        </svg>
      </button>
    </div>
    <transition name="fade" appear mode="out-in">
      <LayoutMobileMenu
        v-show="mobileMenu"
        @closeMobileMenu="mobileMenu = false"
      ></LayoutMobileMenu>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      mobileMenu: false,
      subMenu: false,
      account: undefined,
    }
  },
  mounted() {
    window.onload = async () => {
      if (!window.keplr) {
        alert('Please install kelpr extension')
      } else {
        const chainId = 'umee-1' // 'cosmoshub-4'

        // Enabling before using the Keplr is recommended.
        // This method will ask the user whether or not to allow access if they haven't visited this website.
        // Also, it will request user to unlock the wallet if the wallet is locked.
        await window.keplr.enable(chainId)

        const offlineSigner = window.getOfflineSigner(chainId)

        // You can get the address/public keys by `getAccounts` method.
        // It can return the array of address/public key.
        // But, currently, Keplr extension manages only one address/public key pair.
        // XXX: This line is needed to set the sender address for SigningCosmosClient.
        const accounts = await offlineSigner.getAccounts()

        this.account = accounts[0].address
      }
    }
  },
  methods: {
    showMobileMenu() {
      this.mobileMenu = !this.mobileMenu
    },
  },
}
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
.connect-wallet {
  background: linear-gradient(to right, #fda9ff, #c9b8ff, #4dffe5);
  border-radius: 9999px;
}
</style>
