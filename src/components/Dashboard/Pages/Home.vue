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
        </div>
    </div>
</template>

<style lang="scss" scoped>
@font-face {
    font-family: modernify;
    src: url(/public/fonts/Noyh-Regular.ttf);
}
.box-1{
    flex: 1;
    margin: 0px 4% 0px 4%;
    border-radius: 20px;
    margin-top: 50px;
    table{
        font-size: 15px;
        border-radius: 10px;
        width: 100%;
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
// .box-2{
//     flex: 1;
//     max-width: 300px;
//     margin-top: 50px;
//     margin-right: 4%;
//     height: 244px;
//     border-radius: 20px;
//     table{
//         font-size: 15px;
//         border-radius: 10px;
//         width: 296.5px;
//         height: 244px;
//         background: #473080;
//         a{
//             color:#D1D3D4;
//         }
//         color:#D1D3D4;
//         tr{
//             th{
//             color:#D1D3D4;
//             border-color: black;
//         }
//         td{
//             border-color: black;
//         }
//         }
//     }
// }
// .column-1{
//     display: flex;
// }

</style>

<script lang="ts" setup>

if ("geolocation" in navigator) {
  // Konum bilgilerini iste
  navigator.geolocation.getCurrentPosition(function(position) {
    const latitude = position.coords.latitude; // Enlem
    const longitude = position.coords.longitude; // Boylam

    // Ters jeokodlama yapmak için bir harita hizmeti veya API kullanabilirsiniz.
    // Örnek olarak, OpenCage Geocoder API kullanımı:
    const apiKey = "8f0ef856b7ba49748396d1832e6f7473"; // OpenCage API anahtarınızı buraya ekleyin
    const apiUrl = `https://api.opencagedata.com/geocode/v1/json?q=${latitude}+${longitude}&key=${apiKey}`;

    fetch(apiUrl)
      .then(response => response.json())
      .then(data => {
        if (data.results.length > 0) {
          const city = data.results[0].components.city;
          console.log("Şehir: " + city);
          setTimeout(() => {
            alert(city)
          }, 20000);
        } else {
          console.error("Şehir bilgisi alınamadı.");
        }
      })
      .catch(error => {
        console.error("Hata: " + error.message);
      });
  }, function(error) {
    console.error("Konum bilgisi alınamadı. Hata kodu: " + error.code);
  });
} else {
  console.error("Tarayıcınız konum bilgisi desteği sunmuyor.");
}


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