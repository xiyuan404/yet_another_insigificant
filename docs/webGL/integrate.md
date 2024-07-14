## demo


<iframe  src="https://sbedit.net/3cfece960e8cbd9cbe5478993dd4c11f95468dcd"  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"  allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"  sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>



### objects

we are goint to create an object for each section to illustrate each of them.

To keep things simple , we will use Three.js build in object,. Later, we will change to use  custom model 

in `/src/scrpt.js` (at the  project root )

> ` MeshToonMaterial`：  tool shading is a type of non-photorealistic rendering technique disigned to make 3D computer graphics appear more cartoonish by using less shading color instead of a smooth gradient effect.

```js
// material 
const material = new THREE.MeshToonMaterial({color: parameter.materialColor})

// Meshs
const mesh1 = new THREE.Mesh(
	new THREE.TorusGeometry(1, .4, 16, 60)
  material
)
const mesh2 = new THREE.Mesh(
	new THREE.ConeGeometry(1,2,32),
  material
)
const mesh3 = new THREE.Mesh(
	new THREE.TorusKnotGeometry(.8, .35, 100,16)
  material
)
scene.add(mesh1, mesh2, mesh3)
```



`MeshToonMaterial` is one of the material that appears only when there is  light

Add one DirectionalLight to the scene

```js
const directionalLight = new THREE.DirectionalLight('#ffffff', 3)
directionalLight.position.set(1,1,0)
scene.add(directionalLight)
```

we are using the color stored in the `parameters` object, but changing this value with the Tweaker doesn't change the material itself

To fix that, we can listen to change event on the already existing tweak and update the material accordingly

```js
gui.addColor(parameters, 'materialColor').onChange((changeToValue)=> material.color.set(changeToValue))
```

`MeshToonMaterial` will have one color for the light side, one darker color for the shade side,

gradient images provided in the `/static/textures/gradients/` folder.

instantiate the `TextureLoader` before instaniating the `material`. then load the `textures/gradients/3.jpg`

```js
const textureLoader = new THREE.TextureLoader()
const gradientTexture = textureLoader.load('textures/gradients/3.jpg')
```

use it as the value of `gradientMap` property of the material

```js
const material = new THREE.MeshToonMaterial({
  color: parameters.materialColor,
  gradientMap: gradientTexture
})
```

By default three.js try to inperpolate the pixels instead of picking the nearest pixel on the texture.

To fix that, we need to set the `magFilter` of the texture to the `THREE.NearestFilter` so that hte closest pixel is used without interpolating i