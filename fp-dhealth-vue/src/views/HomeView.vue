<template>
  <div class="home">
    <button
        class="text-xl border border-black w-1/3 mx-auto block transform transition-all hover:bg-gray-200 hover:scale-125"
        @click="connectWallet">Connect Wallet
    </button>
    <button
        class="text-xl border border-black w-1/3 mt-6 mx-auto block transform transition-all hover:bg-gray-200 hover:scale-125"
        @click="attachContract">Attach Contract
    </button>
    <div class="mx-auto">
      <img class="mx-auto w-1/4" src="../assets/logo_dhealth.png" alt="Logo">
    </div>
    <div class="xl:w-4/5 xl:mx-auto">
      <div class="text-center mb-10 mx-8">
        <h1 class="text-2xl font-bold">Welcome, account {{ this.currentAccount }}</h1>
      </div>
    </div>
  </div>
</template>

<script>
import Users from '../../../fp-dhealth-hardhat/artifacts/contracts/Users.sol/Users.json';
import {ethers} from "ethers";

export default {
  name: 'HomeView',
  data() {
    return {
      currentAccount: null,
      currentContract: null,
      contractAddress:
          "0x4419C04C75BC89f8858d19eb8df39957c2134fE2",
    };
  },
  methods: {
    checkIfWalletIsConnected: async function () {
      try {
        const {ethereum} = window;
        if (!ethereum) {
          alert("Make sure you have the Metamask browser plugin!");
          return;
        } else {
          console.log("We have the ethereum object", ethereum);
        }
        const accounts = await ethereum.request({method: "eth_accounts"});
        if (accounts.length !== 0) {
          const account = accounts[0];
          console.log("Found an authorized account:", account);
          this.currentAccount = account;
        } else {
          console.log("No authorized account found");
        }
      } catch (error) {
        console.log(error);
      }
    },
    connectWallet: async function () {
      try {
        const {ethereum} = window;
        if (!ethereum) {
          alert("Get MetaMask!");
          return;
        }
        const accounts = await ethereum.request({
          method: "eth_requestAccounts",
        });
        console.log("Connected", accounts[0]);
        this.currentAccount = accounts[0];
      } catch (error) {
        console.log(error);
      }
    },
    attachContract: async function () {
      const {ethereum} = window;
      const provider = new ethers.providers.Web3Provider(ethereum);
      const signer = provider.getSigner();

      // const network = await provider.getNetwork();
      // const chainId = network.chainId;
      // console.log(chainId);

      const abi = Users.abi;
      this.currentContract = new ethers.Contract(this.contractAddress, abi, signer);
      if (this.currentContract != null) {
        console.log("It worked!");
      }
    }
  },
  mounted() {
    this.checkIfWalletIsConnected();
  }
};
</script>
