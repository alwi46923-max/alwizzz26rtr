
<html>
<head>
    <title>HP LOST TRACKER</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        body {
            background: black;
            color: #00ff00;
            font-family: monospace;
            padding: 20px;
        }
        button {
            background: black;
            color: #00ff00;
            border: 1px solid #00ff00;
            padding: 10px 15px;
            cursor: pointer;
        }
        #terminal {
            margin-top: 20px;
            white-space: pre-line;
        }
    </style>
</head>
<body>

<h3>dev:alwizz:~#</h3>
<button onclick="cekLokasi()">cek_lokasi --device</button>

<div id="terminal"></div>

<script>
function cekLokasi() {
    const term = document.getElementById("terminal");
    term.innerHTML = "Mengakses GPS...\n";

    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(async function(position) {

            const lat = position.coords.latitude;
            const lon = position.coords.longitude;

            term.innerHTML += "Koordinat ditemukan:\n";
            term.innerHTML += "LAT: " + lat + "\n";
            term.innerHTML += "LON: " + lon + "\n\n";
            term.innerHTML += "Melakukan reverse geocoding...\n";

            const response = await fetch(
                `https://nominatim.openstreetmap.org/reverse?format=json&lat=${lat}&lon=${lon}`
            );
            const data = await response.json();

            const alamat = data.address;

            term.innerHTML += "\nHASIL LOKASI:\n";
            term.innerHTML += (alamat.country || "-") + " / ";
            term.innerHTML += (alamat.state || "-") + " / ";
            term.innerHTML += (alamat.city || alamat.town || "-") + " / ";
            term.innerHTML += (alamat.road || "-") + " ";
            term.innerHTML += (alamat.house_number || "") + "\n";

            term.innerHTML += "\nStatus: SUCCESS\n";

        }, function() {
            term.innerHTML += "Gagal mendapatkan izin lokasi.\n";
        });
    } else {
        term.innerHTML += "Browser tidak mendukung GPS.\n";
    }
}
</script>

</body>
</html>
