<template>
    <div class="map-index">
        <div id="map"></div>
        <div id="features"></div>
    </div>
</template>

<script>
    import {accessToken, mapStyle, mapCerter, mapZoom, mapBearing, mapPitch} from "@/consts/MapConfigConst"
    import {onMounted, toRefs, toRef} from "vue"

    export default {
        name: "Mapbox",

        props: {
            mapOnLoadCB: {
                required: false,
                type: Function,
                default: () => {
                }
            }
        },

        setup(props) {

            let initMapInstance = () => {
                mapboxgl.accessToken = accessToken;

                let map = new mapboxgl.Map({
                    container: "map",
                    style: mapStyle.streets,
                    center: mapCerter,
                    zoom: mapZoom,
                    bearing: mapBearing,
                    pitch: mapPitch,
                });

                mapboxgl.setRTLTextPlugin("https://api.mapbox.com/mapbox-gl-js/plugins/mapbox-gl-rtl-text/v0.2.3/mapbox-gl-rtl-text.js");
                map.addControl(new MapboxLanguage({
                    defaultLanguage: "zh-Hans", // zh-Hant
                }));

                map.on("load", function () {
                    props.mapOnLoadCB(map);
                });
            };

            onMounted(initMapInstance);

            return {}
        }
    }
</script>

<style scoped lang="stylus">
    .map-index
        width: 100%;
        height: 100%;
        position: relative
        #map
            width: 100%;
            height: 100%;
            >>> .mapboxgl-control-container
                .mapboxgl-ctrl-bottom-left, .mapboxgl-ctrl-bottom-right
                    display: none;
        #features
            display none
            position: absolute
            right: 0
            top: 0
            width: 400px
            height: 100%
            background-color rgba(255, 255, 255, 0.8);
            overflow auto
</style>