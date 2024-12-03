<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GeoIndo - Jelajah Indonesia</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
      background-color: #e0e3eb;
      color: #333;
      padding: 10px;
    }
    h1 {
      color: #01080e;
      text-align: center;
      margin-bottom: 20px;
      font-size: 24px;
    }
    .container {
      max-width: 800px;
      margin: auto;
      text-align: center;
    }
    .info-box {
      margin-top: 20px;
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 5px;
      background-color: #ffffff;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    }
    input {
      padding: 14px;
      font-size: 18px;
      margin-bottom: 20px;
      width: 80%;
    }
    img {
      max-width: 100%;
      height: auto;
      border-radius: 5px;
      margin-top: 15px;
      display: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>GeoIndo - Jelajah Indonesia</h1>
    <p>Masukkan nama provinsi untuk melihat informasi:</p>
    <input 
      type="text" 
      id="provinceInput" 
      placeholder="Masukkan nama provinsi" 
      oninput="checkProvince()"
    />

    <div id="infoBox" class="info-box" style="display: none;">
      <h2 id="provinceName"></h2>
      <p><strong>Suku:</strong> <span id="ethnicity"></span></p>
      <p><strong>Makanan Khas:</strong> <span id="food"></span></p>
      <p><strong>Baju Adat:</strong> <span id="traditionalClothes"></span></p>
      <p><strong>Ibukota:</strong> <span id="capital"></span></p>
      <img id="provinceImage" alt="Gambar Provinsi">
    </div>
  </div>

  <script>
    const provinces = {
      jakarta: {
        name: "DKI Jakarta",
        ethnicity: "Betawi",
        food: "Kerak Telor",
        traditionalClothes: "Baju Sadariah",
        capital: "Jakarta",
        image: "https://www.jakarta.go.id/uploads/contents/content--20230330031636.png"
      },
      jawa_barat: {
        name: "Jawa Barat",
        ethnicity: "Sunda",
        food: "Peuyeum",
        traditionalClothes: "Kebaya Sunda",
        capital: "Bandung",
        image: "https://jabarprov.go.id/images/about/logo.svg"
      },
      bali: {
        name: "Bali",
        ethnicity: "Bali",
        food: "Ayam Betutu",
        traditionalClothes: "Pakaian Adat Bali",
        capital: "Denpasar",
        image: "https://iconlogovector.com/uploads/images/2023/06/lg-e5eea3f633cc7443d1608746d4844e1558.jpg"
      },
      aceh: {
        name: "Aceh",
        ethnicity: "Aceh",
        food: "Mie Aceh",
        traditionalClothes: "Pakaian Adat Aceh",
        capital: "Banda Aceh",
        image: "https://upload.wikimedia.org/wikipedia/commons/thumb/1/14/Flag_of_Aceh.svg/1200px-Flag_of_Aceh.svg.png"
      },
      sumatra_utara: {
        name: "Sumatra Utara",
        ethnicity: "Batak",
        food: "Bika Ambon",
        traditionalClothes: "Ulos Batak",
        capital: "Medan",
        image: "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a5/Flag_of_North_Sumatra.svg/1200px-Flag_of_North_Sumatra.svg.png"
      },
      sumatra_barat: {
        name: "Sumatra Barat",
        ethnicity: "Minangkabau",
        food: "Rendang",
        traditionalClothes: "Baju Kurung Minangkabau",
        capital: "Padang",
        image: "https://upload.wikimedia.org/wikipedia/commons/6/60/Flag_of_West_Sumatra.svg"
      },
      riau: {
        name: "Riau",
        ethnicity: "Melayu",
        food: "Sate Riau",
        traditionalClothes: "Baju Kurung Melayu",
        capital: "Pekanbaru",
        image: "https://upload.wikimedia.org/wikipedia/commons/2/29/Flag_of_Riau.svg"
      },
      jambi: {
        name: "Jambi",
        ethnicity: "Melayu",
        food: "Tempoyak",
        traditionalClothes: "Baju Kurung Melayu",
        capital: "Jambi",
        image: "https://upload.wikimedia.org/wikipedia/commons/c/ca/Flag_of_Jambi.svg"
      },
      sumatera_selatan: {
        name: "Sumatra Selatan",
        ethnicity: "Palembang",
        food: "Pempek",
        traditionalClothes: "Pakaian Adat Palembang",
        capital: "Palembang",
        image: "https://upload.wikimedia.org/wikipedia/commons/9/9e/Flag_of_South_Sumatra.svg"
      },
      bengkulu: {
        name: "Bengkulu",
        ethnicity: "Bengkulu",
        food: "Rendang",
        traditionalClothes: "Baju Adat Bengkulu",
        capital: "Bengkulu",
        image: "https://upload.wikimedia.org/wikipedia/commons/d/d9/Flag_of_Bengkulu.svg"
      },
      lampung: {
        name: "Lampung",
        ethnicity: "Lampung",
        food: "Seruit",
        traditionalClothes: "Baju Adat Lampung",
        capital: "Bandar Lampung",
        image: "https://upload.wikimedia.org/wikipedia/commons/e/ef/Flag_of_Lampung.svg"
      },
      kepulauan_riau: {
        name: "Kepulauan Riau",
        ethnicity: "Melayu",
        food: "Gulai Ikan",
        traditionalClothes: "Baju Kurung Melayu",
        capital: "Tanjungpinang",
        image: "https://upload.wikimedia.org/wikipedia/commons/9/95/Flag_of_Kepulauan_Riau.svg"
      },
      bangka_belitung: {
        name: "Bangka Belitung",
        ethnicity: "Melayu",
        food: "Lempah Kuning",
        traditionalClothes: "Baju Kurung Melayu",
        capital: "Pangkal Pinang",
        image: "https://drive.google.com/file/d/1v9MoMs_l_bU3zztH8mwsV-R8CRZbMMrd/view?usp=drive_link"
      },
      diy: {
        name: "DIY Yogyakarta",
        ethnicity: "Jawa",
        food: "Gudeg",
        traditionalClothes: "Kebaya Yogyakarta",
        capital: "Yogyakarta",
        image: "https://upload.wikimedia.org/wikipedia/commons/9/91/Flag_of_Yogyakarta.svg"
      },
      jawa_tengah: {
        name: "Jawa Tengah",
        ethnicity: "Jawa",
        food: "Nasi Liwet",
        traditionalClothes: "Kebaya Jawa",
        capital: "Semarang",
        image: "https://upload.wikimedia.org/wikipedia/commons/c/c9/Flag_of_Central_Java.svg"
      },
      jawa_timur: {
        name: "Jawa Timur",
        ethnicity: "Jawa",
        food: "Rawon",
        traditionalClothes: "Kebaya Jawa Timur",
        capital: "Surabaya",
        image: "https://upload.wikimedia.org/wikipedia/commons/7/71/Flag_of_East_Java.svg"
      },
      banten: {
        name: "Banten",
        ethnicity: "Sunda",
        food: "Sate Bandeng",
        traditionalClothes: "Kebaya Sunda",
        capital: "Serang",
        image: "https://upload.wikimedia.org/wikipedia/commons/d/d2/Flag_of_Banten.svg"
      },
      ntb: {
        name: "Nusa Tenggara Barat",
        ethnicity: "Sasak",
        food: "Ayam Taliwang",
        traditionalClothes: "Baju Sasak",
        capital: "Mataram",
        image: "https://upload.wikimedia.org/wikipedia/commons/3/3d/Flag_of_West_Nusa_Tenggara.svg"
      },
      ntt: {
        name: "Nusa Tenggara Timur",
        ethnicity: "Flores",
        food: "Ikan Bakar",
        traditionalClothes: "Baju Adat NTT",
        capital: "Kupang",
        image: "https://upload.wikimedia.org/wikipedia/commons/3/39/Flag_of_East_Nusa_Tenggara.svg"
      },
      kalimantan_barat: {
        name: "Kalimantan Barat",
        ethnicity: "Dayak",
        food: "Sate Pompom",
        traditionalClothes: "Pakaian Adat Dayak",
        capital: "Pontianak",
        image: "https://upload.wikimedia.org/wikipedia/commons/a/a6/Flag_of_West_Kalimantan.svg"
      },
      kalimantan_tengah: {
        name: "Kalimantan Tengah",
        ethnicity: "Dayak",
        food: "Ikan Patin",
        traditionalClothes: "Pakaian Adat Dayak",
        capital: "Palangkaraya",
        image: "https://upload.wikimedia.org/wikipedia/commons/a/a2/Flag_of_Central_Kalimantan.svg"
      },
      kalimantan_selatan: {
        name: "Kalimantan Selatan",
        ethnicity: "Banjar",
        food: "Soto Banjar",
        traditionalClothes: "Pakaian Adat Banjar",
        capital: "Banjarmasin",
        image: "https://upload.wikimedia.org/wikipedia/commons/d/d7/Flag_of_South_Kalimantan.svg"
      },
      kalimantan_timur: {
        name: "Kalimantan Timur",
        ethnicity: "Kutai",
        food: "Ikan Bakar",
        traditionalClothes: "Pakaian Adat Kutai",
        capital: "Samarinda",
        image: "https://upload.wikimedia.org/wikipedia/commons/3/38/Flag_of_East_Kalimantan.svg"
      },
      kalimantan_utara: {
        name: "Kalimantan Utara",
        ethnicity: "Dayak",
        food: "Kwetiau",
        traditionalClothes: "Pakaian Adat Dayak",
        capital: "Tanjung Selor",
        image: "https://upload.wikimedia.org/wikipedia/commons/6/69/Flag_of_North_Kalimantan.svg"
      },
      sulawesi_utara: {
        name: "Sulawesi Utara",
        ethnicity: "Minahasa",
        food: "Cakalang Fufu",
        traditionalClothes: "Pakaian Adat Minahasa",
        capital: "Manado",
        image: "https://upload.wikimedia.org/wikipedia/commons/2/2f/Flag_of_North_Sulawesi.svg"
      },
      sulawesi_tengah: {
        name: "Sulawesi Tengah",
        ethnicity: "Bugis",
        food: "Ikan Bakar",
        traditionalClothes: "Baju Adat Bugis",
        capital: "Palu",
        image: "https://upload.wikimedia.org/wikipedia/commons/9/94/Flag_of_Central_Sulawesi.svg"
      },
      sulawesi_selatan: {
        name: "Sulawesi Selatan",
        ethnicity: "Bugis",
        food: "Coto Makassar",
        traditionalClothes: "Baju Adat Makassar",
        capital: "Makassar",
        image: "https://upload.wikimedia.org/wikipedia/commons/3/3f/Flag_of_South_Sulawesi.svg"
      },
      sulawesi_tenggara: {
        name: "Sulawesi Tenggara",
        ethnicity: "Buton",
        food: "Ikan Tuna",
        traditionalClothes: "Baju Adat Buton",
        capital: "Kendari",
        image: "https://upload.wikimedia.org/wikipedia/commons/d/d7/Flag_of_Southeast_Sulawesi.svg"
      },
      gorontalo: {
        name: "Gorontalo",
        ethnicity: "Gorontalo",
        food: "Ikan Bakar",
        traditionalClothes: "Baju Adat Gorontalo",
        capital: "Gorontalo",
        image: "https://upload.wikimedia.org/wikipedia/commons/0/0e/Flag_of_Gorontalo.svg"
      },
      sulawesi_barat: {
        name: "Sulawesi Barat",
        ethnicity: "Mandar",
        food: "Kue Mandar",
        traditionalClothes: "Baju Adat Mandar",
        capital: "Mamuju",
        image: "https://upload.wikimedia.org/wikipedia/commons/6/69/Flag_of_West_Sulawesi.svg"
      },
      maluku: {
        name: "Maluku",
        ethnicity: "Maluku",
        food: "Papeda",
        traditionalClothes: "Pakaian Adat Maluku",
        capital: "Ambon",
        image: "https://upload.wikimedia.org/wikipedia/commons/9/91/Flag_of_Maluku.svg"
      },
      maluku_utara: {
        name: "Maluku Utara",
        ethnicity: "Ternate",
        food: "Ikan Bakar",
        traditionalClothes: "Pakaian Adat Ternate",
        capital: "Ternate",
        image: "https://upload.wikimedia.org/wikipedia/commons/5/56/Flag_of_North_Maluku.svg"
      },
      papua: {
        name: "Papua",
        ethnicity: "Papua",
        food: "Sate Kelinci",
        traditionalClothes: "Pakaian Adat Papua",
        capital: "Jayapura",
        image: "https://upload.wikimedia.org/wikipedia/commons/e/ec/Flag_of_Papua_Province.svg"
      },
      papua_barat: {
        name: "Papua Barat",
        ethnicity: "Papua",
        food: "Ikan Bakar",
        traditionalClothes: "Pakaian Adat Papua Barat",
        capital: "Manokwari",
        image: "https://upload.wikimedia.org/wikipedia/commons/6/68/Flag_of_West_Papua.svg"
      }
    };

    function checkProvince() {
      const input = document.getElementById("provinceInput").value.toLowerCase().trim();
      const formattedInput = input.replace(/\s+/g, "_"); // Mengganti spasi dengan underscore
      const infoBox = document.getElementById("infoBox");
      const data = provinces[formattedInput];

      if (data) {
        document.getElementById("provinceName").innerText = data.name;
        document.getElementById("ethnicity").innerText = data.ethnicity;
        document.getElementById("food").innerText = data.food;
        document.getElementById("traditionalClothes").innerText = data.traditionalClothes;
        document.getElementById("capital").innerText = data.capital;
        document.getElementById("provinceImage").src = data.image;
        document.getElementById("provinceImage").style.display = "block";
        infoBox.style.display = "block";
      } else {
        infoBox.style.display = "none";
      }
    }
    
  </script>
</body>
</html>
