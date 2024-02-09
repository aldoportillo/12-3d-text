# 3D Text

## Storage and Loading Font

To store your font - In static folder, create fonts folder and add the font and license to the folder.

To Load your font use THREE.FontLoader class. You can only have one.

```javascript
import {FontLoader} from 'three/examples/jsm/loaders/FontLoader.js'

const fontLoader = new FontLoader()


fontLoader.load('fonts/helvetiker_regular.typeface.json', () => {
    console.log("font loaded")
})
```

## Implement Geometry

```javascript

fontLoader.load('fonts/helvetiker_regular.typeface.json', (font) => {
    const textGeometry = new TextGeometry(
        'Aldo Portillo',
        {
            font: font,
            size: 0.5,
            height: 0.2,
            curveSegments: 5, //Controls triangles around curves
            bevelEnabled: true,
            bevelThickness: 0.03,
            bevelSize: 0.02,
            bevelOffset: 0,
            bevelSegments: 4, //controls triangles between edges
        }
    )
    const textMaterial = new THREE.MeshBasicMaterial()
    textMaterial.wireframe = true
    const text = new THREE.Mesh(textGeometry, textMaterial)
    scene.add(text)
})



```

## Center Text

Add axis helper to help us center the scene.

```javascript

const axesHelper = new THREE.AxesHelper()
scene.add(axesHelper)

```

Use bounding to get information that tells what space is taken by the geometry.

You have box or sphere bounding for Fustrum culling. Fustrum culling is calculation to see if an object is on the screen

```javascript

fontLoader.load('fonts/helvetiker_regular.typeface.json', (font) => {
    const textGeometry = new TextGeometry(
        'Aldo Portillo',
        {
            font: font,
            size: 0.5,
            height: 0.2,
            curveSegments: 5, //Controls triangles around curves
            bevelEnabled: true,
            bevelThickness: 0.03,
            bevelSize: 0.02,
            bevelOffset: 0,
            bevelSegments: 4, //controls triangles between edges
        }
    )

    //new code
    textGeometry.computeBoundingBox()
    console.log(textGeometry.boundingBox)
    textGeometry.translate(
        - (textGeometry.boundingBox.max.x - 0.02) * 0.5, // Subtract bevel size
        - (textGeometry.boundingBox.max.y - 0.02) * 0.5, // Subtract bevel size
        - (textGeometry.boundingBox.max.z - 0.03) * 0.5  // Subtract bevel thickness

    )
    
    const textMaterial = new THREE.MeshBasicMaterial()
    textMaterial.wireframe = true
    const text = new THREE.Mesh(textGeometry, textMaterial)
    scene.add(text)
})

```

We can see that the bevel leads us to get negative values with our computeBoundingBox. So we need to move the geometry not the mesh. And we use the BoundingBox (vector 3) max value to center it but easier method is.

```javascript
textGeometry.center()
```

## Add Texture using matcaps

```javascript

const textureLoader = new THREE.textureLoader
const matcapTexture = textureLoader.load("/textures/matcaps/1.png")
matcapTexture.colorSpace = THREE.SRGBColorSpace

fontLoader.load('fonts/helvetiker_regular.typeface.json', (font) => {
    const textGeometry = new TextGeometry(
        'Aldo Portillo',
        {
            font: font,
            size: 0.5,
            height: 0.2,
            curveSegments: 5, //Controls triangles around curves
            bevelEnabled: true,
            bevelThickness: 0.03,
            bevelSize: 0.02,
            bevelOffset: 0,
            bevelSegments: 4, //controls triangles between edges
        }
    )

    textGeometry.center()
    const textMaterial = new THREE.MeshMatcapMaterial()
    textMaterial.matcap = matcapTexture
    const text = new THREE.Mesh(textGeometry, textMaterial)
    scene.add(text)
})


```

## Add Geometries

```javascript

fontLoader.load('fonts/helvetiker_regular.typeface.json', (font) => {
    const textGeometry = new TextGeometry(
        'Aldo Portillo',
        {
            font: font,
            size: 0.5,
            height: 0.2,
            curveSegments: 5, //Controls triangles around curves
            bevelEnabled: true,
            bevelThickness: 0.03,
            bevelSize: 0.02,
            bevelOffset: 0,
            bevelSegments: 4, //controls triangles between edges
        }
    )

    textGeometry.center()
    const material = new THREE.MeshMatcapMaterial()
    material.matcap = matcapTexture
    const text = new THREE.Mesh(textGeometry, material)
    scene.add(text)

    const donutGeometry = new THREE.TorusGeometry(0.3, 0.2, 20, 45)

    for(let i = 0; i < 100; i++) {

        const donut = new THREE.Mesh(donutGeometry, material)
        
        //Add randomness

        donut.position.x = (Math.random() - 0.5) * 10
        donut.position.y = (Math.random() - 0.5) * 10
        donut.position.z = (Math.random() - 0.5) * 10

        donut.rotation.x = Math.random() * Math.PI
        donut.rotation.y = Math.random() * Math.PI

        const scale = Math.random()
        donut.scale.set(scale, scale, scale)
        scene.add(donut)
    } 

})

```
