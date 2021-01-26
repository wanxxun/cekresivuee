<template>
  <div id="app">
     <title>w6n cek resi</title>
    <main>
     <h3 class="header-title">w6n cek resi</h3>
   
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Masukan Resi.."
          v-model="resi"
          @keypress="fetchResi"
        />
        <select class="select-box" v-model="kurir">
          <option disabled selected>Pilih Kurir...</option>
          <option value="jne">JNE</option>
          <option value="jnt">J&T</option>
          <option value="pos">POS INDONESIA</option>
        </select>
      </div>
         <div v-if="loading" class="loading">
          Loading...
          </div>
      <div class="resi-wrapper" v-if="typeof fullresi.data != 'undefined'">
        <div class="resi-box">
          <h3>Data</h3>
          <div class="resi">Resi : {{ fullresi.data.detail.code }}</div>
          <div class="receiver">
            PENERIMA : {{ fullresi.data.detail.receiver }}
          </div>
          <div class="position">
            POSISI : {{ fullresi.data.detail.current_position }}
          </div>
          <div class="date-receiver">
            WAKTU : {{ fullresi.data.detail.date_received }}
          </div>
        </div>
        <div class="timeline-box" >
          <h3 class="">History</h3>
        <div v-for="item in fullresi.data.detail.history " :key="item.history">
             <div class="position-hisoty" >
            {{ item.position }}
          </div>
             <div class="desc-hisoty">
            {{ item.desc }}
          </div>
          <div class="time-hisoty">
            {{ item.time }}
          </div>
       
        </div>
        
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "W6N Cek Resi",
  data() {
    return {
      loading: false,
      kurir: "",
      resi: "",
      fullresi: {},
    };
  },
  methods: {
    fetchResi(e) {
      if (e.key == "Enter") {
        this.loading = true;
        var myHeaders = new Headers();
        myHeaders.append("Content-Type", "application/x-www-form-urlencoded");

        var urlencoded = new URLSearchParams();
        urlencoded.append("kurir", this.kurir);
        urlencoded.append("resi", this.resi);

        var requestOptions = {
          method: "POST",
          headers: myHeaders,
          body: urlencoded,
        };
        fetch(
          "https://w6nresi.netlify.app/cek-tarif-ongkir/front/resi-amp?__amp_source_origin=https%3A%2F%2Fpluginongkoskirim.com/",
          requestOptions
        )
          .then((response) => response.json())
          .then(this.setResult);
      }
    },
    setResult(result) {
      this.loading = false;
      this.fullresi = result;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  /* background-image: url('./assets/bg.jpg'); */

  background-size: cover;
  background-position: bottom;
  transition: 0.45s;
}
main {
  min-height: 100vh;
  padding: 25px;
}
.header-title{
  display: flex;
  margin-bottom: 20px;
  justify-content: center;
  align-items: center;
  font-size: 36px;
  text-transform: uppercase;
  font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
}
.search-box .select-box{
  width: 100%;
}
.loading{
  margin: 10px 10px;
  text-align: center;
  font-size: 36px;
  text-transform: uppercase;
  background-color: rgba(206, 206, 206, 0.5);
  box-shadow: 0 0 10px 10px rgba(187, 187, 187, 0.3);
  border-radius: 5px 5px 5px 5px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 5px;

  color: #313131;
  font-size: 25px;
  text-align: center;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  background-color: rgba(175, 175, 175, 0.5);

  border-radius: 5px 5px 5px 5px;
  transition: 0.4s;
}
.select-box {
  display: block;
  width: 100%;
  padding: 10px;
  margin-top: 5px;

  color: #313131;
  font-size: 17px;
  text-align: center;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  background-color: rgba(175, 175, 175, 0.5);

  border-radius: 5px 5px 5px 5px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0 0 10px 10px rgba(187, 187, 187, 0.3);
  background-color: rgba(177, 177, 177, 0.5);
  border-radius: 10px 10px 10px 10px;
}

.resi-wrapper .resi-box {
  width: 100%;
  padding: 10px 10px;
  margin-top: 10px;
  margin-bottom: 10px;
  background: #d8d8d842;
  border-radius: 10px 10px 10px 10px;
  box-shadow: 0 10px 10px 5px #a0a0a085;
  font-family: "Courier New", Courier, monospace;
  font-weight: bold;
}
.resi-wrapper .timeline-box {
  width: 100%;
  padding: 10px 10px;
  margin-top: 10px;
  margin-bottom: 10px;
  background: #d8d8d842;
  border-radius: 10px 10px 10px 10px;
  box-shadow: 10px 10px 10px 10px #a0a0a085;
  font-family: "Courier New", Courier, monospace;
}
.timeline-box{
  margin-bottom: 10px;
}
.time-hisoty{
  border-bottom: 2px solid rgba(177, 177, 177, 0.5);
  margin-bottom: 20px;
  font-weight: bold;
}
</style>
