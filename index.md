<!--********************************************************************
* Copyright© 2000 - 2019 SuperMap Software Co.Ltd. All rights reserved.
*********************************************************************-->
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="jsbridge-mini.js"></script>
    <script src="ol.js"></script>
    <link rel="stylesheet" href="index.css">
    <link rel="stylesheet" href="ol.css">
</head>
<body>
<div id="map" style="width: 100%;height:100%"></div>
<script type="text/javascript">
    var map;
    var projection = ol.proj.get('EPSG:4326');
    let projectionExtent = projection.getExtent();
    let size = ol.extent.getWidth(projectionExtent) / 256;
    let resolutions = [];
    for (let z = 2; z < 24; ++z) {
        resolutions[z] = size / Math.pow(2, z);
    }
    var layers = [
        new ol.layer.Tile({
            source: new ol.source.XYZ({
                url: 'https://t0.tianditu.gov.cn/DataServer?T=img_w&x={x}&y={y}&l={z}&tk=d7ecfd92b803b1a5f4dde0673945e0f4'
            }),
            isGroup: true,
            name: 'lebal',
            zIndex: 0
        })
        ]
    map = new ol.Map({
        target: 'map',
        layers: this.layers,
        view: new ol.View({
            // center: [115.9145, 31.7286],
            center: [117.227, 31.772],
            zoom: 17.5,
            projection: 'EPSG:4326',
            maxZoom: 20.5,
            minZoom: 17,
            // extent: [115.91, 31.72, 115.92, 31.735]
        })
    });


    var locationsource = new ol.source.Vector({wrapX: false});
    var locationvector = new ol.layer.Vector({
        source: locationsource,
        zIndex:101
    });
    locationvector.set("name","locationvector");
    map.addLayer(locationvector);






    //-------------------------------------------------------------h5获取位置---------------------------------------------------------//

    //发起定位，开启后请拿着手机走起来
    //在室外开启GPS定位会更准确
/*    jsBridge.bdloc.getCurrentPosition({
        coorType: 'WGS84',
        watch   : true
    }, function(position){
        locationnow(position.latitude,position.longitude);
    });
    function locationnow(latitude,longitude) {
        locationsource.clear();
        var iconFeature = new ol.Feature({
            geometry: new ol.geom.Point([longitude,latitude])
        });
        var imgsrc = "locaton.png";
        var markstyle = new ol.style.Style({
            image: new ol.style.Icon({
                src: imgsrc,
                scale:0.15
            })
        });
        iconFeature.setStyle(markstyle);
        locationsource.addFeature(iconFeature);
        map.setView( new ol.View({
          center: [longitude,latitude],
        zoom: 17,
        projection: 'EPSG:4326'
        }));
        curcoor = [];
        curcoor.push([longitude,latitude]);

    }*/












    function getPosition(){

        //判断浏览器是否支持HTML5 定位

        if(window.navigator.geolocation){
            //navigator.geolocation.getCurrentPosition(successfulCallback,failCallback,{enableHeightAcuracy:true,timeout:1000,maximumAge:3000})
            navigator.geolocation.watchPosition(showPosition,errobak,{provider:'system',coordsType: 'wgs84',enableHighAccuracy:true,timeout:5000,maximumAge:20000});

        }else{

            alert("你的浏览器不能使用HTML5定位")

        }

    }
    function errobak() {
        alert("GPS网络差");
        // locationsource.clear();
        // var latitude = 31.72971;
        // var longitude = 115.916939;
        // // alert(latitude+"|||"+longitude);
        // var iconFeature = new ol.Feature({
        //     geometry: new ol.geom.Point([longitude,latitude])
        // });
        // var imgsrc = "../../static/img/start_trans.png";
        // var markstyle = new ol.style.Style({
        //     image: new ol.style.Icon({
        //         src: imgsrc,
        //         scale:3
        //     })
        // });
        // iconFeature.setStyle(markstyle);
        // locationsource.addFeature(iconFeature);
        // map.setView( new ol.View({
        //     center: [longitude,latitude],
        //     zoom: 18,
        //     projection: 'EPSG:4326'
        // }));
        // curcoor = [];
        // curcoor.push([longitude,latitude]);
    }








    function showPosition(position)
    {
        locationsource.clear();
        var latitude = position.coords.latitude;
        var longitude = position.coords.longitude;
        var iconFeature = new ol.Feature({
            geometry: new ol.geom.Point([longitude,latitude])
        });
        var imgsrc = "locaton.png";
        var markstyle = new ol.style.Style({
            image: new ol.style.Icon({
                src: imgsrc,
                scale:0.15
            })
        });
        iconFeature.setStyle(markstyle);
        locationsource.addFeature(iconFeature);
        //map.setView( new ol.View({
        //   center: [longitude,latitude],
        //zoom: 18,
        // projection: 'EPSG:4326'
        //}));
        curcoor = [];
        curcoor.push([longitude,latitude]);
    }

    function successfulCallback(position){
        locationsource.clear();
        var latitude = position.coords.latitude-0.0394;
        var longitude = position.coords.longitude-1.3152;
        alert(latitude+"|||"+longitude);
        var iconFeature = new ol.Feature({
            geometry: new ol.geom.Point([longitude,latitude])
        });
        var imgsrc = "../../static/img/start_trans.png";
        var markstyle = new ol.style.Style({
            image: new ol.style.Icon({
                src: imgsrc,
                scale:3
            })
        });
        iconFeature.setStyle(markstyle);
        locationsource.addFeature(iconFeature);
        map.setView( new ol.View({
            center: [longitude,latitude],
            zoom: 20,
            projection: 'EPSG:4326'
        }));

    }

    function failCallback(error){

        var text ;

        switch(error.code){

            case error.PERMISSION_DENIED:

                text ="用户拒绝对获取地理位置的请求。";

                break;
            case error.POSITION_UNAVAILABLE:

                text ="位置信息是不可用的。";

                break;

            case error.TIMEOUT:

                text ="请求用户地理位置超时。";

                break;

            case error.UNKNOWN_ERROR:

                text ="未知错误。";

                break;
        }
        alert(text);

    }


    /*
      调用安卓端获取实时gps位置
     */
     getPosition();

    //-------------------------------------------------------------h5获取位置---------------------------------------------------------//




</script>
</body>
</html>
