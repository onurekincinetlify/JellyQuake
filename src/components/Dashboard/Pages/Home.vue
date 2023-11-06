<template>
    <div class="home-container">
        <!-- <button @click="Deprem" class="button">Check</button>
        <h1 id="veriler"></h1> -->
        <div class="column-1">
            <div class="box-1">
          <table class="table">
            <thead>
                <tr>
                    <th>Tarih</th>
                    <th>Saat</th>
                    <th>Enlem(N)</th>
                    <th>Boylam(E)</th>
                    <th>Derinlik(KM)</th>
                    <th>ML(Büyüklüğü)</th>
                    <th>Yer</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <th id="Tarih-1"></th>
                    <td id="Saat-1"></td>
                    <td id="Enlem-1"></td>
                    <td id="Boylam-1"></td>
                    <td id="Derinlik-1"></td>
                    <td id="ML-1"></td>
                    <td id="Yer-1"></td>
                </tr>
                <tr>
                    <th id="Tarih-2"></th>
                    <td id="Saat-2"></td>
                    <td id="Enlem-2"></td>
                    <td id="Boylam-2"></td>
                    <td id="Derinlik-2"></td>
                    <td id="ML-2"></td>
                    <td id="Yer-2"></td>
                </tr>
                <tr>
                    <th id="Tarih-3"></th>
                    <td id="Saat-3"></td>
                    <td id="Enlem-3"></td>
                    <td id="Boylam-3"></td>
                    <td id="Derinlik-3"></td>
                    <td id="ML-3"></td>
                    <td id="Yer-3"></td>
                </tr>
            </tbody>
        </table>  
        </div>

        <!-- <div class="box-2">
            <table class="table">
            <thead>
                <tr>
                    <th>Tarih</th>
                    <th>Düzeltilen Hata Sayısı</th>
                    <th>Verisyon</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <th>2023.12.11</th>
                    <td>27</td>
                    <td>1.2</td>
                </tr>
                <tr>
                    <th>2023.11.06</th>
                    <td>15</td>
                    <td>1.1</td>
                </tr>
                <tr>
                    <th>2023.10.14</th>
                    <td>42</td>
                    <td>1.0</td>
                </tr>
            </tbody>
        </table>  
        </div> -->
        </div>
    </div>
</template>

<style lang="scss" scoped>
@font-face {
    font-family: modernify;
    src: url(../../public/fonts/Noyh-Regular.ttf);
}
.box-1{
    flex: 1;
    margin: 0px 0px 0px 4%;
    border-radius: 20px;
    margin-top: 50px;
    table{
        font-size: 15px;
        border-radius: 10px;
        width: 450px;
        background: #473080;
        a{
            color:#D1D3D4;
        }
        color:#D1D3D4;
        tr{
            th{
            color:#D1D3D4;
            border-color: black;
        }
        td{
            border-color: black;
        }
        }
    }
}
.box-2{
    flex: 1;
    max-width: 300px;
    margin-top: 50px;
    margin-right: 4%;
    height: 244px;
    border-radius: 20px;
    table{
        font-size: 15px;
        border-radius: 10px;
        width: 296.5px;
        height: 244px;
        background: #473080;
        a{
            color:#D1D3D4;
        }
        color:#D1D3D4;
        tr{
            th{
            color:#D1D3D4;
            border-color: black;
        }
        td{
            border-color: black;
        }
        }
    }
}
.column-1{
    display: flex;
}

</style>

<script lang="ts" setup>

function EarthQuake(){
    fetch('https://api.orhanaydogdu.com.tr/deprem/kandilli/live?limit=3')
  .then(response => response.json())
  .then(json => {
    const results = json.result;
    for (let i = 0; i < 3; i++) {
      const tarihElement = document.getElementById(`Tarih-${i + 1}`);
      const saatElement = document.getElementById(`Saat-${i + 1}`);
      const enlemElement = document.getElementById(`Enlem-${i + 1}`);
      const boylamElement = document.getElementById(`Boylam-${i + 1}`);
      const derinlikElement = document.getElementById(`Derinlik-${i + 1}`);
      const mlElement = document.getElementById(`ML-${i + 1}`);
      const yerElement = document.getElementById(`Yer-${i + 1}`);

      if (
        tarihElement &&
        saatElement &&
        enlemElement &&
        boylamElement &&
        derinlikElement &&
        mlElement &&
        yerElement
      ) {
        tarihElement.innerHTML = results[i].date.split(" ")[0];
        saatElement.innerHTML = results[i].date.split(" ")[1];
        enlemElement.innerHTML = results[i].geojson.coordinates[1];
        boylamElement.innerHTML = results[i].geojson.coordinates[0];
        derinlikElement.innerHTML = results[i].depth;
        mlElement.innerHTML = results[i].mag;
        yerElement.innerHTML = results[i].title;
      }
    }
  });
}

function baslangicFonksiyonu() {
  EarthQuake()
  setInterval(function() {
    EarthQuake()
  }, 20000);
}
baslangicFonksiyonu();
</script>