# three-orbit-controls

[![stable](http://badges.github.io/stability-badges/dist/stable.svg)](http://github.com/badges/stability-badges)

ThreeJS TransformControls as an npm module. See [test](#testing) for an example.

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

## Usage

[![NPM](https://nodei.co/npm/three-orbit-controls.png)](https://nodei.co/npm/three-orbit-controls/)

#### `OrbitControls = require('three-orbit-controls')(THREE)`

This module exports a function which accepts an instance of THREE, and returns an OrbitControls class. This allows you to use the module with CommonJS, globals, etc.

The returned function has the following constructor pattern:

```js
var control = new TransformControls(this.refs.camera, ReactDOM.findDOMNode(this.refs.react3));

control.attach( plane);)
```

#### Versioning

This uses an unusual versioning system to better support ThreeJS's (lack of) versioning. The major version of this repo will line up with ThreeJS breaking releases (`69.0.0` => `r69`). Often the module will continue to work (i.e. `69.0.0` should work with r70).

The minor will be reserved for any new features, and patch for bug fixes and documentation/readme updates. In some rare cases, a minor feature may introduce a breaking change; so it's generally safest to use tilde or `--save-exact` for this module.

If you see any version issues, open a ticket!

## testing

Git clone, `npm install` and then run `npm start` to spin up a development server. Open `localhost:9966` in your browser to see the `test.js` file in action.
