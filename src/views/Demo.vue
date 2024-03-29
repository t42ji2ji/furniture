<template lang="pug">
.demoPage
  .outer(:class="{hideDrawer: hideDrawer}")
    .tabs
      .tab(@click="selectedRoom='臥室'" :class="{ activeColor: selectedRoom == '臥室'}") 臥室
      .tab(@click="selectedRoom='客廳'"  :class="{ activeColor: selectedRoom == '客廳'}") 客廳
      .tab(@click="selectedRoom='廚房'"  :class="{ activeColor: selectedRoom == '廚房'}") 廚房
    .box(style="height:5px")
    .tabs
      .tab(@click="selectStyleTab('style')"  :class="{ activeTab: selectedStyleTab == 'style'}") 版型
      .tab(@click="selectStyleTab('texture')"  :class="{ activeTab: selectedStyleTab == 'texture'}") 色號
    .drawer 
      .grid
        .textImg(v-for="style in styles")
          .img(@click="selectStyle(style)" :class="{ active: chosenStyle(style)}" :style="{backgroundImage: 'url('+ require(`../assets/demo/style/${style}.jpg`) +')'}")
          .text {{style}}
      .w(style="width:10px")
      .grid.gridRight
        .textImg(v-for="texture in textures")
          .img(@click="selectTexture(texture)" :class="{ active: chosenTexture(texture)}" :style="{backgroundImage: 'url('+ require(`../assets/demo/texture/${texture}.jpg`) +')'}"  )
          .text {{texture}}
  .x(style="width:100%;height:100%;position='relative'")
    a-scene(@mouseenter="inScene = true"
      @mouseleave="inScene = false" device-orientation-permission-ui="enabled: false" :vr-mode-ui="`enabled: ${vrMode}`" embedded=true v-if="selectedTexture && selectedStyle")
      a-sky(:src="`${prefixUrl}${this.selectedStyle}/${this.selectedRoom}-${this.selectedStyle}_${this.selectedTexture}.jpg`")
      a-entity(id="rig" :position="cameraPosition" rotation="0 90 0")
        a-entity( id="camera" :camera="`zoom:${zoom}`" look-controls-o hand-tracking-controls)
    .monitor 
      .spec-preview
        img(:src='require(`../assets/demo/preview/${selectedStyle}+${selectedTexture}.jpg`)', alt="" style="width:100px;height:120px")
        .event(@mousemove="handler")
        .big
          .pd(ref="big" )
            img(:src='require(`../assets/demo/preview/${selectedStyle}+${selectedTexture}.jpg`)', alt="", )
        //- 遮罩层
        .mask(ref="mask")
      //- .preview(:style="{backgroundImage: 'url('+ require(`../assets/demo/preview/${selectedStyle}+${selectedTexture}.jpg`) +')'}")
      //- div(style="width:100px;height:120px")
      //-   ZoomImage(:src='require(`../assets/demo/preview/${selectedStyle}+${selectedTexture}.jpg`)',
      //-             class="zoom-image"
      //-             :options="{paneContainer: '#zoomContainer' ,inlinePane: false, zoomFactor:2}"
      //-           )
      //-   .absolute#zoomContainer.w-full.h-full.z-50.pointer-events-none
      .text 版型：{{selectedStyle}}
      .text 色號：{{selectedTexture}}
      .text.drawerBtn(@click="hideDrawer = !hideDrawer;init()") {{hideDrawer ?'顯示側欄':'隱藏側欄'}}
</template>

