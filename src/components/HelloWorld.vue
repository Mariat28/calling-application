<template>
  <div class="w-full h-full flex flex-col items-center gap-3">
    <div class="flex gap-2 justify-center">
      <button
        class="bg-green-600 font-semibold text-white rounded-sm shadow-sm p-2 hover:text-green-600 hover:border hover:border-green-600 hover:bg-transparent"
        @click="initiateMediaDevices"
      >
        Call patient
      </button>
      <button
        class="bg-red-500 font-semibold text-white rounded-sm shadow-sm p-2 hover:text-red-500 hover:border hover:border-red-500 hover:bg-transparent"
        @click="initiateMediaDevices"
      >
        Hang up
      </button>
      <button
        class="bg-slate-500 font-semibold text-white rounded-sm shadow-sm p-2 hover:text-slate-500 hover:border hover:border-slate-500 hover:bg-transparent"
        @click="initiateMediaDevices"
      >
        View logs
      </button>
    </div>
    <div class="bg-black h-[500px] w-1/2" v-if="isVideoOn">caller div</div>
    <div class="bg-slate-100 h-96 w-1/3 p-2" v-else>
      <span>Select preferred device from the connected devices list</span>
      <div v-for="(device, index) in deviceList" :key="index">
        <input type="text" class="outline-none p-2" :value="device" readonly />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      isVideoOn: false,
      deviceList: [],
    };
  },

  methods: {
    initiateMediaDevices() {
      const constraints = {
        video: true,
        audio: true,
      };
      navigator.mediaDevices
        .getUserMedia(constraints)
        .then((stream) => {
          console.log("got media stream", stream);
          this.getConnectedDevices("video");
        })
        .catch((error) => {
          console.error("Error accessing media devices", error);
        });
    },
    async getConnectedDevices(type) {
      this.devices = await navigator.mediaDevices.enumerateDevices();
      console.log("connected devices", this.devices);
      return this.devices.filter((device) => device.kind === type);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
