# localTerrainForCesium

<b>Skapa quantized mesh från en TIF fil</b><br>
1. Installera docker desktop och skapa ett konto
2. Starta docker, och se till att de körs i linux-container mode
3. Öppna PowerShell
4. docker pull tumgis/ctb-quantized-mesh, här blir ni promtad för log in för att kunna hämta imaget
5. Kopiera och kör följande i PowerShell, här kopplar ni mot mappen ni har tif filen (här ligger det i mappen grey): <br>
docker run -it --name ctb `
    -v "C:\users\avalna\docker-ctb\grey:/data" `
  tumgis/ctb-quantized-mesh
6. Skapa en map för output filer: `mkdir -p terrain`
7. Skapa en virtuell fil: `gdalbuildvrt tiles.vrt *.tif`
8. Inspektera filen: `gdalinfo tiles.vrt`
9. Skapa quantized mesh: `ctb-tile -f Mesh -C -N -o terrain tiles.vrt`
10. Skapa layer.json fil: `ctb-tile -f Mesh -C -N -l -o terrain tiles.vrt`
11. Kör detta skript för att unzippa filerna [unzip_terrain_tiles.sh](https://github.com/bertt/cesium_terrain/blob/main/unzip_terrain_tiles.sh)
<br><br>
Du bör nu ha mappar från 0-17 eller en lägre siffra (beror lite på hur stort data är) och en layer.json fil i din mapp terrain.<br>
I CesiumJS kodningen sätter du terrängen genom `Cesium.CesiumTerrainProvider.fromUrl("/mappen terrain/")`


<br><br>
<b>Att notera:</b><br> Vid stora filer krävs många kärnor och mycket RAM-minne för att kunna slutföra körningen på punkt 9. Det går att öka andelen resurser docker tillåts använda genom en wsl.config fil placerad under C:\users\user 

<br><br>
Hur själva terrängen strömmas går att konfigueras på flera sätt. Ex kan [Cesium-terrain-server](https://hub.docker.com/r/geodata/cesium-terrain-server/dockerfile) användas eller en IIS-lösning. Stegen ovan fungerar till att läsa från en virtuell mapp i IIS.