<script>
import * as THREEJS from 'aframe';
import ZoomImage from '../components/ZoomImage.vue';
export default {
  components: {
    ZoomImage,
  },
  name: 'Demo',
  props: {
    vrMode: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      prefixUrl:
        'https://media.githubusercontent.com/media/t42ji2ji/furniture_photo/main/',
      selectedTexture: 'U10',
      hideDrawer: false,
      inScene: true,
      selectedStyle: 'W',
      zoom: 1,
      pinchDelta: 0,
      selectedStyleTab: 'style',
      selectedRoom: '臥室',
      test: '',
      test1: '',
      styles: [
        'F',
        'F-1',
        'F-2',
        'F-4',
        'H',
        'H-1',
        'H-2',
        'U1',
        'U2',
        'U3',
        'U4',
        'U5',
        'M8',
        'W',
        'W1',
        'W2',
        'X6',
        'X6-1',
        'X7',
        'V1',
        'V2',
        'V3',
      ],
      textures: [
        'U10',
        'U20',
        'U30',
        'U40',
        'U50',
        'U60',
        'U70',
        'U80',
        'K01',
        'K02',
        'K03',
        'K04',
        'K05',
        'K06',
        'K07',
        'K08',
        'K09',
        'Y15',
        'Y35',
        'Y45',
        'Y55',
        'Y65',
        'Y95',
        'Y105',
        'Y115',
      ],
    };
  },
  computed: {
    renderUrl() {
      return `../assets/demo/render/${this.selectedStyle}/臥室-${this.selectedStyle}_${this.selectedTexture}.jpg`;
    },
    cameraPosition() {
      return this.selectedRoom == '廚房' ? '80 10 0' : '25 10 0';
    },
  },
  mounted() {
    this.init();
  },
  methods: {
    handler(event) {
      let mask = this.$refs.mask;
      let bigImg = this.$refs.big;
      // 遮罩层可移动的范围：鼠标的当前坐标到该元素的距离（左侧、顶部）减去 遮罩层宽、高的一半(遮罩层是一个以鼠标为中心的正方形)
      let left = event.offsetX - mask.offsetWidth / 2;
      let top = event.offsetY - mask.offsetHeight / 2;

      // 约束遮罩层可移动的范围
      if (left <= 0) left = 0;
      if (left >= mask.offsetWidth) left = mask.offsetWidth;
      if (top <= 0) top = 0;
      if (top >= mask.offsetHeight) top = mask.offsetHeight;

      // 修改元素的left|top属性值
      mask.style.left = left + 'px';
      mask.style.top = top + 'px';
      bigImg.style.left = -2 * left + 'px';
      bigImg.style.top = -2 * top + 'px';
    },

    init() {
      THREEJS;
      const AFRAME = window.AFRAME;
      const THREE = window.THREE;
      var vm = this;

      window.addEventListener('wheel', (event) => {
        if (!vm.inScene) {
          return;
        }
        // small increments for smoother zooming
        const delta = event.wheelDelta / 120 / 100;
        var finalZoom = vm.zoom + delta;

        // limiting the zoom
        if (finalZoom < 0.8) finalZoom = 0.8;
        if (finalZoom > 2) finalZoom = 2;
        this.zoom = finalZoom;
      });

      function bind(fn, ctx /* , arg1, arg2 */) {
        return (function(prependedArgs) {
          return function bound() {
            // Concat the bound function arguments with those passed to original bind
            var args = prependedArgs.concat(
              Array.prototype.slice.call(arguments, 0)
            );
            return fn.apply(ctx, args);
          };
        })(Array.prototype.slice.call(arguments, 2));
      }

      var PI_2 = Math.PI / 2;

      AFRAME.registerComponent('look-controls-o', {
        dependencies: ['position', 'rotation'],

        schema: {
          enabled: { default: true },
          magicWindowTrackingEnabled: { default: true },
          pointerLockEnabled: { default: false },
          reverseMouseDrag: { default: false },
          reverseTouchDrag: { default: false },
          touchEnabled: { default: true },
          mouseEnabled: { default: true },
        },

        init: function() {
          this.deltaYaw = 0;
          this.previousHMDPosition = new THREE.Vector3();
          this.hmdQuaternion = new THREE.Quaternion();
          this.magicWindowAbsoluteEuler = new THREE.Euler();
          this.magicWindowDeltaEuler = new THREE.Euler();
          this.position = new THREE.Vector3();
          this.magicWindowObject = new THREE.Object3D();
          this.rotation = {};
          this.deltaRotation = {};
          this.savedPose = null;
          this.pointerLocked = false;
          this.setupMouseControls();
          this.bindMethods();
          this.previousMouseEvent = {};

          this.setupMagicWindowControls();

          // To save / restore camera pose
          this.savedPose = {
            position: new THREE.Vector3(),
            rotation: new THREE.Euler(),
          };

          // Call enter VR handler if the scene has entered VR before the event listeners attached.
          if (this.el.sceneEl.is('vr-mode') || this.el.sceneEl.is('ar-mode')) {
            this.onEnterVR();
          }
        },

        setupMagicWindowControls: function() {
          // var magicWindowControls;
          // var data = this.data;
          // Only on mobile devices and only enabled if DeviceOrientation permission has been granted.
          //   if (vm.isMobile() || vm.isMobileDeviceRequestingDesktopSite()) {
          //     magicWindowControls = this.magicWindowControls = new THREE.DeviceOrientationControls(
          //       this.magicWindowObject
          //     );
          //     if (
          //       typeof DeviceOrientationEvent !== 'undefined' &&
          //       DeviceOrientationEvent.requestPermission
          //     ) {
          //       magicWindowControls.enabled = false;
          //       if (
          //         this.el.sceneEl.components['device-orientation-permission-ui']
          //           .permissionGranted
          //       ) {
          //         magicWindowControls.enabled = data.magicWindowTrackingEnabled;
          //       } else {
          //         this.el.sceneEl.addEventListener(
          //           'deviceorientationpermissiongranted',
          //           function() {
          //             magicWindowControls.enabled =
          //               data.magicWindowTrackingEnabled;
          //           }
          //         );
          //       }
          //     }
          //   }
        },

        update: function(oldData) {
          var data = this.data;

          // Disable grab cursor classes if no longer enabled.
          if (data.enabled !== oldData.enabled) {
            this.updateGrabCursor(data.enabled);
          }

          // Reset magic window e-rulers if tracking is disabled.
          if (
            oldData &&
            !data.magicWindowTrackingEnabled &&
            oldData.magicWindowTrackingEnabled
          ) {
            this.magicWindowAbsoluteEuler.set(0, 0, 0);
            this.magicWindowDeltaEuler.set(0, 0, 0);
          }

          // Pass on magic window tracking setting to magicWindowControls.
          if (this.magicWindowControls) {
            this.magicWindowControls.enabled = data.magicWindowTrackingEnabled;
          }

          if (
            oldData &&
            !data.pointerLockEnabled !== oldData.pointerLockEnabled
          ) {
            this.removeEventListeners();
            this.addEventListeners();
            if (this.pointerLocked) {
              this.exitPointerLock();
            }
          }
        },

        tick: function() {
          var data = this.data;
          if (!data.enabled) {
            return;
          }
          this.updateOrientation();
        },

        play: function() {
          this.addEventListeners();
        },

        pause: function() {
          this.removeEventListeners();
          if (this.pointerLocked) {
            this.exitPointerLock();
          }
        },

        remove: function() {
          this.removeEventListeners();
          if (this.pointerLocked) {
            this.exitPointerLock();
          }
        },

        bindMethods: function() {
          this.onMouseDown = bind(this.onMouseDown, this);
          this.onMouseMove = bind(this.onMouseMove, this);
          this.onMouseUp = bind(this.onMouseUp, this);
          this.onTouchStart = bind(this.onTouchStart, this);
          this.onTouchMove = bind(this.onTouchMove, this);
          this.onTouchEnd = bind(this.onTouchEnd, this);
          this.onEnterVR = bind(this.onEnterVR, this);
          this.onExitVR = bind(this.onExitVR, this);
          this.onPointerLockChange = bind(this.onPointerLockChange, this);
          this.onPointerLockError = bind(this.onPointerLockError, this);
        },

        /**
         * Set up states and Object3Ds needed to store rotation data.
         */
        setupMouseControls: function() {
          this.mouseDown = false;
          this.pitchObject = new THREE.Object3D();
          this.yawObject = new THREE.Object3D();
          this.yawObject.position.y = 10;
          this.yawObject.add(this.pitchObject);
        },

        /**
         * Add mouse and touch event listeners to canvas.
         */
        addEventListeners: function() {
          var sceneEl = this.el.sceneEl;
          var canvasEl = sceneEl.canvas;

          // Wait for canvas to load.
          if (!canvasEl) {
            sceneEl.addEventListener(
              'render-target-loaded',
              bind(this.addEventListeners, this)
            );
            return;
          }

          // Mouse events.
          canvasEl.addEventListener('mousedown', this.onMouseDown, false);
          window.addEventListener('mousemove', this.onMouseMove, false);
          window.addEventListener('mouseup', this.onMouseUp, false);

          // Touch events.
          canvasEl.addEventListener('touchstart', this.onTouchStart);
          window.addEventListener('touchmove', this.onTouchMove);
          window.addEventListener('touchend', this.onTouchEnd);

          // sceneEl events.
          sceneEl.addEventListener('enter-vr', this.onEnterVR);
          sceneEl.addEventListener('exit-vr', this.onExitVR);

          // Pointer Lock events.
          if (this.data.pointerLockEnabled) {
            document.addEventListener(
              'pointerlockchange',
              this.onPointerLockChange,
              false
            );
            document.addEventListener(
              'mozpointerlockchange',
              this.onPointerLockChange,
              false
            );
            document.addEventListener(
              'pointerlockerror',
              this.onPointerLockError,
              false
            );
          }
        },

        /**
         * Remove mouse and touch event listeners from canvas.
         */
        removeEventListeners: function() {
          var sceneEl = this.el.sceneEl;
          var canvasEl = sceneEl && sceneEl.canvas;

          if (!canvasEl) {
            return;
          }

          // Mouse events.
          canvasEl.removeEventListener('mousedown', this.onMouseDown);
          window.removeEventListener('mousemove', this.onMouseMove);
          window.removeEventListener('mouseup', this.onMouseUp);

          // Touch events.
          canvasEl.removeEventListener('touchstart', this.onTouchStart);
          window.removeEventListener('touchmove', this.onTouchMove);
          window.removeEventListener('touchend', this.onTouchEnd);

          // sceneEl events.
          sceneEl.removeEventListener('enter-vr', this.onEnterVR);
          sceneEl.removeEventListener('exit-vr', this.onExitVR);

          // Pointer Lock events.
          document.removeEventListener(
            'pointerlockchange',
            this.onPointerLockChange,
            false
          );
          document.removeEventListener(
            'mozpointerlockchange',
            this.onPointerLockChange,
            false
          );
          document.removeEventListener(
            'pointerlockerror',
            this.onPointerLockError,
            false
          );
        },

        /**
         * Update orientation for mobile, mouse drag, and headset.
         * Mouse-drag only enabled if HMD is not active.
         */
        updateOrientation: function() {
          var object3D = this.el.object3D;
          var pitchObject = this.pitchObject;
          var yawObject = this.yawObject;
          var sceneEl = this.el.sceneEl;

          // In VR or AR mode, THREE is in charge of updating the camera pose.
          if (
            (sceneEl.is('vr-mode') || sceneEl.is('ar-mode')) &&
            sceneEl.checkHeadsetConnected()
          ) {
            // With WebXR THREE applies headset pose to the object3D internally.
            return;
          }

          this.updateMagicWindowOrientation();

          // On mobile, do camera rotation with touch events and sensors.
          object3D.rotation.x =
            this.magicWindowDeltaEuler.x + pitchObject.rotation.x;
          object3D.rotation.y =
            this.magicWindowDeltaEuler.y + yawObject.rotation.y;
          object3D.rotation.z = this.magicWindowDeltaEuler.z;
        },

        updateMagicWindowOrientation: function() {
          var magicWindowAbsoluteEuler = this.magicWindowAbsoluteEuler;
          var magicWindowDeltaEuler = this.magicWindowDeltaEuler;
          // Calculate magic window HMD quaternion.
          if (this.magicWindowControls && this.magicWindowControls.enabled) {
            this.magicWindowControls.update();
            magicWindowAbsoluteEuler.setFromQuaternion(
              this.magicWindowObject.quaternion,
              'YXZ'
            );
            if (
              !this.previousMagicWindowYaw &&
              magicWindowAbsoluteEuler.y !== 0
            ) {
              this.previousMagicWindowYaw = magicWindowAbsoluteEuler.y;
            }
            if (this.previousMagicWindowYaw) {
              magicWindowDeltaEuler.x = magicWindowAbsoluteEuler.x;
              magicWindowDeltaEuler.y +=
                magicWindowAbsoluteEuler.y - this.previousMagicWindowYaw;
              magicWindowDeltaEuler.z = magicWindowAbsoluteEuler.z;
              this.previousMagicWindowYaw = magicWindowAbsoluteEuler.y;
            }
          }
        },

        /**
         * Translate mouse drag into rotation.
         *
         * Dragging up and down rotates the camera around the X-axis (yaw).
         * Dragging left and right rotates the camera around the Y-axis (pitch).
         */
        onMouseMove: function(evt) {
          var direction;
          var movementX;
          var movementY;
          var pitchObject = this.pitchObject;
          var previousMouseEvent = this.previousMouseEvent;
          var yawObject = this.yawObject;

          // Not dragging or not enabled.
          if (!this.data.enabled || (!this.mouseDown && !this.pointerLocked)) {
            return;
          }

          // Calculate delta.
          if (this.pointerLocked) {
            movementX = evt.movementX || evt.mozMovementX || 0;
            movementY = evt.movementY || evt.mozMovementY || 0;
          } else {
            movementX = evt.screenX - previousMouseEvent.screenX;
            movementY = evt.screenY - previousMouseEvent.screenY;
          }
          this.previousMouseEvent.screenX = evt.screenX;
          this.previousMouseEvent.screenY = evt.screenY;

          // Calculate rotation.
          direction = this.data.reverseMouseDrag ? 1 : -1;
          var rotateLimitDegree = this.selectedRoom == '廚房' ? 0.5 : 0.88;
          if (
            yawObject.rotation.y + movementX * 0.002 * direction <
            -rotateLimitDegree
          ) {
            yawObject.rotation.y = -rotateLimitDegree;
          } else if (
            yawObject.rotation.y + movementX * 0.002 * direction >
            rotateLimitDegree
          ) {
            yawObject.rotation.y = rotateLimitDegree;
          } else {
            yawObject.rotation.y += movementX * 0.002 * direction;
          }

          pitchObject.rotation.x += movementY * 0.002 * direction;
          pitchObject.rotation.x = Math.max(
            -PI_2 * 2,
            Math.min(PI_2 * 2, pitchObject.rotation.x)
          );
        },

        /**
         * Register mouse down to detect mouse drag.
         */
        onMouseDown: function(evt) {
          var sceneEl = this.el.sceneEl;
          if (
            !this.data.enabled ||
            !this.data.mouseEnabled ||
            ((sceneEl.is('vr-mode') || sceneEl.is('ar-mode')) &&
              sceneEl.checkHeadsetConnected())
          ) {
            return;
          }
          // Handle only primary button.
          if (evt.button !== 0) {
            return;
          }

          var canvasEl = sceneEl && sceneEl.canvas;

          this.mouseDown = true;
          this.previousMouseEvent.screenX = evt.screenX;
          this.previousMouseEvent.screenY = evt.screenY;
          this.showGrabbingCursor();

          if (this.data.pointerLockEnabled && !this.pointerLocked) {
            if (canvasEl.requestPointerLock) {
              canvasEl.requestPointerLock();
            } else if (canvasEl.mozRequestPointerLock) {
              canvasEl.mozRequestPointerLock();
            }
          }
        },

        /**
         * Shows grabbing cursor on scene
         */
        showGrabbingCursor: function() {
          this.el.sceneEl.canvas.style.cursor = 'grabbing';
        },

        /**
         * Hides grabbing cursor on scene
         */
        hideGrabbingCursor: function() {
          this.el.sceneEl.canvas.style.cursor = '';
        },

        /**
         * Register mouse up to detect release of mouse drag.
         */
        onMouseUp: function() {
          this.mouseDown = false;
          this.hideGrabbingCursor();
        },

        /**
         * Register touch down to detect touch drag.
         */
        onTouchStart: function(evt) {
          if (
            evt.touches.length !== 1 ||
            !this.data.touchEnabled ||
            this.el.sceneEl.is('vr-mode') ||
            this.el.sceneEl.is('ar-mode')
          ) {
            return;
          }
          this.touchStart = {
            x: evt.touches[0].pageX,
            y: evt.touches[0].pageY,
          };
          this.touchStarted = true;
        },

        /**
         * Translate touch move to Y-axis rotation.
         */
        onTouchMove: function(evt) {
          var direction;
          var canvas = this.el.sceneEl.canvas;
          var deltaY;
          var deltaX;
          var yawObject = this.yawObject;
          var pitchObject = this.pitchObject;

          if (!this.touchStarted || !this.data.touchEnabled) {
            return;
          }
          vm.test = evt.touches.length;
          if (
            evt.touches.length === 2 &&
            (vm.isMobile() || vm.isMobileDeviceRequestingDesktopSite())
          ) {
            const distance = (event) => {
              return Math.hypot(
                event.touches[0].pageX - event.touches[1].pageX,
                event.touches[0].pageY - event.touches[1].pageY
              );
            };
            if (vm.pinchDelta == 0) {
              vm.pinchDelta = distance(evt);
              return;
            }
            evt.preventDefault(); // Prevent page scroll

            var delta = distance(evt);
            var finalZoom = vm.zoom + (delta - vm.pinchDelta) / 100 / 30;

            // limiting the zoom
            if (finalZoom < 0.8) finalZoom = 0.8;
            if (finalZoom > 1.8) finalZoom = 1.8;
            vm.test1 = vm.pinchDelta;
            vm.zoom = finalZoom;
            return;
          }

          deltaY =
            (2 * Math.PI * (evt.touches[0].pageX - this.touchStart.x)) /
            canvas.clientWidth /
            2;
          deltaX =
            (2 * Math.PI * (evt.touches[0].pageY - this.touchStart.y)) /
            canvas.clientHeight /
            2;

          direction = this.data.reverseTouchDrag ? 1 : -1;
          // Limit touch orientaion to to yaw (y axis).
          if (yawObject.rotation.y - deltaY * 0.8 * direction < -0.8) {
            yawObject.rotation.y = -0.8;
          } else if (yawObject.rotation.y - deltaY * 0.8 * direction > 0.8) {
            yawObject.rotation.y = 0.8;
          } else {
            yawObject.rotation.y -= deltaY * 0.8 * direction;
          }

          pitchObject.rotation.x += deltaX * 0.8 * direction;
          pitchObject.rotation.x = Math.max(
            -PI_2,
            Math.min(PI_2, pitchObject.rotation.x)
          );

          this.touchStart = {
            x: evt.touches[0].pageX,
            y: evt.touches[0].pageY,
          };
        },

        /**
         * Register touch end to detect release of touch drag.
         */
        onTouchEnd: function() {
          this.touchStarted = false;
          vm.pinchDelta = 0;
          vm.test1 = vm.pinchDelta;
        },

        /**
         * Save pose.
         */
        onEnterVR: function() {
          var sceneEl = this.el.sceneEl;
          if (!sceneEl.checkHeadsetConnected()) {
            return;
          }
          this.saveCameraPose();
          this.el.object3D.position.set(0, 0, 0);
          this.el.object3D.rotation.set(0, 0, 0);
          if (sceneEl.hasWebXR) {
            this.el.object3D.matrixAutoUpdate = false;
            this.el.object3D.updateMatrix();
          }
        },

        /**
         * Restore the pose.
         */
        onExitVR: function() {
          if (!this.el.sceneEl.checkHeadsetConnected()) {
            return;
          }
          this.restoreCameraPose();
          this.previousHMDPosition.set(0, 0, 0);
          this.el.object3D.matrixAutoUpdate = true;
        },

        /**
         * Update Pointer Lock state.
         */
        onPointerLockChange: function() {
          this.pointerLocked = !!(
            document.pointerLockElement || document.mozPointerLockElement
          );
        },

        /**
         * Recover from Pointer Lock error.
         */
        onPointerLockError: function() {
          this.pointerLocked = false;
        },

        // Exits pointer-locked mode.
        exitPointerLock: function() {
          document.exitPointerLock();
          this.pointerLocked = false;
        },

        /**
         * Toggle the feature of showing/hiding the grab cursor.
         */
        updateGrabCursor: function(enabled) {
          var sceneEl = this.el.sceneEl;

          function enableGrabCursor() {
            sceneEl.canvas.classList.add('a-grab-cursor');
          }
          function disableGrabCursor() {
            sceneEl.canvas.classList.remove('a-grab-cursor');
          }

          if (!sceneEl.canvas) {
            if (enabled) {
              sceneEl.addEventListener(
                'render-target-loaded',
                enableGrabCursor
              );
            } else {
              sceneEl.addEventListener(
                'render-target-loaded',
                disableGrabCursor
              );
            }
            return;
          }

          if (enabled) {
            enableGrabCursor();
            return;
          }
          disableGrabCursor();
        },

        /**
         * Save camera pose before entering VR to restore later if exiting.
         */
        saveCameraPose: function() {
          var el = this.el;

          this.savedPose.position.copy(el.object3D.position);
          this.savedPose.rotation.copy(el.object3D.rotation);
          this.hasSavedPose = true;
        },

        /**
         * Reset camera pose to before entering VR.
         */
        restoreCameraPose: function() {
          var el = this.el;
          var savedPose = this.savedPose;

          if (!this.hasSavedPose) {
            return;
          }

          // Reset camera orientation.
          el.object3D.position.copy(savedPose.position);
          el.object3D.rotation.copy(savedPose.rotation);
          this.hasSavedPose = false;
        },
      });
      this.scene = new THREE.Scene();
    },
    selectStyleTab(tabName) {
      this.selectedStyleTab = tabName;
      if (tabName == 'texture') {
        this.moveToTexture();
      } else {
        this.moveToTexture(-1);
      }
    },
    selectTexture(texture) {
      this.selectedTexture = texture;
    },
    selectStyle(style) {
      this.selectedStyle = style;
    },
    chosenTexture(texture) {
      if (this.selectedTexture === texture) {
        return true;
      } else {
        return false;
      }
    },
    chosenStyle(style) {
      if (this.selectedStyle === style) {
        return true;
      } else {
        return false;
      }
    },
    moveToTexture(dis) {
      var tg = document.querySelector('.drawer');
      if (dis == -1) {
        tg.style.WebkitTransform = `translateX(0px)`;
      } else {
        tg.style.WebkitTransform = `translateX(-50%)`;
      }
    },
    isMobile() {
      function isIOS() {
        return /iPad|iPhone|iPod/.test(window.navigator.platform);
      }
      function isTablet(mockUserAgent) {
        var userAgent = mockUserAgent || window.navigator.userAgent;
        return /ipad|Nexus (7|9)|xoom|sch-i800|playbook|tablet|kindle/i.test(
          userAgent
        );
      }
      function isAndroid() {
        var ua = navigator.userAgent.toLowerCase();
        var isAndroid = ua.indexOf('android') > -1; //&& ua.indexOf("mobile");
        return /Android/.test(window.navigator.userAgent) || isAndroid;
      }
      function isR7() {
        return /R7 Build/.test(window.navigator.userAgent);
      }
      var _isMobile = false;
      (function(a) {
        // eslint-disable-next-line no-useless-escape
        if (
          /(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|mobile.+firefox|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows ce|xda|xiino/i.test(
            a
          ) ||
          /1207|6310|6590|3gso|4thp|50[1-6]i|770s|802s|a wa|abac|ac(er|oo|s-)|ai(ko|rn)|al(av|ca|co)|amoi|an(ex|ny|yw)|aptu|ar(ch|go)|as(te|us)|attw|au(di|-m|r |s )|avan|be(ck|ll|nq)|bi(lb|rd)|bl(ac|az)|br(e|v)w|bumb|bw-(n|u)|c55\/|capi|ccwa|cdm-|cell|chtm|cldc|cmd-|co(mp|nd)|craw|da(it|ll|ng)|dbte|dc-s|devi|dica|dmob|do(c|p)o|ds(12|-d)|el(49|ai)|em(l2|ul)|er(ic|k0)|esl8|ez([4-7]0|os|wa|ze)|fetc|fly(-|_)|g1 u|g560|gene|gf-5|g-mo|go(\.w|od)|gr(ad|un)|haie|hcit|hd-(m|p|t)|hei-|hi(pt|ta)|hp( i|ip)|hs-c|ht(c(-| |_|a|g|p|s|t)|tp)|hu(aw|tc)|i-(20|go|ma)|i230|iac( |-|\/)|ibro|idea|ig01|ikom|im1k|inno|ipaq|iris|ja(t|v)a|jbro|jemu|jigs|kddi|keji|kgt( |\/)|klon|kpt |kwc-|kyo(c|k)|le(no|xi)|lg( g|\/(k|l|u)|50|54|-[a-w])|libw|lynx|m1-w|m3ga|m50\/|ma(te|ui|xo)|mc(01|21|ca)|m-cr|me(rc|ri)|mi(o8|oa|ts)|mmef|mo(01|02|bi|de|do|t(-| |o|v)|zz)|mt(50|p1|v )|mwbp|mywa|n10[0-2]|n20[2-3]|n30(0|2)|n50(0|2|5)|n7(0(0|1)|10)|ne((c|m)-|on|tf|wf|wg|wt)|nok(6|i)|nzph|o2im|op(ti|wv)|oran|owg1|p800|pan(a|d|t)|pdxg|pg(13|-([1-8]|c))|phil|pire|pl(ay|uc)|pn-2|po(ck|rt|se)|prox|psio|pt-g|qa-a|qc(07|12|21|32|60|-[2-7]|i-)|qtek|r380|r600|raks|rim9|ro(ve|zo)|s55\/|sa(ge|ma|mm|ms|ny|va)|sc(01|h-|oo|p-)|sdk\/|se(c(-|0|1)|47|mc|nd|ri)|sgh-|shar|sie(-|m)|sk-0|sl(45|id)|sm(al|ar|b3|it|t5)|so(ft|ny)|sp(01|h-|v-|v )|sy(01|mb)|t2(18|50)|t6(00|10|18)|ta(gt|lk)|tcl-|tdg-|tel(i|m)|tim-|t-mo|to(pl|sh)|ts(70|m-|m3|m5)|tx-9|up(\.b|g1|si)|utst|v400|v750|veri|vi(rg|te)|vk(40|5[0-3]|-v)|vm40|voda|vulc|vx(52|53|60|61|70|80|81|83|85|98)|w3c(-| )|webc|whit|wi(g |nc|nw)|wmlb|wonu|x700|yas-|your|zeto|zte-/i.test(
            a.substr(0, 4)
          )
        ) {
          _isMobile = true;
        }
        if (isIOS() || isTablet() || isR7() || isAndroid()) {
          _isMobile = true;
        }
      })(window.navigator.userAgent || window.navigator.vendor || window.opera);

      return function() {
        return _isMobile;
      };
    },
  },
};
</script>

