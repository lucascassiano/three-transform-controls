# three-transform-controls

ThreeJS TransformControls as an npm module. 
See example with react-three-renderer.

![Alt Text](https://github.com/lucascassiano/three-transform-controls/raw/master/docs/moving_plane.gif )

## Install
```
npm install three-transform-controls
```

## Example

```js
import React, { Component } from 'react';
import React3 from 'react-three-renderer';
import * as THREE from 'three';
import ReactDOM from 'react-dom';

var TransformControls = require('../controls/TransformControls')(THREE);

export default class Example extends Component {
  constructor(props, context) {
    super(props, context);

    this.cameraPosition = new THREE.Vector3(0, 0, 5);

    this.state = {
      cubeRotation: new THREE.Euler(),
    };

    this._onAnimate = () => {
      // we will get this callback every frame
    };
  }


  componentDidMount() {

    var material = new THREE.MeshBasicMaterial({
      color: "#1300FF",
      transparent: true,
      depthWrite: false
    });

    var object = new THREE.Object3D();
    var plane = new THREE.Mesh(new THREE.PlaneGeometry(25, 5), material);
    var control = new TransformControls(this.refs.camera, ReactDOM.findDOMNode(this.refs.react3));
    //control.addEventListener('change', render);
    control.attach( plane);
    object.add(control);
    object.add(plane);
    

    this.refs.group.add(object);
  }


  render() {
    const width = window.innerWidth; // canvas width
    const height = window.innerHeight; // canvas height

    return (
      <React3
        mainCamera="camera" // this points to the perspectiveCamera which has the name set to "camera" below
        width={width}
        height={height}
        ref="react3"
        onAnimate={this._onAnimate}
        antialias
        alpha={false}
      >
        <module
          ref="mouseInput"
          descriptor={MouseInput}
        />

        <scene>
          <perspectiveCamera
            ref="camera"
            name="camera"
            fov={75}
            aspect={width / height}
            near={0.1}
            far={1000}
            position={this.cameraPosition}
          />

          <group ref='group' />
          <gridHelper
            size={10}
            colorGrid={"#040404"}
          />

          <mesh
            rotation={this.state.cubeRotation}
          >
            <boxGeometry
              width={1}
              height={1}
              depth={1}
            />
            <meshBasicMaterial
              color={0x00ff00}
            />
          </mesh>


        </scene>
      </React3>
    );
  }
}

```



