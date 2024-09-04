<template>
    <div>
        <button @click="geoFind">위치찾기</button>
        <p>{{ textContent }}</p>
        <a id="map-link" target="_blank"></a>
        <p>From : 37.681272,126.759484</p>
        <p>To : 37.680342,126.761587</p>
        <p>Distance : {{ Math.floor(distance*1000)/1000 }} km, {{  Math.floor(distance * 1000) }} m</p>

        <p>위도/경도 계산하기</p>
        <p><label>From Latitue:</label><input id="fromLat" name="fromLat" value="37.681272" /></p>
        <p><label>From Longtitude:</label><input id="fromLong" name="fromLong" value="126.759484" /></p>
        <p><label>To Latitue:</label><input id="toLat" name="toLat" value="37.680342" /></p>
        <p><label>To Longtitude:</label><input id="toLong" name="toLong" value="126.761587"/></p>
        <button @click="calculateDistance">거리계산하기</button>
        <p>Distance : {{ Math.floor(calcDistance*1000)/1000 }} kilo meters</p>
        <p>Distance : {{ Math.floor(calcDistance*1000) }} meters</p>
        
        <p>현재 위치표시하기</p>
        <button @click="displayMarker">현재위치</button>
        <div id="map"></div>

        <p>지도에서 마커를 이용해서 노드를 추가한다.</p>
        <p>노드를 추가후, 노드간의 연결정보를 추가하면 각 노드간의 거리를 계산하여 edge에 저장한다</p>
        <p>저장된 vertex, edge 정보를 이용하여 다익스트라 알고리즘으로 최단거리를 계산한다.</p>
        
    </div>
</template>
<script>
export default {
    name: "GeoView",
    props: {
       title: String, 
    },
    data() {
        return {
            latitude:'37.681272',
            longitude:'126.761587',
            textContent:'',
            distance: 0,
            calcDistance:0, 
            map: null,
        }
    },
    mounted() {
        // eslint-disable-next-line
        if(window.kakao && window.kakao.maps) {
            this.loadMap()
        } else {
            this.loadScript()   
        }
    },
    computed: {
    },
    methods: {
        loadScript() {
            const script=document.createElement("script")
            script.src="//dapi.kakao.com/v2/maps/sdk.js?appkey=0eb41667821ebe2f23b7405baef8bcbb&autoload=false"
            // eslint-disable-next-line
            script.onload= () => window.kakao.maps.load(this.loadMap)
            document.head.appendChild(script)

        },
        loadMap() {
            const container = document.getElementById('map');
            const options = {
                // eslint-disable-next-line
                center: new window.kakao.maps.LatLng(this.latitude, this.longitude),
                level: 3,
            };

            // eslint-disable-next-line
            this.map = new kakao.maps.Map(container, options);

            this.loadMarker(this.latitude, this.longitude)
        },
        loadMarker(lat, lng) {
            console.log(lat,lng)
            const markerPosition = new window.kakao.maps.LatLng(lat, lng)
            const marker = new window.kakao.maps.Marker(
                {
                    position: markerPosition,
                }
            )
            marker.setMap(this.map);
        },
        displayMarker() {
            this.loadMap()
            // this.loadMarker(this.latitude, this.longitude)
        },
        // TODO: 지도 거리 계산하기
        geoFind() {

            if(!("geolocation" in navigator)) {
                this.textContent="Geolocation is not available"
                return
            }
            this.textContent="Locating...."
            navigator.geolocation.getCurrentPosition(pos=>{
                this.latitude=pos.coords.latitude
                this.longitude=pos.coords.longitude
                this.textContent='Your location data is ' + this.latitude + ', ' + this.longitude
                
                const mapLink=document.querySelector("#map-link")
                mapLink.textContent=""
                mapLink.href=`https://www.openstreetmap.org/#map=18/${this.latitude}/${this.longitude}`;
                mapLink.textContent = `Latitude: ${this.latitude} °, Longitude: ${this.longitude} °`;

                // this.distance=this.geoDistance(37.566381,126.977717,37.565577,126.978082)
                this.distance=this.geoDistance(37.681272,126.759484,37.680342,126.761587)
                
            }, err=> {
                this.textContent=err.message
            },
            {
                enableHighAccuracy: true, // 정확도 우선 모드
                timeout: 10000,           // 10초 이내에 응답 없으면 에러 발생
                maximumAge: 0             // 항상 최신 위치 정보 수집
            })
            
        },
        // TODO: 거리계산 함수
        geoDistance(fLat, fLong, tLat, tLong) {
            // from - to distance
            // 위도와 경도의 도, 분, 초 차이 제곱을 계산한다.
            const fLatDo = Math.floor(fLat)
            const fLatBun1 = (fLat - fLatDo) * 60
            const fLatBun = Math.floor( fLatBun1 )
            const fLatCho1 = (fLatBun1 - fLatBun) * 60
            const fLatCho = Math.trunc(fLatCho1*100)/100
            
            const fLongDo = Math.floor(fLong)
            const fLongBun1 = (fLong - fLongDo) * 60
            const fLongBun = Math.floor( fLongBun1 )
            const fLongCho1 = (fLongBun1 - fLongBun) * 60
            const fLongCho = Math.trunc(fLongCho1*100)/100
            
            const tLatDo = Math.floor(tLat)
            const tLatBun1 = (tLat - tLatDo) * 60
            const tLatBun = Math.floor( tLatBun1 )
            const tLatCho1 = (tLatBun1 - tLatBun) * 60
            const tLatCho = Math.trunc(tLatCho1*100)/100
            
            const tLongDo = Math.floor(tLong)
            const tLongBun1 = (tLong - tLongDo) * 60
            const tLongBun = Math.floor( tLongBun1 )
            const tLongCho1 = (tLongBun1 - tLongBun) * 60
            const tLongCho = Math.trunc(tLongCho1*100) / 100
            
            console.log("Do:",fLatDo, fLongDo, tLatDo, tLongDo)
            console.log("Bun:",fLatBun, fLongBun, tLatBun, tLongBun)
            console.log("Cho:",fLatCho, fLongCho, tLatCho, tLongCho)
            

            // 위도와 경도를 계산하는 상수 값
            const LatDo = 111.3194 * (tLatDo - fLatDo)
            const LatBun = 1.8553 * (tLatBun - fLatBun)
            const LatCho = 0.0309 * (tLatCho - fLatCho)
            const LongDo = 88.9036 * (tLongDo - fLongDo)
            const LongBun = 1.4817 * (tLongBun - fLongBun)
            const LongCho = 0.0246 * (tLongCho - fLongCho)

            console.log("calcLat:",LatDo,LatBun,LatCho)
            console.log("calcLong:",LongDo,LongBun,LongCho)
            const dLat = LatDo + LatBun + LatCho
            const dLong = LongDo + LongBun + LongCho
            const distance = Math.sqrt(Math.pow(dLat,2) + Math.pow(dLong,2))
            return distance
        },
        // TODO: 좌표를 입력받아서 거리계산하기
        calculateDistance() {
            const fromLatitude = document.querySelector("#fromLat").value
            const fromLongitude = document.querySelector("#fromLong").value
            const toLatitude = document.querySelector("#toLat").value
            const toLongitude = document.querySelector("#toLong").value
            this.calcDistance = this.geoDistance(fromLatitude,fromLongitude,toLatitude,toLongitude)
        },
    }
}
</script>

<style scoped>
    #map {
        width: 100%;
        height:400px;
    }
</style>