<style lang="scss" scoped>
canvas {
  border-radius: 10px;
}

.preview {
  height: 120px;
  width: 100px;
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  margin-bottom: 10px;
}

.tabs {
  display: flex;
  width: 100%;
  .tab {
    width: 50%;
    text-align: center;
    padding: 10px 0;
    cursor: pointer;
    color: rgb(143, 143, 143);
    &.activeTab {
      border-radius: 5px;
      background-color: #6221c3;
      color: rgb(255, 255, 255);
    }
    &.activeColor {
      border-radius: 5px;
      background-color: #2c3daf;
      color: rgb(255, 255, 255);
    }
  }
}

.sceneStyle {
  margin: 10px;
  height: initial !important;
  box-sizing: border-box;
  border-radius: 10px;
}
.demoPage {
  height: 90vh;
  position: relative;
  padding: 0vw 1vw 1vw 1vw;
  box-sizing: border-box;
  display: flex;
}

.textImg {
  width: 100%;
  position: relative;
  cursor: pointer;
}
.img {
  box-sizing: border-box;
  width: 100%;
  aspect-ratio: 2/3;
  background-size: cover;
  background-position: center;
  border-radius: 5px;
  position: relative;
  box-shadow: 0 0 15px 0 rgba(0, 0, 0, 0.268);
}

