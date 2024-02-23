<template>
  <div id="cesiumContainer"></div>
</template>

<script setup lang="ts">
  import * as Cesium from 'cesium';
  import { isArrayLiteralExpression } from 'typescript';
  import { onMounted, ref } from 'vue';

  const viewer = ref();
  onMounted(async () => {
    Cesium.Ion.defaultAccessToken =
      'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2YzYwNDIzOC01MzU0LTQ5ZDktYmI2ZC0zNDk2YmUyYjFiMGYiLCJpZCI6MTk2OTU3LCJpYXQiOjE3MDg1MTE5ODB9.lJLauNKD4LxSXXUNlmvmF_155ermOZcWiraAl9VAZdo';
    viewer.value = new Cesium.Viewer('cesiumContainer', {
      infoBox: false, // 禁用沙箱，解决控制台报错
      shouldAnimate: true
      // timeline: false,
      // animation:false,
    });

    //设置地形
    viewer.value.scene.setTerrain(new Cesium.Terrain(Cesium.CesiumTerrainProvider.fromIonAssetId(1)));
    const position = Cesium.Cartesian3.fromDegrees(116.39, 39.912, 1500);
    //设置观察点
    // viewer.value.camera.setView({
    //   destination: position //设置目的地
    //   // orientation: {
    //   //   //设置视口方向
    //   //   heading: Cesium.Math.toRadians(0), //控制视口方向水平旋转,为0表示正北方向
    //   //   pitch: Cesium.Math.toRadians(-40), //视口上下旋转,-90度俯视朝向地面
    //   //   roll: 0 //控制视口的翻转角度
    //   // }
    // });
    // viewer.value.camera.viewBoundingSphere(
    //   new Cesium.BoundingSphere(position, 1000),
    //   new Cesium.HeadingPitchRange(30, 0, 0)
    // );
    // let redPolygon = viewer.value.entities.add({
    //   id: 'RedPolygon',
    //   polygon: {
    //     hierarchy: Cesium.Cartesian3.fromDegreesArray([116.39, 39.91, 116.39, 39.915, 116.395, 39.91]),
    //     material: Cesium.Color.RED,
    //     extrudedHeight: 200 //拉伸高度
    //   }
    // });
    // let bluePolygon = viewer.value.entities.add({
    //   id: 'BluePolygon',
    //   polygon: {
    //     hierarchy: Cesium.Cartesian3.fromDegreesArray([116.38, 39.92, 116.38, 39.915, 116.4, 39.92]),
    //     material: Cesium.Color.BLUE,
    //     extrudedHeight: 200 //拉伸高度
    //   }
    // });
    // let yellowPolygon = viewer.value.entities.add({
    //   id: 'YellowPolygon',
    //   polygon: {
    //     hierarchy: Cesium.Cartesian3.fromDegreesArray([116.375, 39.905, 116.37, 39.915, 116.375, 39.92]),
    //     material: Cesium.Color.YELLOW,
    //     extrudedHeight: 200 //拉伸高度
    //   }
    // });
    // viewer.value.entities.remove(bluePolygon);
    // viewer.value.entities.getById('RedPolygon').polygon.material = Cesium.Color.GREEN;
    // viewer.value.entities.removeAll();
    // viewer.value.entities.add({
    //   id: 'planeLogo',
    //   position: Cesium.Cartesian3.fromDegrees(116.39, 39.91, 0),
    //   plane: {
    //     plane: new Cesium.Plane(Cesium.Cartesian3.UNIT_Z, 0.0),
    //     dimensions: new Cesium.Cartesian2(400, 400), //面的长度和宽度
    //     material: '../src/libs/images/blog.png', //显示材质
    //     outline: true, //显示边框
    //     outlineColor: Cesium.Color.BLACK
    //   },
    //   description: `欢迎加入`
    // });
    // let handler = new Cesium.ScreenSpaceEventHandler(viewer.value.scene.canvas); //创建一个屏幕控制实例
    // //监听各种点击、移入事件等
    // //第一个参数传入回调函数；第二个传入监听鼠标点击事件类型
    // handler.setInputAction(function (movement: any) {
    //   let pick = viewer.value.scene.pick(movement.position);
    //   if (Cesium.defined(pick) && pick.id.id === 'planeLogo') {
    //     alert('欢迎关注我的博客');
    //   }
    // }, Cesium.ScreenSpaceEventType.LEFT_CLICK);
    // viewer.value.camera.flyTo({
    //   destination: new Cesium.Cartesian3(-2703640.80485846, -4261161.990345464, 3887439.511104276),
    //   orientation: new Cesium.HeadingPitchRoll(0.22426651143535548, -0.2624145362506527, 0.000006972977223185239),
    //   duration: 0
    // });
    //加载3D Tiles数据
    // const tileset = viewer.value.scene.primitives.add(await Cesium.Cesium3DTileset.fromIonAssetId(96188));
    // viewer.value.clock.shouldAnimate = true;
    // viewer.value.clock.multiplier = 1000;
    // let start = Cesium.JulianDate.fromIso8601('2011-01-05');
    // let end = Cesium.JulianDate.fromIso8601('2011-01-20');
    // viewer.value.timeline.zoomTo(start, end);
    //添加飞机模型
    const orientation = Cesium.Transforms.headingPitchRollQuaternion(position, new Cesium.HeadingPitchRoll(-90, 0, 0));
    var entity = viewer.value.entities.add({
      position: position,
      orientation: orientation,
      model: {
        uri: '../src/SampleData/models/CesiumAir/Cesium_Air.glb',
        minimumPixelSize: 100,
        maximumScale: 10000,
        show: true
      }
    });
    viewer.value.camera.viewBoundingSphere(
      new Cesium.BoundingSphere(position, 20),
      new Cesium.HeadingPitchRange(0, 0, 0)
    );
    //加载粒子类的数据需要使用primitives 该数据类型更接近底层的图形开发
    //ParticleSystem 粒子系统管理粒子集合的更新和显示
    viewer.value.scene.primitives.add(
      new Cesium.ParticleSystem({
        image: '../src/SampleData/fire.png', //粒子样式
        imageSize: new Cesium.Cartesian2(20, 20), //粒子大小
        startScale: 1.0, //初始大小
        endScale: 4.0, //结束大小
        particleLife: 3.0, //粒子存在的时间
        speed: 5.0,
        emitter: new Cesium.CircleEmitter(0.5),
        emissionRate: 5.0,
        modelMatrix: entity.computeModelMatrix(viewer.value.clock.startTime, new Cesium.Matrix4()),
        lifetime: 16.0
      })
    );
    // viewer.value.entities.add({
    //   position: Cesium.Cartesian3.fromDegrees(116.39, 39.912, 810),
    //   label: {
    //     text: '3D模型飞机',
    //     font: '50px Helvetica',
    //     fillColor: Cesium.Color.SKYBLUE
    //   }
    // });
    //添加实体-线段
    // viewer.value.entities.add({
    //   polyline: {
    //     show: true,
    //     positions: Cesium.Cartesian3.fromDegreesArrayHeights([116.39, 39.91, 100, 116.4, 39.91, 200]),
    //     width: 5,
    //     material: new Cesium.Color(0, 0, 1, 1)
    //   }
    // });

    //  viewer.value.imageryLayers.addImageryProvider(
    //  await Cesium.IonImageryProvider.fromAssetId(4),
    //  );
    //添加建筑模型
    //   const titleset = viewer.value.scene.primitives.add(
    //     await Cesium.Cesium3DTileset.fromIonAssetId(96188),
    //   );
    //   //改变视口，将位置定位在陆家嘴
    //   viewer.value.camera.setView({
    //     destination: Cesium.Cartesian3.fromDegrees(121.49, 31.23, 2000),
    //     orientation: {
    //       heading: 20,
    //       pitch: -20,
    //       roll:0
    //     }
    //   })

    //   titleset.style = new Cesium.Cesium3DTileStyle({
    //     color: "color('blue',0.5)",
    //     show: true
    // })
  });
  // viewer.value.scene.globe.show = false;
  //scene类使用
  // viewer.value.scene.camera.setView({
  //   destination:Cesium.Cartesian3.fromDegrees(116.39,39.9,1500)
  // })

  // entity使用
  // const entity = viewer.value.entities.add({
  //   position: Cesium.Cartesian3.fromDegrees(116.39, 39.91, 400),
  //   point: {
  //     pixelSize: 100,
  //     color: new Cesium.Color(0,1,0,1)
  //   }
  // })
  // viewer.value.trackedEntity = entity;
  // viewer.value.dataSource.add(
  //   Cesium.GeoJsonDataSource.load("../../**.topojson")
  // )
  //setView方法

  //   viewer.value.camera.flyTo({
  //     destination: position,
  //       orientation: {//设置视口方向
  //       heading: Cesium.Math.toRadians(0),//控制视口方向水平旋转,为0表示正北方向
  //       pitch: Cesium.Math.toRadians(-90),//视口上下旋转,-90度俯视朝向地面
  //       roll:0//控制视口的翻转角度
  //     },
  //     duration:5,
  //  })
  //lookAt方法
  // const heading = Cesium.Math.toRadians(50);
  // const pitch = Cesium.Math.toRadians(-90);
  // const range = 2500;
  // viewer.value.camera.lookAt(position, new Cesium.HeadingPitchRange(heading, pitch, range));
  //viewBoundingSphere方法
  // const position = Cesium.Cartesian3.fromDegrees(116.39, 39.91, 1500);

  // });
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
  html,
  body,
  #cesiumContainer {
    width: 100vw;
    height: 100vh;
    padding: 0;
    margin: 0;
    overflow: hidden;
  }
</style>
