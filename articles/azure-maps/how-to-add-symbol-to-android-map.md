---
title: Android maps에 기호 계층 추가 | Microsoft Azure 맵
description: 지도에 표식을 추가 하는 방법에 대해 알아봅니다. Azure Maps Android SDK를 사용 하 여 데이터 원본의 지점 기반 데이터를 포함 하는 기호 계층을 추가 하는 예제를 참조 하세요.
author: anastasia-ms
ms.author: v-stharr
ms.date: 04/26/2019
ms.topic: how-to
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: acd5f06a5383308ce736f2860810ebee7e5bce28
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92897112"
---
# <a name="add-a-symbol-layer-to-a-map-using-azure-maps-android-sdk"></a>Azure Maps Android SDK를 사용 하 여 지도에 기호 계층 추가

이 문서에서는 Azure Maps Android SDK를 사용 하 여 데이터 소스의 요소 데이터를 맵의 기호 계층으로 렌더링 하는 방법을 보여 줍니다.

## <a name="prerequisites"></a>사전 준비 사항

이 문서의 단계를 완전히 따르려면 지도를 로드 하는 [Azure Maps Android SDK](./how-to-use-android-map-control-library.md) 를 설치 해야 합니다.

## <a name="add-a-symbol-layer"></a>기호 레이어 추가

기호 계층을 사용 하 여 지도에 표식을 추가 하려면 다음 단계를 수행 합니다.

1. **res**  >  **layout**  >  다음 XML 처럼 표시 되도록 res 레이아웃 **activity_main.xml** 를 편집 합니다.
    
    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <FrameLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.microsoft.azure.maps.mapcontrol.MapControl
            android:id="@+id/mapcontrol"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:mapcontrol_centerLat="47.64"
            app:mapcontrol_centerLng="-122.33"
            app:mapcontrol_zoom="12"
            />

    </FrameLayout>
    ```

2. 클래스의 **onCreate ()** 메서드에 다음 코드 조각을 복사 `MainActivity.java` 합니다.

    ```Java
    mapControl.onReady(map -> {
    
        //Create a data source and add it to the map.
        DataSource dataSource = new DataSource();
        map.sources.add(dataSource);
    
        //Create a point feature and add it to the data source.
        dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
    
        //Add a custom image icon to the map resources.
        map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
    
        //Create a symbol layer and add it to the map.
        map.layers.add(new SymbolLayer(dataSource,
            iconImage("my-icon")));
        });
    
    ```
    
    위의 코드 조각은 먼저 **Onready ()** 콜백 메서드를 사용 하 여 Azure Maps map 컨트롤 인스턴스를 가져옵니다. 그런 다음 **DataSource** 클래스를 사용 하 여 데이터 소스 개체를 만들어 맵에 추가 합니다. 그런 다음 점 기 하 도형을 포함 하는 **기능** 을 추가 합니다. 그러면 빨간색 표식 이미지가 기호에 대 한 아이콘으로 설정 됩니다. **기호 계층** 은 텍스트 또는 아이콘을 사용 하 여 데이터 원본에 래핑된 지점 기반 데이터를 맵의 기호로 렌더링 합니다. 그런 다음 기호 계층이 만들어지고 데이터 원본이 렌더링에 전달 된 다음 지도 계층에 추가 됩니다.
    
    위의 코드 조각을 추가한 후에 `MainActivity.java` 는 아래와 같이 표시 됩니다.
    
    ```Java
    package com.example.myapplication;
    
    import android.app.Activity;
    import android.os.Bundle;
    import com.mapbox.geojson.Feature;
    import com.mapbox.geojson.Point;
    import com.microsoft.azure.maps.mapcontrol.AzureMaps;
    import com.microsoft.azure.maps.mapcontrol.MapControl;
    import com.microsoft.azure.maps.mapcontrol.layer.SymbolLayer;
    import com.microsoft.azure.maps.mapcontrol.source.DataSource;
    import static com.microsoft.azure.maps.mapcontrol.options.SymbolLayerOptions.iconImage;
    public class MainActivity extends AppCompatActivity {
        
        static{
                AzureMaps.setSubscriptionKey("<Your Azure Maps subscription key>");
            }
    
        MapControl mapControl;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mapControl = findViewById(R.id.mapcontrol);
    
            mapControl.onCreate(savedInstanceState);
    
            mapControl.onReady(map -> {
    
                //Create a data source and add it to the map.
                DataSource dataSource = new DataSource();
                map.sources.add(dataSource);
            
                //Create a point feature and add it to the data source.
                dataSource.add(Feature.fromGeometry(Point.fromLngLat(-122.33, 47.64)));
            
                //Add a custom image icon to the map resources.
                map.images.add("my-icon", R.drawable.mapcontrol_marker_red);
            
                //Create a symbol layer and add it to the map.
                map.layers.add(new SymbolLayer(dataSource,
                    iconImage("my-icon")));
            });
        }
    
        @Override
        public void onStart() {
            super.onStart();
            mapControl.onStart();
        }
    
        @Override
        public void onResume() {
            super.onResume();
            mapControl.onResume();
        }
    
        @Override
        public void onPause() {
            super.onPause();
            mapControl.onPause();
        }
    
        @Override
        public void onStop() {
            super.onStop();
            mapControl.onStop();
        }
    
        @Override
        public void onLowMemory() {
            super.onLowMemory();
            mapControl.onLowMemory();
        }
    
        @Override
        protected void onDestroy() {
            super.onDestroy();
            mapControl.onDestroy();
        }
    
        @Override
        protected void onSaveInstanceState(Bundle outState) {
            super.onSaveInstanceState(outState);
            mapControl.onSaveInstanceState(outState);
        }
    }
    ```
    
이제 응용 프로그램을 실행 하는 경우 다음과 같이 맵에 마커가 표시 됩니다.

<center>

![Android 지도 핀](./media/how-to-add-symbol-to-android-map/android-map-pin.png)</center>

> [!TIP]
> 기본적으로 기호 레이어는 겹치는 기호를 숨겨 기호 렌더링을 최적화 합니다. 확대 하는 동안 숨겨진 기호가 표시 됩니다. 이 기능을 사용 하지 않도록 설정 하 고 모든 기호를 항상 렌더링 하려면 `iconAllowOverlap` 옵션을로 설정 `true` 합니다.

## <a name="next-steps"></a>다음 단계

맵에 추가 정보를 추가 하려면 다음을 참조 하세요.

> [!div class="nextstepaction"]
> [Android 맵에 도형 추가](./how-to-add-shapes-to-android-map.md)

> [!div class="nextstepaction"]
> [기능 정보 표시](display-feature-information-android.md)