<template>
  <div class="tygismap">
    <div id="gis-map"></div>
  </div>
</template>
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
<script>
import Map from 'ol/Map';
import View from 'ol/View';
import axios from 'axios';
import ImageArcGISRest from 'ol/source/ImageArcGISRest';
import ImageLayer from 'ol/layer/Image';
import VectorSource from 'ol/source/Vector';
import VectorLayer from 'ol/layer/Vector';
//import EsriJSON from 'ol/format/EsriJSON';
import GeoJSON from 'ol/format/GeoJSON';
import { Style, Text, Fill } from 'ol/style';
import Point from 'ol/geom/Point';
import Feature from 'ol/Feature';
import 'ol/ol.css';
export default {
  name: 'gis-map',
  components: {},
  props: {
    onLoadSuccess: {
      type: Function,
      default: () => {
        console.log('gis-map Load Success');
      },
      required: false,
    },
    mapType: {
      type: Number,
      default: 0,
      required: false,
    },
    msg: String,
  },
  mounted: function () {
    let arcgisUrl =
      'http://47.100.34.182:6080/arcgis/rest/services/HeYanLu/v3A2mud/MapServer';
    let arcgisSource = new ImageArcGISRest({
      url: arcgisUrl,
    });
    let arcgisLayer = new ImageLayer({
      source: arcgisSource,
    });
    // let vecSource = new VectorSource({
    //   format: new GeoJSON(),
    //   url:
    //     'http://47.100.34.182:6080/arcgis/rest/services/HeYanLu/v3A2mud/MapServer/1/query?where=1%3D1&outFields=*&returnGeometry=true&f=pjson',
    // });


    let geojsonObject = { "type": "FeatureCollection", "crs": { "type": "name", "properties": { "name": "EPSG:3857" } }, "features": [{ "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [778.75537109375, -145.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [778.75537109375, -173.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [976.75537109375, -771.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [976.75537109375, -799.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [31.75537109375, -72.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [121.75537014007568, -26.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [21.582517623901367, -26.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [350.75537109375, -26.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [248.18310546875, -26.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [31.75537109375, -114.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [31.75537109375, -162.267578125] } }, { "type": "Feature", "properties": { "name": "1 m3/h" }, "geometry": { "type": "Point", "coordinates": [133.75537000596523, -114.267578125] } },] }
    let vecSource = new VectorSource({
      features: (new GeoJSON()).readFeatures(geojsonObject)
    });

    let style = new Style({
      text: new Text({
        font: 'bold 12px Microsoft YaHei',
        offsetX: 20,
        offsetY: -5,
        fill: new Fill({
          color: '#8a2be2',
        }),
      }),
    });

    let styleFunc = function (feature) {
      style.getText().setText(feature.get('name'));
      return style;
    };

    let vecLayer = new VectorLayer({
      source: vecSource,
      style: styleFunc,
    });

    let map = new Map({
      layers: [arcgisLayer, vecLayer],
      target: 'gis-map',
      renderer: 'canvas',
      resolution: 'resolution',
      view: new View({
        center: [699.079, -389.213],
        // center: [25.514963714000032, -331.26747581899997],
        // extent: [
        //   -16.799623007523146e-4,
        //   -0.006911708872603376,
        //   0.012884308435221536,
        //   10.380463273920597e-4,
        // ],
        zoom: 17.3,
        // maxZoom: 18,
        // minZoom: 17.2,
        enableRotation: false,
        projection: 'EPSG:3857',
      }),
      controls: [],
      // interactions: [new DragAndDrop()],
      // pixelRatio: 1
    });
    console.log(map.getInteractions());
    // let coords = [0.010119521000035547, -0.0063509979999594179]; // 坐标
    // let coords = [958.71632905500019, -305.26755766399998];
    let coords = [31.75537109375, -72.267578125];
    let text = 'hahah';
    let anchorValue = new Feature({
      geometry: new Point(coords),
    });
    anchorValue.setStyle(
      new Style({
        text: new Text({
          font: 'bold 12px Microsoft YaHei',
          text: text,
          offsetX: 5,
          offsetY: -5,
          fill: new Fill({
            // color: '#1fe086',
            color: '#8a2be2',
          }),
        }),
      })
    );
    vecLayer.getSource().addFeature(anchorValue);
  },
};

</script>
<style>
#gis-map {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0 px;
  bottom: 0 px;
  background-color: rgba(0, 0, 0, 0.9);
  z-index: 1000;
}
.tygismap {
  width: 100%;
  height: 100%;
}
</style>