.active {
  pointer-events: none;
  margin: 1px;
  border: 3px solid rgba(255, 28, 28, 0.821);
}
.outer {
  display: flex;
  flex-direction: column;
  min-width: 15vw;
  max-width: 150px;
  position: relative;
  overflow-x: hidden;
  &.hideDrawer {
    min-width: 0px !important;
    width: 0 !important;
    max-width: 0;
  }
}
.drawer {
  display: flex;
  width: 200%;
  height: 100%;
  overflow: hidden;
  transition: 0.5s;
  margin: 10px 0px;
}
.rightGrid {
  transform: translateX(100%);
}
.grid {
  width: 100%;
  display: grid;
  height: max-content;
  max-height: 100%;
  grid-template-columns: 1fr 1fr;
  box-sizing: border-box;
  overflow-y: scroll;
  gap: 0.5rem;
}

.monitor {
  position: absolute;
  text-align: left;
  font-size: 0.8rem;
  top: 0;
  background-color: #aeaeae6d;
  z-index: 100;
  padding: 0.5rem;
}

.drawerBtn {
  border: 1px solid #aeaeae;
  border-radius: 5px;
  padding: 0.1rem;
  cursor: pointer;
  text-align: center;
  margin-top: 0.5rem;
  background-color: #aeaeae6d;
  transition: 0.5s;
  &:hover {
    background-color: #aeaeae;
  }
}

a-scene {
  width: 100%;
  height: 100%;
}

.spec-preview {
  position: relative;
  width: 100px;
  height: 120px;
  padding: 10px 0px;
  img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }
  .event {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 998;
  }

  .mask {
    width: 50%;
    height: 50%;
    background-color: rgba(0, 255, 0, 0.3);
    position: absolute;
    left: 0;
    top: 0;
    display: none;
  }

  .big {
    width: 200%;
    height: 200%;
    position: absolute;
    top: -1px;
    left: 120%;
    overflow: hidden;
    z-index: 998;
    display: none;
    background: rgba(0, 0, 0, 0.268);
    .pd {
      width: 150%;
      height: 150%;
      position: absolute;
      object-fit: contain;
      left: 0;
      top: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      img {
        width: 90%;
        height: 90%;
        object-fit: contain;
      }
    }
  }
  .event:hover ~ .mask,
  .event:hover ~ .big {
    display: block;
  }
}
</style>
